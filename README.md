# Business Insights 360 Dashboard

### Dashboard Link : https://app.powerbi.com/groups/me/reports/7ecb1b02-23b1-44ca-8adf-209f2c460357/ReportSection579726641e0343db90d0?experience=power-bi

## Problem Statement

This dashboard provides a holistic view of customer, product, market, and forecast data for business insights. It helps businesses analyze customer demographics, forecast metrics, product engagement, and market reach. By visualizing critical KPIs, segmentation metrics, and trends, businesses can make informed strategic decisions, identify growth opportunities, and optimize customer engagement across segments.


### Steps followed 

- Step 1 : Load data into Power BI Desktop, dataset is a csv file.
- Step 2 : Open power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options.
- Step 3 :Cleaned and verified data using Power Query Editor.
- Step 4 :Created relationships between fact and dimension tables for data modeling.
- Step 5 : Inserted slicers and filters based on:

         Customer Segment

         Product Category

         Market Region

         Date/Month
- Step 6 : Added visualizations to represent performance indicators:

         KPI cards (Forecast, Revenue, Customer Count)

         Bar and Column Charts (Customer Segmentation, Product Sales)

         Pie/Donut Charts (Market Share)

         Line Charts (Trend Analysis)

         Table Visuals (Detailed Breakdown)

 
- Step 7 : Applied consistent theme and branding using company colors and logo.
- Step 8 : Created calculated columns and measures (to be populated in full .pbix file):

         Example: Customer Age Group

         Example: Total Forecast = SUM([Forecast Value])
- Step 9 : Published report to Power BI Service for cloud access and dashboard sharing.
           
- Step 10 :⚙️ Key DAX Measures Used
🔢 Forecast Error & Accuracy

    Net Error = [ForeCast Quantity] - [Sales Quantity]

Calculates the deviation between forecasted and actual sales quantity.

    Net Error % = DIVIDE([Net Error], [ForeCast Quantity], 0)

Expresses the error as a percentage of forecasted quantity.

    ABS Error =
    SUMX(
    DISTINCT(Dim_Date[Date]),
    SUMX(
        DISTINCT(dim_product[product_code]),
        ABS([Net Error])
    )
    )

Aggregates absolute error across all date-product combinations.

      ABS Error % = DIVIDE([ABS Error], [ForeCast Quantity], 0)

Expresses the absolute error as a percentage.

    Forecast Accuracy % = IF([ABS Error %] <> BLANK(), 1 - [ABS Error %] BLANK())

Inverse of ABS Error %, indicating how accurate the forecast is.

📅 Time Intelligence
Measures like ... LY use SAMEPERIODLASTYEAR(Dim_Date[Date]) to compare current performance against the same period last year, including:

    ABS Error % LY

    Forecast Accuracy % LY

    Net Error LY

    GM % LY

    Net Sales $ LY

    Net Profit % LY

    P & L LY

💰 Sales & Profitability
    Net Sales $ = SUM(FactActualEstimates[net_sales_amount])

    Gross Margin $ = [Net Sales $] - [Total COGS $]

    GM % = [Gross Margin $] / [Net Sales $]

    GROSS MARGIN / UNIT = DIVIDE([Gross Margin $], [Quantity], 0)

💸 Costs
    Freight Cost = SUM(FactActualEstimates[freight_cost])

    Manufacturing Cost$ = SUM(FactActualEstimates[manufacturing_cost])

    Other Cost $ = SUM(FactActualEstimates[other_cost])

    Total COGS $ = [Manufacturing Cost$] + [Freight Cost] + [Other Cost $]

🧾 Invoice & Deductions
    GrossSales$ = SUM(FactActualEstimates[net_sales_amount])

    NetInvoiceSales$ = SUM(FactActualEstimates[Net_Invoice_Sales_amount])

    PreInvoiceDeductions $ = [GrossSales$] - [NetInvoiceSales$]

    PostInvoiceDeductions $ = SUM(FactActualEstimates[post_invoice_deductions_amount])

    Post Invoice Other Deduction $ = SUM(FactActualEstimates[post_invoice_other_deductions_pct])

    Total Post Invoice Deduction $ = [PostInvoiceDeductions $] + [Post Invoice Other Deduction $]

📦 Quantity & Sales
   
    ForeCast Quantity = SUM(fact_forecast_monthly[forecast_quantity])

    Sales Quantity =
        CALCULATE(
         [Quantity],
         FactActualEstimates[date] <= MAX(LastSalesMonth[LastSalesMonth])
    )
 
    Quantity = SUM(FactActualEstimates[Quantity])

🧠 Risk Analysis
   
    Risk =
    IF(
    [Net Error] > 0, "Excess Inventory",
    IF([Net Error] < 0, "Out Of Stock", BLANK())
    )

📈 Performance & Visualization Controls
    Selected P & L Row =

    IF(
    HASONEVALUE('P & L Rows'[Description]),
    SELECTEDVALUE('P & L Rows'[Description]),
    "Net Sales"
    )
Top / Bottom Products & Customers =


    "Top / bottom Products & Customers by " & [Selected P & L Row]
    
    Performance Visual Title =


    [Selected P & L Row] & " Performance Over Time"
📊 P&L Comparison
    
    P & L YOY Chg = [P and L Values] - [P & L LY]

    P & L Yoy Chg % = DIVIDE([P & L YOY Chg], [P & L LY], 0) * 100


