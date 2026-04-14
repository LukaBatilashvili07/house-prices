# House-Prices Advanced Regression Techniques

## კონკურსის მიმოხილვა
კონკურსში მოცემულია აიოვას შტატის ქალაქ ეიმსში სახლების მონაცემები. მონაცემთა ბაზა შეიცავს 79 ცვლადს, რომლებიც სრულად აღწერენ სახლის ყველა მახასიათებელს. კონკურსის მიზანია სახლებისთვის საბოლოო ფასის პროგნოზი(ბაზაში თითოეული Id-სთვის SalePrice-ის პროგნოზი).

## მიდგომა პრობლემის გადასაჭრელად
1. მონაცემების გაწმენდა და missing value-ების შევსება
2. Feature Engineering
3. კატეგორიული ცვლადების რიცხვით მნიშვნელობაში გადაყვანა
4. Feature Selection
5. სხვადასხვა მოდელის გამოყენება
6. საუკეთესო მოდელის Hyperparameter Tuning RandomizedSearchCV-ით
7. ყველა მოდელის MLFlow Tracking
8. საბოლოო მოდელის არჩევა და Kaggle-ს submission-ის შექმნა

## რეპოზიტორიის სტრუქტურა
#კონკურსის საწყისი მონაცემები
- data/
    - data_description.txt  
    #submission მაგალითი
    - sample_submission.csv
    #სატესტო მონაცემები
    - test.csv
    #სატრენინგო მონაცემები
    - train.csv
#მოდელის ტესტირება და დატრენინგება
- model_experiment.ipynb
#საუკეთესო მოდელის მიხედვით პროგნოზის გაკეთება
- model_inference.ipynb
#პროექტის აღწერა
- README.md
#Kaggle submission, საბოლოო პროგნოზი
- submission.csv

## Feature Engineering

დამატებითი features:

- TotalSF = Basement + 1st + 2nd floor
- TotalBath = Full + Half + Basement baths
- TotalPorchSF
- Age = YrSold - YearBuilt
- RemodAge = YrSold - YearRemodAdd
- HasPool
- HasGarage
- HasBsmt
- HasFireplace
- IsRemodeled

1. None მნიშვნელობები - კატეგორიული ცვლადებისთვის, სადაც NaN ნიშნავს ცვლადის არარსებობას. მაგალითად სახლს თუ არ აქვს აუზი, PoolQC ველი ცარიელი იქნება და არა უცნობი.
2. 0-ით შევსება - რიცხვითი ცვლადებისთვის, სადაც Nan ნიშნავს 0-ს ჩაიწერება 0.
3. mode და median შევსება

კატეგორიული ცვლადების რიცხვითში გადაყვანა:
- Ordinal Encoding - ხარისხის ცვლადებისთვის: Ex=5, Gd=4, TA=3, Fa=2, Po=1, None=0
- Category Codes - დანარჩენი კატეგორიული ცვლადებისთვის.

Cleaning მიდგომები:
- Outlier-ების წაშლა: GrLivArea > 4000 და SalePrice < 300000 - 2
- Skewness Fix

## Feature Selection
RandomForestImportance - 28 features

## Training
ტესტირებული მოდელები:
1. Linear Regression
2. Lasso(alpha = 0.1, 1.0, 10.0)
3. Ridge(alpha = 0.1, 1.0, 10.0)
4. Random Forest
5. XGBoost
6. LightGBM
7. XGBoost Tuned

Hyperparameter ოპტიმიზაციის მიდგომა:
- XGBoost-ისთვის RandomizedSearchCV

საბოლოო მოდელის შერჩევის დასაბუთება;
- საბოლოო მოდელი არის XGBoost Tuned: აქვს ყველაზე დაბალი val RMSE, train და val RMSE სხვაობა დაბალია.

## MLflow Tracking
MLflow ექსპერიმენტების ბმული:
- https://dagshub.com/LukaBatilashvili07/house-prices.mlflow

ჩაწერილი მეტრიკების აღწერა:
- train_rmse სატრენინგო RMSE
- val_rmse სავალიდაციო RMSE
- train_r2 სატრენინგო R^2
- val_r2 სავალიდაციო R^2

საუკეთესო მოდელის შედეგები:
- val_rmse 0.37
- val_r2 0.19
- train_rmse 0.26
- train_r2 0.56
- Kaggle Score - 0.21571
