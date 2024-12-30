# Car-Sales-analysis

## Table of contents
- [Project overview](#Project-overview)
- [Data sources](#Data-sources)
- [Tools](#Tool)
- [Data Cleaning/Preparation](#Data-Cleaning/Preparation)
- [Exploratory Data Analysis (EDA)](#Exploratory-Data-Analysis-(EDA))
- [Data analysis](#Data-analysis)
- [Results/Findings](#Results/Findings)
- [Recommendations](#Recommendations)

### Project overview

![Car sales Dashboard](https://github.com/user-attachments/assets/653e8e96-2e3a-4d6b-a4a9-6863e8f34baf)


This project focuses on leveraging SQL, Python, and Tableau to analyze Car Sales data, uncover actionable insights, and monitor key workforce metrics. The project involves building a data pipeline to acquire, clean, and process Car sales datasets, followed by exploratory data analysis (EDA) to identify trends. Finally, interactive KPI dashboards are created in Tableau to visualize and track essential workforce metrics, enabling data-driven decision-making involving the sales of cars.

### Data sources

Sales Data: The primary dataset used for this analysis is the 'Car Sales Data.xlsx' file, which contains detailed information about the Car sales.

### Tool

- SQL Server - Data Analysis
- Tableau - Creating reports
- Python(Jupyter notebook) - Data cleaning and analysis

### Data Cleaning/Preparation

In the initial data preparation phase, we performed the following tasks:
1. Data loading and inspection.
2. Handling missing values.
3. Data Cleaning and formatting.

### Exploratory Data Analysis (EDA)
EDA involved exploring the company's workforce to answer various questions, such as;

1.What are the best-selling car models, and how do they contribute to overall sales?
2.Which dealer regions are performing the best, and which require improvement?
3.How do seasonal trends impact sales performance across different car categories?


### Data analysis

Includes some codes/features worked with

```sql
SQL 
1.Sales Overview:
YTD Total Sales (2021):
SELECT SUM(price) AS ytd_total_sales FROM sales_data
WHERE date >= '2021-01-01' AND date <= '2021-12-31';
YOY Growth in Total Sales:
SELECT 
    (SUM(CASE WHEN date >= '2021-01-01' AND date <= '2021-12-31' THEN price ELSE 0 END) - 
     SUM(CASE WHEN date >= '2020-01-01' AND date <= '2020-12-31' THEN price ELSE 0 END)) / 
     SUM(CASE WHEN date >= '2020-01-01' AND date <= '2020-12-31' THEN price ELSE 0 END) * 100 AS yoy_growth
FROM sales_data;
2.Average Price Analysis:
YTD Average Price (2021):
SELECT AVG(price / car_id) AS ytd_avg_price FROM sales_data
WHERE date >= '2021-01-01' AND date <= '2021-12-31';
YOY Growth in Average Price:
SELECT 
    (AVG(CASE WHEN date >= '2021-01-01' AND date <= '2021-12-31' THEN price / car_id ELSE 0 END) - 
     AVG(CASE WHEN date >= '2020-01-01' AND date <= '2020-12-31' THEN price / car_id ELSE 0 END)) / 
     AVG(CASE WHEN date >= '2020-01-01' AND date <= '2020-12-31' THEN price / car_id ELSE 0 END) * 100 AS yoy_avg_price_growth FROM sales_data;
3.Cars Sold Metrics:
YTD Cars Sold (2021):

SELECT SUM(car_id) AS ytd_cars_sold FROM sales_data
WHERE date >= '2021-01-01' AND date <= '2021-12-31';
YOY Growth in Cars Sold:
SELECT 
    (SUM(CASE WHEN date >= '2021-01-01' AND date <= '2021-12-31' THEN car_id ELSE 0 END) - 
     SUM(CASE WHEN date >= '2020-01-01' AND date <= '2020-12-31' THEN car_id ELSE 0 END)) / 
     SUM(CASE WHEN date >= '2020-01-01' AND date <= '2020-12-31' THEN car_id ELSE 0 END) * 100 AS yoy_cars_sold_growth FROM sales_data;

Problem Statement 2: Charts Requirement
1.YTD Sales Weekly Trend:
SELECT EXTRACT(week FROM date) AS week, SUM(price) AS weekly_sales
FROM sales_data
WHERE date >= '2021-01-01' AND date <= '2021-12-31'GROUP BY week
ORDER BY week;
2.YTD Total Sales by Body Style (Pie Chart):
SELECT body_style, SUM(price) AS total_sales FROM sales_data
WHERE date >= '2021-01-01' AND date <= '2021-12-31'
GROUP BY body_style;
3.YTD Total Sales by Color (Donut Chart):
SELECT color, SUM(price) AS total_sales FROM sales_data
WHERE date >= '2021-01-01' AND date <= '2021-12-31'
GROUP BY color;
4.YTD Cars Sold by Dealer Region (Bar Chart):
SELECT dealer_region, SUM(car_id) AS cars_sold FROM sales_data
WHERE date >= '2021-01-01' AND date <= '2021-12-31'GROUP BY dealer_region;
Company-Wise Sales Trend in Grid Form (Table):
SELECT company_name, SUM(price) AS total_sales FROM sales_data
GROUP BY company_name;
```

### Python code

```
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
hr_data=pd.read_csv('HR Data.csv')
hr_data

# checking for missing values
hr_data.isnull().sum()
# checking for duplicated values
hr_data.duplicated().sum()
# Data analysis

# Convert the 'date' column to datetime
data['Date'] = pd.to_datetime(data['Date'])


# Filter the data for 2021 and 2020
data_2021 = data[data['Date'].dt.year == 2021]
data_2020 = data[data['Date'].dt.year == 2020]

# KPI Calculations

## 1. YTD Total Sales
ytd_sales_2021 = data_2021['Price ($)'].sum()
ytd_sales_2020 = data_2020['Price ($)'].sum()

## 2. YOY Growth in Total Sales
yoy_sales_growth = ((ytd_sales_2021 - ytd_sales_2020) / ytd_sales_2020) * 100

## 3. YTD Average Price
# Compute YTD Average Price for 2021
ytd_avg_price_2021 = data_2021['Price ($)'].sum() / data_2021['Car_id'].nunique()

# Compute YTD Average Price for 2020
ytd_avg_price_2020 = data_2020['Price ($)'].sum() / data_2020['Car_id'].nunique()


## 4. YOY Growth in Average Price
yoy_avg_price_growth = ((ytd_avg_price_2021 - ytd_avg_price_2020) / ytd_avg_price_2020) * 100

## 5. YTD Cars Sold
ytd_cars_sold_2021 = sum(data_2021['Car_id'].value_counts())
ytd_cars_sold_2020 = sum(data_2020['Car_id'].value_counts())

## 6. YOY Growth in Cars Sold
yoy_cars_sold_growth = ((ytd_cars_sold_2021 - ytd_cars_sold_2020) / ytd_cars_sold_2020) * 100

# Print KPIs
print(f"YTD Total Sales (2021): ${ytd_sales_2021:,.2f}")
print(f"YTD Total Sales (2020): ${ytd_sales_2020:,.2f}")
print(f"YOY Sales Growth: {yoy_sales_growth:.2f}%")
print(f"YTD Average Price (2021): ${ytd_avg_price_2021:,.2f}")
print(f"YTD Average Price (2020): ${ytd_avg_price_2020:,.2f}")
print(f"YOY Average Price Growth: {yoy_avg_price_growth:.2f}%")
print(f"YTD Cars Sold (2021): {ytd_cars_sold_2021}")
print(f"YTD Cars Sold (2020): {ytd_cars_sold_2020}")
print(f"YOY Cars Sold Growth: {yoy_cars_sold_growth:.2f}%")



# Exploratory Data Analysis (EDA) Visualizations

# 1. YTD Sales Weekly Trend (Line Chart)
data_2021['week'] = data_2021['Date'].dt.isocalendar().week
weekly_sales_2021 = data_2021.groupby('week')['Price ($)'].sum()

plt.figure(figsize=(10, 6))
plt.plot(weekly_sales_2021.index, weekly_sales_2021.values, marker='o')
plt.title('YTD Sales Weekly Trend (2021)', fontsize=14)
plt.xlabel('Week', fontsize=12)
plt.ylabel('Total Sales ($)', fontsize=12)
plt.grid(True)
plt.show()

# 2. YTD Total Sales by Body Style (Pie Chart)
body_style_sales = data_2021.groupby('Body Style')['Price ($)'].sum()

plt.figure(figsize=(8, 8))
plt.pie(body_style_sales, labels=body_style_sales.index, autopct='%1.1f%%', startangle=140)
plt.title('YTD Total Sales by Body Style (2021)', fontsize=14)
plt.show()

# 3. YTD Total Sales by Color (Donut Chart)
color_sales = data_2021.groupby('Color')['Price ($)'].sum()

# Create a basic Pie chart and add a "hole" to make it a donut chart
fig, ax = plt.subplots(figsize=(8, 8))
ax.pie(color_sales, labels=color_sales.index, autopct='%1.1f%%', startangle=140)
centre_circle = plt.Circle((0,0),0.60,fc='white')
fig.gca().add_artist(centre_circle)
plt.title('YTD Total Sales by Color (2021)', fontsize=14)
plt.show()

# 4. YTD Cars Sold by Dealer Region (Bar Chart)
# Ensure 'car_id' is numeric and 'dealer_region' and 'Transmission' are valid

# Group the data by both 'dealer_region' and 'Transmission' and sum the 'car_id' to get total cars sold per region and transmission
region_transmission_sales = data_2021.groupby(['Dealer_Region','Transmission']).count().unstack()['Price ($)']

# Check if the resulting DataFrame has numeric data
print(region_transmission_sales)

# Plotting the data (stacked bar chart for better comparison between transmission types)
if not region_transmission_sales.empty:
    region_transmission_sales.plot(kind='bar', stacked=True, figsize=(12, 8), color=['skyblue', 'lightcoral', 'lightgreen'])

    # Adding title and labels
    plt.title('YTD Cars Sold by Dealer Region and Transmission (2021)', fontsize=14)
    plt.xlabel('Dealer Region', fontsize=12)
    plt.ylabel('Price ($)', fontsize=12)

    # Rotate x-axis labels for better readability
    plt.xticks(rotation=45, ha='right')

    # Show the plot
else:
    print("No valid numeric data available for plotting.")
plt.show()


# 5. Company-Wise Sales Trend (Table)
company_sales = data.groupby('Customer Name')['Price ($)'].sum()
print("\nCompany-Wise Sales Trend:")
print(company_sales)

# 6. Distribution of Price
plt.figure(figsize=(10, 6))
sns.histplot(data_2021['Price ($)'], kde=True, bins=30, color='blue')
plt.title('Distribution of Price (2021)', fontsize=14)
plt.xlabel('Price ($)', fontsize=12)
plt.ylabel('Frequency', fontsize=12)
plt.show()

# 7. Distribution of Cars Sold
plt.figure(figsize=(10, 6))
sns.histplot(data_2021['Car_id'], kde=True, bins=30, color='green')
plt.title('Distribution of Cars Sold (2021)', fontsize=14)
plt.xlabel('Cars Sold (Quantity)', fontsize=12)
plt.ylabel('Frequency', fontsize=12)
plt.show()
```

### Results/Findings

The analysis results are summarized as follows;
1. Best-Selling Car Models: Bar charts reveal the top-performing car models and their sales contribution, helping focus on high-demand products.
2. Dealer Region Performance: Regional bar charts highlight top-performing and underperforming dealer regions, enabling targeted interventions.
3. Seasonal Sales Trends: Line charts show sales peaks and dips across weeks or months, helping identify seasonal demand patterns.

### Recommendations
Based on the analysis, the following are recommended;
1. Focus marketing efforts on promoting best-selling models while addressing low-performing ones.
2. Allocate resources and support to underperforming dealer regions to enhance their sales.
3. Use seasonal trends to plan inventory and launch targeted promotional campaigns effectively.
