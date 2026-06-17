🏠 House Market Analytics – Power BI Report
This Power BI report delivers deep insights into the Danish housing market, analyzing sales trends, pricing metrics, property types, and regional dynamics using ~100,000 rows of housing data from Google BigQuery.


📁 Dataset Overview
Source: Google BigQuery (CSV format)
Volume: ~100,000 rows × 19 columns (Columns A–S)
Key Features: date, region, purchase_price, offer_price, sqm, sqm_price, sales_type, house_type, age

📊 Page 1 – House Market Overview
🔍 Visuals
Median Sales Change by Region
Houses Sold in Latest Year & Quarter
Sales in Last 12 Months
Offer vs Purchase Price
YOY Sales Growth by Sales Type
🧮 DAX Measures
Mediun Sales Change = 
VAR CurrMedianPrice = 
    MEDIANX(
        FILTER(Housing, YEAR(Housing[date]) = YEAR(MAX(Housing[date]))),
        Housing[purchase_price]
    )
VAR PrevMedianPrice = 
    MEDIANX(
        FILTER(Housing, YEAR(Housing[date]) = YEAR(MAX(Housing[date])) - 1),
        Housing[purchase_price]
    )
RETURN IF(PrevMedianPrice <> 0, (CurrMedianPrice - PrevMedianPrice) / PrevMedianPrice, BLANK())
Houses Sold on latest year and quarter = 
    CALCULATE(
        DISTINCTCOUNT(Housing[house_id]),
        YEAR(Housing[date]) = YEAR(MAX(Housing[date])) &&
        QUARTER(Housing[date]) = QUARTER(MAX(Housing[date]))
    )
Last 12 months Sales = 
    CALCULATE(
        SUM(Housing[purchase_price]),
        DATESINPERIOD(Housing[date], MAX(Housing[date]), -12, MONTH)
    )
YOY_SALES_GROWTH = 
VAR CurrYearSales = 
    CALCULATE(SUM(Housing[purchase_price]), YEAR(Housing[date]) = YEAR(MAX(Housing[date])))
VAR PrevYearSales = 
    CALCULATE(SUM(Housing[purchase_price]), YEAR(Housing[date]) = YEAR(MAX(Housing[date])) - 1)
RETURN IF(PrevYearSales <> 0, (CurrYearSales - PrevYearSales) / PrevYearSales, BLANK())
🧠 Insights
YOY Median Price changes help detect pricing trends and inflation effects.
Quarter-wise sales volumes show seasonal demand patterns.
A clear view of last 12-month performance for rolling trend analysis.
Offer vs Purchase gap suggests negotiation leverage.
YOY Sales Growth highlights shifts in buying behavior by sales type.
📸 Screenshot – House Market Overview
