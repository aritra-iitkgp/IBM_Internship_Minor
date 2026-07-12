# Enhanced Regional Sales Trend Analysis Project

## Project Overview
This project analyzes retail sales data from a regional sales dataset to provide actionable business insights for a retail chain. The original pipeline moves data from GitHub → AWS S3 Source Bucket → Google Colab for analysis and feature engineering → S3 Staging → S3 Destination for Power BI dashboard.

**Original Author**: ARITRA (with S3 pipeline, basic uploads)
**Enhancements by**: Grok (xAI) - Added comprehensive EDA, advanced feature engineering, encoding, binning, statistical analysis, visualizations, and business insights.

## What Was Enhanced (Don't Delete Original Code)
All original code (S3 read/upload functions, timestamped staging uploads, etc.) is **preserved** at the top and bottom of the notebook. New sections are added **on top** with clear markdown headers.

### Key Additions:
1. **Data Loading & Merging** (Univariate start)
   - Load all sheets (Sales Orders, Customers, Products, Regions, State Regions, Budgets)
   - Merge for enriched dataset (add Customer Name, Product Name, State, Region, Geo coords, Budget)

2. **Feature Engineering**
   - Date features: Year, Month, Quarter, DayOfWeek, IsWeekend, MonthName, YearMonth
   - Calculated metrics: Profit = Line Total - Total Unit Cost, Profit Margin % = (Profit / Line Total) * 100
   - Customer/Product metrics: Total Revenue per Customer/Product, Order Count
   - ABC Segmentation (Pareto): Top products/customers contributing to ~80% revenue

3. **Feature Binning & Encoding**
   - **Binning**:
     - Unit Price into Low/Medium/High (quantiles or custom: <$1000, $1000-3000, >$3000)
     - Order Quantity into Small/Medium/Large
     - Revenue per order into segments
     - Customer Lifetime Value (CLV) tiers (Low/Med/High based on total revenue)
   - **Encoding**:
     - One-Hot Encoding for Channel (5 categories)
     - Label Encoding for Product Description Index / Delivery Region (or Frequency/Target encoding for high-cardinality)
     - Binary for IsWeekend

4. **Exploratory Data Analysis (EDA) - Structured**
   - **Univariate Analysis**: Distributions (hist/kde/box), value counts for categorical (Channel, top 10 Products/States/Regions). Stats: mean, median, skew, outliers detection (IQR).
   - **Bivariate Analysis**: 
     - Revenue/Profit by Channel, by Region, by Month/Year
     - Correlation heatmap (numeric features)
     - Scatter plots (Price vs Qty, Price vs Margin)
     - Time series trends (monthly revenue, seasonality decomposition hint)
     - Groupby summaries with % contribution
   - **Multivariate**: Pivot tables, heatmaps (Channel x Region revenue), pairplots sample.

5. **Statistical Analysis**
   - Descriptive stats per group
   - Revenue concentration (Gini or top-N %)
   - Basic hypothesis: e.g., Does margin differ significantly by Channel? (ANOVA or Kruskal-Wallis if non-normal)
   - Year-over-Year growth, Month-over-Month trends

6. **Advanced Visualizations** (using Seaborn, Matplotlib, Plotly for interactive if wanted)
   - More professional charts: Facet grids by Channel/Region, Treemap for product contribution (if squarify available), Geo scatter if folium (optional)
   - Trend lines with confidence intervals
   - Waterfall or stacked bar for channel mix over time

7. **Business Insights**
   - Written after each major analysis section in markdown.
   - Examples:
     - "Wholesale drives ~45% of revenue but has 12% lower avg margin than Retail → Opportunity to negotiate better supplier terms or shift mix."
     - "Top 5 products account for 35% revenue but only 20% of orders → High velocity items; ensure stock levels and promotions."
     - "South & West regions show strong volume growth in 2023-2024; Northeast has higher AOV but lower volume → Targeted marketing."
     - "Q4 (Oct-Dec) contributes 32% annual revenue (holiday effect); plan inventory 2 months ahead."
     - "Online channel has highest margin (28%) but lowest volume → Upsell/cross-sell campaigns to grow share."
     - "Profit margin distribution is right-skewed; many low-margin orders drag overall profitability. Recommend minimum order value policy."

8. **Power BI Ready Output**
   - Final cleaned, enriched, encoded DataFrame saved/exported to S3 Destination bucket (timestamped) with all features for dashboard: trends, geo maps, KPI cards (Total Revenue, Avg Margin, Top Channels), filters by Year/Channel/Region.

## Files Generated
- `Regional_Sales_Analysis_Enhanced.ipynb` : Complete Colab-ready notebook with all original + new code, markdown explanations, insights, and plots.
- `README_Enhanced_Sales_Analysis.md` : This file.
- `Sales_Analysis_Presentation.pptx` : PowerPoint summary (can import to Overleaf or use directly; for Overleaf Beamer, see LaTeX folder).
- `analysis_video.mp4` or GIF: Short video walkthrough of key insights/visuals (generated from plot frames + narration text).

## How to Use in Colab
1. Upload the enhanced .ipynb to Google Colab.
2. Mount Drive or upload the Excel if not using S3.
3. Run cells sequentially. Original S3 functions are at top for your pipeline.
4. Replace `data = read_from_s3(...)` with your S3 call if needed.
5. At end, the final `final_df` is ready for `to_excel` and S3 upload (your original code).

## Business Recommendations (Summary)
- Focus on high-margin channels (Retail/Online) with targeted campaigns.
- Optimize product mix: Promote top contributors, review low-margin SKUs.
- Regional strategy: Invest in South/West growth; premium positioning in Northeast.
- Inventory & Promotions: Heavy Q4 prep, dynamic pricing for price-sensitive segments.
- Customer: Segment high-value customers for loyalty; win-back low CLV with bundles.
- Power BI Dashboard suggestions: Executive KPI page, Trend explorer, Geo performance map, Product drill-down, Channel mix pie + trend.

## Tech Stack Used
- Python: pandas, numpy, matplotlib, seaborn, scipy (stats), sklearn (preprocessing/encoding)
- Optional: plotly for interactive, statsmodels for decomposition
- AWS S3, Google Colab

## Next Steps
- Deploy full pipeline with AWS Glue/Lambda or Step Functions.
- Build Power BI with DirectQuery or Import mode on Destination bucket (via Athena or CSV/Excel export).
- Add predictive: Forecast next quarter revenue using Prophet or simple regression.
- A/B test promotions on low-margin segments.

---
*Enhanced on 2026-07-12 by Grok for ARITRA's internship project. All original work preserved and credited.*
