# Financial Data Analysis Project Summary: FestMan Stores

This document provides a comprehensive overview of the financial data analysis project for FestMan Stores, outlining the project's goal, the defined Key Performance Indicators (KPIs), the visualizations developed, and concrete insights derived from the dashboard.

## 1. Project Goal

The primary objective of this project is to create a robust and interactive Power BI dashboard for FestMan Stores. This dashboard aims to provide deep insights into sales performance, profitability, order patterns, and discount effectiveness. By visualizing key financial metrics across various dimensions like products, countries, and segments, the project facilitates data-driven decision-making and enables year-over-year performance comparisons.

![Screenshot 2025-05-26 234740](https://github.com/user-attachments/assets/fc972a42-7866-4996-80c8-1a2030f5bf67)


## 2. Data Preparation and Modeling

### 2.1. Data Source
The project utilized the following datasets:
* **Dataset Used:**: <a href="Financial Sample (2).xlsx">Financial Sample (2).xlsx</a>
* **Final Dashboard Report**: <a href="[Dashboard FestMan.pdf]"> [Dashboard FestMan.pdf]</a>

### 2.2. Power Query Transformations
Initial data transformation and cleaning were performed in Power Query (accessed via "Transform Data" in Power BI).

### 2.3. Data Model Enhancements

* **Date Table Creation**: A dedicated 'Date Table' was created using DAX to support time-intelligence functions and establish relationships:
    ```
    Date Table = ADDCOLUMNS(CALENDARAUTO(),
        "Month No", MONTH([Date]),
        "Month Name", FORMAT([Date],"MMM"),
        "Year", YEAR([Date])
    )
    ```

* **Relationships**: A "many-to-one" relationship was established between the main financial table (likely named `Financials`) and the new 'Date Table'.

* **Column Renaming**: The 'Sales' column in the `Financials` table was renamed to 'Net Sales' for clarity and consistency.

* **Analysis Table**: A new table named "Analysis table" was created directly in the Report View. This table serves as a central location to house all calculated measures (KPIs), keeping the model organized.

## 3. Key Performance Indicators (KPIs)

The following core KPIs were defined using DAX measures within the "Analysis table", providing both current period performance and year-over-year comparisons:

### 3.1. Current Period KPIs

* **Sales Amount**: Sum of net sales.
    DAX: `Sales Amount = SUM(Financials[Net Sales])`
* **Orders**: Sum of units sold.
    DAX: `Orders = SUM(Financials[Units Sold])`
* **Profit**: Sum of profit.
    DAX: `Profit = SUM(Financials[Profit])`
* **Profit Margin**: Profit divided by Sales Amount, indicating profitability per sale.
    DAX: `Profit Margin = DIVIDE([Profit],[Sales Amount])`
* **Discount Offered**: Sum of discounts provided.
    DAX: `Discount Offered = SUM(Financials[Discounts])`

### 3.2. Time Intelligence (Last Year - LY) KPIs

These measures enable direct year-over-year performance comparison:

* **Sales Amount LY**: Sales Amount for the same period last year.
    DAX: `Sales Amount LY = CALCULATE([Sales Amount], DATEADD('Date Table'[Date], -1, YEAR))`
* **Discount Offered LY**: Discount Offered for the same period last year.
    DAX: `Discount Offered LY = CALCULATE([Discount Offered], DATEADD('Date Table'[Date], -1, YEAR))`
* **Orders LY**: Orders for the same period last year.
    DAX: `Orders LY = CALCULATE([Orders], DATEADD('Date Table'[Date], -1, YEAR))`
* **Profit LY**: Profit for the same period last year.
    DAX: `Profit LY = CALCULATE([Profit], DATEADD('Date Table'[Date], -1, YEAR))`
* **Profit Margin LY**: Profit Margin for the same period last year.
    DAX: `Profit Margin LY = CALCULATE([Profit Margin], DATEADD('Date Table'[Date], -1, YEAR))`

### 3.3. Supporting Measures for Visualizations

* **Top 3 Product by Sales**: Calculates sales specifically for the top 3 products, used for focused analysis.
    DAX: `Top 3 Product by Sales = CALCULATE([Sales Amount], TOPN(3, ALLSELECTED(Financials[Product]), [Sales Amount], DESC), VALUE(Financials[Product]))`
* **Top Highlight**: Used for conditional formatting to highlight specific data points (e.g., top products).
    DAX: `Top Highlight = IF(ISBLANK([Top 3 Product by Sales]), 0, 1)`

## 4. Report Development and Visualizations

The dashboard features a customized theme and background for enhanced aesthetics and user experience. The following charts and visuals are used to present the KPIs and insights, including their conditional formatting:

![Screenshot 2025-05-26 210502](https://github.com/user-attachments/assets/c6ce867e-be10-4f1d-9ebf-2be37cf54459)


* **KPI Cards (Single Value Display)**:
    * **Purpose**: Prominently display current performance and year-over-year growth for all core KPIs.
    * **KPIs Displayed**: Sales Amount (92,311,095, +249.46% YoY), Orders (861,132, +225.36% YoY), Profit (13,015,238, +235.58% YoY), Profit Margin (14.1%, -3.97% YoY), Discount Offered (7,059,717, +229.04% YoY).
 
    ![image](https://github.com/user-attachments/assets/c0c8fa46-08f0-470e-a690-6db34a26a6b9)


* **Sales Amount by Year and Month (Line Chart)**:
    * **Purpose**: Illustrates the trend of Sales Amount over time to identify seasonality and growth patterns.
    * **KPIs Displayed**: Sales Amount.
    * **Visual Insight**: Shows fluctuations in sales over time, indicating a peak in early 2015.

* **Orders by Country (Column Chart)**:
    * **Purpose**: Compares the total Orders volume across different Countries.
    * **KPIs Displayed**: Orders.
    * **Visual Insight**: Canada leads in orders (247.43K), followed closely by France (240.93K) and the United States (232.63K).

* **Profit Margin by Country (Column Chart)**:
    * **Purpose**: Displays the Profit Margin percentage for each Country.
    * **KPIs Displayed**: Profit Margin.
    * **Visual Insight**: Germany (15.7%) and France (15.5%) have the highest profit margins, while the United States has the lowest at 12.0% among the displayed countries.

* **% Discount Offered by Discount Band (Donut Chart)**:
    * **Purpose**: Shows the distribution of Discount Offered by Discount Band.
    * **KPIs Displayed**: Percentage of Discount Offered.
    * **Visual Insight**: A significant majority of discounts fall under the 'High' band (57.8%), followed by 'Medium' (32.6%), and 'Low' (9.6%).

* **Profit Margin by Segment (Bar Chart)**:
    * **Purpose**: Compares Profit Margin across different Segments.
    * **KPIs Displayed**: Profit Margin.
    * **Visual Insight**: 'Channel Partners' exhibit the highest profit margin (73%), followed by 'Midmarket' (28%) and 'Government' (22%). 'Enterprise' segment shows a negative profit margin (-3%), indicating a significant area of concern.   

* **Top 3 Products by Sales (Bar Chart)**:
    * **Purpose**: Highlights the Sales Amount generated by the top-performing Products.
    * **KPIs Displayed**: Sales Amount (for top 3 products).
    * **Conditional Formatting**: This chart utilizes the Top Highlight measure (`IF(ISBLANK([Top 3 Product by Sales]), 0, 1)`) to apply conditional formatting, visually emphasizing the top 3 products on the chart.
    * **Visual Insight**: 'Paseo' is the top product by sales (33,011K), followed by 'VTT' (20,512K) and 'Velo' (18,250K).

![image](https://github.com/user-attachments/assets/20534f0a-f09c-4a17-b018-62c5a5ac5df0)


* **Profit Margin by Segment and Products (Matrix Visual)**:
    * **Purpose**: Provides a detailed tabular breakdown of Profit Margin for each Product within each Segment, allowing for granular performance review.
    * **KPIs Displayed**: Profit Margin.
    * **Conditional Formatting**: To intuitively represent profitability, red triangles are used to visually indicate a loss, and green triangles indicate profit. This allows for quick identification of profitable and unprofitable product-segment combinations.

![image](https://github.com/user-attachments/assets/5166c45b-ee1d-4b14-a689-a44692472376)


    * **Visual Insight**: While many products show positive margins, the 'Enterprise' segment consistently shows negative profit margins across all its products (e.g., 'Paseo' at -1.55%, 'Amarilla' at -3.60%), reinforcing the concern from the Profit Margin by Segment chart. Even 'Montana' and 'Velo' in the 'Enterprise' segment are losing money. Products like 'Velo' (22.48%) and 'VTT' (22.35%) generally have high profit margins across segments.

![Screenshot 2025-05-26 234343](https://github.com/user-attachments/assets/8487495a-e64c-476d-ba78-be24232965da)


## 5. Key Insights

Based on the dashboard and the defined KPIs, here are some key insights for FestMan Stores:

* **Strong Overall Growth (Sales, Orders, Profit)**: The company experienced significant year-over-year growth in Sales (+249.46%), Orders (+225.36%), and Profit (+235.58%), indicating strong expansion.
* **Declining Profit Margin (YoY)**: Despite strong growth in other metrics, the overall Profit Margin saw a slight decrease (-3.97% YoY), suggesting potential cost increases or pricing pressures that need investigation.
* **Geographical Performance Discrepancies**:
    * Germany and France are highly profitable countries in terms of Profit Margin (15.7% and 15.5% respectively).
    * The United States has the lowest Profit Margin (12.0%) among the major countries, indicating a need to analyze costs or pricing strategies in this region.
* **Segment Profitability Concerns**:
    * 'Channel Partners', 'Midmarket', and 'Government' segments are highly profitable.
    * The 'Enterprise' segment is a significant area of concern, consistently showing a negative Profit Margin (-3%). This is critical and requires immediate attention to understand the underlying causes (e.g., high operational costs, aggressive discounting, or unfavorable pricing contracts).
    * Specific products within the 'Enterprise' segment are contributing to these losses (e.g., 'Amarilla' at -3.60%, 'Velo' at -2.37% in Enterprise), as clearly highlighted by the conditional formatting in the Profit Margin by Segment and Products matrix.
* **Top Products Driving Sales**: 'Paseo', 'VTT', and 'Velo' are the top three products by sales, contributing significantly to revenue.
* **Discount Strategy Observation**: A large portion of discounts are in the 'High' band (57.8%), which might be impacting the overall Profit Margin if not managed effectively, especially in less profitable segments.
* **Product-Level Profitability Varies**: Even products with high overall profit margins like 'Velo' and 'VTT' can experience losses within specific segments (like 'Enterprise'), highlighting the importance of granular analysis provided by the matrix visual with its conditional formatting.

This dashboard provides a powerful tool for FestMan Stores to monitor their financial health, identify areas of strength, and pinpoint critical areas for improvement to enhance overall profitability.
