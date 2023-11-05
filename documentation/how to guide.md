# How to Guide

## 1. Update Page
![image](https://github.com/farrosmufid/Antimicrobial-Consumption-Dashboard-V4/assets/93821446/cb0d1dac-5abc-4c3c-a869-e4337479d765)

### Description
The update page contains 2 areas to upload antibiotic orders data and susceptibility results data. 
<br>Upload your raw data in the relevant boxes for cleaning and transformation. If the file is not in the accepted format, it will throw an error. 
<br>Files uploaded will have their minimum and maximum date listed. 
<br>If you've uploaded a file that you wish to remove, simply go to the orders_data_dump folder and delete that file. Then proceed to click "Refresh order data" or "Refresh susceptibility data" to reload the cleaning and transformation process.
<br>Click "Download cleaned data csv" to get a cleaned copy of your raw data.

## 2. High Level Trends
| Chart | Description | Visualisation |
| :-: | - | - |
| **Tabular Filters** | The date filter on top, which is specific for the table below, allows users to decide the period of interest. The button on the right allows you to remove the filter and show the full date range of stats. The five options on top of table allows users to check DOT, DDD and dASC stats specific to each medical service, ward, doctor, and AMS indication. ||
| **Chart Filters** | These filters have overall control of high-level visualisations. These filters give users the ability to specify the type of order status, order generic, medical service, ward, doctor and AMS indication that they want to look at. We have 5 tabs and each tab allows users to see the trends of average dASC, total DOT, total DDD and average DOT against time. ||

## 3. Deep Dive
| Chart | Description | Visualisation |
| :-: | - | - |
| **Correlation Analysis** | Filter and observe the relationship between 2 variables for analysis. <br>The scatterplot on the left shows the relationship between two chosen metrics. <br>The charts on the right show the trends of those metrics based on the data point (ORDER_GENERIC) that is hovered on. <br>There is an option to download a csv of the report you have displayed. | ![image](https://github.com/farrosmufid/Antimicrobial-Consumption-Dashboard-V4/assets/93821446/649965c8-a7f9-4988-84fd-af8685eeeae1)|
| **Age, Weight, Exact Dose (3D Scatter Plot)** | Used the filter to select a type of order generic and analyse the relationship between dosage, patient’s weight and age. <br>The 3D scatterplot is interactive, the user can zoom in and out and drag the chart to move the view. <br>Only 1 order generic can be selected at a time. <br>There is an option to download a csv of the report you have displayed. | ![image](https://github.com/farrosmufid/Antimicrobial-Consumption-Dashboard-V4/assets/93821446/1345e0d6-b841-4251-9b5f-a20d3b97aa47) |
| **EGFR and First 24 hr Dose (2D Scatter Plot)** | This 2D scatter plot shows the EGFR values in relation to a patient’s first 24 hours dosage. <br>There is an option to download a csv of the report you have displayed. <br>The filters at the top affect the data points in this chart. | ![image](https://github.com/farrosmufid/Antimicrobial-Consumption-Dashboard-V4/assets/93821446/97a79a6d-5490-4d9c-ad88-7a716b54e497)|
| **Length of Stay vs. Number of Patients (Bar Graph)** | This bar chart displays the number of patients and their length of stay. <br>There are separate filters for selection for this chart, including Order Generic, AMS Indication, Frequency and Dose. | ![image](https://github.com/farrosmufid/Antimicrobial-Consumption-Dashboard-V4/assets/93821446/3ebaa9c3-cf6f-42e3-b86b-e1dc42ca9492)|

## 4. Forecast
The forecast page shows three visualisations that include current and forecasting value of the monthly average dASC of antibiotic types. The three line chart visualisations show increasing trend, stable trend, and decreasing trend separately. The visualisations would only display antibiotic types that have at least 4 different monthly records in the current imported data.
| Chart | Description | Visualisation |
| :-: | - | --- |
| **Charts** | The forecast page shows three line charts for the current and forecasting value of the monthly average dASC of antibiotic types. The three line charts show increasing trend, stable trend, and decreasing trend separately. They would only display antibiotic types that have at least 4 different monthly records in the current imported data. | <img src="https://github.com/farrosmufid/Antimicrobial-Consumption-Dashboard-V4/blob/main/assets/forecast-chart.png" width="8220px" />|
| **Filters** | The forecast section has two filters that apply to the three visualisations. On top of the page, a date range picker would allow the user to select the time frame of the data. Based on the selected date range, the visualisation would show the forecast result. Underneath the textual introduction, the dropdown menu allows users to single select antibiotic type for display in the forecast visualisation. <br> **Note**: It's important to emphasise that forecast would only be produced if there are at least 4 monthly records. As a result, if the selected date range includes less than 3 months data, there would not be forecasting results visualised in the charts.  | <img src="https://github.com/farrosmufid/Antimicrobial-Consumption-Dashboard-V4/blob/main/assets/forecast-filters.png" width="4000px" />|
| **Buttons** | The button with text “View full date range” at the top of the forecast page can reset the data after an arbitrary date range is selected in the date picker. At the bottom of the forecast page, there are four buttons. The first button allows users to reset to original data so that the visualisation would only display the monthly mean dASC value for all current imported data. The second button would allow users to view a 6 month forecast, and the third for a 12 month forecast. The fourth button would allow users to download the raw forecasting data based on selection. | <img src="https://github.com/farrosmufid/Antimicrobial-Consumption-Dashboard-V4/blob/main/assets/forecast-buttons.png" />|

## 5. Susceptibility
| Chart | Description | Visualisation |
| :-: | - | - |
| **Top 10 by Lowest Susceptibility** | There are two horizontal bar charts that give insight into the antibiotics and organisms with the top 10 lowest susceptibility rates based on any filters applied, to give the user an idea of potential antibiotics or organisms to be wary of for the period of time selected.  | ![image](https://github.com/farrosmufid/Antimicrobial-Consumption-Dashboard-V4/assets/93821446/f9d906c5-d8fb-4f08-afe5-165948276cf5) |
| **Antibiogram** |
