# Super Store-Report

### Dashboard Link : https://app.powerbi.com/groups/me/reports/1bc4ea1e-8dbf-47ca-9087-40ef864531b3/7539bddd99bc98fde1b0?experience=power-bin

## Problem Statement

The "Super Store" is experiencing uneven sales and profitability across different regions, product categories, and customer segments. Despite achieving a significant total sales volume, the profitability and return rates suggest underlying inefficiencies and potential areas of improvement. The key issues to be addressed include:

1. Product Performance Variability: There is a noticeable disparity in sales across different product sub-categories. For instance, while Phones and Chairs lead in sales, other categories like Appliances and Bookcases show significantly lower sales, which could indicate either lower demand or insufficient marketing efforts.

2. Regional Sales Discrepancies: Sales performance varies widely across different states and regions, indicating potential untapped markets or regions where sales strategies may need to be reevaluated.

3. Customer Segment Insights: The sales contribution from different customer segments suggests that the store may need to realign its marketing and product offerings to better cater to the most profitable segments or expand its reach to underperforming ones.

4. Profitability Concerns: Although the total sales are high, the profit margins appear to be under pressure, which could be due to high operational costs, inefficient pricing strategies, or lower-margin product categories.

5. Product Return Rate: With 296 products returned, understanding the reasons behind these returns is crucial to reducing return rates and improving customer satisfaction.


### Steps followed 

- Step 1 : Load data into Power BI Desktop, dataset is a csv file.
- Step 2 : Open power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options.
- Step 3 : Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on entire dataset".
- Step 4 : It was observed that in none of the columns errors & empty values were present.
- Step 5 : We go to the report view next and select a background to start adding our visuals. 
- Step 6 : We add 3 card Visuals to represent produsts sold, products returned and best selling products.
- Step 7 :  Two line charts are added to represent the total sales and total profit 
- Step 8 : Visual filters (Slicers) were added for four fields named "State", "Year", "Month" & "Quarter".
- Step 9 : Two Donut charts were added to the report design area representing the sales amount by category and the sales amount by segment.
    
- Step 10 : A bar chart was also added to the report design area representing the total sales by each sub-category
- Step 11 : A ribbon chart was also added to the report design area representing the region wise sales

- Step 12 : And finally we add a shape map to the report design are representing the state wise sales.
  
- Step 12 : In the report view, under the insert tab, a text boxe was added to the canvas,we added super store sales report to the text box.
- Step 13 : In the report view, under the insert tab, using shapes option from elements group a rectangle was inserted & similarly using image option icons were added to the report design area. 
- Step 14 : A new calculated table was created in which, in which we add the start date and end date from the order date column in the orders table

for creating new table following DAX expression was written;
       
        DimDate = 
        
        var s_date=YEAR(MIN(Orders[Order Date]))
        
        var e_date=YEAR(MAX(Orders[Order Date]))

        
        RETURN
        
       CALENDAR(DATE(s_date,1,1),DATE(e_date,12,31))
        
Snap of new calculated table ,

![Snap_1](https://github.com/user-attachments/assets/e4b4fddd-bc07-4575-bff2-35ba885800ee)

        
- Step 15 : A new calculated columns were added to the new calculated table

  (a). Year
Following DAX expression was written,
        
       Year = YEAR(DimDate[Date])        

  (b). Month
Following DAX expression was written,
       
       Month = FORMAT(DimDate[Date],"mmm")

  (c). Quarter
Following DAX expression was written,
 
       Quarter = "Qtr" & QUARTER(DimDate[Date])

  (d). Fiscal Year
Following DAX expression was written,

        F_Year = 
                 var yr=YEAR(DimDate[Date])
                 var mo=month(DimDate[Date])
        RETURN
        IF(mo<=9,yr-1 &"-"& yr , yr &"-"& yr+1)

 (e). Month Number
Following DAX expression was written,

        MonthNo = MONTH(DimDate[Date])

 Snap of all the columns in the new table 

 ![snap2](https://github.com/user-attachments/assets/0b71ff50-dde6-4c7a-8049-ae6823cda16d)

 - Step 16 : The Following measures were created in a new column named metrics

(a) BEST SELLING PRODUCT = 
   
     FIRSTNONBLANK(TOPN(1,VALUES(Orders [Product Name]),CALCULATE(SUM(Orders[Quantity]))),1)
    
 A card visual was used to represent Best Selling product.

 ![snap3](https://github.com/user-attachments/assets/b06a15b7-b163-4d39-975a-a3890e9e5174)

(b) Total Sales = 
           
            (SUM(Orders[Sales])) 

(c) Total Profit = 
               
               SUM(Orders[Profit])

(d) Ly sales = 
       
        CALCULATE(
            [Total Sales],
            DATEADD(DimDate[Date],
            -1,
            YEAR)
        )

(e) Ly Profit = 
        
        CALCULATE(
            [Total Profit],
            DATEADD(DimDate[Date],
            -1,
            YEAR)
        )

(f) PRODUCTS SOLD = 

        SUM(Orders[Quantity])

(e) PRODUCTS RETURNED = 

        CALCULATE(COUNT('Return'[Order ID]),'Return'[Returned]="yes")

(f) YOY Growth = 

      DIVIDE([Total Sales]-[Ly sales],[Ly sales])

(g) YOY Growth_P = 

     DIVIDE([Total Profit]-[Ly Profit],[Ly Profit],0)
    
(h) Sales Main Title = 

     "TOTAL SALES" &REPT(UNICHAR(10),2) & REPT(" ",30) &
      FORMAT([YOY Growth], "+0.0%;-0.0%" ) &"   △ LY  "&
      IF([YOY Growth] >0 , "🟢", "🔴")

(i) Sales Sub Title = 

     FORMAT([Total Sales],"#,##0,.0 K")

 the previous 2 measures were used to customize our line chart which represents total sales over a perod.
 
Lets take a look at our customized visual.

![snap4](https://github.com/user-attachments/assets/efeca760-1172-4ba6-a6b0-a907dad48c23)

(j) Profit Main Title = 

      "TOTAL PROFIT" &REPT(UNICHAR(10),2) & REPT(" ",25) &
       FORMAT([YOY Growth_P], "+0.0%;-0.0%" ) &"   △ LY  "&
       IF([YOY Growth_p] >=0 , "🟩","🟥")

(k) Profit Sub Title = 

    FORMAT([Total Profit],"#,##0,.0 K")

Similarly the previous 2 measures were used to customize our line chart which represents total profit over a perod.

Lets take a look at our customized visual.

![snap5](https://github.com/user-attachments/assets/319b9b0d-e795-4e5d-b75f-7068fd8b9cde)

(l) Sub-Cat Rank Cy = 
                
         RANKX(
         ALL(Orders[Sub-Category]),[Total Sales])

(m) Sub-Cat Rank Py = 
               
     RANKX( ALL(Orders[Sub-Category]),[Ly sales])

(n) Ranking Label = 
         
        "#" & [Sub-Cat Rank Cy] 

(o) Ranking Development Label = 

    var _SubCatrankchange=[Sub-Cat Rank Cy]-[Sub-Cat Rank Py]
    var Label=
    SWITCH(TRUE,
    _SubCatrankchange = 0, "#" & [Sub-Cat Rank Cy] & " " & "—",
    _SubCatrankchange > 0, "#" & [Sub-Cat Rank Cy] & " " & "▼" & _SubCatrankchange,
    _SubCatrankchange < 0, "#" & [Sub-Cat Rank Cy] & " " & "▲" &ABS( _SubCatrankchange)
    )
    RETURN
    Label
 
(p) Ranking Development Color = 

     var _SubCatrankchange=[Sub-Cat Rank Cy]-[Sub-Cat Rank Py]
     var Color=
    SWITCH(
    TRUE,
    _SubCatrankchange = 0, "black", 
    _SubCatrankchange > 0, "red",
    _SubCatrankchange < 0, "green"
    )
    RETURN
    Color

 
 - Step 18 : The report was then published to Power BI Service.
 
 
![Publish_Message](https://github.com/user-attachments/assets/a6bd4d31-daae-47f7-9978-e34e12897b52)

# Snapshot of Dashboard (Power BI Service)

![dashboard_snapo](https://github.com/user-attachments/assets/d3facfd8-d936-47cb-aad6-c37fab47e465)

 
 # Report Snapshot (Power BI DESKTOP)

 
![Dashboard_upload](https://github.com/user-attachments/assets/cac77850-1bec-4a00-ba50-1652ef3ee573)

# Key Points

A single page report was created on Power BI Desktop & it was then published to Power BI Service.

Following inferences can be drawn from the dashboard;

  Total Number of Products Sold = 38k

   Number of Products Returned = 296 

   Total sales = 2,297.2k

   Total profit = 286.4k

## 1. Focus on High-Performing Products:
Insight: Phones and Chairs are currently the top-performing sub-categories, contributing significantly to overall sales. The store should continue to invest in these categories by exploring opportunities to expand their product lines, enhance marketing efforts, and maintain competitive pricing.
Recommendation: Consider bundling these top products with related accessories or services to increase the average order value and enhance customer satisfaction.
## 2. Address Underperforming Categories:
### Insight: 
Appliances and Bookcases are lagging in sales, indicating potential issues such as low demand, ineffective marketing, or misaligned product offerings.
### Recommendation: 
Conduct market research to understand customer preferences and identify the reasons for low sales in these categories. Based on findings, either improve the product offerings or reallocate resources to more profitable categories.
## 3. Regional Strategy Optimization:
Insight: Sales vary significantly across different states and regions, suggesting that certain areas may be underperforming due to untapped market potential or ineffective sales strategies.
Recommendation: Analyze the specific characteristics of underperforming regions to tailor marketing campaigns, optimize inventory distribution, and explore local partnerships. Additionally, consider introducing region-specific promotions to boost sales.
## 4. Enhance Customer Segmentation:
Insight: Sales contribution from different customer segments reveals an opportunity to realign marketing efforts to target the most profitable segments more effectively. There may also be untapped potential in underperforming segments.
Recommendation: Implement targeted marketing campaigns that focus on the most profitable customer segments. Additionally, explore personalized offers or loyalty programs to increase engagement with these key customer groups while also trying to convert underperforming segments into higher-value customers.
## 5. Improve Profit Margins:
Insight: Despite strong sales, profit margins are under pressure, likely due to high operational costs, inefficient pricing strategies, or the focus on lower-margin product categories.
Recommendation: Reevaluate pricing strategies, particularly for lower-margin products, and consider adjusting prices to better reflect their value. Additionally, identify and reduce unnecessary operational costs, such as excess inventory or inefficient supply chain practices.
## 6. Reduce Product Returns:
Insight: The return of 296 products indicates a potential issue with product quality, customer expectations, or purchase experience.
Recommendation: Analyze return data to identify common reasons for returns. If quality issues are identified, work with suppliers to address them. For mismatches in customer expectations, improve product descriptions, images, and customer education efforts. Implement a more transparent and efficient return policy to enhance customer satisfaction while reducing return rates.
## 7. Leverage Best-Selling Products for Cross-Selling Opportunities:
Insight: Staples are identified as the best-selling product. This popularity can be leveraged to cross-sell related items.
Recommendation: Develop cross-selling strategies where customers purchasing Staples are recommended complementary products, increasing the overall basket size and profitability.
By addressing these insights, the "Super Store" can optimize its sales strategies, improve profitability, and enhance customer satisfaction, ultimately leading to more sustainable growth.



