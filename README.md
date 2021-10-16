# Amazon_Vine_Analysis

## Overview: 

The Amazon Vine program is a service that allows manufacturers and publishers to receive reviews for their products. Companies like SellBy pay a small fee to Amazon and provide products to Amazon Vine members, who are then required to publish a review. I am analyzing a dataset from amazon which contains reviews of US digital music purchases. The image below describes the data contained in each column. 


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



