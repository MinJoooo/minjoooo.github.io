---
layout: single
title: "[Hands-On Machine Learning 1회독] Chapter 2. 머신러닝 프로젝트 처음부터 끝까지"
categories: "핸즈온머신러닝1회독"
tag: [python, Machine Learning]
toc: false
---

<br>
<br>
<br>

###### 2022.05.19.
###### Hands-On Machine Learning 2판
###### Chapter 2. 머신러닝 프로젝트 처음부터 끝까지
###### 1회독 - 내용 단순 요약











Project 단계

1. 큰 그림을 본다.

2. data를 구한다.

3. data로부터 insight를 얻기 위해 탐색하고 visualization한다.

4. machine learning 알고리즘을 위해 data를 준비한다.

5. model을 select하고 training한다.

6. model을 fine-tuning한다.

7. solution을 제시한다.

8. system을 launching하고 monitoring하고 maintenance한다.





Data를 구하기 좋은 곳

- UC Irvine machine learning repository

- Kaggle datasets

- Amazon's AWS datasets

- Data portals (공개 데이터 저장소가 나열됨)

- Open data monitor (공개 데이터 저장소가 나열됨)

- Quandl (공개 데이터 저장소가 나열됨)

- Wikipedia's list of machine learning datasets (공개 데이터 저장소가 나열됨)

- Quora.com (공개 데이터 저장소가 나열됨)

- Dataset subreddit (공개 데이터 저장소가 나열됨)





Data pipeline

- data 처리 component들이 연속되어 있는 것

- machine learning system은 data를 조작하고 변환할 일이 많아 pipeline을 사용하는 일이 매우 흔함

- 보통 component들은 비동기적으로 동작함. 각 component는 많은 data를 추출해 처리하고 그 결과를 다른 data 저장소로 보냄. 그러면 일정 시간 후 pipeline의 다음 component가 그 data를 추출해 자신의 출력 결과를 만듦.

- 각 component는 완전히 독립적임. 즉, component 사이의 interface는 data 저장소뿐임. 이는 system을 이해하기 쉽게 만들고, 각 team이 각자의 component에 집중할 수 있게 함. 한 component가 다운되더라도 하위 component는 문제가 생긴 component의 마지막 출력을 사용해 한동안은 평상시와 같이 계속 동작할 수 있음. 그래서 system이 매우 견고해짐.

- 그러나 monitoring이 적절히 되지 않으면 고장난 component를 한동안 모를 수 있음. data가 만들어진지 오래되면 전체 system의 성능이 떨어짐.





용어

- multiple regression (다중 회귀): 예측에 사용할 feature가 여러 개임.

- univariate regression (단변량 회귀): 하나의 대상에 대해 하나의 value 예측

- multivariate regression (다변량 회귀): 하나의 대상에 대해 여러 개의 value 예측

- Map Reduce: data가 매우 클 때 batch 학습을 여러 server로 분할하는 기술. 대표적인 프레임워크로 Apache Hadoop이 있음.





성능 측정 지표

- Root mean square error (RMSE, 평균 제곱근 오차)

- Mean Absolute Error (MAE, 평균 절대 오차)







책에서 사용하는 표기법

- m: sample 수

- x^(i): i번째 sample의 전체 feature value의 vector

- y^(i): 해당 label

- X: dataset에 있는 모든 sample의 모든 feature value (label 제외)를 포함하는 matrix. sample 하나가 하나의 row이어서 i번째 row는 x^(i)의 전치와 같고 (x^(i))^T로 표기함.

- h(hypothesis, 가설): system의 예측 function. system이 하나의 x^(i)를 받으면 그에 대한 예측값 hat(y^(i)) = h(x^(i))를 출력함. 예측 error는 hat(y^(i)) - y^(i)








Norm (Distance measures, 거리 측정)

- Euclidean norm: RMSE, l_2 norm

- Manhattan norm: 절댓값의 합을 계산, l_1 norm

- l_k norm: 원소가 n개인 vector v의 l_k norm은 다음과 같이 정의함. l_0는 단순히 vector에 있는 0이 아닌 원소의 수이고, l_∞는 vector에서 가장 큰 절댓값이 됨.


- norm 지수가 클수록 큰 값의 원소에 치우치며 작은 값이 무시됨 => RMSE가 MAE보다 조금 더 이상치에 민감함. 하지만 이상치가 매우 드물면 RMSE가 잘 맞음





데이터를 추출하는 코드

import os
import tarfile
import urllib.request

DOWNLOAD_ROOT = "https://raw.githubusercontent.com/rickiepark/handson-ml2/master/"
HOUSING_PATH = os.path.join("datasets", "housing")
HOUSING_URL = DOWNLOAD_ROOT + "datasets/housing/housing.tgz"

def fetch_housing_data(housing_url=HOUSING_URL, housing_path=HOUSING_PATH):
    if not os.path.isdir(housing_path):
        os.makedirs(housing_path)
    tgz_path = os.path.join(housing_path, "housing.tgz")
    urllib.request.urlretrieve(housing_url, tgz_path)
    housing_tgz = tarfile.open(tgz_path)
    housing_tgz.extractall(path=housing_path)
    housing_tgz.close()




데이터를 읽어들이는 코드

- dataframe 객체를 반환함

import pandas as pd

def load_housing_data(housing_path=HOUSING_PATH):
    csv_path = os.path.join(housing_path, "housing.csv")
    return pd.read_csv(csv_path)




데이터 훑어보기

- head(): 처음 다섯 행 확인

housing = load_housing_data()
housing.head()

- info(): 데이터에 대한 간략한 설명. 전체 row 수, 각 attribute의 data type의 null이 아닌 value의 개수를 확인하는 데 유용함.

housing.info()

- value_counts(): 어떤 category가 있고 각 category마다 얼마나 많은 구역이 있는지

housing["ocean_proximity"].value_counts()

- describe(): 숫자형 특성의 요약 정보

housing.describe()

- hist(): 모든 숫자형 특성에 대한 histogram 출력

hist()는 matplotlib을 사용하고 화면에 graph를 그리기 위해 user computer의 graphic backend를 필요로 함. 그래서 graph를 그리기 전에 matplotlib이 사용할 backend를 지정해줘야 함. 이때 Jupyter의 %matplotlib inline을 사용하면 편리함. 이 명령은 matplotlib이 Jupyter 자체의 backend를 사용하도록 설정하고, 그러면 graph는 notebook 안에 그려지게 됨. Jupyter notebook에서 graph를 그릴 때 show()를 호출하는 것은 선택사항이고, Jupyter는 cell이 실행될 때 자동으로 graph를 그려줌.

%matplotlib inline
import matplotlib.pyplot as plt
housing.hist(bins=50, figsize=(20,15))
save_fig("attribute_histogram_plots")
plt.show()





Test set 만들기

- 이 단계에서 data 일부를 자진해서 떼어놓아야 함 => 그렇지 않으면 data snooping bias 문제가 생김.

- data snooping bias: 우리 뇌는 overfitting되기 쉽기 때문에, 만약 test set를 너무 자세하게 들여다보면 test set의 겉으로 드러난 pattern에 속아 특정 model을 선택하게 될지도 모름. 이 test set로 generalization error를 estimate하면 매우 낙관적인 estimate가 되어 system을 launching 했을 때 기대한 성능이 나오지 않게 됨.

- 이때 test set는 program을 실행할 때마다 바뀌는 것이 아니라, 고정된 set여야 함. 이를 위해 index 등을 이용함. => 만약 그렇지 않으면, test set이 계속 새롭게 update되어, 전체 data를 training 과정에서 사용하게 됨.

from zlib import crc32

def test_set_check(identifier, test_ratio):
    return crc32(np.int64(identifier)) & 0xffffffff < test_ratio * 2**32

def split_train_test_by_id(data, test_ratio, id_column):
    ids = data[id_column]
    in_test_set = ids.apply(lambda id_: test_set_check(id_, test_ratio))
    return data.loc[~in_test_set], data.loc[in_test_set]
from sklearn.model_selection import train_test_split
train_set, test_set = train_test_split(housing, test_size=0.2, random_state=42)
- 만약 index column이 없으면 다음과 같이 index column을 생성하면 됨.

housing_with_id = housing.reset_index() # 'index' column이 추가된 dataframe이 반환됨.
train_set, test_set = split_train_test_by_id(housing_with_id, 0.2, "index")




Stratified sampling (계층적 샘플링)

- sampling bias가 생기지 않도록 strata(계층)를 나눈 후 전체 data의 strata 비율에 맞춰 각 strata에서 sampling => 전체 data의 strata 비율과 sampling data의 strata 비율이 같게 됨

- 중요한 feature를 strata로 나눠야 함. 이때 strata마다 dataset에 충분한 sample 수가 있어야 함. 즉, strata의 수 (category 개수)가 너무 많으면 안됨.

housing["income_cat"] = pd.cut(housing["median_income"],
                               bins=[0., 1.5, 3.0, 4.5, 6., np.inf],
                               labels=[1, 2, 3, 4, 5])
income category에서 0~1.5는 category 1, 1.5~3.0는 category 2와 같이 범위를 나눔.

- Scikit-learn의 StratifiedShuffleSplit을 사용해 Stratified sampling

from sklearn.model_selection import StratifiedShuffleSplit

split = StratifiedShuffleSplit(n_splits=1, test_size=0.2, random_state=42)
for train_index, test_index in split.split(housing, housing["income_cat"]):
    strat_train_set = housing.loc[train_index]
    strat_test_set = housing.loc[test_index]




Data Visualization

housing.plot(kind="scatter", x="longitude", y="latitude", alpha=0.4,
             s=housing["population"]/100, label="population", figsize=(10,7),
             c="median_house_value", cmap=plt.get_cmap("jet"), colorbar=True,
             sharex=False)
plt.legend()
save_fig("housing_prices_scatterplot") # Save picture
alpha: data point가 밀집된 영역을 잘 보여줌

s: radius of a circle. population을 나타냄.

c: color. price을 나타냄.

cmap (color map): jet 사용 => blue(low prices) ~ red(high prices)






Correlation (상관관계)

- Standard correlation coefficient (표준 상관계수) or Pearson's r (피어슨의 r)

- 상관관계의 범위: -1 ~ 1

- -1는 음의 상관관계, 0은 상관관계가 없음, 1은 양의 상관관계

corr_matrix = housing.corr()
corr_matrix["median_house_value"].sort_values(ascending=False)

from pandas.plotting import scatter_matrix

attributes = ["median_house_value", "median_income", "total_rooms",
              "housing_median_age"]
scatter_matrix(housing[attributes], figsize=(12, 8))
save_fig("scatter_matrix_plot")

왼쪽 위에서 오른쪽 아래로 가는 대각선 방향은 각 변수 자신에 대한 것이라 그냥 직선이 됨 => pandas는 이곳에 각 attribute의 histogram을 그림 

- 여러 가지 dataset에 나타난 standard correlation coefficient






데이터 정제

- 해당 row 또는 column 제거

- 전체 attribute 삭제

- null을 0, mean, median과 같은 value로 채움

# drop()은 data copy를 만들고 원본 data인 strat_train_set에는 영향을 주지 않음
housing = strat_train_set.drop("median_house_value", axis=1) # training set를 위해 label 삭제
housing_labels = strat_train_set["median_house_value"].copy()

# 해당 row 또는 column 제거
sample_incomplete_rows.dropna(subset=["total_bedrooms"])

# 전체 attribute 삭제
sample_incomplete_rows.drop("total_bedrooms", axis=1)

# null을 median으로 채움
median = housing["total_bedrooms"].median()
sample_incomplete_rows["total_bedrooms"].fillna(median, inplace=True)
- Scikit-learn의 SimpleImputer: null을 median으로 대체함

from sklearn.impute import SimpleImputer
imputer = SimpleImputer(strategy="median")

# training set transform
X = imputer.transform(housing_num)
housing_tr = pd.DataFrame(X, columns=housing_num.columns,
                          index=housing_num.index)




Scikit-learn의 설계 철학

- 일관성: 모든 object가 일관되고 단순한 interface를 공유함.

estimator (추정기): dataset을 기반으로 일련의 model parameters를 estimate하는 object. (ex. imputer object) estimation 자체는 fit()에 의해 수행되고 하나의 parameter로 하나의 dataset만 전달함. estimation 과정에서 필요한 다른 parameters는 모두 hyperparameter로 간주되고 (ex. imputer object의 strategy parameter) instance variable로 저장됨. 

transformer (변환기): dataset을 transform하는 estimator. transformation은 dataset을 parameter로 전달받은 transform()이 수행함. 그리고 transform된 dataset을 return 함. 이런 transformation은 일반적으로 imputer의 경우와 같이 학습된 model parameter에 의해 결정됨. 모든 transformer는 fit()과 transform()을 연달아 호출하는 것과 동일한 fit_transform()을 가지고 있음.

predictor (예측기): 일부 estimator는 주어진 dataset에 대해 prediction를 만들 수 있음. (ex. LinearRegression model) predictor의 predict()는 new dataset을 받아 이에 상응하는 prediction value를 return 함. 또한 test set를 사용해 prediction의 품질을 측정하는 score()를 가짐.

- 검사 기능: 모든 estimator의 hyperparameter는 public instance variable로 직접 접근할 수 있음. (ex. imputer.strategy) 모든 estimator의 학습된 model parameter도 접미사로 밑줄을 붙여 public instance variable로 제공됨. (ex. imputer.statistics_)

- class 남용 방지: dataset을 별도의 class가 아니라 NumPy array나 SciPy sparse(희소) matrix로 표현함. hyperparameter는 보통 python 문자열이나 숫자임.

- 조합성: 기존의 구성요소를 최대한 재사용함. (ex. 여러 transformer를 연결한 다음 마지막에 estimator 하나를 배치한 pipeline estimator를 쉽게 만들 수 있음)

- 합리적인 기본값: Scikit-learn은 일단 돌아가는 기본 system을 빠르게 만들 수 있도록 대부분의 parameter에 합리적인 기본값을 지정해둠.





Text와 categorical attributes 다루기

- Scikit-learn의 OrdinalEncoder: 각 value가 category를 나타냄

from sklearn.preprocessing import OrdinalEncoder
ordinal_encoder = OrdinalEncoder()
housing_cat_encoded = ordinal_encoder.fit_transform(housing_cat)
housing_cat_encoded[:10]

value의 크기에 대한 상관성이 없다는 문제가 있음. (ex. 0과 1이 0과 4보다 더 가까운 특성이라는 보장이 없음)

- Scikit-learn의 OneHotEncoder: categorical value를one-hot vector로 바꿔줌.

one-hot encoding: 한 특성만 1 (hot), 나머지는 0 (cold)

dummy attributes: new attributes

출력은 NumPy array가 아닌 SciPy sparse matrix임 => 이는 수천 개의 category가 있는 categorical attribute일 경우 더 효율적인데, 이런 특성을 one-hot encoding하면 column이 수천 개인 matrix로 변하고 각 row는 1이 하나뿐이고 그 외에는 모두 0으로 채워져 있음. 0을 모두 memory에 저장하는 것은 낭비이므로 sparse matrix는 0이 아닌 원소의 위치만 저장함. 이 matrix를 거의 일반적인 2차원 array처럼 사용할 수 있고, 만약 NumPy array로 바꾸려면 toarray()를 호출하면 됨.

만약 category 수가 많다면 one-hot encoding은 많은 수의 input feature를 만들어 훈련을 느리게 만듦. 이때 categorical input을 numerical feature로 바꾸고 싶을 것임. 또는 각 category를 embedding이라 부르는 학습 가능한 저차원 vector로 바꿀 수 있음. 이는 representation learning(표현 학습)의 한 예임.

from sklearn.preprocessing import OneHotEncoder
cat_encoder = OneHotEncoder()
housing_cat_1hot = cat_encoder.fit_transform(housing_cat)
housing_cat_1hot
housing_cat_1hot.toarray()





Transformer 만들기

- 특별한 정제 작업이나 어떤 특성들을 조합하는 등의 작업을 위해 자신만의 transformer를 만들어야 할 때가 있음 => 내가 만든 transformer를 Scikit-learn의 기능과 연동하고 싶을 것임. Scikit-learn은 duck typing을 지원하므로 fit(), transform(), fit_transform()을 구현한 python class를 만들면 됨.

- 마지막 method는 TransformerMixin을 상속하면 자동으로 생성됨. 또한 BaseEstimator를 상속하고 생성자에 *args나 **kargs를 사용하지 않으면 hyperparameter tuning에 필요한 get_params()와 set_params()를 추가로 얻게 됨. 

from sklearn.base import BaseEstimator, TransformerMixin

# 열 인덱스
rooms_ix, bedrooms_ix, population_ix, households_ix = 3, 4, 5, 6

class CombinedAttributesAdder(BaseEstimator, TransformerMixin):
    def __init__(self, add_bedrooms_per_room=True): # *args 또는 **kargs 없음
        self.add_bedrooms_per_room = add_bedrooms_per_room
    def fit(self, X, y=None):
        return self  # 아무것도 하지 않습니다
    def transform(self, X):
        rooms_per_household = X[:, rooms_ix] / X[:, households_ix]
        population_per_household = X[:, population_ix] / X[:, households_ix]
        if self.add_bedrooms_per_room:
            bedrooms_per_room = X[:, bedrooms_ix] / X[:, rooms_ix]
            return np.c_[X, rooms_per_household, population_per_household,
                         bedrooms_per_room]
        else:
            return np.c_[X, rooms_per_household, population_per_household]

attr_adder = CombinedAttributesAdder(add_bedrooms_per_room=False)
housing_extra_attribs = attr_adder.transform(housing.to_numpy())
이 경우 transformer가 add_bedromms_per_room hyperparameter 하나를 가지며 기본값을 true로 지정함. 이 특성을 추가하는 것이 도움이 될지 안 될지 이 hyperparameter로 쉽게 확인해볼 수 있음. 일반적으로 100% 확신이 없는 모든 data 준비 단계에 대해 hyperparameter를 추가할 수 있음. 이런 data 준비 단계를 자동화할수록 더 많은 조합을 자동으로 시도해볼 수 있고 최상의 조합을 찾을 가능성을 매우 높여줌.





Feature scaling

- Normalization (정규화) or min-max scaling: 값이 0~1 사이에 들도록 조정. Scikit-learn의 MinMaxScaler transformer 사용 가능.

- Standardization (표준화): 값의 분포가 평균이 0, 분산이 1이 되게 조정. Scikit-learn의 StandardScaler transformer 사용 가능.

- 모든 transformer에서 scaling은 전체 data가 아닌 training data에 대해서만 fit()을 적용해야 함. 이후 training set와 test set에 대해 transform()을 사용함.





Transformation pipeline

- transformation 단계가 많으면 정확한 순서대로 실행되어야 함 => pipeline class가 연속된 transformation을 순서대로 처리할 수 있도록 도와줌

from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler

num_pipeline = Pipeline([
        ('imputer', SimpleImputer(strategy="median")),
        ('attribs_adder', CombinedAttributesAdder()),
        ('std_scaler', StandardScaler()),
    ])

housing_num_tr = num_pipeline.fit_transform(housing_num)
- pipeline은 연속된 단계를 나타내는 이름/추정기 쌍의 목록을 input으로 받음

- 마지막 단계에는 transformer와 estimator를 모두 사용할 수 있고 그 외에는 모두 transformer여야 함. 즉, fit_transform()을 가지고 있어야 함.

- pipeline의 fit()을 호출하면 모든 transformer의 fit_transform()을 순서대로 호출하면서 한 단계의 output을 다음 단계의 parameter로 전달함. 마지막 단계에서는 fit()만 호출함.

- pipeline object는 마지막 estimator과 동일한 method를 제공함. 이 예에서는 마지막 estimator가 transformer StandardScaler이므로 pipeline이 data에 대해 모든 transformation을 순서대로 적용하는 transform()을 가지고 있음.





Model selection과 training

1. model을 selection 후 training (ex. LinearRegression, DecisionTreeRegressor)

2. model에 대한 성능 측정 (ex. RMSE)





Cross-validation을 사용한 평가

from sklearn.model_selection import cross_val_score
scores = cross_val_score(tree_reg, housing_prepared, housing_labels,
                         scoring="neg_mean_squared_error", cv=10)
tree_rmse_scores = np.sqrt(-scores)
- K-fold cross-validation: training set를 fold라 불리는 10개의 subset으로 randomly split. 이후 다음 decision tree model을 10번 train하고 evaluate하는데, 매번 다른 fold를 select해 evaluate에 사용하고 나머지 9개 fold는 train에 사용함. 10개의 evaluation score가 담긴 array가 result가 됨.





용어

- Ensemble Learning (앙상블 학습): 여러 다른 model을 모아 하나의 model을 만드는 것





Model fine-tuning

- Grid search: 만족할 만한 hyperparameter 조합을 찾을 때까지 수동으로 hyperparameter를 조정하는 것. Scikit-learn의 GridSearchCV 사용 가능. 탐색하고자 하는 hyperparameter와 시도해볼 값을 지정하기만 하면 됨. 그러면 가능한 모든 hyperparameter 조합에 대해 cross-validation을 사용해 evaluate함.

from sklearn.model_selection import GridSearchCV

param_grid = [
    # 12(=3×4)개의 하이퍼파라미터 조합을 시도합니다.
    {'n_estimators': [3, 10, 30], 'max_features': [2, 4, 6, 8]},
    # bootstrap은 False로 하고 6(=2×3)개의 조합을 시도합니다.
    {'bootstrap': [False], 'n_estimators': [3, 10], 'max_features': [2, 3, 4]},
  ]

forest_reg = RandomForestRegressor(random_state=42)
# 다섯 개의 폴드로 훈련하면 총 (12+6)*5=90번의 훈련이 일어납니다.
grid_search = GridSearchCV(forest_reg, param_grid, cv=5,
                           scoring='neg_mean_squared_error',
                           return_train_score=True)
grid_search.fit(housing_prepared, housing_labels)
- Random search: Grid search와 거의 같은 방식으로 사용하지만 가능한 모든 조합을 시도하는 대신 각 반복마다 hyperparameter에 임의의 수를 대입하여 지정한 횟수만큼 evaluation. hyperparameter의 탐색 공간이 커졌을 때 사용하면 좋음.

grid search에서는 hyperparameter마다 몇 개의 value만 탐색하지만, random search에서는 만약 100회 반복하도록 실행하면 hyperparameter마다 각기 다른 100개의 value를 탐색함.

단순히 반복 횟수를 조절하는 것만으로 hyperparameter 탐색에 투입할 computing 자원을 제어할 수 있음.

- Ensemble method (앙상블 방법): 최상의 model을 연결해보는 것

- 최상의 model과 error 분석: 최상의 model을 분석하면 문제에 대한 좋은 insight를 얻는 경우가 많음.

예를 들어 RandomForestRegressor가 정확한 prediction을 만들기 위해 각 특성의 상대적인 중요도를 알려줌.

- test set로 system 평가: test set에서 prediction value와 label을 얻은 후 full_pipeline을 사용해 data를 transformation하고 test set에서 최종 model을 evaluation함.

hyperparameter tuning을 많이 했다면 cross-validation을 사용해 측정한 것보다 조금 성능이 낮은 것이 보통임. 우리 system이 validation data에서 좋은 성능을 내도록 세밀하게 tuning되었기 때문에 new dataset에는 잘 작동하지 않을 가능성이 크기 때문임.





Launch

- 전체 전처리 pipeline과 prediction pipeline이 포함된 훈련된 Scikit-learn model을 save. 이후 이 훈련된 model을 상용 환경에서 load하고 predict()를 호출해 prediction을 만듦. user가 predict button을 누르면 이 data를 포함한 query가 web server로 전송되어 web application으로 전달될 것임. 결국 이 application code가 model의 predict()를 호출할 것임. (model을 사용할 때마다 load하지 않고 server가 시작할 때 model을 load하는 것이 좋음) 또는 web application이 REST API를 통해 질의할 수 있는 전용 web service로 model을 감쌀 수 있음. 이렇게 하면 주 application을 건드리지 않고 model을 new version으로 upgrade하기 쉬움. 필요한 만큼 web service를 시작하고 web application에서 web service로 오는 요청을 load balancing할 수 있기 때문에 규모를 확장하기도 쉬움. 또한 web application을 python이 아니라 다른 어떤 언어로도 작성할 수 있음.

- model을 Google cloud AI platform과 같은 cloud에 배포함. 이를 사용해 model을 저장하고 Google cloud strorage (GCS)에 업로드 함. 이후 Google cloud AI platform으로 이동해 new model version을 만들고 GCS file을 지정함. load balancing과 자동 확장을 처리하는 간단한 web service를 만들어줌. 입력 data를 담은 JSON 요청을 받고 prediction을 담은 JSON 응답을 return 함. 이제 web site에서 이 web service를 사용할 수 있음.





Monitor, Maintain

- 하위 system의 지표로 model 성능 추정 가능
