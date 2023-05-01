# Amazon-Gourmet-Groceries-Recommendar-System
## Problem & Dataset
* How to find the best recommendations for the user based on their likes and dislikes
* Dataset : Grocery and Gourmet Food + the metadata
<img src="https://user-images.githubusercontent.com/31558571/235461584-51b16937-7841-40e9-b6f1-3d90c9bda3ad.png" width="40%" height="40%">

## Exploratory Data Analysis
* The amazon dataset comes with a lot of interesting features, we have used (ASIN, ReviewerID, Rating, Category, Title, Description)
* Average number of reviews per product 27.69
* Average number of reviews per reviewer ~ 9 reviews 
* And 5* seems to be the most popular rating given
* The User-item matrix has a sparsity of 0.794​
  <div style="display: flex; flex-wrap: wrap;">
    <img src="https://user-images.githubusercontent.com/31558571/235461687-3b9137a0-7491-48cd-b053-ced84320302c.png" width="30%" height="30%">
    <img src="https://user-images.githubusercontent.com/31558571/235461702-a84efa61-1b61-4fd2-bef4-737445348704.png" width="50%" height="50%">
    <img src="https://user-images.githubusercontent.com/31558571/235461780-f77c5ac1-4707-4d78-8dcf-a25dbeb662a4.png" width="50%" height="50%">
    <img src="https://user-images.githubusercontent.com/31558571/235461801-e52d2853-4fdf-4f98-afa8-eecd230332db.png" width="30%" height="30%">
    <img src="https://user-images.githubusercontent.com/31558571/235485094-90b2441e-64cb-408c-ad2e-053a16d0ca8c.png" width="20%" height="20%">
  </div>

## Data Preping

* Matrix Sparsity leads to data not fitting in the RAM as we have the matrix dimensions of 127496*41320. 
* Step 1: Drop columns which has no impact to the rating predictions: image, reviewername, summary etc.
* Step 2: Keep those rows for which the reviewer is verified.  
* Step 3: Remove the reviewers who have reviewed less than 20 products. 
* Step 4: Group by the reviewer such that each unique reviewer maps to the products they reviewed, and rating provided.
* Step 5: Use Train test split to split the processed data in the 80-20%.

## Modelling
We have explored the following approaches for Rating prediction. Comparison of performance of the algorithms is done :
1. Baseline
2. Singular Value Decomposition (SVD)
3. k Nearest Neighbors (kNN)
4. Slope One
5. Matrix Factorization

### Rating Prediction Approach 1
K Nearest Neighbors: 
Feature similarity to predict new data points. 
User based CF:  
* Tries to identify users with the most similar 'Interaction Profile'. 
* Suggest items that are the most popular among these neighbors. 
Item based CF: 
* Items like the ones the user already 'positively' interacted.
* Suggest items such that most users interact with those items. Eg milk, eggs in grocery dataset.
<img src="https://user-images.githubusercontent.com/31558571/235463250-530e64c2-e4d1-4b31-ba0e-b502d705a430.png" width="30%" height="30%">

### Rating Prediction Approach 2
Slope One (Weighted)– 
Additional info used in Slope One – 
* Ratings by users who have rated some common item
* Ratings of other items by the user
<img src="https://user-images.githubusercontent.com/31558571/235463666-5c5ea9f1-8fd2-4b0d-bcd1-41ba7826d5d0.png" width="20%" height="20%">

### Rating Prediction Approach 3
Latent Factor Model - SVD – 
* Users and items are mapped to latent factor space
* qi – item-concept mapping for item i
* pu – user-concept mapping for user u
  
  <img src="https://user-images.githubusercontent.com/31558571/235484363-f40ad4f9-b030-40f7-b9de-ea1e195f24b1.png" width="30%" height="30%">
* Funk SVD was used for minimization using learning rate = 0.009, and regularization constant = 0.05
  
  <img src="https://user-images.githubusercontent.com/31558571/235463817-4ef58d05-5c32-42e9-a916-c0a10d1b233e.png" width="20%" height="20%">

## Item Recommendation
* Step 1: Create prediction dataframe from the algorithm. Consists of reviewerid, productid and predicted rating value. 
* Step 2: Create a nested list such that for each reviewer we have the tuple (productid, predicted_rating). 
* Step 3: Sort the list such that we get the top 10 values for each user. 

## Metrics
<img src="https://user-images.githubusercontent.com/31558571/235463927-75215ac0-77b2-4d3f-ba5e-ee5725e8090d.png" width="50%" height="50%">

## Future Work
* Incorporate more features – price ranges, seasonality of products, 
* Effect of NLP, including implicit feedback. 
* Fairness and Unbiases of Recommender Systems (Providing users with feedback on their recommendations. This can be done by showing users the factors that contributed to a particular recommendation, or by allowing users to flag recommendations that they do not think are relevant)
* Privacy Protection for Recommender Systems - (we can collect only the data that is necessary, also giving users control over their data and anonymization of data)

The approaches and the results are described in detail in the attached report. 
