# Business-Performance-Intelligence-Power-BI-Sales-Analysis
End-to-end Power BI project analysing sales, profit, customers, products, expenses, inventory and sales targets for a Nigerian IT-products distributor. Challenge: IFEXA Data Analysis Online Class — Business Intelligence Master Dataset (Sept 2026) Analyst: Gerald Verallo

Project Objective

Turn a multi-table business dataset into an interactive Power BI report that helps management answer:

How is revenue trending month to month?
Which states and regions perform best?
Which product categories and products drive revenue and profit?
Which customer segments and sales channels are strongest or weakest?
Which salespeople generate the most sales and profit?
What should the business do next to grow revenue and protect margin?

Tools & Skills
Area	Detail
Tool	Microsoft Power BI Desktop
Source	Excel workbook IFEXA_BUSINESS_INTELLIGENCE_MASTER_DATASET.xlsx (8 sheets)
ETL	Power Query (promote headers, set data types)
Modeling	Star schema, 8 many-to-one relationships
Calculations	DAX measures and calculated columns
Visualisation	KPI cards, line / column / bar / donut charts, treemap, table, slicers
UX	Drill-through pages, custom tooltip page, cross-filtering slicers

Data Model

Star schema with Fact_Sales at the centre.

Table	Type	Rows	Description
Fact_Sales	Fact	30,250	Orders: channel, payment method, status, quantity, price, discount, gross/net sales
Fact_Expenses	Fact	4,000	Expenses by department, type and approval status
Fact_Inventory	Fact	5,000	Warehouse stock movement: opening, in, out, reorder level, closing
Fact_Targets	Fact	1,000	Monthly sales and customer targets per employee
Dim_Customers	Dimension	2,500	Segment, state, city, region, join date, rating
Dim_Products	Dimension	250	Product name, category, brand, unit cost
Dim_Employees	Dimension	80	Department, job level, region
Dim_Date	Dimension	1,096	Calendar 2024-2026: year, month, quarter, week, weekday flag
KEY MEASURES	Measure table	–	Holds the headline DAX measures

DAX Used

Measures (KEY MEASURES)

DAX
Total Net Sales      = SUM ( Fact_Sales[Net_Sales] )
TOTAL GROSS SALES    = SUM ( Fact_Sales[Gross_Sales] )
Total Order          = COUNTROWS ( Fact_Sales )
Total Customers      = DISTINCTCOUNT ( Fact_Sales[Customer_ID] )
Total Quantity       = SUM ( Fact_Sales[Quantity] )
Average Order Value  = DIVIDE ( [Total Net Sales], [Total Order] )
MARGIN               = DIVIDE ( [Total Net Sales], [TOTAL GROSS SALES] )

Calculated columns (Fact_Sales)

##DAX
Cost_Product = RELATED ( Dim_Products[Unit_Cost] )
Total Cost   = Fact_Sales[Quantity] * Fact_Sales[Cost_Product]
profit       = Fact_Sales[Net_Sales] - Fact_Sales[Total Cost]
Margin       = DIVIDE ( Fact_Sales[profit], Fact_Sales[Net_Sales] )
Profit Margin (measure) = AVERAGE ( Fact_Sales[Margin] )
🧹 Data Preparation (Power Query)
Connected to the Excel workbook and loaded each sheet as its own table.
Promoted first rows to headers (including Dim_Employees, where headers loaded as Column1–5).
Set data types: text for IDs, date for dates, whole/decimal numbers for quantities and amounts, logical for Is_Weekend.
Built a Dim_Date calendar and linked it to inventory and customer join dates.
📑 Report Pages
#	Page	Purpose
1	Executive Dashboard	Headline KPIs (Net Sales, Orders, Customers, AOV, Profit, Profit Margin) plus revenue by month, state, region, category and product. Slicers: State, Region, Channel
2	Sales & Profit Analysis	Net Sales vs Profit by month, region, channel, category and top salespeople
3	Management Analysis	Best/worst segment by profit, profit by category treemap, product table (sales, profit, margin), Key Insights & Recommendations
4	6 Questions	Written answers to the six challenge questions
5	Product Name (Drill-through)	Right-click a product to see its detail
6	City (Drill-through)	Right-click a city to see its detail
7	Tool Tip	Custom hover tooltip
🔑 Key Results
KPI	Value
Total Net Sales	≈ 183.09 billion
Total Profit	≈ 42.7 billion
Profit Margin (avg. order margin)	≈ 22%
Total Orders	30,250
Unique Customers	2,500
Period covered	Jan 2024 – Dec 2026

Highlights

🏆 Top states: Kano (≈ 20.8bn), then Edo (≈ 20.0bn), Abia, Imo, Delta.
🏆 Top region (by salesperson region): North Central (≈ 50.4bn).
🏆 Top categories: Computers (≈ 46.9bn) and Storage (≈ 40.6bn). The top products are Flash Drive and Laptop.
👥 Segments: Retail is the strongest (≈ 74.5bn). Government is the weakest (≈ 15.6bn).
📦 Channels: Walk-in, Social Media and Corporate Sales lead. All five channels are close in value.
💡 Insights & Recommendations (from the report)

Insights

Revenue is ≈ 183.09bn at a ≈ 22% margin.
Kano and Edo are the top states.
Storage (flash drives) and Computers (laptops) lead the categories.
North Central is the best region, and Retail is the strongest segment.
Cheap, high-volume products (e.g. Flash Drive) sell well, while high-value products underperform in some periods.

Recommendations

Investigate why the weakest segment/channel underperforms, then run targeted campaigns to reach more customers.
Add support and campaigns for the Accessories category to lift sales.
Keep the current strategy for Computers / Laptops, a high-value, high-margin line.
Review pricing, discounts and procurement for products with high volume but low margin.
⚠️ Data Quality Notes

Issues found while profiling the model. Fixing them in Power Query is a good next step:

250 duplicate Order_ID values in Fact_Sales.
Inconsistent channel label: " online" (leading space/lower-case, 182 rows) vs "Online".
Payment method has both Transfer and Bank Transfer.
Order_Status includes Cancelled, Returned and Pending orders, which are currently counted in sales.
242 inventory rows with negative Closing_Stock.
The MARGIN measure divides Net by Gross Sales (a discount ratio, not profit margin). Use Profit Margin for profitability.
The Excel source path is hard-coded to a local drive. Update it under Transform data → Data source settings.
🚀 How to Use
Clone or download this repository.
Open IFEXE-DA-0486-GERALD_VERALLO2.pbix in Power BI Desktop.
If prompted, point the data source to your local copy of IFEXA_BUSINESS_INTELLIGENCE_MASTER_DATASET.xlsx, then Refresh.
Use the slicers, and right-click a product or city to drill through.
📁 Repository Structure
├── IFEXE-DA-0486-GERALD_VERALLO2.pbix                # Power BI report
├── IFEXA_BUSINESS_INTELLIGENCE_MASTER_DATASET.xlsx   # Source data (optional)
├── screenshots/                                      # Page screenshots
└── README.md
📸 Screenshots

Add screenshots of each page here, e.g. ![Executive Dashboard](screenshots/executive-dashboard.png)

##👤 Author

Gerald Verallo — Data Analysis (IFEXA Online Class) GitHub: @your-username
