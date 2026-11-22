# Task 2 Build Guide: Power BI Executive Sales & Profit Dashboard

Estimated time: 45 to 60 minutes. Everything you need is in this folder.

**Files**
| File | Use |
|---|---|
| `data_model/Fact_Orders.csv` | Order lines (9,994 rows) |
| `data_model/Dim_Customer.csv` | 793 customers |
| `data_model/Dim_Product.csv` | 1,862 products |
| `data_model/Dim_Date.csv` | Calendar 2015 to 2018 |
| `DAX_Measures.dax` | All measures, ready to paste |
| `EXPECTED_VALUES.md` | Numbers to check your dashboard against |

The CSVs are the cleaned version of the Superstore data (blank rows and the pasted Returns/People tables removed, postal codes fixed, dates in yyyy-mm-dd). The cleaning steps are shown in Task 1.

## Step 1. Load the four tables
1. Power BI Desktop > **Get data > Text/CSV**. Load all four files from `data_model/`.
2. In **Transform data** (Power Query), check the data types:
   - Fact_Orders: Order Date and Ship Date = Date, Sales and Profit = Fixed decimal number, Discount = Decimal number, Quantity = Whole number, Postal Code = Text.
   - Dim_Date: Date = Date, Year/Month Number/Day = Whole number.
3. **Close & Apply.**

## Step 2. Build the star schema
Open **Model view** and create these relationships (all Many to One, single direction):

| From (many) | To (one) |
|---|---|
| Fact_Orders[Order Date] | Dim_Date[Date] (active) |
| Fact_Orders[Ship Date] | Dim_Date[Date] (inactive, optional) |
| Fact_Orders[Customer ID] | Dim_Customer[Customer ID] |
| Fact_Orders[Product ID] | Dim_Product[Product ID] |

Then right-click Dim_Date > **Mark as date table** > choose the Date column.
Sort Dim_Date[Month Name] by Dim_Date[Month Number], and Dim_Date[Month Short] by Month Number.

Region, State and City stay in Fact_Orders because one customer can order from different places. Note: 32 Product IDs in the raw data have two product names. Dim_Product keeps the first name for each ID.

## Step 3. Add the DAX measures
1. **Enter data** > create a table named `_Measures` with one empty column, then delete the column's data (you only need the table to hold measures).
2. Open `DAX_Measures.dax` and paste the measures one by one with **New measure**.
3. Set formats: Total Sales and Total Profit = Currency ($, 0 decimals); Profit Margin %, YoY Sales Growth %, YoY Profit Growth % = Percentage (1 decimal).

The four measures asked for in the task are all included: `[Total Sales]`, `[Total Profit]`, `[Profit Margin %] = DIVIDE([Total Profit],[Total Sales],0)` and `[YoY Sales Growth %]`.

## Step 4. Build three pages

### Page 1: Executive Overview
| Visual | Fields |
|---|---|
| Slicer (Between / Dropdown) | Dim_Date[Date] |
| Slicer (Tile) | Dim_Date[Year] |
| Slicer (Dropdown or Tile) | Fact_Orders[Region] |
| 4 KPI cards | Total Sales, Total Profit, Total Orders, Profit Margin % |
| 2 small cards (or KPI visuals) | YoY Sales Growth %, YoY Profit Growth % |
| Line chart | X = Dim_Date[Year-Month], Y = Total Sales and Total Profit |
| Treemap | Group = Dim_Product[Category] > Sub-Category, Values = Total Sales, Tooltip = Total Profit, Profit Margin % |
| Clustered bar | Axis = Fact_Orders[Region], Values = Total Sales, Total Profit |

For the monthly chart the task suggests a Waterfall or Line. Use the line chart, and optionally add a Waterfall with Axis = Dim_Date[Year] and Values = Total Sales to show the year-to-year change.

### Page 2: Product Profitability
| Visual | Fields |
|---|---|
| Matrix | Rows = Dim_Product[Category] > Sub-Category. Values = Total Sales, Total Profit, Profit Margin %, Avg Discount % |
| Scatter chart | X = Avg Discount %, Y = Profit Margin %, Details = Sub-Category, Size = Total Sales |
| Bar chart | Axis = Sub-Category, Values = Total Profit (sorted ascending, to show the loss makers) |
| Slicers | Segment (Dim_Customer[Segment]), Year |

Conditional formatting on the matrix: select Profit Margin % > Cell elements > Background colour > **Field value** > `Margin Colour`. Do the same for Total Profit with `Profit Colour` font colour.

### Page 3: Region and Customers
| Visual | Fields |
|---|---|
| Treemap | Group = Fact_Orders[Region] > State, Values = Total Sales, Tooltip = Total Profit |
| Filled map or Map (optional) | Location = State, Size = Total Sales, Colour saturation = Profit Margin % |
| Table | Top 10 Dim_Customer[Customer Name] by Total Sales (Visual-level filter: Top N = 10), show Total Profit |
| Donut | Segment, Total Sales |
| Column chart | Fact_Orders[Ship Mode], Total Orders |

## Step 5. Add the insights text
Add a text box on Page 1 with the headline findings (from Task 1): sales up 51% since 2015, Furniture margin 2.5%, discounts above 20% cause losses, Nov and Dec are the peak months.

## Step 6. Polish
- Use one colour pair: blue for sales, orange-red for profit or loss.
- Align visuals to a grid, same title style on every visual.
- Rename the pages: Executive Overview, Product Profitability, Region and Customers.
- Test the slicers: pick Year 2018 and Region West and watch every visual change.
- Check the numbers against `EXPECTED_VALUES.md`.

## Step 7. Export the deliverables
1. **File > Save as** `Superstore_Executive_Dashboard.pbix`
2. **File > Export > Export to PDF**. This is the high-resolution PDF report the task asks for.
3. Put both files in this folder (`Task_2_PowerBI_Superstore_Dashboard/`) and push to GitHub. A .pbix is usually under 100 MB, so GitHub accepts it.
4. Take one screenshot of Page 1 and save it as `dashboard_preview.png` for the README.
