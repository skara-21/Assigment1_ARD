# 🏠 House Price Prediction

პროექტის მიზანია საცხოვრებელი სახლების ფასის პროგნოზირება სხვადასხვა რეგრესიაზე დაფუძნებული მოდელების გამოყენებით. გამოყენებულია Sklearn, XGBoost, Feature Engineering, და MLflow მოდელის ტრენინგისთვის, ხოლო Dagshub გამოიყენება როგორც პლატფორმა მოდელების ვერსიირებისა და ლოგირებისათვის.

## პროექტის სტრუქტურა

- `model_experiment.ipynb`  
  - ძირითადი ფაილი.
  - შეიცავს შემდეგ დანაყოფებს
    - **Data Cleaning**
    - **Feature Selection**
    - **Feature Engineering**
    - **Training**
    - **ML Flow Tracking**
    - **Result saving**

- `model_inference.ipynb`  
  - წინა ფაილის ბოლოს შევინახე როგორც მოდელი, ისე დამუშავებული მონაცემები.
  - ამ ფაილში **Model Registry**-დან დავა-Load-ე მოდელი
  - შემოვიტანე დამუშავებული მონაცამები და გავაკეთე პროგნოზი.
  - საბოლოო პროგნოზს მივეცი competition-ისთვის შესაფერისი სახე და ავტვირთე.
  - **შედეგი 0.13371**

- `README.md`  
  - შეიცავს პროექტის მოკლე მიმოხილვას




## გამოყენებული ტექნოლოგიები

- pandas, numpy
- scikit-learn
- XGBoost
- MLflow
- Dagshub

## მოდელები

შედარებული იქნა შემდეგი რეგრესიული მოდელები:

- Linear Regression
- Ridge Regression
- Lasso Regression
- Random Forest
- XGBoost

შეფასება მოხდა `R2 Score`-ის და `RMSLE`-ის მიხედვით Cross-Validation-ით (KFold).

## Model Evaluation

საუკეთესო შედეგი აჩვენა **XGBoost** მოდელმა, თუმცა სხვა მოდელებმაც დაადასტურეს სტაბილური შედეგები. შეფასება ხორციელდება როგორც Cross-Validation-ზე, ასევე Validation სეტზე.

### Evaluation Metrics:
- R² Score
- Adjusted R² Score
- Root Mean Squared Log Error (RMSLE)
- Mean Absolute Error (MAE)
- F-statistic

## Feature Engineering/Selection
- პირველ რიგში გადავუყევი მონაცემებს და წავშალე ის სვეტები რომლებიც 80%-ზე მეტ NaN მნიშვნელობებს შეიცავდა
- ამის შემდეგ შევავსე დარჩენილ სვეტებში NaN მნიშვნელობები შესაბამისი მონაცემით (კონკრეტული სვეტის მოდით)
- ამის შემდეგ მონაცემებს მოვაშორე outlier-ები, რადგან ვიცით რომ მსგავსი წერტილები დიდ გავლენას ახდენს წრფივ მოდელებზე
- შემდეგ, ამოვიღე ისეთი სვეტები რომლეიბიც 97% ერთი და იგივე მნიშვნელობებს შეიცავდა, რადგან მათი არსებობით ღირებულ ინფორმაციას ვერ ვიღებდით.
- მთავარი გამოწვევა იყო object ტიპის მონაცემების რიცხვითში გადაყვანა. ამისთვის გამოვიყენე **WOEEncoder** და სხვადასხვანაირად გადავიყვანე ბინარული (ორი განსხვავებული მნიშვნელობის მქონე) და არაბინარული სვეტები რიცხვით მონაცემებში
- ბოლოს კი მოვძებნე ისეთი სვეტები რომლებიც ძალიან კორელირებული იყო ერთმანეთთან და ერთ-ერთ გადავაგდე, საბოლოო მონაცემების რაოდენობის შესამცირებლად.

## Training
- ამ ეტაპზე დავტესტე 5 მოდელი:
    - **LinearRegression**
    - **RandomForest**
    - **XGBoost**
    - **Lasso**
    - **Ridge**
- Hyperparameter ოპტიმიზაციისთვის გამოვიყენე **KFold**-ის და **GridSearchCV**-ის კომბინაცია
- ზემოთ ვახსენე რა პარამეტრებს ვაქცევდი ყურადღებას, შესაბამისად საბოლოო მოდელი ავარჩიე ამ პარამეტრებზე დაყრდნობით

## MLFow Tracking
| Model           | Training R² | Validation R² | Validation RMSE | MAE       | MSLE   | F-statistic | p-value | MLflow Run |
|-----------------|-------------|---------------|------------------|-----------|--------|-------------|---------|------------|
| [LinearRegression](https://dagshub.com/skara-21/Assignment1_ARD.mlflow/#/experiments/0/runs/f157cbfec6064388be772ef3f80880e1) | 0.8625      | 0.8390        | 35136.9262       | 20857.5339 | 0.0260 | 108.09      | 0.0000  | 🔗 |
| [RandomForest](https://dagshub.com/skara-21/Assignment1_ARD.mlflow/#/experiments/0/runs/7b57110eac71412a8bdefec9949775e1) | 0.9791      | 0.8898        | 29069.5874       | 17043.7247 | 0.0215 | -           | -       | 🔗 |
| [XGBoost](https://dagshub.com/skara-21/Assignment1_ARD.mlflow/#/experiments/0/runs/1a3db518053d4ae3a34b4855a4b4f46b) | 0.9814      | 0.9142        | 25647.3774       | 16045.6185 | 0.0181 | -           | -       | 🔗 |
| [Lasso](https://dagshub.com/skara-21/Assignment1_ARD.mlflow/#/experiments/0/runs/2c63b09143b64228be186facb2bd63ce) | 0.8625      | 0.8393        | 35112.4377       | 20832.8629 | 0.0259 | 108.09      | 0.0000  | 🔗 |
| [Ridge](https://dagshub.com/skara-21/Assignment1_ARD.mlflow/#/experiments/0/runs/fcf2dc41f19445fd9151a45e5513c960) | 0.8621      | 0.8399        | 35038.1316       | 20721.6985 | 0.0258 | 107.74      | 0.0000  | 🔗 |



## საუკეთესო მოდელი **XGBoost**