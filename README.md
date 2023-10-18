# Amazon Vine Program Analysis 

## Overview: 

The Amazon Vine program is a service that allows manufacturers and publishers to receive reviews for their products. Companies pay a small fee to Amazon and provide products to Amazon Vine members who are then required to publish a review. Amazon invites customers who’ve earned trust in the Amazon community for their insightful reviews to serve as Vine Voices. Voices request products they want to review and try them out free of charge and share their honest, unbiased opinions in a review. I've chosen a dataset from amazon containing reviews of US digital music purchases to determine if there is bias in the reviews in the dataset. The image below describes the data contained in each column. 


<img width="1352" alt="Screen Shot 2021-10-16 at 12 06 30 PM" src="https://user-images.githubusercontent.com/84936545/137594392-b06fb75f-ecd5-46ae-a5c5-53b8b6cd6ab2.png">

## Analysis: 

For this analysis I've created an Amazon Web Services RDS database with tables in PgAdmin (an open source management tool for PostgreSQL). I extracted data from the reviews of entire US digital music purchases dataset from amazon into a spark DataFrame. Then transformed the DataFrame into four separate DataFrames that matched table schema in PgAdmin. The four dataframes are shown below. 

#### Customers DF 


<img width="259" alt="Screen Shot 2021-10-16 at 12 21 37 PM" src="https://user-images.githubusercontent.com/84936545/137594872-f4ff6309-116c-4dbb-b1cc-f34c1090cc6e.png">


#### Review id DF

<img width="569" alt="Screen Shot 2021-10-16 at 12 21 59 PM" src="https://user-images.githubusercontent.com/84936545/137594909-e525aeaf-791f-4067-a1f9-0dc1ee326499.png">


#### Products DF

<img width="307" alt="Screen Shot 2021-10-16 at 12 21 29 PM" src="https://user-images.githubusercontent.com/84936545/137594946-9b5acf25-3cf3-4364-9dfc-c793df157760.png">

#### Vine DF

<img width="684" alt="Screen Shot 2021-10-16 at 12 22 18 PM" src="https://user-images.githubusercontent.com/84936545/137594981-8d34755f-2e22-4ac7-a32f-5611239f2cdf.png">

I uploaded the transformed data in these dataframes into the appropriate tables in pgadmin. I ran queries using postgresql to confirm that the data had been uploaded.

## Conclusion (examining for bias): 

The goal of this analysis was to determine if there was any bias towards reviews that were written as part of the Vine program in this dataset. More specifically whether having a paid Vine review made a difference in the percentage of 5-star reviews the products in this dataset recieved. To do this I exported as a csv through PgAdmin then used pandas to analyze the dataset. The conslusions I found during this analysis are discussed below.

#### 1. How many Vine reviews and non-Vine reviews were there?

The overall amount of reviews in this dataset was 1688884. After examining the data and filtering to determine how many were vine reviews I observed that there were no vine reviews listed in the data. The amount of values that was listed as not vine was 1688884. It's possible that no one wrote a vine review for these products or there could have been an outside reason as to why they wouldn't review these products within the vine program. 

#### 2. How many Vine reviews were 5 stars? How many non-Vine reviews were 5 stars?

Because there were no vine reviews, there were 0 vine reviews with 5 stars. I chose to determine the amount of ratings for each of the 5 possible values in the star_ratings column. To do this I used the for loop below. 

```
for x in range(1, df.star_rating.nunique()+ 1):
    length = len(df[(df['star_rating'] == x)])
    print(f'Star_count =  {x} amount = {length} ')
```
The results ended up being: 

<img width="289" alt="Screen Shot 2021-10-16 at 12 56 33 PM" src="https://user-images.githubusercontent.com/84936545/137595888-e2cc4c27-9011-4f04-ad06-57cb195bd201.png">

#### 3. What percentage of Vine reviews were 5 stars? What percentage of non-Vine reviews were 5 stars?

Because there was no vine reviews in the dataset, there weren't any vine reviews that earned 5 stars. The percentage of non-vine reviews that earned 5-star is 0.7964703318878028. 

While it is possible that this statistic accurately represents individual buyers opinion of the product, this high five star percentage could be a sign of bias in the dataset. Two types of self-selection bias are common in product reviews and could possibly affect this dataset. Acquisition bias (mostly consumers with a favorable predisposition acquire a product and hence write a product review) and underreporting bias (consumers with extreme, either positive or negative, ratings are more likely to write reviews than consumers with moderate product ratings) could affect percentage of 5-star reviews. 

Because this dataset contains no (reported) vine reviews I have no way to determine how that program would affect the likeliness of bias in this dataset, and there isn't any additional analysis I could do to support this statement. 
