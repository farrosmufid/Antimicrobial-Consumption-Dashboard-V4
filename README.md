# Antimicrobial Consumption Dashboard V4

<img width="1505" alt="Update_data_page" src="https://github.com/farrosmufid/Antimicrobial-Consumption-Dashboard-V4/assets/31735132/f97469d8-d0eb-4ee0-ab7c-a93905b06438">

## Installation:
1. Install Python 3.10 [Install Python 3.10](https://www.python.org/downloads/release/python-3100/).
2. Install the dependencies (using pip) `pip3 install -r requirements.txt`.
3. Make sure you are in the same directory as app.py, then run `python3 app.py`.
4. Navigate to http://127.0.0.1:8050/ on your web browser.

## Prerequisites (How to run the dashboard):


### 1. Orders Data

1. To make it work, ensure that `final_df.csv` is located in the `orders_data_clean` folder.
2. Ensure that there's at least one month of data in the `orders_data_dump` folder. You can add or delete contents from the `orders_data_dump` folder, that folder is for the raw data that hasn't been transformed.
3. Once executed, the program will generate both `clean.csv` and `final_df.csv` in the `orders_data_clean` folder.
4. DO NOT DELETE `final_df.csv` as this file is the primary source from which the dashboard loads the entire dataset.
5. `clean.csv` contains data that hasn't been transformed, whereas `final_df.csv` contains the transformed data.
6. Whenever new data is uploaded in the update_data section of the antibiotic orders, both `clean.csv` and `final_df.csv` will be updated.
7. To remove or edit data from the dashboard, delete clean.csv. Any deletions or edits should be made in the orders_data_dump_folder. This folder contains all the data that has been uploaded to the dashboard. The dashboard will automatically generate a new `clean.csv` for you.
8. After the dashboard starts, click "refresh order data." It will process all the files in the orders_data_dump folder, clean them, and then load them onto the dashboard.
9. If you want to append data, there is no need to delete `clean.csv`. You can use the update_data section to add more data. There, you can upload multiple new files.
10. Another way to add data is to directly place it in the `orders_data_dump` folder and click "refresh order data".

### 2. Resistance Data

1. To ensure functionality, make sure you have at least one month's worth of resistance data in the `resistance_data_dump` folder.
2. This process will generate `resistance_df.csv` (the combined resistance data), `resistance_count_df.csv` (the cleaned and transformed data), and `resistance_col_df.csv` (for antibiogram).
3. The visualization will display all the contents from the `resistance_data_dump` folder. You can add or remove files in this location.
4. To remove or edit data, navigate to the resistance_data_dump folder. This folder contains all the data uploaded to the dashboard. Make your edits or deletions within this folder.
5. After the dashboard starts, click "refresh susceptibility data." It will process all the files in the resistance_data_dump folder, clean them, and then load them onto the dashboard.
6. To add a new dataset, visit the update_data page and navigate to the update_data section for resistance. There, you can upload multiple new files.
