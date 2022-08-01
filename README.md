# Amazon_Review_ETL
## Project Overview
This is a Big Data Analysis Project showcasing PySpark, AWS, and SQL.  

Utilizing Python on Google Collab, we imported data regarding Amazon Video Game reviews. After importing the data, it was read and placed into a DataFrame where we were able to filter and separate into smaller more readable dataframes.

The next step was to create an Relational Database Service(RDS) on Amazon Web Services that we then linked to PGAdmin on my local device.  AWS allows a user to process large amounts of data on their server which would take much more time to process this 'Big Data' on a PC than it does on their more powerful servers.

![AWS to RDS](https://github.com/MXV0921/Amazon_Review_ETL/blob/main/Images/aws_to_rds.png)

## Reading Data via PGAdmin
After our data was imported to our RDS, we linked our database to our local PGAdmin where it could be more easily accessible for further processing and reading.

Amazon offers a program called 'Vine' for its members.  We took our Video Game data and sorted it by members and non members of the vine program for comparitive review.

Data was filtered and we looked at reviews that were over 50% and there was over 20 votes cast for the item. From this data we then created two more tables, one for Vine Members and ONe for non-Vine users.

![filtered_votes](https://github.com/MXV0921/Amazon_Review_ETL/blob/main/Images/votes_over_20.png)

## Results
The processes for filtering this data into the tables we were looking for was similar after breaking into 'Vine Yes or No" tables we used similar syntax to find the following information.

[!Vine Yes No](https://github.com/MXV0921/Amazon_Review_ETL/blob/main/Images/Vine_Y_N.png)

### Vine Reviews
* There were 94 total Vine 'Y' Reviews
* There were 48 'Y' Vine 5 Star Reviews
* 51% of the Vine Reviews were 5 Stars

![Vine_Yes](https://github.com/MXV0921/Amazon_Review_ETL/blob/main/Images/Vine_Yes_Total.png)

### Non Vine Reviews
* There were 40471 total Vine 'N' Reviews
* There were 15663 'N' Vine 5 Star Reviews
* 39% of the Non Vine Reviews were 5 Stars

![Vine_No](https://github.com/MXV0921/Amazon_Review_ETL/blob/main/Images/Vine_No_Total.png)

```
INSERT INTO total_paid_reviews(total_reviews)
SELECT COUNT(vine)
FROM paid_reviews;
```
```
INSERT INTO paid_five_star_reviews(paid_five_S_reviews)
SELECT COUNT(vine)
FROM paid_reviews
WHERE star_rating = 5;
```
```
SELECT TPR.total_reviews, PFSR.paid_five_S_reviews
INTO percentage_paid
FROM total_paid_reviews as TPR
INNER JOIN paid_five_star_reviews as PFSR
ON 1=1;

-- percentage of 5-star reviews for paid reviews: 
SELECT total_reviews, paid_five_S_reviews,
CONCAT(ROUND (CAST(paid_five_S_reviews AS FLOAT)/
	CAST(total_reviews AS FLOAT)*100), '%')
	As percent_paid_five
INTO percent_paid_five_analysis
FROM percentage_paid;
```


## Analysis
### Vine User Bias
Our dataset of Vine Members is very small compared to the total dataset.  From the Amazon website it states that "Amazon offers Vine members free products that have been submitted to the program by participating selling partners."  This may directly impact the results we see in our analyis.  12% more products are given 5 Stars reviews by vine members.  As a consumer, we may be more critcal of our purchase than an item that is given for free.  A free product will definitely impacts a consumers perceived value of a product.

### Filtering Bias
As part of the filtering process we eliminated product with less than 20 reviews.  While making the data more accurate for 5 Star reviews we are also skewing the data to not see how products with less reviews may have impacted our 5 Star percentage rating for the analysis.  
