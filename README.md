## Resources

- **Software**: Python 3.7.6, Conda 4.10.1  
- **Processing Engine**: Spark  
- **Database**: AWS D3  
- **Interface**: PgAdmin Version 5.2  
- **Environment**: Jupyter Notebook  



# Data Analysis Project: Investigating Potential Bias in Amazon Vine Reviews

The Amazon Vine program is a service through which manufacturers and publishers can receive customer reviews for their products. Participating companies pay a fee to Amazon and provide products to Amazon Vine members, who are then required to publish a review. Vine members, known as **Vine Voices**, are selected based on their history of insightful reviews within the Amazon community, which establishes their credibility and trustworthiness.

Vine Voices choose products they wish to review, use them free of charge, and share their honest, unbiased opinions in written reviews. This program serves as a structured mechanism for gathering product feedback while maintaining a level of transparency about the review process.

For this project, I have selected a dataset from Amazon that contains reviews of digital music purchases in the United States. The primary objective is to analyze the dataset to determine whether there is any discernible bias in the reviews. The analysis will focus on identifying patterns or trends that may indicate discrepancies in review behavior or ratings among Vine and non-Vine participants.

Below is an image summarizing the data structure, including details of each column in the dataset:


<img width="1352" alt="Screen Shot 2021-10-16 at 12 06 30 PM" src="https://user-images.githubusercontent.com/84936545/137594392-b06fb75f-ecd5-46ae-a5c5-53b8b6cd6ab2.png">

### Shortened description analysis 


## Deliverable 1: Perform ETL on Amazon Product Reviews

Reference Amazon Vine Analysis ETL [here](https://github.com/estridge2014/Amazon_Vine_Analysis/blob/main/Amazon_Reviews_ETL.ipynb)

For this analysis, I created an **Amazon Web Services (AWS) RDS database** and used **PgAdmin**, an open-source management tool for PostgreSQL, to manage the database tables. 

To process the data, I extracted the reviews from the complete **US Digital Music Purchases** dataset provided by Amazon into a **Spark DataFrame**. The DataFrame was then transformed into four separate DataFrames, each designed to match the schema of the tables in PgAdmin. These tables are shown below.



#### Customers DF 


<img width="259" alt="Screen Shot 2021-10-16 at 12 21 37 PM" src="https://user-images.githubusercontent.com/84936545/137594872-f4ff6309-116c-4dbb-b1cc-f34c1090cc6e.png">


#### Review id DF

<img width="569" alt="Screen Shot 2021-10-16 at 12 21 59 PM" src="https://user-images.githubusercontent.com/84936545/137594909-e525aeaf-791f-4067-a1f9-0dc1ee326499.png">


#### Products DF

<img width="307" alt="Screen Shot 2021-10-16 at 12 21 29 PM" src="https://user-images.githubusercontent.com/84936545/137594946-9b5acf25-3cf3-4364-9dfc-c793df157760.png">

#### Vine DF

<img width="684" alt="Screen Shot 2021-10-16 at 12 22 18 PM" src="https://user-images.githubusercontent.com/84936545/137594981-8d34755f-2e22-4ac7-a32f-5611239f2cdf.png">

I uploaded the transformed data in these dataframes into the appropriate tables in pgadmin. I ran queries using postgresql to confirm that the data had been uploaded.

## Deliverable 2: Analysis (examining for bias): 

Reference Amazon Vine Program analysis code [here](https://github.com/estridge2014/Amazon_Vine_Analysis/blob/main/Vine_Review_Analysis.ipynb) 

The goal of this analysis was to determine if there was any bias in the reviews written as part of the Vine program in this dataset. Specifically, I aimed to examine whether having a paid Vine review influenced the percentage of 5-star reviews that products received. 

To conduct this analysis, I exported the data as a CSV file through **PgAdmin** and used **Pandas** for data analysis. The conclusions drawn from this analysis are discussed below.

## Key Findings

#### 1. How many Vine reviews and non-Vine reviews were there?
The total number of reviews in the dataset was **1,688,884**. Upon filtering and examining the data for Vine reviews, I observed that there were **no Vine reviews** listed in the dataset. The count of non-Vine reviews was **1,688,884**, accounting for all reviews in the dataset.

#### 2. How many Vine reviews were 5 stars? How many non-Vine reviews were 5 stars?
Since there were no Vine reviews in the dataset, there were **0 Vine reviews with 5 stars**. 

To further analyze the distribution of star ratings among non-Vine reviews, I calculated the number of reviews for each of the 5 possible star ratings. Below is the code snippet used for this calculation:

```
for x in range(1, df.star_rating.nunique()+ 1):
    length = len(df[(df['star_rating'] == x)])
    print(f'Star_count =  {x} amount = {length} ')
```
The results ended up being: 

<img width="289" alt="Screen Shot 2021-10-16 at 12 56 33 PM" src="https://user-images.githubusercontent.com/84936545/137595888-e2cc4c27-9011-4f04-ad06-57cb195bd201.png">

#### 3. What percentage of Vine reviews were 5 stars? What percentage of non-Vine reviews were 5 stars?

Because there are no Vine reviews in the dataset, there were no Vine reviews that earned 5 stars. The percentage of non-Vine reviews that earned 5 stars is **79.65%** (calculated as 0.7964703318878028).

## Deliverable 3: Conclusion 

While it is possible that this statistic accurately represents individual buyers' opinions of the product, this high percentage of 5-star reviews could also indicate potential bias in the dataset. Two common types of self-selection bias in product reviews may have influenced these results:

1. **Acquisition Bias**: Consumers with a favorable predisposition toward a product are more likely to acquire it and subsequently write a review.  
2. **Underreporting Bias**: Consumers with extreme opinions, either highly positive or negative, are more likely to write reviews than those with moderate ratings.

These biases could have impacted the observed percentage of 5-star reviews.

Since this dataset contains no reported Vine reviews, I am unable to evaluate how the Vine program might influence the likelihood of bias within this dataset.

