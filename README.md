
# Housing Market Analysis
### Dashboard link
https://app.powerbi.com/groups/505feeff-ec3b-4253-9d20-f0a3c4ec1ab7/reports/23f423b6-93a8-44ad-a5f6-d0db1960b2bb/4478bff2eec77b1fcb3a?experience=power-bi
## Executive Summary
This project aims to analyze the housing market by tracking property prices, sales trends, and regional patterns. Using Power BI, the dashboard provides actionable insights into sales distribution, price fluctuations, and property characteristics to support investment and policy-making decisions.

## Data Cleaning & Preparation
1. Before analysis, the dataset was cleaned and prepared to ensure accuracy:

2. Removed duplicates and irrelevant columns.

3. Handled missing values (median for numeric fields, mode for categorical).

4. Standardized categorical fields (Region, City, Sales Type, House Type).

5. Corrected data types (numeric for prices/area, categorical for types, date for year/quarter).

6. Derived new columns (Age , offer price
- Age = 
ABS(YEAR('Housing'[date] ) - Housing[year_build])
- Offer Price = 
(100 * Housing[purchase_price]) / 
(100-Housing[%_change_between_offer_and_purchase])

## KPIs & Metrics
- Total Transactions – Number of properties sold.

- Total Sales Value – Sum of all purchase prices.

- Average Price per SQM – Purchase price divided by property size.

- YOY Growth % – Annual percentage change in property prices.

- Offer–Purchase Price Gap – Difference between listing and final purchase price.

## Dashboard Pages & Visuals
### Page 1 – Market Overview
- Page Snap :https://github.com/user-attachments/assets/8a8c7a17-1355-4e04-87dc-e16691e532d5"

- KPI Cards: Total Sales Value, Total Transactions, Average Price per SQM

- Line Chart: YOY Growth % over years

- Stacked bar chart: Sales distribution by region

- Scatter Plot: Offer VS Purchase Price

### DAX Measure
- Median Sales Price Change = 
VAR CurrMedianPrice =
        MEDIANX(FILTER('Housing',YEAR(Housing[date].[Date]) = 
        YEAR(MAX('Housing'[date].[Date]))),'Housing'[purchase_price])

VAR PreMedianPrice =
        MEDIANX(FILTER('Housing',YEAR('Housing'[date].[Date]) =
        YEAR(MAX('Housing'[date].[Date])) -1),Housing[purchase_price])

RETURN
    IF(PreMedianPrice <>0, (CurrMedianPrice - PreMedianPrice) /
    PreMedianPrice , blank())

- Last 12 Months Sales = 
 CALCULATE(SUM('Housing'[purchase_price]),DATESINPERIOD('Housing'[date] ,MAX('Housing'[date]),-12,MONTH))

- Units Sold In Year & Quarter = 
 CALCULATE(DISTINCTCOUNT('Housing'[house_id]),
 YEAR('Housing'[date]) = YEAR(MAX('Housing'[date])) 
 && 
 QUARTER('Housing'[date]) = QUARTER(MAX('Housing'[date])))

- YOY Sales Growth = 
 VAR CurrYearSales = CALCULATE(
    SUM(Housing[purchase_price]),
        YEAR('Housing'[date])=YEAR(MAX('Housing'[date])))

 VAR PreYearSales = CALCULATE(
    SUM(Housing[purchase_price]),
        YEAR('Housing'[date])=YEAR(MAX('Housing'[date]))-1)

 RETURN 
    IF(PreYearSales <>0 , (CurrYearSales - PreYearSales)/PreYearSales , Blank())


### Impact Note:
Provides a high-level market snapshot, enabling management to quickly understand sales volume, pricing trends, and the distribution of property transactions across regions and sales types.

### Page 2 – Regional & Property Analysis
- Page Snap : https://github.com/user-attachments/assets/8fbdf1c0-4ff5-4d38-9119-99392b8e3b32 
- Stacked Bar Chart:Sales by Region

- Key Influencers : Age and sales

- Table Visual : Date, Total YTD sales, sum of purchase price

Pie Chart: Average SQM price by region

### DAX Measure
- Sales By Region = 
        CALCULATE(SUM('Housing'[purchase_price]),ALLEXCEPT('Housing',Housing[region]))

- Offer To SQM Ratio = 
 DIVIDE(SUM('Housing'[Offer Price]),SUM(Housing[sqm]))

- Total YTD Sales = 
 TOTALYTD(SUM('Housing'[purchase_price]),'Housing'[date].[Date])

### Impact Note:
Highlights regional demand patterns and property type performance, helping stakeholders identify strong-performing regions, spot opportunities, and allocate resources for targeted marketing and investment.

### Page 3 – Pricing Insights & Buyer Behavior
- Page Snap : https://github.com/user-attachments/assets/02f786f3-06aa-4ca0-8529-3c58fef672da
- Slicers : Area,City,Sales Type,Region

- Clustered bar chart : AVG Offer / Purchase By House Type

- Clustered bar chart : AVG Inflation/Interest/Yield Price By House Type

- Line & Stacked column chart : AVG SQM/SQM Price By House Type

### Impact Note:
This page provides deep insights into property type performance, showing how offer and purchase prices align, how external economic factors (inflation, interest, yield) affect different property types, and how property size vs price per SQM varies across categories.

# Expected Business Impact (Overall)
- Improved Pricing Strategies: Insights from offer vs purchase price gaps enable real estate firms to set competitive yet profitable prices.

- Targeted Investments: Regional analysis identifies high-performing cities and property types, guiding investors to maximize ROI.

- Market Forecasting: YOY growth trends provide forward-looking insights for developers and policy makers to anticipate demand.

- Customer-Centric Decisions: Buyer behavior and income group analysis help companies tailor offerings to different affordability segments.

- Operational Efficiency: Clean, standardized data ensures consistent reporting across stakeholders, reducing decision delays.


