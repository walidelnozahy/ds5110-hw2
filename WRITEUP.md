# Homework 2 write-up

## Step 0: Descriptive analysis

| File | Shape | Tidy? |
| --- | --- | --- |
| `company_quarterly_revenue_raw.csv` | 5 × 13 | **No** |
| `orders.csv` | 20 × 5 | Yes |
| `shipments.csv` | 35 × 2 | Yes, but one row is a shipment, not an order |

The revenue file breaks **rule 1** (every variable is a column): *quarter* is a variable, but its twelve values are column headers, and *revenue* is spread across those twelve columns. It also breaks **rule 2** (every observation is a row): one row holds twelve (company, quarter) observations. Rule 3 holds, since each cell has one number.

`orders.csv` satisfies all three rules: `order_id` is unique, so each row is one order, and each column is one variable. `shipments.csv` also satisfies them, but for a different unit: `tracking` is unique, so each row is one *package*. The raw charts (`figures/step0_raw_data.png`) already show several `order_id`s with two or three shipment rows.

## Step 1: Revenue table

It is wide. I used `melt` to move the quarter headers into a `quarter` column, giving 60 rows (5 companies × 12 quarters), one per (company, quarter). The main violation was **rule 1**: a variable's values were stored as column names, which is what forced each row to hold several observations (rule 2).

## Step 2: Duplicate keys

`order_id` is unique in `orders` (20 rows, 20 ids) but **not** in `shipments` (35 rows, 20 ids: 9 orders have one shipment, 7 have two, 4 have three). So the join is **one-to-many**. Every order has at least one shipment and every shipment has an order, so I predicted 1 × 35 = 35 rows. The join returned 35, and `validate="one_to_many"` would have raised an error if the orders side ever stopped being unique.

## Step 3: Mean amount per customer

On the joined table the means are wrong for four customers. The largest gap is C107: 106.0 on the joined table versus 96.5 on `orders`. **The `orders` number is correct.** `amount` is a property of the order, and the join copies it once per shipment. C107's 144 order shipped in three packages, so it counted three times, pulling the mean up. To join correctly I first counted shipments per order, then joined that count onto `orders`. That join is one-to-one, so the table stays at 20 rows and the means match `orders` exactly.

## Step 4: Analysis-ready table

The final table has one row per **order** (20 rows), with columns `order_id`, `customer_id`, `company`, `quarter`, `order_amount`, `n_shipments` and `company_quarter_revenue`. I chose the order as the unit because it is the finest level at which every variable has one value. Revenue was joined on (company, quarter), which is unique in the tidy revenue table but repeats across orders, so the join is many-to-one and the row count stayed at 20, as predicted. Renaming `amount` and `revenue` keeps an order's value distinct from company revenue. Caveat: revenue repeats when a company has several orders in one quarter, so it should never be summed down this table.

## Step 5: Visualization

`figures/step5_before_after.png` puts the raw revenue heatmap next to a scatter of each order's amount against its company's revenue that quarter, with point size showing the number of shipments. Before tidying, that revenue figure was hidden in a column *name*, and the shipment count was spread over several rows of a different file. Now x, y, colour and size are each one column, so the chart is a single `scatterplot` call, and no order is counted twice.
