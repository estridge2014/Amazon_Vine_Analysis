# Amazon_Vine_Analysis

## Overview: 

The Amazon Vine program is a service that allows manufacturers and publishers to receive reviews for their products. Companies like SellBy pay a small fee to Amazon and provide products to Amazon Vine members, who are then required to publish a review. I am analyzing a dataset from amazon which contains reviews of US digital music purchases in order to determine if there is any bias in the reviews in the dataset. The image below describes the data contained in each column. 


<img width="1352" alt="Screen Shot 2021-10-16 at 12 06 30 PM" src="https://user-images.githubusercontent.com/84936545/137594392-b06fb75f-ecd5-46ae-a5c5-53b8b6cd6ab2.png">

## Analysis: 

For this analysis, I created an Amazon Web Services RDS database with tables in pgAdmin. Then I extracted the data from the reviews of entire US digital music purchases data from amazon into a spark DataFrame. Then transformed the DataFrame into four separate DataFrames that matched the table schema in pgAdmin. The dataframes are shown below. 

Customers DF: 


<img width="259" alt="Screen Shot 2021-10-16 at 12 21 37 PM" src="https://user-images.githubusercontent.com/84936545/137594872-f4ff6309-116c-4dbb-b1cc-f34c1090cc6e.png">


Review id DF

<img width="569" alt="Screen Shot 2021-10-16 at 12 21 59 PM" src="https://user-images.githubusercontent.com/84936545/137594909-e525aeaf-791f-4067-a1f9-0dc1ee326499.png">


Products DF

<img width="307" alt="Screen Shot 2021-10-16 at 12 21 29 PM" src="https://user-images.githubusercontent.com/84936545/137594946-9b5acf25-3cf3-4364-9dfc-c793df157760.png">

Vine DF

<img width="684" alt="Screen Shot 2021-10-16 at 12 22 18 PM" src="https://user-images.githubusercontent.com/84936545/137594981-8d34755f-2e22-4ac7-a32f-5611239f2cdf.png">

 after creating these dataframes I uploaded the transformed data into the appropriate tables and ran queries in pgAdmin to confirm that the data had been uploaded.

## Conslusion (examining for bias): 

The goal of this analysis was to determine if there was any bias towards reviews that were written as part of the Vine program, more specifically whether having a paid Vine review makes a difference in the percentage of 5-star reviews the product recieved. To do this I examined the vine dataframe which I exported as a csv through pg admin. Using pandas to continue the analysis, the conslusions I came to are below.

1. How many Vine reviews and non-Vine reviews were there?

The overall amount of reviews was 1688884. After examining the data and filtering to determine how many were vine reviews, I observed that there were no vine reviews listed in the data. The amount of values that was listed as not vine was 1688884 (the entire dataset). It's possible that there really wasn't anyone who wrote a vine review, or there could have been some outside reason as to why they wouldn't report that they were part of the vine program. 

2. How many Vine reviews were 5 stars? How many non-Vine reviews were 5 stars?

Because there were no vine reviews, there were 0 vine reviews with 5 stars. I chose to determine the amount of ratings for each of the 5 possible values in the star_ratings column. To do this I used a loop because the dataset is too large to count them on my own. 

```
for x in range(1, df.star_rating.nunique()+ 1):
    length = len(df[(df['star_rating'] == x)])
    print(f'Star_count =  {x} amount = {length} ')
```
The results ended up being: 

<img width="289" alt="Screen Shot 2021-10-16 at 12 56 33 PM" src="https://user-images.githubusercontent.com/84936545/137595888-e2cc4c27-9011-4f04-ad06-57cb195bd201.png">

3. What percentage of Vine reviews were 5 stars? What percentage of non-Vine reviews were 5 stars?

Because there was no vine reviews in the dataset, there weren't any that had 5 stars. The percentage of non-vine reviews that were 5-star is 0.7964703318878028. 

This number seems very high, which could possibly be because of multiple reasons. Maybe many of those reviews were made by automated bots. Or, it could be accurate and the percentage is that high because if you are choosing to post a review without being paid to do so, you probably feel strongly about whatever your purchase was (either negatively or positively), and most people really just were very happy with their purchase. If the second is the case, thats a positive sign because it would mean that the majority of people are happy with your product. 









