# Metric Definition & Data Processing Steps

## 1. Metric Definition

| Attribute | Documentation Page| Definition | 
| :--------: | :--------: | -------- |
| **ASC Score** | High Level Trends| A score ranging from 2 to 15. Antibiotic spectrum coverage score is a scoring system for antibiotic spectrums. For further information please see https://academic.oup.com/cid/article/75/4/567/6463001 |
| **dASC** | High Level Trends|Days of antibiotic spectrum coverage is calculated by multiplying ASC score and DOT. <br>ASC Score x DOT = dASC |
| **DOT** | High Level Trends| Days of Therapy = The number of days a patient was subject to a single type of antibiotic treatment within the same admission. <br> Stop Date - Start Date + 1 (Aggregated by the same antibiotic type) | Days of Therapy = The number of days a patient was subject to a single type of antibiotic treatment within the same admission. <br> Stop Date - Start Date + 1 (Aggregated by the same antibiotic type) |
| **Length of Stay** | High Level Trends| The number of days between admission date and discharge date. <br> Discharge Date - Admit Date + 1 |
| **DDD** | High Level Trends| Defined Daily Dose = The assumed average maintenance dose per day for a drug used for its main indication in adults. The prescribed exact total dose of antibiotic A divided by antibiotic A’s DDD (match administration route). |
| **Susceptibility Rate** | Resistance | The susceptibility rate refers to the proportion of organisms that are not resistant to a given antimicrobial. <br>S (Susceptible) Tests + S-DD (Susceptible Dose Dependent) Tests + S-IE (Susceptible Increased Exposure) Tests / Total Tests |
| **Count of Tests** | Resistance | The total number of tests completed during a specified timeframe and with filters applied. Only the first antibiotic/organism test result per patient in the given timeframe will be counted to limit duplication and inflation of results. <br>S (Susceptible) Tests + S-DD (Susceptible Dose Dependent) Tests + S-IE (Susceptible Increased Exposure) Tests + I (Intermediate) Tests + R (Resistant) Tests = Total Tests |

## 2. Data Processing Steps

### Cleaning & Update Process
#### a) Cleaning Function
1. Remove empty rows
2. Select columns of interest
3. Remove dummy patient
4. Extract number from column Age, Actual_weight, Height
5. Process EGFR, leave as ‘>90’
6. Extract first word to fill in order_generic
7. Drop missing value in location of patient
8. Remove rows that are shifted
9. Convert datetime column from string to datetime type
10. Fill in missing dttm with order palced time if frequency is once
11. Remove medical officer and senior medical officer
12. Transforming frequency to numeric values

#### b) Update Function
1. Extract rows in existing file which have same MRN as in the new file
2. Join the extract file with new file and remove duplicate
3. Fill in discharge date, weight, height
4. Join all dataframe together and remove duplicates

### Defined Daily Dose (DDD) Calculation Process
Total_DDD = DOSE x FREQUENCY x (START_DTTM - STOP_DTTM + 1) / DDD_metric (match ORDER_GENERIC and RX_ROUTE).

1. Exclude prescription orders with topical routes that cannot be quantified. <br>VOLUME_DOSE = ‘drop’, ‘application’, and RX_ROUTE = ‘EYE’, ‘NOSTRIL’. 
2. Exclude rows with conflicting information in ORDER_NAME and VOLUME_DOSE. <br>e.g. ‘tablets’ in ORDER_NAME but ‘mL’ in VOLUME_DOSE. 
3. Go through each row and check for validity of the DOSE value.
4. If DOSE is valid, convert DOSE value with only ‘mg’ and ‘unit’.
5. If DOSE is not valid, extract information from ORDER_NAME and VOLUME_DOSE to use ORDER_NAME x VOLUME_DOSE as new DOSE. 
6. Retain rows that have a valid STOP_DTTM so that the number of days each order lasts for can be known. 
7. Convert RX_ROUTE into three categories to match with the route type with WHO DDD information: O as Oral, P as Intravenous, and Inhal.solution as Inhalation. 
8. Go through each row and calculate the total_dosage. Match the ORDER_GENERIC and RX_ROUTE with the values from the WHO DDD metric table.
9. Calculate Total_DDD.

If the below fields are missing values, the DDD metric will not be derived.
1. (Quantifiable) DOSE / ORDER_NAME + VOLUME_DOSE
2. (Quantifiable) FREQUENCY
3. START_DTTM
4. STOP_DTTM
5. RX_ROUTE (O, P, Inhal.solution)
6. DDD metric (if there is a WHO DDD metric for that ORDER_GENERIC administered in that RX_ROUTE)
   
Instructions to make changes to the WHO DDD metric table (csv):
- The current WHO DDD metric is saved in the cleaned_cur_antibioDDDinfo.csv file.
- It has five columns: ‘ATC code’, ‘name’, ‘DDD’, ‘U’, ‘Adm.R’ and ‘Note’. Only the ‘name’, ‘DDD’, ‘Adm.R’, and ‘U’ columns would be used for DDD calculations. ‘Name’ for antibiotic name, ‘DDD’ for DDD metric, ‘Adm.R’ for administered route, and ‘U’ for unit. For easy calculation, only ‘mg’ and ‘unit’ are kept in the ‘U’ column.
- If there is a new WHO DDD metric of an antibiotic type (or combination) that has not been included in the table, please manually fill the information in the table.
- The value in the ‘name’ field needs to be in all lower-case and have the exact spelling as the one in hospital data ORDER_GENERIC field.
- The value in the DDD field needs to be a number with no comma (convert if the original DDD metric has ‘g’ as unit).
- The value in the ‘U’ field needs to be either ‘mg’ or ‘unit’, no exception.
- The value in the Adm.R needs to be either ‘O’, ‘P’, or ‘R’, no exception. If this new change has DDD metric for multiple routes, the name columns must be filled. 
- If WHO has changed the DDD metric for an antibiotic type (or combination), please manually change the ‘DDD’ field accordingly (convert value if the unit is ‘g’).

### Days of Therapy (DOT) & Days of Antibiotic Spectrum Coverage (dASC) Calculation Process
1. Clean data frame is processed and records where MRN, ADMIT_DATE, ORDER_GENERIC, START_DTTM and STOP_DTTM are dropped. The availability of these fields are required to produce the metrics.
2. Remove duplicate records present across the 5 fields.
3. Partition the data by MRN, ADMIT_DATE, ORDER_GENERIC and START_DTTM and sort in ascending order.
4. Go through each row, take the current record and compare against the next. (To deal with overlapping treatment dates.)
5. If MRN, ADMIT_DATE, ORDER_GENERIC don’t match, capture the current record.
6. If MRN, ADMIT_DATE, ORDER_GENERIC match, deal with any overlapping time periods (START_DTTM to STOP_DTTM) to ensure overcalculation doesn’t occur. If they’re no overlaps, capture the current record.
7. The captured records should contain a unique row for each MRN, ADMIT_DATE and ORDER_GENERIC. 
8. Take all the captured records and calculate Days of Therapy (DOT) = STOP_DTTM - START_DTTM + 1 (Days between Stop Date and Start Date aggregated by antibiotic type.)
9. Take the sum of therapy days for the same ORDER_GENERIC value to produce the final aggregated view for DOT.
10. Calculate Days of Antibiotic Spectrum Coverage (dASC) by multiplying DOT and ASC_SCORE. ASC_SCORE is an editable file containing a list of all ORDER_GENERIC types in our reports that have a ASC score listed on https://academic.oup.com/cid/article/75/4/567/6463001.
11. Join the new DOT and dASC metric with the first START_DTTM for each antibiotic type per admission. The first of these types of records will be summary rows for these metrics.

### Susceptibility 
#### a) Cleaning Process
1. COLLECT_DT_TM converted to date format: DD/MM/YYYY
2. CLIENT is set to Westmead Hospital to only include test results from Westmead
3. Columns that are kept include: ORDERABLE, ACCESSION, MRN, DATE_OF_BIRTH, COLLECT_DT_TM, ORGANISM_NAME, CLIENT, NURSE_LOC, ADMITTING_MO, MED_SERVICE, ANTIBIOTIC, INTERP. All other columns are removed for performance
4. Rows containing only a valid result from INTERP are selected: S, R, S-IE, S-DD, I
5. The words “Medical Officer” and “Specialist Medical Officer” are removed from values of ADMITTING_MO, leaving only name of doctor
6. Any duplicate rows are removed
7. This cleaned data is then pivoted to create file that can be downloaded, with information for MRN, DATE_OF_BIRTH, ACCESSION, COLLECT_DT_TM, ORDERABLE, MED_SERVICE, NURSE_LOC, ADMITTING_MO, ORGANISM_NAME remaining the same but then the all the values from ANTIBIOTIC are pivoted to be individual columns and the test result of INTERP to be the value under each of these columns

#### b) Aggregation Process
1. ACCESSION is removed as not required for calculations and duplicate rows removed
2. 6 Columns are created: Count_S, Count_R, Count_S-IE, Count_S-DD, Count_I and sum
3. The sum of each of the test results is calculated aggregated by the existing columns
4. In order to fix a data issue, any row that has multiple results for the same test, a hierarchy is created taking the most severe result. For example if a patient has a resistant and a susceptible result for the same test, the resistant one is chosen. The hierarchy is R, S-IE, S
5. Null values for MED_SERVICE are replaced by N/A
