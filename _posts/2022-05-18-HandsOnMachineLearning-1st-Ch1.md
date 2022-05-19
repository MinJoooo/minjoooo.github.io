---
layout: single
title: "[Hands-On Machine Learning 1회독] Chapter 1. 한눈에 보는 머신러닝"
categories: Hands-On-1st
tag: [python, Machine Learning]
toc: false
---

<br>
<br>
<br>

###### 2022.05.18.
###### Hands-On Machine Learning 2판
###### Chapter 1. 한눈에 보는 머신러닝
###### 1회독 - 내용 단순 요약

<br>
<br>
<br>

### Machine learning

- 명시적인 규칙을 coding하지 않고 기계가 data로부터 학습하여 어떤 작업을 더 잘하도록 만드는 연구 분야

- 대부분의 machine learning 작업은 예측을 만드는 것. 주어진 training data로 학습하고 training data에서는 본 적 없는 new data에서 좋은 예측을 만드는 것.


<br>


### Machine learning project의 형태

1. data 분석

2. model 선택

3. training data로 model을 훈련시킴

4. Inference (추론): new data에 model을 적용해 예측함. 이 model이 잘 generalize(일반화)되기를 기대함.


<br>


### 용어

- training set: system이 학습하는 데 사용하는 sample

- training instance or sample: 각 training data


<br>


### Data Mining

- machine learning 기술을 적용해서 대용량의 data를 분석하여 겉으로는 보이지 않던 pattern을 발견하는 것


<br>


### Machine learning을 사용하기에 좋은 분야

- 전통적인 방법으로는 해결 방법이 없는 복잡한 문제

- 기존 solution으로는 많은 수동 조정과 규칙이 필요한 문제 => 하나의 machine learning model이 code를 간단하게 만들고 전통적인 방법보다 더 잘 수행되도록 도와줌.


<br>


### Machine learning system의 종류

- 지도/비지도/준지도/강화 학습: 사람의 감독하에 훈련하는 것인지 아닌지

- 배치/온라인 학습: 실시간으로 점진적인 학습을 하는지 아닌지. 입력 data의 stream부터 점진적으로 학습할 수 있는지.

- 사례기반/모델기반 학습: 어떻게 generalize 되는지. 단순하게 알고있는 data point와 new data point를 비교하는 것인지 아니면 과학자들이 하는 것처럼 training data set에서 pattern을 발견하여 예측 model을 만드는지.


<br>


### Supervised learning (지도 학습)

- 알고리즘에 주입하는 training data에 label이라는 원하는 답이 포함됨.

- Classification(분류)

- Regression(회귀): predictor variable(예측 변수)이라 부르는 feature(특성)을 사용해 target(타깃) 수치를 예측하는 것

- 일부 regression 알고리즘을 classification에 사용할 수 있고, 일부 classification 알고리즘을 regression에 사용할 수 있음. (ex. logistic regression은 class에 속할 확률을 출력함 (ex. 이 메일이 스팸일 가능성이 20%))


<br>


### Supervised learning 알고리즘

- K-nearest neighbors (K-최근법 이웃)

- Linear regression (선형 회귀)

- Logistic regression

- Support Vector Machine (SVM)

- Decision Tree (결정 트리), Random forest

- Neural Networks (NN, 신경망)

- AutoEncoder(오토인코더)나 Restricted Boltzmann machine(제한된 볼츠만 머신)과 같은 일부 NN 구조는 unsupervised learning일 수 있음.

- NN 또한 Deep belief networks(심층 신뢰 신경망)나 unsupervised pretraining(비지도 사전훈련)처럼 semisupervised learning일 수도 있음.


<br>


### Unsupervised learning (비지도 학습)

- training data에 label이 없음

- Dimensionality reduction(차원 축소): 너무 많은 정보를 잃지 않으면서 data를 간소화 (ex. feature extraction(특성 추출): 상관관계가 있는 여러 feature을 하나로 합침)

- Association rule learning (연관 규칙 학습): 대량의 data에서 feature 간의 흥미로운 관계를 찾음


<br>


### Unsupervised learning 알고리즘 - 1. Clustering (군집)

- K-means

- DBSCAN

- Hierachical cluster analysis (HCA, 계층 군집 분석)

- Outlier detection (이상치 탐지), Novelty detection (특이치 탐지)

- One-class

- Isolation forest


<br>


### Unsupervised learning 알고리즘 - 2. Visualization (시각화), Dimensionality reduction (차원 축소)

- Principal component analysis (PCA, 주성분분석)

- Kernel PCA

- Locally-linear embedding (LLE, 지역적 선형 임베딩)

- t-distributed stochastic neighbor embedding (t-SNE)


<br>


### Unsupervised learning 알고리즘 - 3. Association rule learning (연관 규칙 학습)

- Apriori

- Eclat


<br>


### Semisupervised learning (준지도 학습)

- 일부만 label이 있음.

- ex. 가족사진 data에서 하나의 사진에만 label을 추가해도 사진에 있는 모든 사람의 이름을 알 수 있음.

- 대부분의 semisupervised learning 알고리즘은 supervised learning과 unsupervised learning의 조합으로 이루어져 있음. (ex. Deep belief network(DBN, 심층 신뢰 신경망)는 여러 겹으로 쌓은 Restricted Blotzmann machine이라 불리는 unsupervised learning에 기초함)


<br>


### 용어

- 속성: attribute. data type. column name.

- 특성: 일반적으로 '속성+값'. 하지만 문맥에 따라 여러 의미를 가짐.

- 많은 사람이 속성과 특성을 구분하지 않고 사용하고 있음.


<br>


### Reinforcement learning (강화 학습)

- Agent: 학습하는 system

- Environment(환경)을 관찰해서 Action(행동)을 실행하고 그 결과로 Reward(보상) 또는 Penalty(벌점)을 받음.

- Policy(정책): 주어진 상황에서 agent가 어떤 action을 선택할지 정의. 시간이 지나면서 가장 큰 reward를 얻기 위해 policy라 부르는 최상의 전략을 스스로 학습함.

- ex. Deep Mind의 AlphaGo


<br>


### Batch learning (배치 학습)

- system이 점진적으로 학습할 수 없음. 가용한 data를 모두 사용해 훈련시켜야 함.

- 일반적으로 시간과 자원을 많이 소모하기 때문에 보통 offline에서 수행됨.

- Offline learning (오프라인 학습): system을 훈련시키고 제품 system에 적용하면 더 이상 학습없이 실행됨. 즉, 학습한 것을 단지 적용만 함.

- Batch learning system이 new data에 대해 학습하려면 전체 data를 사용하여 system의 새로운 버전을 처음부터 다시 훈련해야 함.


<br>


### Online learning (온라인 학습)

- data를 순차적으로 한 개씩 또는 mini batch라 부르는 작은 묶음 단위로 주입하여 system을 훈련시킴.

- 매 학습 단계가 빠르고 비용이 적게 들어 system은 data가 도착하는 대로 즉시 학습할 수 있음.

- 연속적으로 data를 받고 빠른 변화에 스스로 적응해야 하는 system에 적합함.

- new sample data를 학습하면 학습이 끝난 data는 더 필요하지 않으므로 (재사용할 생각이 없다면) 버림 => 많은 공간 절약 가능

- Out-of-core (외부 메모리) 학습: 컴퓨터 한 대의 main memory에 들어갈 수 없는 아주 큰 data set를 학습하는 system에도 online 학습 알고리즘을 사용할 수 있음. (Out-of-core 학습은 보통 offline으로 실행되어 online 학습이라는 이름이 혼란을 줄 수 있음. 따라서 Incremental learning(점진적 학습)이라 생각하면 편함.)

- 가장 큰 문제점은 system에 나쁜 data가 주입되었을 때 system 성능이 점진적으로 감소한다는 것 => system을 면밀히 모니터링하고 성능 감소가 감지되면 즉각 학습을 중지시켜야 하고, 가능하면 이전 운영 상태로 되돌려야 함. 입력 data를 모니터링해서 비정상 data를 잡아낼 수도 있음. (outlier 알고리즘 사용)


<br>


### Learning rate (학습률)

- 변화하는 data에 얼마나 빠르게 적응할 것인지

- online learning system에서 중요한 parameter 중 하나임

- learning rate를 높게 함 => system이 data에 빠르게 적응하지만 예전 data는 금방 잊어버림.

- learning rate를 낮게 함 => system의 관성이 더 커져서 더 느리게 학습됨. 하지만 new data에 있는 잡음이나 대표성 없는 data point에 덜 민감해짐.


<br>


### Instance-based learning (사례기반 학습)

- system이 training sample을 기억함으로써 학습

- similarity measure (유사도 측정) => new data와 학습한 sample을 비교하는 식으로 generalize


<br>


Model-based learning (모델기반 학습)

- training set에 model을 맞추기 위해 model parameter를 조정 => new data에서도 좋은 예측을 만들거라 기대


<br>


### Model의 성능 측정 지표

- model이 얼마나 좋은지 측정: Utility function (효용 함수), Fitness Function (적합도 함수)

- model이 얼마나 나쁜지 측정: Cost function (비용 함수)


<br>


### 용어

- sampling noise: sample이 작을 때 생김. 우연에 의한 대표성 없는 data.

- sampling bias: sample이 클 때 생김. 표본 추출 방법이 잘못되어 대표성을 띠지 못하는 것.


<br>


### Hyperparameter

- model이 아닌 학습 알고리즘의 parameter

- 학습하는 동안 적용할 규제의 양을 결정함 =>machine learning system을 구축할 때 hyperparameter tuning이 매우 중요함.

- 학습 알고리즘으로부터 영향을 받지 않으며, training 전에 미리 지정되고, training하는 동안에는 상수로 남아있음.

- 규제 hyperparameter를 매우 큰 값으로 지정 => 기울기가 0에 가가운 거의 평편한 model을 얻게 됨. overfitting 될 가능성은 거의 없지만 좋은 model을 찾지 못함.


<br>


### Underfitting

- model이 너무 단순해서 data의 내재된 구조를 학습하지 못할 때 일어남.

- 해결방법: model parameter가 더 많은 강력한 model 선택, 학습 알고리즘에 더 좋은 feature 제공, model의 constraint를 줄임 (ex. 규제 hyperparameter 감소)


<br>


### Test와 Validation

- Generalization error (일반화 오차) or Out-of-sample error (외부 샘플 오차): new sample에 대한 오류 비율. test set에서 model을 평가함으로써 이 error에 대한 estimation(추정값)을 얻음. 이 값은 이전에 본 적 없는 new sample에 model이 얼마나 잘 작동할지를 알려줌.

- training set에서 model의 error가 적지만 generalization error가 높음 => model이 overfitting 되어있다는 뜻임.

- model을 training set에서 훈련한 후 train-dev set(훈련-개발 세트)에서 평가 => model이 잘 작동하면 overfitting된 것이 아님. 하지만 validation set에서 나쁜 성능을 낸다면 data 불일치 문제가 있는 것임. 


<br>


### Holdout validation (홀드아웃 검증)

- overfitting을 피하기 위한 일반적인 해결방법

- validation set (검증 세트) or development set (dev set, 개발 세트): training set의 일부를 떼어낸 것

- validation set를 이용해 여러 후보 model을 평가하고 가장 좋은 model을 선택함


<br>


### Cross-validation (교차 검증)

- training set가 적을 때 수행

- 작은 validation set를 여러 개 사용해 반복적으로 검증을 수행


<br>


### No free lunch (NFL, 공짜 점심 없음 이론)

- 경험하기 전에 더 잘 맞을 거라고 보장할 수 있는 model은 없다. 어떤 model이 최선인지 확실히 아는 유일한 방법은 모든 model을 평가해보는 것뿐이다.

- 하지만 이것이 불가능하기 때문에 실전에서는 data에 관해 타당한 가정을 하고 적절한 model 몇 가지만 평가함.

<br>
