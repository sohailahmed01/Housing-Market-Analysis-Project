# 🏠 House Market Analytics – Power BI Report

This Power BI report delivers deep insights into the **Danish housing market**, analyzing sales trends, pricing metrics, property types, and regional dynamics using ~100,000 rows of housing data from **Google BigQuery**.

---

## 📁 Dataset Overview

- **Source**: Google BigQuery (CSV format)
- **Volume**: ~100,000 rows × 19 columns (Columns A–S)
- **Key Features**: `date`, `region`, `purchase_price`, `offer_price`, `sqm`, `sqm_price`, `sales_type`, `house_type`, `age`

---

## 📊 Page 1 – House Market Overview

### 🔍 Visuals

- Median Sales Change by Region
- Houses Sold in Latest Year & Quarter
- Sales in Last 12 Months
- Offer vs Purchase Price
- YOY Sales Growth by Sales Type

### 🧮 DAX Measures

```
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
```

```
Houses Sold on latest year and quarter =
    CALCULATE(
        DISTINCTCOUNT(Housing[house_id]),
        YEAR(Housing[date]) = YEAR(MAX(Housing[date])) &&
        QUARTER(Housing[date]) = QUARTER(MAX(Housing[date]))
    )
```

```
Last 12 months Sales =
    CALCULATE(
        SUM(Housing[purchase_price]),
        DATESINPERIOD(Housing[date], MAX(Housing[date]), -12, MONTH)
    )
```

```
YOY_SALES_GROWTH =
VAR CurrYearSales =
    CALCULATE(SUM(Housing[purchase_price]), YEAR(Housing[date]) = YEAR(MAX(Housing[date])))
VAR PrevYearSales =
    CALCULATE(SUM(Housing[purchase_price]), YEAR(Housing[date]) = YEAR(MAX(Housing[date])) - 1)
RETURN IF(PrevYearSales <> 0, (CurrYearSales - PrevYearSales) / PrevYearSales, BLANK())
```

### 🧠 Insights

- YOY Median Price changes help detect pricing trends and inflation effects.
- Quarter-wise sales volumes show seasonal demand patterns.
- A clear view of last 12-month performance for rolling trend analysis.
- Offer vs Purchase gap suggests negotiation leverage.
- YOY Sales Growth highlights shifts in buying behavior by sales type.

### 📸 Screenshot – House Market Overview

*(Insert the Page 1 screenshot here — re-upload the image to this repo so the link doesn't expire, e.g. `![Page 1 Screenshot](images/page1.png)`)*

---

## 📊 Page 2 – Sales Performance

### 🔍 Visuals

- Sales by Region
- Key Influencers (AI Visual)
- Year-to-Date Sales Table
- Offer-to-SQM Ratio by Sales Type
- Average SQM Price by Region

### 🧮 DAX Measures

```
Sales by region =
    CALCULATE(
        SUM(Housing[purchase_price]),
        ALLEXCEPT(Housing, Housing[region])
    )
```

```
Offer to SQM Ratio =
    DIVIDE(SUM(Housing[Offer Price]), SUM(Housing[sqm]))
```

```
Total YTD Sales =
    TOTALYTD(SUM(Housing[purchase_price]), Housing[date].[Date])
```

### 🧠 Insights

- Easily compare how regions perform in terms of revenue.
- Key Influencers AI visual uncovers price drivers like region, sqm, and house type.
- YTD sales gives a cumulative view and compares performance across years.
- Offer-to-SQM reveals how much buyers are offering per square meter.
- SQM Price trends help identify urban vs suburban areas.

### 📸 Screenshot – Sales Performance

*(Insert the Page 2 screenshot here — re-upload the image to this repo, e.g. `![Page 2 Screenshot](images/page2.png)`)*

---

## 📊 Page 3 – House Type Insights

### 🔍 Visuals

- Avg Offer & Purchase Price by House Type
- Avg Inflation / Interest Rate / Yield by House Type
- Avg SQM and SQM Price by House Type

### 🧮 DAX Measure

```
Average SQM Price =
    AVERAGE(Housing[sqm_price])
```

### 🧠 Insights

- Highlights pricing trends by house type (e.g., detached, apartments).
- Yield and inflation by category inform better investment decisions.
- SQM & Price per SQM compare density and valuation by category.

### 📸 Screenshot – House Type Insights

*(Insert the Page 3 screenshot here — re-upload the image to this repo, e.g. `![Page 3 Screenshot](images/page3.png)`)*

---

## 🛠️ Tools & Tech Stack

- **Data Source**: Google BigQuery (CSV)
- **Visualization**: Power BI Desktop
- **Scripting**: DAX
- **AI Integration**: Key Influencers (Power BI Visual)

---

## 🚀 How to Use

1. Open the `.pbix` file in Power BI Desktop.
2. Navigate across the report pages to explore the data.
3. Use slicers for filtering:
   - Region
   - Sales Type
   - House Type
   - Year and Quarter
