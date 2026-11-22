# Task 4 Build Guide: Corporate Financial & Margin Performance Dashboard

Estimated time: 45 to 60 minutes.

**Files**
| File | Use |
|---|---|
| `data_model/Financial_Clean.csv` | Financial Sample, cleaned (700 rows) |
| `data_model/Dim_Date.csv` | Calendar 2013 and 2014 |
| `DAX_Measures.dax` | All measures, ready to paste |
| `EXPECTED_VALUES.md` | Numbers to check your dashboard against |

What was cleaned from the Microsoft Financial Sample: column names trimmed (the source has a column called ` Sales` with a leading space, renamed `Net Sales`), 53 blank Discount Band values set to `None` (all have zero discount), and Quarter and Year-Quarter columns added. Checks: Gross Sales minus Discounts equals Net Sales, and Net Sales minus COGS equals Profit, for every row.

## Important: the budget is an assumption
The Financial Sample has **no budget column**. The task asks for variance against budget targets, so this kit builds the budget from two adjustable assumptions and says so on the dashboard:
- Budget discount rate = 5% of gross sales (actual is 7.2%)
- Budget COGS ratio = 85% of net sales (actual is 85.8%)

Both are slider parameters, so anyone can change them. Add a text box on the page: "Budget is an assumption: planned discount rate and COGS ratio, adjustable with the sliders."

Also note: 2013 contains only September to December. Do not compare 2013 with 2014 as full years.

## Step 1. Load the data
1. **Get data > Text/CSV**: load `Financial_Clean.csv` and `Dim_Date.csv`.
2. Set types: Date = Date; Gross Sales, Discounts, Net Sales, COGS, Profit = Fixed decimal number; Units Sold = Decimal number.
3. Close & Apply.

## Step 2. Model
- Relationship: Financial_Clean[Date] (many) to Dim_Date[Date] (one), single direction.
- Mark Dim_Date as the date table. Sort Month Name and Month Short by Month Number.

## Step 3. Add the measures
1. Create the `_Measures` table (Enter data, one empty column).
2. Create the calculated tables with **New table**: `Waterfall Steps`, `Volume Change`, `Discount Rate Change`, `COGS Change`, `Budget Discount Rate`, `Budget COGS Ratio` (all shown in `DAX_Measures.dax`).
   Or use **Modeling > New parameter > Numeric range** for the five slider tables. It creates the table, the value measure and the slider for you. Use these ranges:
   | Parameter | Min | Max | Step | Default |
   |---|---|---|---|---|
   | Volume Change | -20% | 20% | 1% | 0% |
   | Discount Rate Change | -5% | 5% | 0.5% | 0% |
   | COGS Change | -10% | 10% | 1% | 0% |
   | Budget Discount Rate | 0% | 15% | 0.5% | 5% |
   | Budget COGS Ratio | 75% | 95% | 0.5% | 85% |
3. Paste all measures. The task asks for `[Gross Profit %]`, `[COGS Ratio]` and `[Segment Margin]`; all three are included.
4. Format: sales and cost measures as Currency with 0 decimals (display units: Millions), ratios as Percentage with 1 decimal.

## Step 4. Build three pages

### Page 1: Financial Overview
| Visual | Fields |
|---|---|
| Slicers | Dim_Date[Year], Segment, Country |
| 5 KPI cards | Total Gross Sales, Total Net Sales, Total COGS, Total Profit, Gross Profit % |
| Waterfall (stacked column) | Axis = Waterfall Steps[Step], Values = WF Base and WF Bar. Set WF Base fill to No fill and hide it from the legend |
| Column + line chart | X = Dim_Date[Year-Quarter], Columns = Total Net Sales, Line = Gross Profit % |
| Matrix | Rows = Segment, Values = Total Net Sales, Total COGS, Total Profit, Segment Margin, COGS Ratio |

The waterfall must show: Gross Sales > Discounts > Net Sales > COGS > Profit. Set the data labels to show values in millions.

### Page 2: Budget Variance
| Visual | Fields |
|---|---|
| Sliders | Budget Discount Rate, Budget COGS Ratio |
| Cards | Total Net Sales, Budget Net Sales, Net Sales Variance, Profit Variance |
| Clustered bar, geographic segments | Axis = Country, Values = Total Net Sales, Budget Net Sales |
| Clustered bar, product lines | Axis = Product, Values = Total Net Sales, Budget Net Sales |
| Matrix, cost centers | Rows = Product (manufacturing cost centre), Values = Total COGS, COGS Ratio, Total Profit, Profit Variance, with `Variance Colour` as font colour |

Here Product is used as the manufacturing cost centre, because the dataset has no separate cost centre field.

### Page 3: Scenario Forecasting
| Visual | Fields |
|---|---|
| 3 sliders | Volume Change, Discount Rate Change, COGS Change |
| Cards | Scenario Net Sales, Scenario COGS, Scenario Profit, Scenario Margin %, Scenario Profit Impact |
| Clustered column | Axis = Segment, Values = Total Profit (actual) and Scenario Profit |
| Text box | Explains each slider in one line |

Test: Volume +10%, Discount Rate -2 points, COGS 0% should give Scenario Net Sales about $133.4M and Scenario Profit about $21.4M.

## Step 5. Polish and export
- Same colour pair on every page: blue for actuals, orange for budget or scenario.
- Rename the pages and add titles that state the message (for example "Enterprise sells below cost").
- Check the numbers against `EXPECTED_VALUES.md`.
- **File > Save as** `Financial_KPI_Dashboard.pbix`, put it in this folder, push to GitHub.
- Take a screenshot of each page for the README.
