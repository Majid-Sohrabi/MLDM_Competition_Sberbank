# MLDM_Competition_Sberbank

Competition: [Click here!](https://www.kaggle.com/c/sberbank-russian-housing-market)

## Report:
 
### 1. Data Preprocessing 

For data preprocessing we tried to clean the data in some steps:
  
* `full_sq: total area in square meters, including loggias, balconies, and other non-residential areas`
* `life_sq: living area in square meters, excluding loggias, balconies, and other non-residential areas`

* The life square should be less than the full square and if life: if life_sq > full_sq we put NaN

* The life square is at least 5 compared to the full square: So, we put NaN when life_sq < 5

* The full square is at least 5, So, if full_sq < 5 we put NaN

* The value of kitchen square at index 13117 = 1970.0 should be for the build year.

* The kitchen square should be less than life square then: if kitch_sq >= life_sq, we put NaN

* The number of rooms should be more than 1: If num_room = 0, we put NaN

* If num room >= 6 we put NaN because all records the number of rooms <= 6 except some of the records.

* The floor started from 1, so we put NaN if floor = 0 and the same for the max floor should be more than 0, so we put NaN if max floor = 0

* The floor number should be less and equal max floor, so we put floor number, NaN, if floor number > max floor.

* there are 4 apartment conditions, so we put Nan if apartment condition >= 5

Second attempt to clean the data are describes as follows:

* We encoded categorical features to numerical. We had 12 ["No", "Yes"] features and we mapped them to [0, 1].

* There was a feature named 'ecology' with the categorical values ['no data', 'poor', 'satisfactory', 'good', 'excellent'] and we mapped them to [0, 1, 2, 3, 4].

* Where was a feature named 'product_type' whih the categorical values ['Investment', 'OwnerOccupier'] and we mapped them to [0, 1] values.

* We replaced the Nan values with the mean value of the column, -inf with the minimum value in the column, and inf with the maximum value in the column.

* In the next step we removed the outliers, as an example 3M < price < 100M was selected, but if we do it we will discard about 9% of the dataset which is not logical. Consequently, we can conclude that the data has a lot of unreal and curropted values.

* In the final step we normalized the dataset, for train and test we used MinMaxScaler(function), and for the target we standardized it by standatd deviation (std)

### 2. Feature Engineeting

  * We add some features of engineering:
  * Relative floor = number of floor / max floor.
  * Relative kitchen square = kitchen square / full square.
  * Room size = life square / number of rooms.
  * Then we encode object column values with values between 0 and (n_classes-1).

### 3. Feature Selection

* We splitted the Date into two columns day and month after that we drop the column ???time stamp???. We used all features for training.
* In another trial we extracted the important features by XGBRegressor and just selected the most 12 important features for exeriments.


### 4. Model Selection

* Then we use XGBRegressor for many reasons it is Highly Flexible, it uses the power of parallel processing, and it is designed to handle missing data with its in-build features (the dataset contains many NaN values)


### 5. Model Development

* But first, we use GridSearchCV to search over specified parameter values for an estimator (XGBRegressor) to find the best parameters. (Tuning stage)

### 6. Model Quality Assesment

* We did many trials and we could achive ~0.87% accuracy for training.
* We submitted in Kaggle, we got a score = 0.3161, and our rank was around 157.

### 7. Future Investigation

* For the next step, we are planning to apply Neural Network to improve the score, and also we will try to find the best parameters for XGBRegressor.

### 7.1 

* As further analysis, we implemented random forest, linear regression, two different neural networks (one without LSTM and another with LSTM) in TensorFlow. Additionally we implemented the second stage of the normalization to fill NaN, -inf, and inf with proper values.
* For random forest we are witness that the algorithm has a good performance in our data, on the other hand, linear regression was not able to achieve a good result.
* For the neural network (both with LSTM and without it) we split the training data into train and validation parts to monitor if the model will become overfitted on our data or not (because we don't have the test data). Additionally, we sorted both training and validation data, and based on that we plotted the true value for target and predicted value by our model to see if the result fitted each other or not.
* We applied our custom network, and we investigated with many layers, batch size, learning rate, drop out, ... but finally we found that our model can achieve the better performance with the default value for batch size, learning rate, and without drop out.

In conclusion, when we uploaded the result into the Kaggle, we noticed that XGBRegressor had a better performance against other algorithms we implemented, and the reason is this method can accept NaN values so we don't need to manipulate the data so much. But for the other methods, we had to apply the second phase of the preprocessing to handle NaN, inf, and -inf values. Based on the fact that we have far much missing value then this phase will change the nature of the data, then those methods couldn't achieve good performance.
