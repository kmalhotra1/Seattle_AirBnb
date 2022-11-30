# Analyzing Seattle Airbnb Data
![alt text](http://u.realgeeks.media/stevekennedy/old_wp/_wp-content_uploads_2017_01_AirbnbSeattle470x246.jpg)

**Caveat before reading further**: The full analysis may be found [here](https://nbviewer.org/github/KRM444/Seattle_AirBnb/blob/main/Seattle%20AirBnb%20Analysis.ipynb) because GitHub is not trusting the file to render the folium plots.

In this journey we will *briefly* analyze [Seattle Airbnb Open Data](https://www.kaggle.com/datasets/airbnb/seattle) while adhering to the **Cr**oss **I**ndustry **S**tandard **P**rocess for **D**ata **M**ining (*CRISP-DM*) below:

![CRISP-DM Process](http://optimumsportsperformance.com/blog/wp-content/uploads/2019/12/Screen-Shot-2019-12-14-at-10.12.35-PM-768x423.png)

*- Image provided courtesy of [Patrick Ward, Ph.D.](http://optimumsportsperformance.com/blog/data-analysis-template-in-r-markdown-jupyter-notebook/).*


# 👨‍💼🤝👨‍💼 Business Understanding
Since its inception in August 2008, Airbnb has connected guests and hosts by offering homestays and experiences. Although many individuals would agree that Airbnb has seized fractional market share from a handful of lodgings, it has successfully grown into an exceptional service used and trusted worldwide. Thus, the company has vast reservation data at its fingertips. 

By analyzing this data, one may deduce essential business insights such as the following: seasonality tendencies, which areas are in low/high demand, which feature(s) of a rental unit appear to impact price most, understanding client (host and guest) behavior through ratings or textual reviews, and many more.

To that extent, this project will answer the following three useful, but common, business questions:
- What are some seasonality trends that are confirmed by the data?  *(Clean the data and plot relatable features with respect to time)*
- Where are the most popular areas in Seattle by volume?  *(Plot area rental frequency)*
- Which features tend to have the most impact on price?  *(Create a simple Machine Learning model and interpret its accuracy/usefulness)*

# 🤔👨‍💻 Data Understanding
The [dataset(s)](https://www.kaggle.com/datasets/airbnb/seattle) contain Seattle Airbnb data for one year (01/2016 - 01/2017). Altogether, there are 3 datasets: 
- **listings.csv**, includes complete descriptions and average review score
- **calendar.csv**, includes listing id and the price and availability for that day
- **reviews.csv**, includes a unique id for each reviewer and detailed comments 

Of these 3 datasets, the first 2 will be leveraged to answer the above business questions.

**However, before analyzing the data, one must try and understand it because it will prove helpful in preparing it for analysis.
Following are the initial takeaways for the datasets:**

### Listings:
- The listings.csv file contains 3,818 rows and 92 columns. More importantly, it includes 3,818 listings and 2,751 hosts.
- There are numerous columns with an abundant amount of missing values
- Having missing values in specific columns is correlated with having missing values in other columns (this was accomplished through the "missingno" library, a rarely used but relatively simple implementation: <msno.dendogram(dataframe)>)
- Columns require **Data Manipulation**:
    - Certain columns need to be converted from string to numeric
    - Certain NULL values require imputation
    - "Meaningless" columns can be dropped: square feet (many missing values), listing_url, scrape_id, last_scraped, etc. because a) they are meaningless for our analysis, and b) they will increase processing speed. Thus, we can keep relevant features for our analysis: 'price', 'accommodates', 'bathrooms', 'bedrooms', 'beds', 'weekly_price', 'monthly_price', 'cleaning_fee', 'instant_bookable', 'reviews_per_month', 'cancellation_policy', etc.

### Calendar:
- The calendar.csv file contains 1,393,570 rows and 4 columns.
- ~33% of "Price" column data is NULL
- Columns require Data Manipulation:
    - "Price" should be converted to float
    - "Listing_id" should also be converted to float
    - Dates should be extracted from "date" column

# 🧹 Data Preparation
Before conducting the analysis, we must prepare the data by cleaning it. Therefore, to accomplish this:
- "Meaningless" columns will be dropped
- A portion of the missing data will be imputed
- Certain feature data types will be changed

# 🤖 Modeling
Before we begin, it would be helpful to gain a macro perspective on the following: 
- Visualizing where rental properties are located geographically (perhaps some knowledge outside the scope of the dataset may prove helpful in future analysis, ex: Amazon's real estate venture(s), properties by certain coasts/tourist sites/restaurants have low/high demand etc.)
- Insight into our target variable, price (distribution, etc.)
- Feature Correlation with the target variable, price
- Machine Learning (Random Forest model implementation + Feature importance)

# 🧐💻 Evaluation
- **Price**
    - ![image](https://user-images.githubusercontent.com/35614192/204840519-e4a4082b-fb99-46e6-a7b9-e566d1c73b40.png)
    - ![Screen Shot 2022-11-30 at 10 58 01 AM](https://user-images.githubusercontent.com/35614192/204846595-c85b815e-c173-413c-9f93-b3d074267628.png)
    - The distribution of the target variable (price) is right-skewed, with approximately ~90% of the AirBNB rental units being less than ≤$250.
    - Minimum price per listing: $10.0 **|** Maximum price per listing: $1,650.0 **|** Average price per listing: $137.94 **|** Median price per listing : $109.0
    - The minimum price listing of $10.0 seems a little too low (it might be worth exploring data validation on this instance as well as neighboring samples)
    - Listings are spread out throughout Seattle. However, most of the listing volumes appear to be centralized near Belltown, First Hill, Capitol Hill, Madison Valley, and Central District. 

- **Seasonality & Grouping**
    - ![image](https://user-images.githubusercontent.com/35614192/204839627-405775c1-4573-4bb4-bf5e-3b7399d13678.png)
    - Seasonality trends depict a monotonic increase in average rental price by month from January to July, followed by a gradual decline until November and a small uptick to December
    - The increased average price between summer months may be attributed to less listing availability. Therefore, **there is an inverse relationship between price and availability concerning seasonality due to market saturation.**
    - When uniquely grouping listings, the average price of most listings is **$50≤ $listingµ ≤$250**, making it affordable for most tourists.
    - On average, Downtown, Magnolia, and Queen Anne tend to be the priciest neighborhoods with a mean listing price of **$175<**

- **Feature Correlation with Price**
    - ![image](https://user-images.githubusercontent.com/35614192/204838297-92c9f8db-ddd3-43b4-a0e2-4b9a8aef6860.png)
    - Accommodations 68%
    - Bedrooms 63%
    - Beds 61% (although this one may be somewhat synonymous since more bedrooms imply more beds, generally speaking)
    - Bathrooms 52%
    - Broadly speaking, it seems somewhat rather evident that these features are most positively correlated with price because *generally* `more square feet => more bedrooms => more beds => more people => greater $.`

- **Random Forest Feature Importance**
- ![image](https://user-images.githubusercontent.com/35614192/204841466-3238bacb-14c1-4a0f-a258-0402dcbef62f.png)

    | Model Scores  | Train         | Test          | 
    | ------------- | ------------- | ------------- | 
    | RMSE          | 14.48         | 14.98         | 
    | R^2           | 0.9809        | 0.9799        | 

#### Overall, the plot illustrates the top 15 features with respect to importance. However, the RMSE indicates that the model could be improved because there is a large difference between the predicted and observed values. Therefore, we may conduct either (or both) of the following, if necessary, at a future date:
- Some fine tuning (i.e., feature engineering, etc.) 
- Implement additional models to see which one provides a superior fit, if any

# 🚀 Deployment
- Publish to GitHub & Medium.
