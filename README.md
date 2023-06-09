
# Power BI Analysis Project: Retail Toy Stores in Mexico - 2023
📚 This project aims to analyze a dataset containing information about stores, products, and sales transactions in Mexico. The dataset source comes from Maven Analytics and can be found [here](https://www.mavenanalytics.io/data-playground).

![Illustration of silhouetted heads](mxtoystore.jpg)

## Data Dictionary
The dataset is structured using the following data dictionary:
- 🏢 Store_ID =	Store ID
- 🏪 Store_Name = Store name
- 🌆 Store_City =	City in Mexico where the store is located
- 🗺️ Store_Location =	Location in the city where the store is located
- 📅 Store_Open_Date =	Date when the store was opened
- 📦 Product_ID =	Product ID
- 📝 Product_Name =	Product name
- 🏷️ Product_Category =	Product Category
- 💲 Product_Cost =	Product cost ($USD)
- 💲 Product_Price =	Product retail price ($USD)
- 💼 Sale_ID =	Sale ID
- 📅 Date =	Date of the transaction
- 🔢 Units =	Units sold
- 📊 Stock_On_Hand =	Stock quantity of the product in the store (inventory)

## 🔍 The main objectives of this project are:

- Perform comprehensive data analysis to gain insights into the stores, products, and sales transactions.
- Identify patterns, trends, and relationships within the dataset.
- Conduct statistical analysis to uncover key metrics and performance indicators.
- Build predictive models to forecast sales and optimize inventory management.
- Develop interactive and visually appealing dashboards to present findings and insights.

# Project Breakdown

## Throughout this project, you can expect to:

- Explore the dataset using Power Query to understand its structure and contents.
- Clean and preprocess the data to handle missing values, outliers, and inconsistencies.
- Perform exploratory data analysis using various statistical and visual techniques.
- Develop calculated columns and measures to derive meaningful insights.
- Create interactive dashboards using Power BI to present key findings and visualizations.

## Data Acquisition & Cleansing 
- Given the nature of this project, I didn't go for any fancy extraction method other than importing the CSV files directly. If I could it again, I'd be probably connect to my local MYSQL server and just extract the data from there. 
- The data required little to no cleaning up or normalization, the only modifications I did was removing "Maven Toys" from the store names, and updating "Ciudad de Mexico" to "Mexico City" in order for the maps visualization to function properly.
- I used Power Query's option to view data distribution and column statistics to do some exploratory analysis. 

## Modeling & Calculations 
- To build the schema we used the primary & foreign keys within each dimension table and connected them to the sales fact table, this was pretty simple and easily done through Power BI's relationship editor. 
- I decided to aggregate a date table using DAX , here's the DAX code I used: 

```TimeTable = 
VAR MinDate = MIN('Sales'[Date])
VAR MaxDate = MAX('Sales'[Date])
RETURN
    ADDCOLUMNS(
        CALENDAR(MinDate, MaxDate),
        "Year", YEAR([Date]),
        "Month", FORMAT([Date], "MMM"),
        "Quarter", "Q" & FORMAT([Date], "Q")
    )
```
- For a better time series analysis, I added the Month/Year group, this allows for an accurate chronological order and facilitates visualizations. 
- We converted the Data Category for Store_City, this allowed the map visualizations to function accurately.
These are several Measures I found to be very useful:
- Target Sales: We needed to establish a target for our total monthly revenue, that way we could create a KPI card to measure our performance on a monthly basis.

```
Target Sales = 24000000
```
- Total Cost: This allows us to calculate how much each product is costing us, then summarizes it. It can be used a granular level. 


```
Total Cost = SUMX(Sales, Sales[Units] * RELATED(Products[Product_Cost])) 
```
- Total Margin: With this measure we calculate the total % of margin.


```
Total Margin = [Total Sales] - [Total Cost]
```
- Total Profit: With this measure we calculate the total profit brought in.


```
Total Profit = SUMX(Sales, (Sales[Units] * RELATED(Products[Product_Price])) - (Sales[Units] * RELATED(Products[Product_Cost])))
 ```
- Total Sales: With this measure we calculate the total sales brought in.


 ```
 Total Sales = SUM('Sales'[Units]) * SUM(products[Product_Price])
 ```
- Margin (by product): With this measure we calculate the % of margin per product.


```
Margin = DIVIDE(Products[Product_Price] - Products[Product_Cost], Products[Product_Price])
```
- Profit (by product): With this measure we calculate the profit per product.


```
Profit by Toy = Products[Product_Price] - Products[Product_Cost]
```
- Stock levels: With this measure we calculate the total inventory. 

```
Stock Levels = SUM(inventory[Stock_On_Hand])
```
## Reports & Dashboard 

### Sales Overview
Objective: Provide an overview of sales performance and trends.
Visualizations:
- Report cards for Total Sales, Total Profit & Stock levels.
- Understanding sales patterns over time is aided by a time-series line graph depicting the evolution of sales. You can identify seasonality or sales trends and make informed business decisions as a result.
- The area chart depicting toy category sales and distribution over time allows you to analyze the performance of various toy categories. You can identify the most popular product categories and monitor their performance over time.
- The bar chart depicting the distribution of sales by store enables you to compare the sales performance of various store locations. You can identify the best-performing stores and analyze how sales are distributed across different locations.
- Distribution of sales by category is depicted in a column chart. 
- The column chart depicting the distribution of sales by category reveals information about the popularity of various toy categories. You can identify the most popular product categories and evaluate their contribution to overall sales.

### Sales Overview
Objective: Provide an overview of sales performance and trends.
Visualizations:
- Report cards for Total Sales, Total Profit & Stock levels.
- Understanding sales patterns over time is aided by a time-series line graph depicting the evolution of sales. You can identify seasonality or sales trends and make informed business decisions as a result.
- The area chart depicting toy category sales and distribution over time allows you to analyze the performance of various toy categories. You can identify the most popular product categories and monitor their performance over time.
- The bar chart depicting the distribution of sales by store enables you to compare the sales performance of various store locations. You can identify the best-performing stores and analyze how sales are distributed across different locations.
- Distribution of sales by category is depicted in a column chart. 
- The column chart depicting the distribution of sales by category reveals information about the popularity of various toy categories. You can identify the most popular product categories and evaluate their contribution to overall sales.
