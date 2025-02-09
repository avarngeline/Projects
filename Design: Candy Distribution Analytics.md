# Power BI Workflow: Candy Distribution Analytics Report

## 1. Overview
**Problem Statement**

The candy distributor in the United States faces challenges in optimising its supply chain efficiency, maximising profitability, and improving customer satisfaction. Key areas of concern include identifying the most profitable regions, understanding product profitability, evaluating factory production efficiency, and analysing shipping logistics.

To address these challenges, this analysis focuses on:

- Sales Performance: Identifying the most profitable regions and understanding sales distribution across product divisions.
- Product Profitability: Analysing profit margins and the relationship between production costs and sales prices across different product categories.
- Factory Production Performance: Assessing factory production efficiency and cost management to enhance operational effectiveness.
- Shipping Analysis: Examining shipping mode preferences and their impact on customer satisfaction across different states.

The objective is to uncover actionable insights that can drive better decision-making, improve operational efficiency, and enhance overall business performance.


## 2. Data Model

**2.1. Relationship and Data Schema**

![Image](https://github.com/user-attachments/assets/efa3a0cf-c60b-4ad7-a090-6eeab4b9936e)

The model follows a **star schema** design, where a central fact table connects to multiple dimension tables. This structure optimises query performance and simplifies business analysis.

**Fact Table**
- **Candy_Sales**: Contains transactional data such as sales, costs, and gross profit.


| Column Name       | Data Type | Description                                      |
|-------------------|-----------|--------------------------------------------------|
| Row ID            | Text | Unique identifier for each row                    |
| Order ID          | Text | Unique order identifier                      |
| Order Date        | Date | The date when the order was placed                |
| Ship Date         | Date | The date when the order was shipped               |
| Ship Mode         | Text | The shipping method used|
| Customer ID       | Text | Unique identifier for the customer                |
| Country/Region    | Text | The country where the order was placed           |
| City              | Text | The city where the order was placed                |
| State/Province    | Text | The state or province where the order was placed |
| Postal Code       | Text | The postal code of the shipping address            |
| Division          | Text | The product category (e.g., Chocolate)              |
| Region            | Text | The geographic region (e.g., Pacific, Atlantic)|
| Product ID        | Text | Unique identifier for the product                 |
| Product Name      | Text | The name of the product                        |
| Sales             | Decimal Number | The total sales amount for the order            |
| Units             | Whole Number   | The number of units sold in the order               |
| Gross Profit      | Decimal Number | The profit earned before deducting costs          |
| Cost              | Decimal Number | The total cost of the products in the order      |


**Dimension Tables:**

- **Candy_Products:**   Stores product-related attributes such as product name, unit cost, and unit price.

| Column Name       | Data Type | Description                                      |
|-------------------|-----------|--------------------------------------------------|
| Division          | Text    | The product category (e.g., Chocolate, Sugar, Other) |
| Product Name      | Text    | The name of the product                        |
| Factory           | Text    | The factory that produces the product          |
| Product ID        | Text    | Unique identifier for each product            |
| Unit Price        | Decimal Number     | The price of a single unit of the product     |
| Unit Cost         | Decimal Number     | The cost to produce a single unit of the product |

- **Candy_Factories:** Contains factory details, including factory name and geographical coordinates (latitude, longitude).  

| Column Name       | Data Type | Description                                      |
|-------------------|-----------|--------------------------------------------------|
| Factory           | Text    | The name of the factory.                        |
| Latitude          | Decimal Number     | The latitude coordinate of the factory location |
| Longitude         | Decimal Number     | The longitude coordinate of the factory location |

- **DateTable:** Supports time-based analysis with a date column for filtering and aggregations.  

| Column Name       | Data Type | Description                                      |
|-------------------|-----------|--------------------------------------------------|
| Date           | Date/Time   | A custom table manually created by DAX |


**2.2. Data Soure**

|Source Name |Location |Type | 
|:-----------|:-----------|:-----------|
| US Candy Distributor     |  [mavenanalytics.io](https://mavenanalytics.io/data-playground?page=2&pageSize=5)    | CSV      |



## 3. Data Transformation (Power Query)

**3.1. Applied Steps**  

Three key transformation steps are taken in Power Query, as shown below.

  **1) Data Preview**  
  
After the data is loaded into Power Query, column quality, column distribution, and column profiling are selected to showcase the data structure.

![Image](https://github.com/user-attachments/assets/7c537170-dcbc-46d1-8473-c4afd4a1aa3f)

This step helps identify value distributions, empty or null entries, and any inconsistencies or errors. A thorough review ensures that the data is clean and ready for further analysis and reporting.

*Table: Candy_Sales*
  ![Image](https://github.com/user-attachments/assets/fc7bd04b-3e48-4e25-ac3e-2358c9d37460) 

*Table: Candy_Products*
![Image](https://github.com/user-attachments/assets/c46a6752-3fe5-4269-a608-38a59a3bed3e)

*Table: Candy_Factories*
![Image](https://github.com/user-attachments/assets/c7fc9a48-c0b7-4c2b-ae4b-4dbf88418251)

  **2) Data Cleaning & Preprocessing**  

*1. Remove Unnecessary Columns*
- Keeping only relevant fields can improve performance.  
- For the Candy_Sales table, columns such as Ship_Date, order_id, customer_id, postal_code, and Product_id were removed as they were not needed for the analysis.

*2. Change Data Types* 
- To convert columns to the correct data types (e.g., Text, Date, Numeric). 
- For the Candy_Sales table, the gross_profit and cost columns were changed to Decimal data type, and the unit column was changed to Whole Number.

*3. Trim & Clean Text* 
- To remove extra spaces, incorrect characters, and formatting issues.  
- For the Candy_Sales and Candy_Products tables, the product_name column was shortened (e.g., from "Wonka Bar - Scrumdiddlyumptious" to "Scrumdiddlyumptious"), and the ship_mode column was renamed to be more meaningful (e.g., from "First Class" to "Next Day").

*4. Transform data*
- To structure the dataset
- For the Candy_Factories table, the "Use First Row as Headers" option was applied to identify the header columns.

*5. Create Custom Columns* 
- To add calculated fields using M language or built-in functions.
- For the Candy_Factories table, the "Use First Row as Headers" option was applied to identify the header columns.

*6. Split Columns* 
- Separating combined text fields (e.g., splitting full names into first & last names).  
- For the Candy_Sales table, the order_date column was split using a delimiter to separate the time from the date, and the time column was removed as it was not used in the analysis, helping reduce space usage.


**3) Loading Data to Power BI**
- Close & Apply → Load transformed data into Power BI for further modeling & visualisation.

**Results**

*Table: Candy_Sales*  

![Image](https://github.com/user-attachments/assets/866540b6-df74-4dd7-ad7b-c71ba3f5303a)

*Table: Candy_Products*  

![Image](https://github.com/user-attachments/assets/ea03889c-06df-4d6f-a028-6dd11827af01)

*Table: Candy_Factories*  

![Image](https://github.com/user-attachments/assets/41d6c7c2-f121-4f93-b93b-697ca3e18eb2)


## 4. DAX Calculations & Measures (Power BI Desktop)

**4.1. Key Measures**  

Three key measures are created for the analysis, mainly to used in the sales performance analysis.

| Measure Name | DAX Formula | Description | 
|:-----------|:-----------|:-----------|
| Cost-to-Sales_ratio     | DIVIDE(SUM(Candy_Sales[cost]), SUM(Candy_Sales[sales]))  | The proportion of total costs relative to total sales      |
| YoY_sales     | CALCULATE(SUM(Candy_Sales[sales]),   SAMEPERIODLASTYEAR(DateTable[Date]))| The Year-over-Year sales, showing total sales for the same period last year|
| YoY_sales_growth %     | DIVIDE(SUM(Candy_Sales[sales]) - [YoY_sales],[YoY_sales], 0)| The percentage growth in sales compared to the same period last year|

**4.2. Calculated Table**

| Table Name | DAX Formula | Description | 
|:-----------|:-----------|:-----------|
| DateTable     | CALENDAR(MIN(Candy_Sales[order_date]), MAX(Candy_Sales[order_date]))  | Date table to support time intelligence     |



## 5. Report Visualisation & Design

**5.1. Page Layout and Key Areas of Analysis**
- Sales Performance: Evaluating overall sales trends to see seasonality and timing, order volumes, and gross profit distribution across different product dicision and regions.
- Product Profitability: Identifying the trend of profit margin and the most profitable product division and assessing cost structures to optimise margins.
- Factory Production Performance: Analysing factory output, comparing cost to sales ratio and identifying the best performer in the view of cost optimisation.
- Shipping Analysis: Determining the most and least efficient factory-to-customer shipping routes to reduce costs and enhance logistics.

**5.2. Key Visuals & Interactions**  

The following visualisations were created in Power BI to represent the data effectively 

**1) Sales Performance Page**  
- **Card: YoY Sales Growth, Total Sales, Total Units**

This chart helps understand the overall business expansion by showing the percentage growth in sales compared to the previous year, total revenue generated, and the total number of units sold.

![Image](https://github.com/user-attachments/assets/f549c8a9-92d1-4759-b157-491bc1e11ca9)

*Data Input:*

![Image](https://github.com/user-attachments/assets/cad4129d-09fc-48ed-97d1-3895b654b80a)

![Image](https://github.com/user-attachments/assets/4fceda2c-f8a8-4eb1-8b14-e1a9ade9b3a8)

![Image](https://github.com/user-attachments/assets/25046adb-ee08-4c43-ba47-ec81a9ee258a)


- **Line Chart: Quarterly Sales Trends by Region**

This chart tracks sales performance over time (2021–2024) across different regions, helping identify seasonal trends and regional performance.

![Image](https://github.com/user-attachments/assets/b23421e5-4a30-4edd-9af6-ae854151041b)

*Data Input:*

![Image](https://github.com/user-attachments/assets/6a07503b-23db-4cc7-afdd-5ffa7b9e1620)

- **Column Chart: Total Units Sold by Region and Division**  

The column chart demonstrates sales volume across different regions, helping target underperforming markets.


![Image](https://github.com/user-attachments/assets/c56f7d87-18ef-4249-910e-f26944ef640c)

*Data Input:*

![Image](https://github.com/user-attachments/assets/1e6681f7-602b-4a09-90cc-05284f1a7ac4)


- **Pie chart: Gross Profit Distribution by Division and Product**

The chart breaks down the gross profit share among different products and divisions, allowing identification of top-performing and low-performing products.

![Image](https://github.com/user-attachments/assets/1f8d7dca-a9ca-4852-ab14-9173e59c8827)

*Data Input:*

![Image](https://github.com/user-attachments/assets/ad7ad86a-9f46-4ef1-9cde-74ab5454328f)


**2) Product Profitability Page**  

- **Line Chart: Yearly Trend of Average Profit Margin**
  
The chart shows how the average profit margin has changed over time (2021–2024), helping visualise profitability growth and identify trends.


![Image](https://github.com/user-attachments/assets/a403f0f7-90f6-4bfb-a8ca-1858835d7d03)

*Data Input:*

![Image](https://github.com/user-attachments/assets/0fa434bf-0600-4929-90c7-cdb80ec7854f)

- **Column Chart: Total Gross Profit by Factory and Division**
  
The chart compares gross profit across different factories, divided by Division, to show which division is more profitable.

![Image](https://github.com/user-attachments/assets/57541807-983a-4cd2-a92e-13823f8664dc)

*Data Input:*

![Image](https://github.com/user-attachments/assets/e6989349-ab51-4d1a-b707-65f747f24ba9)

- **Bar Chart: Average Profit Margin by Division**  

This chart displays profitability levels for the Chocolate, Sugar, and Other divisions. It helps identify which division has the highest and lowest gross profit for each product.

![Image](https://github.com/user-attachments/assets/d11e5598-4214-4e2c-8ad6-b43db38843aa)

*Data Input:*

![Image](https://github.com/user-attachments/assets/fee8e219-d641-4f32-8fc1-799cf149fe2d)

- **Table: Sales and Profit Breakdown by Product**

The chart lists individual candy products along with their unit cost, price, total profit, and margin, helping to identify high-margin versus low-margin products.

![Image](https://github.com/user-attachments/assets/ea6588b1-d067-40a5-84d3-5181269d4fdc)

*Data Input:*

![Image](https://github.com/user-attachments/assets/fa0b1050-4dd7-4bc4-b698-e7edf2e2ed07)


**3) Factory Production Performance Page**  

- **Column Chart: Cost-to-Sales Ratio by Factory and Division**

The chart shows the cost per unit of sales across different factories, identifying which factory has the highest and lowest cost efficiency.

![Image](https://github.com/user-attachments/assets/cc11d1ec-2bc3-4f4e-8244-2614421f6083)

*Data Input:*

![Image](https://github.com/user-attachments/assets/4ed92051-c259-4436-b012-e00177f42f23)

- **Scatter Chart: Cost-to-Sales Ratio, Gross Profit, Sales**

This chart is a 3D visualisation that helps evaluate how cost efficiency affects sales and profits by comparing the Cost-to-Sales Ratio (X-axis), Gross Profit (Y-axis), and Total Sales (Bubble Size).

![Image](https://github.com/user-attachments/assets/90c8f5db-2f14-4daf-97d8-bc9b495fc9ae)

*Data Input:*

![Image](https://github.com/user-attachments/assets/c6d19271-deb8-410b-ae78-7f071852af75)

- **Line Chart: Gross Profit Trend by Factory**

The chart displays factory-wise profitability trends from 2021 to 2024, identifying which factories are growing or declining in profits.

![Image](https://github.com/user-attachments/assets/42d875d8-89cc-48ce-a523-2e3a7b7f2f8b)

*Data Input:*

![Image](https://github.com/user-attachments/assets/8b0a3cdb-2c26-4d1a-a87d-3fdffaf06e67)

**4) Shipping Analysis Page**  

- **Line Graph: Ship Mode Trend by Division**

The chart shows how different shipping methods (3-5 Days, 7 Days, Next Day, Same Day) are used across divisions, helping understand how shipping speed affects the business.

![Image](https://github.com/user-attachments/assets/aed52a1c-27ca-4678-a974-f8d8e1fd6354)

*Data Input:*

![Image](https://github.com/user-attachments/assets/2388e032-b6e8-40cd-9a87-39eabc1e1c2d)

- **Stacked Bar Chart: Regional Breakdown of Shipping Mode**

It displays shipping volume per region (Pacific, Atlantic, Interior, Gulf), helping identify which region prefers each shipping mode.

![Image](https://github.com/user-attachments/assets/9b72be04-5c4c-44a5-9717-884418587cd5)

*Data Input:*

![Image](https://github.com/user-attachments/assets/024ac9de-1aec-4506-a7af-c3d681f4097b)

- **Map Visualisation: Top 10 Sales Locations with Shipping Mode Breakdown**

A geographical map showing top-performing cities in the US and Canada, highlighting the most commonly used shipping modes in each location.

![Image](https://github.com/user-attachments/assets/8c9be728-7c9a-4ffd-88d0-db71e5cafa39)

*Data Input:*

![Image](https://github.com/user-attachments/assets/fc8b2ee8-62cb-4771-88f9-362f8c0581bf)

**5) Report interaction**  

- **Slicer Across Report Page**
Vertical list slicers are added as visual filters that allow interactive data filtering in reports, including Year, Quarter, Region, Division, Factory, and Ship Mode.

![Image](https://github.com/user-attachments/assets/9fb00b17-0a5e-4c5c-b506-7b55b5b3bd17)

- **Drill Down**
Hierarchical visualisations in Power BI automatically include drill-down and expand-all options in the top-right corner. For example, in a Yearly Trend of Average Profit Margin chart, users can drill down to view data at the monthly level.

![Image](https://github.com/user-attachments/assets/92fdd4f9-bbf2-4002-92fe-87ae6374eaaf)

**5.3. Custom Visuals & Theme**  
Once the visualisation is complete, the next step is to enhance its appearance by selecting a theme from the View tab. This customisation improves the report's visual appeal and makes it more engaging.

![Image](https://github.com/user-attachments/assets/c1818fca-0cb9-49db-824e-339f87e177c9)

## 6. Final Report 

Once the visualisations are completed, the **Power BI Report: Candy Distribution Analytics** serves as a powerful tool for data analysis. It helps analyse trends, identify top-selling products, and assess distribution efficiency. Users can apply filters, drill down into data, and compare actual performance against targets to make informed decisions. This report enables better inventory management, pricing strategies, and overall business optimisation.

***Sales Performance***  

![Image](https://github.com/user-attachments/assets/e04bc985-1459-4528-8181-0a24f1b01523)

***Product Profitability***  

![Image](https://github.com/user-attachments/assets/e042e0d4-3145-4742-a52f-36319b8707b3)

***Factory Production Performance***  

![Image](https://github.com/user-attachments/assets/5f839bd1-c784-497f-a7f2-0eb0f9705b74)

***Shipping Analysis***  

![Image](https://github.com/user-attachments/assets/afdeaa72-998e-4d81-95ca-44028767d0e5)

