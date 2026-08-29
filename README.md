# Zepto Inventory & Discount Analysis (Excel)

Data cleaning and analysis of Zepto's product listing data in Excel, identifying high-value out-of-stock products, inventory weight distribution, and top-discounting categories.

## Dataset

Sourced from [Kaggle](https://www.kaggle.com/datasets/palvinder2006/zepto-inventory-dataset/data?select=zepto_v2.csv), originally scraped from Zepto's official product listings.

**Columns:**
| Column | Description |
|---|---|
| `sku_id` | Unique identifier per product |
| `name` | Product name as shown on the app |
| `category` | Product category (Fruits, Snacks, Beverages, etc.) |
| `mrp` | Maximum Retail Price  |
| `discountPercent` | Discount applied on MRP |
| `discountedSellingPrice` | Final price after discount  |
| `availableQuantity` | Units available in inventory |
| `weightInGms` | Product weight in grams |
| `outOfStock` | Boolean flag for stock availability |
| `quantity` | Units per package (mixed with grams for loose produce) |

## Data Cleaning

- Removed duplicate rows
- Removed rows with `0` in the `mrp` column (invalid entries)
- Converted `mrp` and `discountedSellingPrice` from paise to rupees (divided by 100, via Paste Special → Divide)
- Created a `weight group` column, categorizing products as `low`, `medium`, or `bulk` based on `weightInGms`:
  ```
  =IF(weightInGms<=1000,"low",IF(weightInGms<=4000,"medium","bulk"))
  ```
- Created a `total inventory weight` column: `availableQuantity * weightInGms`, to measure actual stock weight per product

## Tasks & Findings

**1. High-MRP products currently out of stock**
Built a PivotTable/PivotChart filtered to `outOfStock = TRUE`, ranking products by average MRP to surface the highest-value items currently unavailable. Top result: **Patanjali Cow's Ghee**, with the highest average MRP (5.65) among out-of-stock products — a clear candidate for restocking priority given its price point.

**2. Total inventory weight per category**
Summed `total inventory weight` grouped by category. Cooking Essentials and Munchies carry by far the heaviest total inventory (~1.4M grams each), well above every other category.

**3. Top 5 categories by average discount**
Ranked categories by average `discountPercent`. Fruits & Vegetables leads with a ~15.5% average discount, notably higher than the next closest category (Meats, Fish & Eggs at ~11%).

Each task includes a PivotTable and an accompanying PivotChart for visual comparison.

## Tools

`Microsoft Excel` — PivotTables, PivotCharts, Paste Special, nested `IF` formulas, Remove Duplicates

## What I'd Explore Next

- Cross-reference at-risk-of-stockout products (low `availableQuantity`) against their discount level, to see if heavily discounted items are also running low
- Break down inventory weight by `weight group` (low/medium/bulk) instead of just by category, for a warehousing/logistics angle
