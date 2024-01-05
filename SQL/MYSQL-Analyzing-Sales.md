### Analyzing Sales

Schema: sales_purchase

| Name     |  Type   |                                  Description |
| :------- | :-----: | -------------------------------------------: |
| oid      |   int   | This is the order id. It is the primary key. |
| purchase |   int   |   Amount at which the product was purchased. |
| sale     |   int   |        Amount at which the product was sold. |
| year     |   int   |      Year in which the transaction happened. |
| month    | char(3) |     Month in which the transaction happened. |

The data of purchases and sales is maintained in a database.

Write a MYSQL query to return the sum if the sale amounts that meet the following criteria.

1. The purchase amount must not be distinct.
2. The (year,month) pair must be distinct.

Here's a sample table and output.

#### Sample Data Table

#### sales_purchase

| oid | purchase | sale | year | month |
| --- | -------- | ---- | ---- | ----- |
| 747 | 240      | 240  | 2019 | JAN   |
| 425 | 240      | 248  | 2019 | JUN   |
| 878 | 200      | 267  | 2019 | APR   |
| 904 | 230      | 279  | 2018 | MAY   |
| 107 | 230      | 270  | 2018 | MAR   |
| 227 | 370      | 388  | 2020 | APR   |
| 534 | 330      | 394  | 2018 | MAR   |
| 305 | 300      | 367  | 2019 | JAN   |
| 145 | 260      | 308  | 2020 | MAY   |
| 202 | 370      | 451  | 2019 | APR   |

| OUTPUT |
| ------ |
| sum    |
| 915    |

NOTE: The records where dates are not distinct are excluded from the report. Of the records with distinct dates, the ones that have non-distinct purchase amounts are included.

### Solution:

```sql
SELECT SUM(sale) AS sum
FROM sales_purchase AS sp1
WHERE EXISTS (
  SELECT 1
  FROM sales_purchase AS sp2
  WHERE sp2.purchase = sp1.purchase
    AND sp2.oid <> sp1.oid
)
AND NOT EXISTS (
  SELECT 1
  FROM sales_purchase AS sp3
  WHERE sp3.year = sp1.year
    AND sp3.month = sp1.month
    AND sp3.oid <> sp1.oid
);
```

### Explanation:

    1. Identify non-distinct purchase amounts:
    - The WHERE EXISTS*** clause checks for each row if there's another row with the same purchase amount but a different order ID, ensuring non-distinct purchase amounts.

    2.  Enforce distinct (year, month) pairs:
    -   The AND NOT EXISTS clause eliminates rows where the (year, month) combination appears multiple times, ensuring distinct date pairs.

    3. Calculate the sum:
     - The SUM(sale) function aggregates the sale amounts from the rows that meet both criteria, providing the desired sum.
