---
layout: single
title: "[Hands-On Machine Learning 1회독] Chapter 3. 분류"
categories: Hands-On-Machine-Learning-1st
tag: [python, Machine Learning]
toc: false
---

<br>
<br>
<br>

###### 2022.05.20.
###### Hands-On Machine Learning 2판
###### Chapter 3. 분류
###### 1회독 - 내용 단순 요약

<br>
<br>
<br>

### MNIST data

- data 살펴보기

```
from sklearn.datasets import fetch_openml

mnist = fetch_openml('mnist_784', version=1, as_frame=False)
X, y = mnist["data"], mnist["target"]
X.shape  # (70000, 784)가 나옴
y.shape  # (70000,)이 나옴
```

image가 70000개 있고, 각 image에는 784개의 feature가 있음. 이는 image가 28*28 pixel이기 때문임.

Each feature는 0 (white) ~ 255 (black) 까지의 pixel intensity(강도)를 represent함.

- data 출력

```
%matplotlib inline
import matplotlib as mpl
import matplotlib.pyplot as plt

some_digit = X[0]
some_digit_image = some_digit.reshape(28, 28)
plt.imshow(some_digit_image, cmap=mpl.cm.binary)
plt.axis("off")

plt.show()
```

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FRVClP%2FbtrCKWGW3IB%2FeTFg8lBfEqnYizLwAiZUZ1%2Fimg.png" width=150>


<br>


### Binary classifier (이진 분류기)

- two class를 distinguish(구분)

- classification을 위한 target vector 만들기: 숫자가 5인지 아닌지를 distinguish

```
y_train_5 = (y_train == 5)  # 숫자가 5이면 True
y_test_5 = (y_test == 5)  # 숫자가 5가 아니면 False
```

- Scikit-learn의 SGDClassifier class를 사용해 Stochastic Gradient Descent (SGD, 확률적 경사 하강법) classifier로 train

```
from sklearn.linear_model import SGDClassifier

sgd_clf = SGDClassifier(max_iter=1000, tol=1e-3, random_state=42)
sgd_clf.fit(X_train, y_train_5)
```

model과 data에 따라 다른데, SGD는 sample을 섞어서 train해야 하는 model임

SGD는 한 번에 하나씩 training sample을 독립적으로 처리함 => 큰 dataset을 효율적으로 처리한다는 장점이 있음


<br>


### Performance measures (성능 측정)

- classifier evaluating는 regressor evaluating보다 훨씬 어려움

- cross-validation을 사용한 accuracy measuring

```
from sklearn.model_selection import StratifiedKFold
from sklearn.base import clone

# shuffle=False가 기본값이기 때문에 random_state를 삭제하던지 shuffle=True로 지정하라는 경고가 발생합니다.
# 0.24버전부터는 에러가 발생할 예정이므로 향후 버전을 위해 shuffle=True을 지정합니다.
skfolds = StratifiedKFold(n_splits=3, random_state=42, shuffle=True)

for train_index, test_index in skfolds.split(X_train, y_train_5):
    clone_clf = clone(sgd_clf)
    X_train_folds = X_train[train_index]
    y_train_folds = y_train_5[train_index]
    X_test_fold = X_train[test_index]
    y_test_fold = y_train_5[test_index]

    clone_clf.fit(X_train_folds, y_train_folds)
    y_pred = clone_clf.predict(X_test_fold)
    n_correct = sum(y_pred == y_test_fold)
    print(n_correct / len(y_pred))
```

이때 accuracy는 95%가 나옴. 하지만 5인지 아닌지를 구분하는 binary classifier에서 전체 dataset의 5의 비율이 10%밖에 되지 않기 때문에 prediction 전체를 '5가 아니다'라고 하여도 90%의 accuracy가 나옴. 따라서 accuracy는 skewed dataset을 다룰 경우 (각 class 안의 data 수의 차이가 많이 나는 경우) classifier의 performance measure 지표로 사용하기에 문제가 있음.


<br>


### Confusion matrix (오차 행렬)

- class A의 instance가 class B로 잘못 classified된 횟수를 count (ex. 5를 3으로 잘못 classified한 횟수를 알고 싶음 => 5행 3열을 보면 됨)

- Scikit-learn의 confusion_matrix()를 사용해 만듦

```
from sklearn.metrics import confusion_matrix

confusion_matrix(y_train_5, y_train_pred)
```

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbAoDlI%2FbtrCMXSrM9p%2FvQD6DuF7kkWpKEvNpy2Xp1%2Fimg.png" width=250>

- row는 actual(실제) class, column은 predicted class를 나타냄 => correctly classified한 수는 53892 + 3530


<br>


### 용어

- True Positive (TP): 진짜 양성

- True Negative (TN): 진짜 음성

- False Positive (FP): 가짜 양성

- False Negative (FN): 가짜 음성

- TPR, TNR, FPR, FNR: TP, TN, FP, FN + Rate(비율)

- Precision (정밀도): 양성 예측의 정확도 => TP / (TP + FP)

- Sensitivity (민감도) or Recall (재현율) or TPR: 정확하게 감지한 양성 샘플의 비율 =>TP / (TP + FN)

- Specificity (특이도) or TNR

- FPR = FP / (FP + TN) = (FP + TN - TN) / (FP + TN) = 1 - (TN / (FP + TN)) = 1 - TNR

- AUC (Area Under the Curve): 곡선 아래의 면적


<br>


### F_1 Score

- precision과 recall의 harmonic(조화) mean

```
from sklearn.metrics import f1_score

f1_score(y_train_5, y_train_pred)
```

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F1NO9G%2FbtrCOKZCmOL%2FOpjvpprizwQoqZkqSkCitK%2Fimg.png" width=500>

precision과 recall이 비슷한 classifier에서 F_1 score가 높음. 하지만 상황에 따라 precision이 더 중요할 수도 있고 recall이 더 중요할 수도 있기 때문에 이것이 항상 바람직한 것은 아님.


<br>


### Precision/Recall Tradeoff

- precision을 올리면 recall이 줄고, recall을 올리면 prediction이 줄음

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmRv4V%2FbtrCJvJ3ECR%2FXkgkCjNbOqARiVrEAaPat1%2Fimg.png" width=600>

화살표의 위치가 5인지 아닌지를 decision하는 decision threshold라고 하였을 때, 화살표의 위치에 따라 다음과 같은 precision과 recall의 value가 나옴

- Scikit-learn의 precision_recall_curve()를 사용해 가능한 모든 threshold에 대해 precision과 recall을 계산할 수 있음

```
def plot_precision_recall_vs_threshold(precisions, recalls, thresholds):
    plt.plot(thresholds, precisions[:-1], "b--", label="Precision", linewidth=2)
    plt.plot(thresholds, recalls[:-1], "g-", label="Recall", linewidth=2)
    plt.legend(loc="center right", fontsize=16) # Not shown in the book
    plt.xlabel("Threshold", fontsize=16)        # Not shown
    plt.grid(True)                              # Not shown
    plt.axis([-50000, 50000, 0, 1])             # Not shown

recall_90_precision = recalls[np.argmax(precisions >= 0.90)]
threshold_90_precision = thresholds[np.argmax(precisions >= 0.90)]

plt.figure(figsize=(8, 4))                                                                  # Not shown
plot_precision_recall_vs_threshold(precisions, recalls, thresholds)
plt.plot([threshold_90_precision, threshold_90_precision], [0., 0.9], "r:")                 # Not shown
plt.plot([-50000, threshold_90_precision], [0.9, 0.9], "r:")                                # Not shown
plt.plot([-50000, threshold_90_precision], [recall_90_precision, recall_90_precision], "r:")# Not shown
plt.plot([threshold_90_precision], [0.9], "ro")                                             # Not shown
plt.plot([threshold_90_precision], [recall_90_precision], "ro")                             # Not shown
save_fig("precision_recall_vs_threshold_plot")                                              # Not shown
plt.show()
```

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FctuqZd%2FbtrCLADHN9m%2F9JSDFaJXU5D7HRm7Co6Lo1%2Fimg.png" width=500>
          
- recall에 대한 precision의 plot
          
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc1Rujb%2FbtrCJLLoaI3%2FtxC09sQUdW42r2yhFKKys1%2Fimg.png" width=400>

recall이 80% 근처일 때 precision이 급격하게 줄어들음. 이 하강점 직전을 precision/recall tradeoff로 select하는 것이 좋음. => 위의 plot에서는 recall이 60% 정도인 지점

- accuracy가 90%인 classifier 만들기

```
threshold_90_precision = thresholds[np.argmax(precisions >= 0.90)] # 3370.02 출력됨
y_train_pred_90 = (y_scores >= threshold_90_precision)
precision_score(y_train_5, y_train_pred_90) # prediction에 대한 precision: 0.90 출력됨
recall_score(y_train_5, y_train_pred_90) # prediction에 대한 recall: 0.48 출력됨
```


<br>


### ROC (Receiver Operating Characteristic, 수신기 조작 특성) curve

- FPR에 대한 TPR의 curve

```
from sklearn.metrics import roc_curve

fpr, tpr, thresholds = roc_curve(y_train_5, y_scores)
def plot_roc_curve(fpr, tpr, label=None):
    plt.plot(fpr, tpr, linewidth=2, label=label)
    plt.plot([0, 1], [0, 1], 'k--') # 대각 점선
    plt.axis([0, 1, 0, 1])                                    # Not shown in the book
    plt.xlabel('False Positive Rate (Fall-Out)', fontsize=16) # Not shown
    plt.ylabel('True Positive Rate (Recall)', fontsize=16)    # Not shown
    plt.grid(True)                                            # Not shown

plt.figure(figsize=(8, 6))                                    # Not shown
plot_roc_curve(fpr, tpr)
fpr_90 = fpr[np.argmax(tpr >= recall_90_precision)]           # Not shown
plt.plot([fpr_90, fpr_90], [0., recall_90_precision], "r:")   # Not shown
plt.plot([0.0, fpr_90], [recall_90_precision, recall_90_precision], "r:")  # Not shown
plt.plot([fpr_90], [recall_90_precision], "ro")               # Not shown
plt.show()
```

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbbKlFs%2FbtrCIlmQeC0%2FgFAaz6ZwesR2YOgOQYZXp0%2Fimg.png" width=450>

TPR이 높을수록 classifier가 만드는 FPR이 늘어남

dotted line(점선)은 purely(완전한) random classifier의 ROC curve를 repreesent함. 좋은 classifier는 dotted line에서 최대한 멀리 떨어져 있어야 함. AUC measure로도 classifiers를 compare할 수 있음. purely classifier는 ROC의 AUC가 1이고, purelyrandom classifier의 ROC의 AUC가 0.5임. 

- ROC curve와 PR (Precision/Recall) curve가 비슷해서 어떤 것을 사용할지 궁금할 수 있음. 일반적으로 positive class가 드물거나 FN보다 TP가 더 중요할 때 PR curve를 사용하고, 그렇지 않으면 ROC curve를 사용함.


<br>


### Multiclass classifier (다중 분류기) or Multinomial classifier (다항 분류기)

- SGD classifier, Random Forest classifier, naive Bayes classifier와 같은 일부 알고리즘은 multiple class를 handling할 수 있는 반면, SVM classifier, linear classifier와 같은 알고리즘은 binary classification만 가능함. 하지만 binary classifier를 여러 개 사용해 multiclass를 classification하는 기법도 많음.

- OvR (One-versus-the-Rest) or OvA (One-versus-All): 각 classifier의 decision score 중에 가장 높은 것을 class로 select함

- OvO (One-versus-One): 0과 1을, 0과 2를, 1과 2를 distinguish하는 것과 같이 각 조합마다 binary classifier를 train함. class가 N개라면 classifier는 N*(N-1)/2 개가 필요함. Each classifier의 trian에 전체 training set 중 distinguish할 two class의 sample만 필요하다는 장점이 있음.

- SVM 같은 일부 알고리즘은 training set의 size에 민감해서 large training set에서 few classifier를 train하는 것보다 small training set에서 많은 classifier를 train하는 쪽이 빠르므로 OvO를 선호함. 하지만 대부분의 binary classification 알고리즘에서는 OvR을 선호함.

- multiclass classification에서 binary classification 알고리즘을 선택하면 Scikit-learn이 알고리즘에 따라 자동으로 OvR 또는 OvO를 실행함.


<br>


### Linear classifier

- each pixel에 weight를 assign 후 new image에 대해 단순히 pixel intensity의 weight sum을 class score로 계산 (ex. SGDClassifier)


<br>


### Multilabel classification (다중 레이블 분류)

- target label이 여러 개임 => 여러 개의 target label이 담긴 y_multilabel array를 만듦


<br>


### Multioutput-multiclass classification (다중 출력 다중 클래스 분류) or Multioutput classification

- multilabel classification에서 each label이 multiclass가 될 수 있도록 generalization한 것

- ex.  MNIST image는 classifier의 output이 multilabel이고 (one label per pixel) each label은 value를 여러 개 가짐 (0~255인 pixel intensity)

