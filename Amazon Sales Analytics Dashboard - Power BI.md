# 📊 Amazon Sales Analytics Dashboard - Power BI

A professional, interactive Power BI dashboard analyzing 7 years of Amazon sales data with advanced analytics, geographic insights, and business intelligence visualizations.

## 🎯 Overview

This project demonstrates a complete business intelligence solution built with Power BI, featuring:
- **3-page interactive dashboard** with 15+ professional visualizations
- **206K+ records** spanning 2020-2026
- **Real-time KPI tracking** and performance metrics
- **Geographic analysis** across 8 countries
- **Advanced DAX measures** for complex calculations
- **Professional branding** with Amazon color scheme

## 📊 Dashboard Pages

### 📍 Page 1: Geographic Analysis
- Interactive world map with country-level sales distribution
- KPI cards tracking revenue, orders, customers, and metrics
- Top countries performance ranking
- Revenue by region analysis (donut chart)
- Revenue trend over 7 years (line chart)
- Interactive filters for country, region, and time periods

**Key Metrics:**
- Total Revenue: **$10.21M**
- Total Orders: **50K**
- Total Customers: **5K**
- Avg Order Value: **$239.72**

### 📈 Page 2: Sales Analytics
- Products sold by category (bar chart)
- Monthly revenue trends with growth indicators
- Order status breakdown (pie chart)
- Customer acquisition trends (line chart)
- Customer tier distribution analysis

**Visualizations:** 5 interactive charts

### 🏆 Page 3: Product Performance & Returns & Quality
- Category revenue analysis
- Total products sold by category
- Refund amount by country
- Return reasons breakdown (pie chart)
- Return rate quality monitoring
- Customer tier distribution
- Refund trends over time

**Visualizations:** 6+ analytical charts

## 📁 Data Structure

### Tables (5)

```
1. Orders (50,000 rows)
   - OrderID, OrderDate, CustomerID
   - TotalAmount, OrderStatus
   - ShippingDate, DeliveryDate

2. OrderDetails (149,767 rows)
   - OrderDetailID, OrderID, ProductID
   - Quantity, UnitPrice, Discount, LineTotal

3. Products (328 rows)
   - ProductID, ProductName, Category
   - Subcategory, Price, Cost

4. Customers (5,000 rows)
   - CustomerID, CustomerName, Email
   - Country, Region, JoinDate
   - CustomerTier, LifetimeSpend

5. Returns (2,129 rows)
   - ReturnID, OrderID, CustomerID
   - ReturnDate, Reason
   - RefundAmount, RestockingFee
```

### Relationships

```
Orders[OrderID] ←→ OrderDetails[OrderID] (1:Many)
OrderDetails[ProductID] ←→ Products[ProductID] (Many:1)
Orders[CustomerID] ←→ Customers[CustomerID] (Many:1)
Orders[OrderID] ←→ Returns[OrderID] (1:Many)
```

## 📐 Data Models & Architecture

### Star Schema Design

```
                  Customers
                      │
                      │ CustomerID
                      ↓
    ┌─────────────────Orders─────────────────┐
    │              │                         │
    │         OrderID                    OrderID
    │              │                         │
    │              ↓                         ↓
    │        OrderDetails                 Returns
    │              │
    │         ProductID
    │              │
    │              ↓
    │           Products
    │
    └─ (Time Dimension via OrderDate)
```

### Dimension Tables
- **Customers:** Demographic and segmentation data
- **Products:** Category and pricing information
- **Dates:** Calendar table for time-based analysis

### Fact Table
- **Orders:** Transactional data with measures
- **OrderDetails:** Line-level details
- **Returns:** Quality and refund analysis

## 🔧 DAX Measures

### Core Measures

```dax
-- Revenue Measures
Total Revenue = 
CALCULATE(
    SUM(Orders[TotalAmount]),
    Orders[OrderStatus] = "Completed"
)

-- Order Metrics
Total Orders = COUNTA(Orders[OrderID])

Completed Orders = 
CALCULATE(
    COUNTA(Orders[OrderID]),
    Orders[OrderStatus] = "Completed"
)

-- Customer Metrics
Total Customers = DISTINCTCOUNT(Orders[CustomerID])

Premium Customers = 
CALCULATE(
    DISTINCTCOUNT(Customers[CustomerID]),
    OR(
        Customers[CustomerTier] = "Gold",
        Customers[CustomerTier] = "Platinum"
    )
)

-- Performance Metrics
Avg Order Value = 
CALCULATE(
    AVERAGE(Orders[TotalAmount]),
    Orders[OrderStatus] = "Completed"
)

Return Rate % = 
DIVIDE(
    COUNTA(Returns[ReturnID]),
    CALCULATE(
        COUNTA(Orders[OrderID]),
        Orders[OrderStatus] = "Completed"
    )
) * 100

-- Category Analysis
Category Revenue = 
CALCULATE(
    SUM(Orders[TotalAmount]),
    Orders[OrderStatus] = "Completed"
)

Category Products Sold = 
SUMX(OrderDetails, OrderDetails[Quantity])

-- Profitability
Gross Profit = 
CALCULATE(SUM(Orders[TotalAmount]), Orders[OrderStatus] = "Completed") - 
SUMPRODUCT(OrderDetails[Quantity], RELATED(Products[Cost]))

Gross Profit Margin % = 
DIVIDE(
    [Gross Profit],
    CALCULATE(SUM(Orders[TotalAmount]), Orders[OrderStatus] = "Completed")
) * 100
```

### Advanced Measures

```dax
-- Growth Metrics
MoM Revenue Growth = 
DIVIDE(
    [Total Revenue],
    CALCULATE([Total Revenue], DATEADD(Orders[OrderDate], -1, MONTH))
)

YoY Revenue Growth = 
DIVIDE(
    [Total Revenue],
    CALCULATE([Total Revenue], DATEADD(Orders[OrderDate], -1, YEAR))
)

-- Customer Analytics
Customer Lifetime Value = 
CALCULATE(
    SUM(Orders[TotalAmount]),
    ALLEXCEPT(Orders, Orders[CustomerID])
)

Repeat Customer Rate = 
CALCULATE(
    DISTINCTCOUNT(Orders[CustomerID]),
    FILTER(
        Orders,
        CALCULATE(COUNTA(Orders[OrderID])) > 1
    )
) / [Total Customers] * 100
```

## 📊 Visualizations

| Visualization | Purpose | Data Source |
|---|---|---|
| Filled Map | Geographic sales distribution | Customers[Country], [Total Revenue] |
| Bar Chart | Top countries by revenue | Customers[Country], Orders[TotalAmount] |
| Line Chart | Revenue trend over time | Orders[OrderDate], [Total Revenue] |
| Donut Chart | Region distribution | Customers[Region], [Total Revenue] |
| Donut Chart | Customer tier breakdown | Customers[CustomerTier], DISTINCTCOUNT |
| Column Chart | Monthly revenue | Orders[OrderDate], [Total Revenue] |
| Pie Chart | Order status distribution | Orders[OrderStatus], [Total Orders] |
| Bar Chart | Category performance | Products[Category], [Total Revenue] |
| Table | Product rankings | Products[ProductName], [Total Revenue] |
| KPI Card | Key metrics display | Various measures |

## 🎨 Design & Branding

### Color Scheme
```
Primary: #1A1A1A (Dark background)
Accent:  #FF9900 (Amazon orange)
Highlight: #FDB913 (Gold)
Text:    #FFFFFF (White)
```

### Design Principles
- **Consistency:** Unified color palette across all pages
- **Clarity:** Clear labeling and intuitive layout
- **Interactivity:** Responsive filters and drill-through
- **Performance:** Optimized for fast loading

## 🚀 Getting Started

### Prerequisites
- Power BI Desktop (Latest version)
- Power BI Pro (for sharing/publishing)
- Excel 2016+ (for data file)

### Installation

1. **Download Files**
```bash
git clone https://github.com/yourusername/amazon-sales-dashboard.git
cd amazon-sales-dashboard
```

2. **Open Dashboard**
   - Open `Amazon_Sales_Dashboard.pbix` in Power BI Desktop
   - Data will automatically load from Excel

3. **Explore**
   - Use slicers to filter by Country, Region, Time
   - Click on charts for drill-through views
   - Hover over visuals for tooltips

### Data Source
- **File:** `Amazon_Sales_Data_2020_2026.xlsx`
- **Period:** January 2020 - December 2026
- **Records:** 206,000+ transactions
- **Tables:** 5 interconnected tables

## 📈 Key Insights

### Revenue Analysis
- Total revenue across 7 years: **$10.21M**
- Growth from 2020 ($0.81M) to 2026 ($2.28M)
- Seasonal peaks in Q4 (holiday season)
- Consistent growth trajectory

### Geographic Performance
- **Top Country:** Australia ($1.39M)
- **Top Region:** South (21.43%)
- **Countries Covered:** 8 major markets
- **Regional Distribution:** Even spread across regions

### Customer Metrics
- **Total Customers:** 5,000
- **Customer Tiers:** Platinum, Gold, Silver, Bronze
- **Repeat Rate:** ~65% of customers make multiple purchases
- **CLV Range:** $100 - $5,000+

### Product Performance
- **Categories:** 10 major product categories
- **Top Seller:** Electronics (highest volume)
- **Best Profit:** Books (highest margin)
- **Return Rate:** 5% (industry benchmark)

## 🛠️ Technical Stack

- **Business Intelligence:** Power BI Desktop
- **Data Source:** Excel (206K+ records)
- **Calculations:** DAX (Data Analysis Expressions)
- **Visualization:** Power BI visualizations
- **Data Modeling:** Star schema design

## 📖 How to Use

### For Viewing
1. Open the `.pbix` file in Power BI Desktop
2. Navigate through 3 pages
3. Use slicers to filter data
4. Hover over visuals for detailed tooltips
5. Click on visuals for drill-through

### For Sharing
1. **PDF Export:** File → Export → Export to PDF
2. **Power BI Service:** File → Publish (requires Pro license)
3. **Embed:** Copy and embed in presentations
4. **Screenshots:** Use for reports and dashboards

### For Customization
1. Edit measures in DAX editor
2. Modify colors in Format pane
3. Add/remove visualizations
4. Change data connections

## 🔍 Data Quality

- ✅ No missing critical values
- ✅ Consistent date formats (2020-2026)
- ✅ Valid relationships between tables
- ✅ Realistic data distributions
- ✅ Proper data types for all fields

## 📊 Performance Metrics

| Metric | Value |
|--------|-------|
| Dashboard Load Time | < 3 seconds |
| Total Data Records | 206K+ |
| Number of Measures | 12+ |
| Visualizations | 15+ |
| Calculation Groups | 3 |
| Pages | 3 |

## 🤝 Contributing

Contributions are welcome! To contribute:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit changes (`git commit -m 'Add improvement'`)
4. Push to branch (`git push origin feature/improvement`)
5. Open a Pull Request

### Improvement Ideas
- Add forecasting visuals
- Implement drill-through reports
- Create mobile-optimized version
- Add AI-powered insights
- Expand to real-time data

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 👨‍💻 Author

**Your Name**
- LinkedIn: [Kishore](https://www.linkedin.com/in/kishore-r-mba/)
- GitHub: [@kishore-hr-analyst](https://github.com/kishore-hr-analyst)


## 🙏 Acknowledgments

- Power BI team for excellent documentation
- DAX community for formula inspiration
- Data visualization best practices references

## 📞 Support

For questions, issues, or feature requests:
- Open an issue on GitHub
- Contact via email
- Check existing discussions

## 📚 Additional Resources

- [Power BI Documentation](https://docs.microsoft.com/power-bi/)
- [DAX Function Reference](https://dax.guide/)
- [Power BI Community](https://community.powerbi.com/)
- [Best Practices Guide](https://docs.microsoft.com/power-bi/developer/embedded/best-practices)


**Last Updated:** December 2024  
**Version:** 1.0  
**Status:** Complete & Production Ready ✅
