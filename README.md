# Monthly AR MIS Reporting Pack

An Excel and Power Query pack that turns monthly invoice and payment files into a receivables aging report, a KPI scorecard and a ledger reconciliation. Built on synthetic Samiya Enterprise data (Apr–Aug 2026) to practise the monthly reporting work an MIS role does.

## Problem

The CFO needs to know what customers owe, how late it is, and whether the numbers can be trusted.

## Data

- Monthly invoice files: InvoiceID, CustomerID, InvoiceDate, DueDate, InvoiceAmount, SalesRep
- Monthly payment files: PaymentID, InvoiceID, PaymentDate, AmountPaid, PaymentMode
- Reference sheets: customer, GL control (month and AR ledger closing balance), targets
- Synthetic data with planted errors

## What I built

- Power Query folder combine for the monthly invoice and payment files, kept as raw queries.
- Cleaning: 11 invoice rows excluded (missing keys or an unreadable date), 1,495 invoices kept.
- Payments checked: no nulls, errors or non-positive amounts, 1,361 payments kept.
- Outstanding per invoice (invoice amount minus total paid, rounded to 2 decimals).
- Aging buckets as of 31 Aug 2026: Current, 1-30, 31-60, 61-90, 90+.
- KPI scorecard with Green/Amber/Red status against targets.
- Reconciliation of the invoice subledger to the GL closing balance.
- A one-page executive summary.

## Findings (Aug 2026)

Open receivables are 138.2M, of which 47.9M (35%) is overdue.

| Bucket | Invoices | Outstanding |
|---|---|---|
| Current | 251 | 90.3M |
| 1-30 | 80 | 21.7M |
| 31-60 | 49 | 11.7M |
| 61-90 | 46 | 9.8M |
| 90+ | 31 | 4.7M |

About 65% of open receivables are not yet due, and the 90+ bucket is 3.4% of the total.

| KPI | Result | Target | Status |
|---|---|---|---|
| Collection rate | 68% | 30% or more | Green |
| Overdue share | 35% | 40% or less | Green |
| DSO | 49.7 days | 50 or less | Green |

DSO is only 0.3 days under its limit, so one slow month would turn it Amber.

**Data quality**

- Five invoices were overpaid, 685K in total. I show them separately, not as receivables.

**Reconciliation**

| Item | Amount |
|---|---|
| GL closing balance (Aug) | 138,440,722.50 |
| Subledger (net outstanding) | 137,533,650.69 |
| Overpayments | -684,799.43 |
| Gross AR | 138,218,450.12 |
| **Gap to GL** | **222,272.38 (0.16%)** |

The gap is under my 0.5% tolerance, but I have not found its cause.

## Tools

Excel, Power Query, PivotTables, SUMIFS.

## Limits

- The data is synthetic.
- The 222K difference to the GL is unexplained.
- The aging date is fixed at 31 Aug 2026 inside the query and does not update by itself.
- Duplicate invoice IDs were not checked.
- Only some of the planted errors were tested (blank keys, unreadable dates, non-positive amounts). 
- I treated the 01-Aug-26 row of the GL as the August month-end balance. That is an assumption.
