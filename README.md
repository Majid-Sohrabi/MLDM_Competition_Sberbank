# MLDM_Competition_Sberbank

Competition: [Click here!](https://www.kaggle.com/c/sberbank-russian-housing-market)

## Report:
 
### 1. Data Preprocessing 


For data preprocessing we tried to clean the data in some steps:
  
full_sq: total area in square meters, including loggias, balconies, and other non-residential areas
life_sq: living area in square meters, excluding loggias, balconies, and other non-residential areas

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


### 2. Feature Engineeting

  We add some features of engineering:
  Relative floor = number of floor / max floor.
  Relative kitchen square = kitchen square / full square.
  Room size = life square / number of rooms.
  Then we encode object column values with values between 0 and (n_classes-1).


### 3. Feature Selection

  We splitted the Date into two columns day and month after that we drop the column ‘time stamp’. We used all features for training.


### 4. Model Selection

- Then we use XGBRegressor for many reasons it is Highly Flexible, it uses the power of parallel processing, and it is designed to handle missing data with its in-build features (the dataset contains many NaN values)


### 5. Model Development

- But first, we use GridSearchCV to search over specified parameter values for an estimator (XGBRegressor) to find the best parameters. (Tuning stage)

### 6. Model Quality Assesment

* We did many trials and we could achive ~0.87% accuracy for training.
* We submitted in Kaggle, we got a score = 0.3161, and our rank was around 157.
* For the next step, we are planning to apply Neural Network to improve the score, and also we will try to find the best parameters for XGBRegressor.
