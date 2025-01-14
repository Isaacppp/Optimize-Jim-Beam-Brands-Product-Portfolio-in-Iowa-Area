# A Mock Business Recommendation: How to Optimize Jim Beam Brands Product Portfolio in Iowa Area

## Introduction to This Project
  
This is a self-curated project using real world data - the "Iowa Liquor Sale".

This project started from a big ambiguous question:

  "How to optimize sale portfolio in Iowa for a vendor?" 

I queried the dataset in Google #Bigquery, and picked Jimbeam as the vendor in this project. By the data that is currently available, I built this mock business recommendations on how to optimize their product portfolio in Iowa.

#### Disclaimer:

**The material and the information contained in this presentation and in any documentation attached to it are for my personal case study purpose only. It does not constitute an offer, true business recommendation to any person and/ or organization.**

![preview](https://user-images.githubusercontent.com/95849080/203690183-fbee9540-79a9-4317-a9c2-be8cc5e1beef.png)

## Business Recommendation

For distribution channels of grocery, liquor and convenience stores in Iowa, the business recommendation I have for Jim Beam Brands are:
  
1. For Whiskey products:
Maintain the current amount of investment in this category and generate as much profit as possible. And use the capital in other areas such as diversification in the production line.

2. For Tequila:
Invest more in this category and try to get as much market share before the category market growth slows down.

3. For Brandy, Cocktails/ RTD, Cordials/ Liqueurs, Vodka, Rum and Gin:
Unless there are some other strategic aims, I recommend that Jim Beam Brands start to gradually divest from these markets and re-allocate the capital to other categories like Tequila.

Especially Rum and Gin, as they have had the least growth rate (near 0%) since 2012.

  
## Table of Contents
  - [About the Dataset](#about-the-dataset)
  - [My Approach to Solve this Problem](#my-approach-to-solve-this-problem)
  - [Technical Approach in This Project](#technical-approach-in-this-project)
  - [Limitation of This Analysis](#limitation-of-this-analysis)
  - [Data Cleaning](#data-cleaning)
  - [Data Analyzing](#data-analyzing)
  - [Key Takeaways](#key-takeaways)
  - [Business Recommendation](#business-recommendation)
  - [Next Step](#next-step)
  - [References](#references)

## About the Dataset

  - This dataset contains the spirits purchase information of Iowa Class “E” liquor licensees by product and date of purchase from January 1, 2012 to September 30th, 2022.

  - There are 24.8 million rows with 24 columns, each row is an individual product purchase.

  - You can find it on Bigquery public dataset or access the same via the link:
  [https://data.iowa.gov/Sales-Distribution/Iowa-Liquor-Sales/m3tr-qhgy](https://data.iowa.gov/Sales-Distribution/Iowa-Liquor-Sales/m3tr-qhgy)

## My Approach to Solve this Problem

I will break this big ambiguous business problem into small pieces and use data to answer each small question. In the end put them together to answer the big question.

The framework I drew and going to use in the analysis:

![framework](https://user-images.githubusercontent.com/95849080/202335082-580e63e1-8a4f-4a1f-b4a8-a9fc037d962b.jpg)

To save time, I will answer the quantitative questions and then investigate further on qualitative questions in a top-down way.

## Technical Approach in This Project
  
  - Data exploration, extraction, processing and analyzing are done by SQL queries in Google Bigquery sandbox. As I used the free version of Bigquery, you might find the data cleaning process unlike normal practice as the DML is not available in the free version.

  - Visualization: Microsoft Power BI

## Limitation of This Analysis

  - This analysis is for the sale portfolio in Iowa area only.
  
  - As the dataset only contains spirits purchase information of Iowa Class “E” liquor licensees, this means the distribution channel only include: grocery, liquor and convenience store and their wholesale sales to on-premise class "A", "B", "C" and "D". The dataset does not include on-premise sales (licensees "A", "B", "C" and "D"), i.e. members and guests of non-profit clubs, hotels and motels, taverns, bars, restaurants, trains, airplanes and watercrafts etc.

## Data Cleaning

#### 1. Data Cleaning Overview

- The dataset contains the sales record of liquor in Iowa from 2012-01-03 to 2022-09-30. There were 24,842,520 records in the dataset, 24,847,481 (-4,961 rows) after cleaning.

- There are 24 fields in this dataset, as per the dimensions that the data is stored, I categorized them into below 4 categories:
  * Fact: [invoice_and_item_number], [date], [bottles_sold], [sale_dollars], [volume_sold_liters], [volume_sold_gallons]
  * Store: [store_number], [store_name], [address], [city], [zip_code], [store_location], [county_number], [county]
  * Liquor: [category], [category_name], [item_number], [item_description], [pack], [bottle_volume_ml], [state_bottle_cost], [state_bottle_retail]
  * Vendor: [vendor_number], [vendor_name]

- As the dimension "Store" is not in the scope to solve the business problem, to save time, I cleaned other fields for analysis.
  
#### 2. Details of Cleaning

#### Fact

- [bottles_sold], [sale_dollars]

  * Checked the outliers
  * Turned negative values to positive
  * The record in “0” is not meaningful in the sale analysis case, thus, they are excluded from the analysis


#### Liquor

- [category_name], [item_description]

  * Cleaned for the consistency of every type, e.g. extra “s” or “.” etc and unify the naming of each type
  * 25,040 records were missing, retrieved the records by what are contained in item_description. Assign category as “Others” to those cannot be found in the end
  * Categories contain sub-genres that are trivial and difficult to comprehend
      → move [category_name] from small genre to main categories as categorized in Iowa ABD: WHISKEY, TEQUILA, VODKA, GIN, BRANDY, RUM, COCKTAILS/ RTD, CORDIALS/ LIQUEURS, SPECIALTY. Reference: [https://shop.iowaabd.com/](https://shop.iowaabd.com/)
    
    For those that are not categorized as above or not listed, assign to “OTHERS”
    
- [state_bottle_retail]
  * Checked the outliers
  * Contains 3847 records that show “0.0” dollar per bottle → excluded these record from table

[Check query record Here](https://github.com/Isaacppp/A-Mock-Business-Recommendation-How-to-Optimize-Jim-Beam-Brands-Product-Portfolio-in-Iowa/blob/main/clean_query_record_liquor.sql)

##### Vendor

- [vendor_name]

  * Exclude NULL values (7 records with total 102 bottles missing)
  * Cleaned for the name consistency of every vendor name, e.g. extra “s” or “.” etc and unify the naming for each vendor. There were 549 distinct records (including NULL) to 427 distinct record (including NULL). **So there are total 425 vendors used in the analysis.**

[Check query record Here](https://github.com/Isaacppp/A-Mock-Business-Recommendation-How-to-Optimize-Jim-Beam-Brands-Product-Portfolio-in-Iowa/blob/main/clean_query_record_vendor.sql)

[[Check Full Code Here]](https://github.com/Isaacppp/A-Mock-Business-Recommendation-How-to-Optimize-Jim-Beam-Brands-Product-Portfolio-in-Iowa/blob/main/clean_full_query.sql)

## Data Analyzing

#### Overview

- The dataset is updated on 2022/11/02. It contains the sales record of liquor in Iowa from 2012-01-03 to 2022-09-30
- Only [date], [bottles_sold], [sale_dollars], [category_name], [item_description], [state_bottle_retail], [vendor_number] are used in this analysis
- I started with deciding which vendor to help and then identifying the main market of their products by price per bottle. Secondly, identifying the market growth and relative market share to see which category of product Jim Beam Brands can further consider to increase investment or liquidate the category.


#### 1. The vendor I help

In fact, in the beginning, the big ambiguous question I asked in this project was "How to optimize the sale portfolio for a vendor?" There wasn't "the who" at the beginning. So, I took a look to see the ranking of vendors (by sales through 2012/01/01 and 2022/09/30)

![Overall Market Share](https://user-images.githubusercontent.com/95849080/201589134-387fbc74-e8e8-4edb-beab-9358f8700984.jpg)

The top 3 are Diageo, Sazerac Company and then JimBeam.

After going over the ranking, I want to go with the analysis to help Jim Beam Brands, the 3rd largest vendor by market share. The reason is: They have room to grow, and the NO.4 Pernod Ricard is so close behind. (or maybe simply because I like their whiskey)

#### 2. Define the market where their products stand

As I picked Jim Beam Brands as client in this project, defining the market for most of their products is my first priority (an incorrectly defined market can lead to poor classification in liquor market)

- I classified the liquor market (in price) into these buckets:

    * USD ≤20
    * USD 21-50
    * USD 51-100
    * USD 101-200
    * USD 201-300
    * USD >300

- Based on the buckets, the price distribution of Jim Beam Brands Products shows as:

![Price Distribution](https://user-images.githubusercontent.com/95849080/201589862-3e6bc764-6ad5-4ab8-8218-efe3e3d054c3.jpg)

Most of Jim Beam Brands products (98.43%) are less than and equal to USD 100.
Therefore, I will proceed the analysis with the market that the price per bottle is ≤ USD 100.

Plus, I want to see where Jim Beam Brands stands in each category, so I will be putting each category in a growth-share matrix. Therefore, I want to get “Market Growth” of each category and “Relative Market Share” next.

#### 3. Market Growth

I used CAGR rather than YoY% to calculate market growth because it gives me an idea of general growth rate.

As the max date in dataset that currently available is 2022-09-30 (end of Q3), to calculate the CAGR over the past 10 years, I used 1st, October 2012 as starting date.

Firstly, in order to draw the middle line in the growth rate axis in the matrix, I used the overall CAGR of Iowa liquor market as that data point.

**The overall CAGR of Iowa liquor market is: 5.55%**
(Note: this is for market that each bottle ≤ USD 100, from Q4 2012 to Q3 2022)

- Then, I got each CAGR as data point in Y-axis for each category.

![CAGR of each category](https://user-images.githubusercontent.com/95849080/201590530-50eb2781-62f4-4d43-b73b-9f7b913fccde.jpg)

[Code in this part of analysis](https://github.com/Isaacppp/A-Mock-Business-Recommendation-How-to-Optimize-Jim-Beam-Brands-Product-Portfolio-in-Iowa/blob/main/analyze_market_growth.sql)

#### 4. Relative Market Share

- To get the data point on X-axis for each category, I queried Jim Beam Brands’ relative market share. For a clearer overview, below table tells the relative market share of Jim Beam Brands (the first right in below table), and who is the leading rival in each category.

![Relative Market Share](https://user-images.githubusercontent.com/95849080/201593164-6dbac934-642b-411f-964e-1c622ff793ef.jpg)

- To draw the middle line of the relative market share in the growth-share matrix, I examined the market share in each category to see the market share distribution first.

![ms_controlled_by_top10](https://user-images.githubusercontent.com/95849080/201594139-bdd17daa-d497-4f05-a2e5-c82197e55d13.jpg)

As the top 10 market share holder in each category control the big majority liquor market in each category, I will use the average relative market share of top 10 vendors as the middle data point to draw the line.

- Average relative market share of top 10 vendors in each category

The average relative market share is: **0.208**

Note: as the leading rival’s relative market share is 1, so every leading rival’s relative market share is excluded (only 9 relative market share in top 10 in each category is used to count the average)

[Code in this part of analysis](https://github.com/Isaacppp/A-Mock-Business-Recommendation-How-to-Optimize-Jim-Beam-Brands-Product-Portfolio-in-Iowa/blob/main/analyze_relative_market_share.sql)

#### 5. Draw the Growth-Share Matrix

After collecting all the data points necessary, the growth-share matrix can be drawn as following: 
(Note: As category: “OTHERS” is not listed in specific market. And “SPECIALTY” mostly contains special packages that are not regular product. I didn’t dive into these 2 categories for the time being.)

- Growth Share Matrix of Jim Beam Brand’s products

![growth_share matrix](https://user-images.githubusercontent.com/95849080/201596098-a5ee5cb3-50b8-42ba-bb09-f6b6d6d3ff39.jpg)

What the matrix tells:

- Whiskey product is Jim Beam Brands’ cash cow that is the most competitive and generating the most revenue among all the categories.

- Tequila product is Jim Beam Brands’ star as the growth rate of the Tequila market is far greater than the overall liquor market and Jim Beam Brands control a competitive amount of market share.

- Other categories are relatively plain in terms of growth rate and Jim Beam Brands' relative market share.

In order to strengthen what the growth-share matrix tells, by finding out the growing momentum of each category will help to make decision.

#### 6. Calculate Growth Rate Momentum of each Category

To get the growing momentum of each category, I queried the CAGR change from long term to short term (from a 9 years interval to 1 year interval) to see the trend.

We will find that in a 3 years interval, other than "Tequila", the growth rate of other categories started to decline and some even turn negative in a 2 years interval.

![cagr_change_over_years](https://user-images.githubusercontent.com/95849080/202111162-7ed63beb-8eb2-4e80-a2a2-c61c87e86bff.jpg)

[Code in this part of analysis](https://github.com/Isaacppp/A-Mock-Business-Recommendation-How-to-Optimize-Jim-Beam-Brands-Product-Portfolio-in-Iowa/blob/main/analyze_growth_rate_momentum.sql)

## Key Takeaways

1. The main market (in price) of Jim Beam Brands is below and equals to USD 100 per bottle with 98.43% in total.

2. The overall CAGR of Iowa liquor market from Q4 2012 to Q3 2022 (for market that each bottle ≤ USD 100) is 5.55%

3. Jim Beam Brands most competitive category by relative market share is “Whiskey” (64%), then “Tequila” (23%) follows.

4. Tequila is the most active market with CAGR of 11.67% over Q3 2012- Q3 2022 and meantime has the best momentum in growth rate compared to all other categories.

## Next Step

For next step, I'd like to look into three areas:

- Profitability: I will look into the profitability of each category and narrow it down to “which” product to identify which exact product(s) to invest in or divest. Then it can be further identified how much the profit would be increased by investing more or divesting from specific categories.

- Sales Trend: After the profitability check, I will dive into the sales trend of each product in each category to support the decision-making of which exact product(s) to invest more or divest.

- Risks: Look into whether divesting the categories will negatively impact the brand name. And investigate whether it will give too much share to competitors.

## References

- License classification: [https://abd.iowa.gov/licensing/license-classifications](https://abd.iowa.gov/licensing/license-classifications)
- Dataset: [https://data.iowa.gov/Sales-Distribution/Iowa-Liquor-Sales/m3tr-qhgy](https://data.iowa.gov/Sales-Distribution/Iowa-Liquor-Sales/m3tr-qhgy) and Google Bigquery
- Liquor categorization: [https://shop.iowaabd.com/](https://shop.iowaabd.com/)
