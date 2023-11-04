### Metric Definition

| Attribute | Documentation Page| Definition | 
| :--------: | :--------: | -------- |
| **ASC Score** | High Level Trends| A score ranging from 2 to 15. Antibiotic spectrum coverage score is a scoring system for antibiotic spectrums. For further information please see https://academic.oup.com/cid/article/75/4/567/6463001 |
| **dASC** | High Level Trends|Days of antibiotic spectrum coverage is calculated by multiplying ASC score and DOT. <br>ASC Score x DOT = dASC |
| **DOT** | High Level Trends| Days of Therapy = The number of days a patient was subject to a single type of antibiotic treatment within the same admission. <br> Stop Date - Start Date + 1 (Aggregated by the same antibiotic type) | Days of Therapy = The number of days a patient was subject to a single type of antibiotic treatment within the same admission. <br> Stop Date - Start Date + 1 (Aggregated by the same antibiotic type) |
| **Length of Stay** | High Level Trends| The number of days between admission date and discharge date. <br> Discharge Date - Admit Date + 1 |
| **DDD** | High Level Trends| Defined Daily Dose = The assumed average maintenance dose per day for a drug used for its main indication in adults. The prescribed exact total dose of antibiotic A divided by antibiotic A’s DDD (match administration route). |
| **Susceptibility Rate** | Resistance | The susceptibility rate refers to the percentage that a particular antibiotic is susceptible to fighting infection. <br>S (Susceptible) Tests + S-DD (Susceptible Dose Dependent) Tests + S-IE (Susceptible Increased Exposure) Tests / Total Tests |
| **Count of Tests** | Resistance | The total number of tests completed during a specified timeframe and with filters applied. Only the first antibiotic/organism test result per patient in the given timeframe will be counted to limit duplication and inflation of results. <br>S (Susceptible) Tests + S-DD (Susceptible Dose Dependent) Tests + S-IE (Susceptible Increased Exposure) Tests + I (Intermediate) Tests + R (Resistant) Tests = Total Tests |

### Data Processing

A: Cleaning Process & Update
Cleaning Function
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

Update Function
1. Extract rows in exisitng file which have same MRN as in the new file
2. Join the extract file with new file and remove duplicate
3. Fill in discharge date, weight, height
4. Join all dataframe together and remove duplicates

B: Defined Daily Dose (DDD)
DOSE * FREQUENCY * (START_DTTM - STOP_DTTM + 1) / DDD_metric (match ORDER_GENERIC and RX_ROUTE) = Total_DDD.
If DOSE not valid, extract information from ORDER_NAME and VOLUME_DOSE and use ORDER_NAME*VOLUME_DOSE as DOSE.
The calculation process first begins with excluding prescriptions with topical routes that cannot be quantified (keywords like ‘drop’, ‘application’ in the VOLUME_DOSE and ‘EYE’, ‘NOSTRIL’ in RX_ROUTE). 
Second, it excludes rows with conflicting information in ORDER_NAME and VOLUME_DOSE (e.g. ‘tablets’ in ORDER_NAME but ‘mL’ in VOLUME_DOSE). 
Then, it traverses through all the rows and checks for validity of the value in the DOSE column. If DOSE is valid, convert DOSE value with only ‘mg’ and ‘unit’. If DOSE is not valid, extract information from ORDER_NAME and VOLUME_DOSE to calculate the DOSE as ORDER_NAME * VOLUME_DOSE. 
Then, it converts the FREQUENCY value into the number of times the prescribed dosage is taken per day. 
After these transformations, the code only keeps the rows that have valid STOP_DTTM so that the number of days each order lasts for can be known. 
Next, the code converts RX_ROUTE into three categories to match with the route type with WHO DDD information: O as Oral, P as Intravenous, and Inhal.solution as Inhalation. 
After this, all the values needed for DDD calculation have been produced. The code goes through all current rows again, calculates the total_dosage, matches the ORDER_GENERIC and RX_ROUTE with what’s in the WHO DDD metric table, and calculates the total DDD (refer to the equation above).
The following are the required dependencies for DDD calculations. If a row/order has missing value in at least one of the columns listed below,  it is impossible to calculate the DDD for that prescription row.
1. [ (Quantifiable) DOSE / ORDER*NAME + VOLUME_DOSE ], 2. (Quantifiable) FREQUENCY, 3. START_DTTM, 4. STOP_DTTM, 5. RX_ROUTE (O, P, Inhal.solution), 6. DDD metric (if there is a WHO DDD metric for that ORDER_GENERIC administered in that RX_ROUTE). 
Direction on making changes to the WHO DDD metric table csv file:
The current WHO DDD metric sits in the cleaned_cur_antibioDDDinfo.csv file. It has five columns: ‘ATC code’, ‘name’, ‘DDD’, ‘U’, ‘Adm.R’ and ‘Note’. Only the ‘name’, ‘DDD’, ‘Adm.R’, and ‘U’ columns would be used for DDD calculations. ‘Name’ for antibiotic name, ‘DDD’ for DDD metric, ‘Adm.R’ for administered route, and ‘U’ for unit. For easy calculation, only ‘mg’ and ‘unit’ are kept in the ‘U’ column.   
If there is a new WHO DDD metric of an antibiotic type (or combination) that has not been included in the table, please manually fill the information in the table. The value in the ‘name’ field needs to be in all lower-case and have the exact spelling as the one in hospital data 

