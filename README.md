# multi-branch-sales-powerquery
Consolidates messy multi-branch retail sales data (4 different Excel formats) into a clean star-schema model using Power Query (M language) and DAX in Power BI.
# Multi-Branch Retail Sales Consolidation | Power Query (M) + Power BI

Consolidates 4 real-world-style, **differently formatted** branch sales exports
into one clean fact table using **Power Query / M language**, then models it
into a star schema for Power BI reporting.

This project was built specifically to demonstrate **Power Query, M language,
and ETL/data-warehousing skills** — the messy source files were designed to
mirror problems analysts hit constantly in real jobs: inconsistent headers,
text-formatted dates, currency symbols stored as text, blank rows, missing
values, and pivoted (wide-format) exports that need unpivoting.

---

## 1. Business Problem

A retail company has 4 branches (Lahore, Karachi, Islamabad, Peshawar). Each
branch exports its own sales data from a different POS system, so every file
has a different structure. Management wants **one consolidated dashboard**
showing sales trends, branch performance, and category performance — but the
raw files can't be combined as-is.

## 2. The Messy Source Data (`/Source_Data`)

| File | Format problem |
|---|---|
| `branch_lahore.xlsx` | 2 title rows above the real header; needs `Table.Skip` |
| `branch_karachi.xlsx` | Date stored as **text** in `DD/MM/YYYY`; price stored as text with `"Rs "` prefix |
| `branch_islamabad.xlsx` | Random **blank spacer rows**; ~10% missing `Category`; ~15% missing `UnitPrice` |
| `branch_peshawar.xlsx` | **Wide/pivoted** format — months as columns instead of rows; needs `Unpivot` |

## 3. What Power Query Does (`/Power_Query_M_Code`)

| Query | Key M functions used |
|---|---|
| `01_Master_Query_CombineAllBranches.pq` | `Table.Skip`, `Table.PromoteHeaders`, `Date.FromText`, `Text.Replace`/`Number.From` (currency-text cleanup), `Table.SelectRows` (blank-row removal), `Table.ReplaceValue` (fill missing category), conditional column (derive missing price), `Table.UnpivotOtherColumns`, `Table.NestedJoin`, `Table.Combine`, `Table.Distinct` |
| `02_Dim_Date.pq` | `List.Dates` calendar table generator |
| `03_Dim_Product.pq` | Distinct product list, referenced (not duplicated) from the fact query |

Open any `.pq` file and paste it straight into **Power BI Desktop → Home →
Transform Data → Advanced Editor** (update the folder path first).

## 4. Star Schema Data Model
