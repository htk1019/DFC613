## 13강. 팩터와 머신러닝의 조합

### 소개

머신러닝을 공부하기로 결심하고 일반적인 권장 사항을 따라 시작하면 일단 선형대수와 다변수 미적분을 배우고 통계학을 배워야 합니다. 원래 대학때 기초과정을 이수한 사람이면 모르겠지만 대부분의 직장인들은 그냥 포기할 가능성이 높습니다.

머신 러닝의 기초가 되는 수학에 이미 익숙하거나 머신 러닝을 배우는 데 흥미가 있다면 이 접근 방식이 적합할 것입니다. 하지만 머신 러닝으로 무언가를 만들고자 하는 것이 주된 목적이라면 이 방법이 적합하지 않을 수도 있습니다.

코딩을 배울 때는 헬로 월드를 고급 언어로 작성하는 것으로 시작했을 가능성이 높습니다. 그런 다음 프로젝트에 약간의 복잡성을 도입하면서 필요에 따라 점점 더 로우 레벨, 즉 근본에 가까운 프로그래밍을 배웠을 것입니다. 직장인이 머신러닝을 배워가는 방법으로도 이 방식이 더 낫습니다.
연구보다 구축에 더 관심이 있다면 이러한 방식으로 ML 학습에 접근해야 합니다.

머신 러닝으로 소프트웨어를 구축하는 것부터 시작하고, 도구, 사전 학습된 모델, ML 프레임워크가 기본 ML 이론을 나중에 챙기시는게 ML을 포기하지 않게 할것입니다. 그런 다음 호기심이 생기거나 프로젝트에 더 많은 복잡성이 필요한 경우 더 자세히 살펴보고 모든 것이 어떻게 작동하는지 
확인하세요.

이번 수업부터 다룰 내용은 다음의 워크플로우 중에서 Algos에 들어가는 각종 머신러닝 모델들을 적용해보고 그 결과를 확인하는 것을 내용으로 합니다.

![](/pic/2023-09-06-10-24-24.png)

*그림1. 머신러닝을 사용한 팩터 투자전략 포트폴리오 구축 워크플로우*

### 데이터 소개

본 실습을 위해 사용할 데이터는 한국,미국,일본에 상장된 2,395개 주식들의 정보입니다. 기간은 2002년 11월 30일 부터 2023년 7월 31일까지입니다.
월간 데이터로 249개의 시점이 있습니다. 각 시점마다 56개의 팩터가 존재합니다.
데이터에는 62개의 열과 311,375개의 행이 있습니다.

사용된 팩터는 다음과 같습니다.
```
1	Research & Development / MarketValue
3	Average Daily Trading Volume over 6 months  / number of shares
4	Close Price / 52 week low price
6	RSI with excess return over market index for 4 months
9	Sales / Price
10	EPS / Price
11	BPS / Price
13	accumulated return over stdev of difference between trend and price
28	1 Month Coefficient of Variation of Volume to Price
29	36 Months Return Volatiliy(Inverse)
30	12 Months Return
31	6 Months Return
38	Non-Operating Interest Income  YoY Percent Chg
39	Income Taxes  YoY Percent Chg
40	Cash & Short Term Investments  YoY Percent Chg
41	Cash - Generic  YoY Percent Chg
46	Property, Plant And Equipment - Gross  YoY Percent Chg
47	Accumulated Depreciation  YoY Percent Chg
51	recency adjustment prev 90 days
52	Inverse value of Max Daily return last 60 days
55	Information Concentration 365 days
56	Gross Income / Total Asset
58	Research & Development / Total Assets
63	EBITDA / Enterprise value
65	Capex to Sales - Average value of Caped Sales for 3 years
67	We calculate cumulative abnormal stock return (Abr) around the latest quarterly earnings announcement date
69	Cash flow / price
71	Operating Cash Flow-to-price
73	Inverse value of Composite Equity Issuance
74	Inverse value of Inventory Changes
77	Cash Based Operating Income / total assets
78	Pretax income / Net Income
79	cumulating annual R&D expenses over the past five years / total Assets
80	operating costs scaled by total assets
83	Stock Daily return skew for prev 1 month
84	Trading Volume 6 Months Momentum
90	ungdroo price pattern factor
94	Price To Target Price Ratio
96	Funds from operation / marketvalue
103	Current Month Trading Volume / Max Value of 1 Year Trading Volume
105	Net Cash % Market Cap
106	MDD for 1 year
114	Difference between price and 365  days moving average
1001 Piotroski F-Score (out of 9)
1002 Benjamin Graham Score(out of 7)
1003 Peter Lynch Score(out of 10)
1004 David Dreman Score(out of 10)
1005 Martin Zweig Score(out of 11)
1006 Ken Fisher Score(out of 7)
1007 Boolio momentum score(out of 8)
1008 Boolio ValueMomentum score(out of 14)
1009 Boolio ValueGrowth score(out of 17)
1010 Boolio GrowthMomentum score(out of 12)
1011 Boolio Balance score(out of 16)
1012 Boolio ShareHolderReturn score(out of 9)
1013 Boolio Growth score(out of 11)
```

데이터 세트에는 R1M_USD, R3M_USD라는 2개의 컬럼이 있으며 이는 주식의 1개월, 3개월 미래/선도 수익률에 해당합니다. 수익은 총 수익입니다. 즉 해당 기간동안의 배당금 지급이 포함됩니다. 예측의 목표가 되는 이 라벨 데이터는 데이터 세트의 마지막 2개 열에 있습니다. 

### 데이터 전처리 ###

머신러닝의 지도 학습에서는 특정 팩터가 어떻게 종속 변수, 즉 예측하고자 하는 목표 변수에 영향을 미치는지 알아보기 위해 데이터의 형태를 변형합니다. 
변형방법은 목적에 따라 다양합니다.

 하나의 특징 X만을 가지고 있고, Y=f(X)+error 형태의 모델을 찾고자 한다고 가정해 봅시다. 여기서 변수들은 모두 실수값입니다. 이때, 평균 제곱 오차, 즉 실제 값과 예측 값의 차이의 제곱의 평균을 최소화하는 함수 f는 '회귀 함수'라고 알려져 있습니다. 회귀분석이 그것입니다.

이 함수는 f(x)=E[Y|X=x]로 표현됩니다.

만약 종속변수 Y가 한 달 후의 수익률이고 예측 변수로 B/P와 R&D/MarketCap을 사용한다고 할 때 두 예측 변수는 '균일화' 되어야 이 회귀식의 예측력이 상승합니다.
어떻게 균일화 해서 사용할지는 사용하고자 하는 모델링 방법에 달려 있습니다.

일단 데이터를 불러봅니다.
```python
import os
import pandas as pd

data_ml = pd.read_csv('data_ml.csv')
```
**1) Missing Data 처리**

누락된 데이터 처리 방법은 주로 '제거'와 '대체' 두 가지가 있습니다. '제거'는 간단하지만 많은 데이터를 잃게 되어 정보의 손실이 많습니다. 반면,'대체'는 종종 선호되지만 정확하게 대체하기가 어렵습니다.

그럼에도 정보의 손실이 지나치게 많다면 대체하기를 추천합니다.
대체방법에는 다음이 있습니다.

 - 중앙값이나 평균값을 채워넣기
 - 시계열 데이터라면 직전 데이터를 채워넣기
 - 모델링을 통해 추정값을 채워 넣기

**2) 이상치(oulier)**

이상치 검출에 대한 여러 연구와 전문 서적이 존재합니다. 매우 정교한 방법들은 많은 노력을 필요로 합니다만 실무에서는 간단한 휴리스틱 방법들 정도가 사용됩니다.
예를들어 가장 많이 사용하는 방법은 특정한 특성에 대해, 평균에서 표준편차의 m배 만큼 벗어난 데이터를 아웃라이어로 간주하는 것입니다. m은 일반적으로 3,5,10 중 하나의 값을 가집니다. 또는 가장 큰 값이 두번째로 큰 값의 m배 보다 크다면 그것 역시 아웃라이어로 분류합니다.

### 피처 엔지니어링(Feature Engineering)

포트폴리오 구성 과정에서 피처 엔지니어링은 매우 중요한 단계입니다. 컴퓨터 과학자들은 종종 "쓰레기 입력, 쓰레기 출력"이라는 말을 인용합니다. 

그러므로, 부적절하게 설계된 변수들로 ML 엔진을 학습시키는 것을 방지하는 것이 중요합니다. 이 주제에 대한 Kuhn과 Johnson(2019)의 최근 연구를 참고하시기 바랍니다. (더 짧은) 학술적인 참고문헌은 Guyon과 Elisseeff(2003)입니다.

#### 피처 선택

첫 번째 단계는 피처 선택입니다. 많은 수의 예측 변수들이 주어졌을 때 선택하는 간단한 방법은 다음과 같습니다.

- 팩터들 간의 상관관계를 파악해서 (0.7이 일반적인 값) 높은 값이 없는지 확인하기
- 선형 회귀를 수행해서 유의하지 않은 변수들은 사용하지 않기
- 팩터들에 클러스터링 분석을 수행하여 그중에서 대표적인 하나의 팩터만 유지하기

이런 방법들 외에도 주성분 분석을 사용하여 주성분을 입력변수로 쓰는 경우도 있고 트리를 적합시켜서 중요도가 높은 피처들만 유지하게 하는 방법도 있습니다.

#### 예측 변수의 스케일링

데이터를 사전 처리할때 우리는 금융 데이터에 대한 기본 상식으로 접근해야 합니다.

- 일간 수익률은 대부분 100%보다 작다.
- 주식의 변동성은 일발적으로 5%~80% 사이이다.
- 시가 총액은 백만 단위 또는 10억 단위로 표시된다.
- 재무값도 각자의 단위가 있다.
- 재무비율은 불균일하게 분포될 수 있다.
- Sentiment, Technical 팩터들도 그들만의 특성이 존재한다.

위의 이유로 이상한 값들은 제거하고 남은 데이터들은 정규화(Z-Score, MinMax) 같은 스케일링을 수행합니다.

대표적인 스케일링 방법은 다음과 같습니다.

![](/pic/2023-08-30-14-20-01.png)

때로는, 큰 값(시가 총액)과 큰 이상치를 가진 변수들에 대해 로그 변환을 적용할 수 있습니다. 스케일링은 이 변환 후에 수행될 수 있습니다. 당연히, 이 기법은 음수 값을 가진 피처들에 대해서는 금지됩니다.

#### 레이블링

팩터 투자에 일반적으로 사용되는 레이블은 다음과 같습니다.

- 수익률
- 미래 상대 수익률(특정 벤치마크 대비 초과 수익률)
- 지정된 임계값 이상의 수익률
- 카테고리 레이블 : 초과성과 달성 여부에 대한 바이너리 변수
- 위험조정수익률

시장 상승기에는 많은 자산이 플러스 수익률을 보이고, 폭락기에는 마이너스 수익률을 보입니다. 이는 불균형한 데이터 분포를 만들어 냅니다. 수익률을 중앙값과 비교하여 둘로 나누면 이 문제가 해결됩니다. 그러나 이런 선택 사항 역시 완벽한 해답은 아닙니다.

**카테고리 레이블**

주식 수익률 예측 모델에서 보편적인 레이블링 방식은 다음과 같습니다.

$$
\begin{equation}
y_{t,i}=

\left\{
    \begin{array}{rll}
        -1 & \text{ if } & \hat{r}_{t,i} < r_- \\
        0 & \text{ if } & \hat{r}_{t,i} \in [r_-,r_+] \\
        +1 & \text{ if } & \hat{r}_{t,i} > r_+ \\
    \end{array} 
\right.

\end{equation}
$$

여기서 $\hat{r}_{t,i}$는 수익률이고, $r_{\pm}$은 임계값입니다. 예측 수익률이 $r_-$ 미만이면 -1, $r_+$ 이상이면 +1, 중간이면 0 으로 분류합니다.

임계값 $r_\pm$은 세 범주가 상대적으로 균형을 이루도록, 즉 비슷한 수의 인스턴스를 갖도록 선택하는 것이 좋습니다.

**수익률 계산 기간**

예측하고자 하는 주식의 수익률 기간을 설정하는 것은 굉장히 중요한 이슈입니다. Jegadeesh와 Titman (1993)에 의한 모멘텀에 관한 기초 학술 논문에서 저자들은 지난 J = 3; 6; 9; 12개월 동안의 수익률을 기반으로 포트폴리오의 수익성을 계산합니다. 무엇이 최적인지에 대한 정답은 역시나 없습니다. 다만 팩터 기반의 모델링에서 예측하고자 하는 수익률 기간은 최소 1개월 이상이 적절하고 12개월 이상은 너무 길다고 여겨집니다.

### Common supervised algorithms

다양한 지도학습 기반의 예측 모델을 소개하고 사용해 보겠습니다.

#### 1. Penalized regressions(LASSO regressions)

패널티 회귀분석은 기본 회귀 모델에 제약 조건을 추가하여 모델의 복잡성을 제한하고 과적합을 방지하는 방법입니다. 이러한 방법 중에서 LASSO (Least Absolute Shrinkage and Selection Operator)는 가장 널리 사용되는 패널티 회귀분석 중 하나입니다.

LASSO 회귀는 선형 회귀의 일종이지만, 계수의 절대값에 대한 제약 조건을 추가하여 일부 계수를 정확히 0으로 만듭니다. 이는 변수 선택의 효과를 가지며, 결과적으로 모델이 더 간단하고 해석하기 쉬워집니다.

LASSO의 주요 특징은 다음과 같습니다

1. 변수 선택: LASSO는 변수 선택을 자동으로 수행합니다. 이는 모델에 포함되는 특징의 수를 줄이고, 불필요한 특징을 제거하여 모델을 단순화하는 데 도움이 됩니다.
2. 과적합 방지: 패널티를 사용하여 모델의 복잡성을 제한함으로써 과적합을 방지합니다. 이는 특히 특징의 수가 관측치보다 많은 경우나 다중 공선성이 있는 경우에 유용합니다.
3. 정규화 파라미터: LASSO에는 정규화 파라미터 λ가 있습니다. λ 값이 크면 더 많은 계수가 0이 되며, λ 값이 작으면 더 많은 계수가 모델에 포함됩니다. 적절한 λ 값을 선택하는 것은 중요하며, 교차 검증을 사용하여 최적의 값을 찾을 수 있습니다.

LASSO 회귀와 일반적인 선형 회귀 (Simple Regression) 모두 선형 모델을 기반으로 하며, 데이터에 대한 선형 관계를 학습하는 데 목적이 있습니다. 그러나 두 모델 간에는 중요한 차이점이 있습니다:

정규화:

- Simple Regression: 정규화 없이 데이터의 선형 관계만을 학습합니다.
- LASSO: L1 정규화를 포함하여, 특성 간의 관계와 특성의 중요도를 함께 학습합니다. 이는 특성 선택의 효과를 가져올 수 있습니다.

특성 선택

- Simple Regression: 모든 특성을 사용하여 모델을 학습합니다.
- LASSO: 정규화 덕분에 불필요한 특성의 계수를 0으로 만들어, 특성 선택의 효과를 얻을 수 있습니다.

다중공선성
- Simple Regression: 다중공선성이 있는 특성이 포함되면 모델의 성능에 문제가 발생할 수 있습니다.
- LASSO: 다중공선성 문제를 완화하는 데 도움을 줍니다.

과적합

- Simple Regression: 큰 데이터셋이나 복잡한 특성 구조에서 과적합의 위험이 있습니다.
- LASSO: 정규화로 인해 과적합을 방지하는 데 도움을 줍니다.

두 모델의 기본 목적은 데이터의 선형 관계를 학습하는 것으로 유사합니다.
얼핏 보기에는 LASSO가 더 우월해 보이지만 현실에서는 사용 목적에 달려있는 것이지 우월관계는 없습니다.

**Lasso Regression 실습**

한국 주식을 대상으로 예측 모델링울 수행합니다.
예측대상(종속변수)은 미래 1개월 수익률입니다.

```python

from sklearn.linear_model import LassoCV
from matplotlib import pyplot as plt
import seaborn as sns
import os
import pandas as pd
from sklearn.preprocessing import MinMaxScaler
from sklearn.linear_model import Lasso
import numpy as np
from sklearn.metrics import mean_squared_error, r2_score

# Function to apply Min-Max Scaling for each group
def min_max_scale_group(group):
    scaler = MinMaxScaler()
    group_scaled = scaler.fit_transform(group)
    return pd.DataFrame(group_scaled, columns=group.columns, index=group.index)

# 현재 작업중인 파일로 옮기기
data_ml = pd.read_csv('data_ml.csv')

# 'region' 열이 'KR'인 행만 추출
data = data_ml[data_ml['region'] == 'KR']

#가능한 모든 입력변수를 가져온다.
f_columns = [col for col in data.columns if col.startswith('f')]

#결측치가 있는경우 해당 날짜에서 가장 작은 숫자를 입력한다.
for col in f_columns:
    data[col].fillna(data.groupby('date_')[col].transform('mean'), inplace=True)

# Apply Min-Max Scaling for f_columns for each date_ group
data[f_columns] = data.groupby('date_')[f_columns].apply(min_max_scale_group)
# Apply Min-Max Scaling for R1M_USD
data['R1M_USD_UNIFORM'] = data.groupby('date_')['R1M_USD'].transform(lambda x: (x - x.min()) / (x.max() - x.min()))

#data_ml.to_csv('data_ml_kr.csv', index=False)
y_penalized = data['R1M_USD_UNIFORM'].values # Dependentvariable
X_penalized = data[f_columns].values # Predictors

alphas = np.arange(1e-4,1.0e-3,1e-5)

lasso_res = {}
for alpha in alphas: # looping through different alpha/lambda values
    lasso = Lasso(alpha=alpha) # model
    lasso.fit(X_penalized,y_penalized)
    lasso_res[alpha] = lasso.coef_ # extractLASSOcoefs

df_lasso_res = pd.DataFrame.from_dict(lasso_res).T
df_lasso_res.columns = f_columns # addingthenamesofthefactors

# 유의미한 팩터로 보이는 피처 선택 0.05는 자의적으로 결정했습니다.
predictors = (df_lasso_res.abs().sum() > 0.05)
selected_features = predictors[predictors].index.tolist()

# Convert 'date_' column to datetime format (if not already in that format)
data['date_'] = pd.to_datetime(data['date_'])

# Split data based on the date
train_data = data[data['date_'] <= '2015-12-31']
test_data = data[data['date_'] > '2015-12-31']

# Separate selected features and target variable
X_train = train_data[selected_features]
y_train = train_data['R1M_USD_UNIFORM']
X_test = test_data[selected_features]
y_test = test_data['R1M_USD_UNIFORM']

lasso_cv = LassoCV(alphas=None, cv=10, max_iter=10000)
lasso_cv.fit(X_train, y_train)
optimal_alpha = lasso_cv.alpha_

# Train a LASSO Regression model using the selected predictors
lasso_selected = Lasso(alpha=optimal_alpha)
lasso_selected.fit(X_train, y_train)

# Predict on the test data
y_pred_selected = lasso_selected.predict(X_test)
test_data['predicted_R1M_USD_UNIFORM'] = y_pred_selected

# Calculate MSE, RMSE, and R^2 score
mse_selected = mean_squared_error(y_test, y_pred_selected)
rmse_selected = np.sqrt(mse_selected)
r2_selected = r2_score(y_test, y_pred_selected)

mse_selected, rmse_selected, r2_selected

```
```
(0.03174977739800619, 0.17818467217470246, 5.712491420639676e-05)
```

해석력($R^2$를 기준) 이 높지 않게 나왔습니다.

그렇지만 상대적으로 높은 예측값을 가진 주식들의 수익률이 더 좋았다면 부족한 가운에서도 유용성이 어느정도 있다고 판단할 수 있을텐데요. 한번 확인해 보겠습니다.

위 코드에서 계속

```python
backtest_data = test_data[['date_','infocode','R1M_USD','predicted_R1M_USD_UNIFORM']]

# 날짜별로 상위 20%와 하위 20%를 구분
top_20 = backtest_data.groupby('date_').apply(lambda x: x.nlargest(int(len(x)*0.2), 'predicted_R1M_USD_UNIFORM'))
bottom_20 = backtest_data.groupby('date_').apply(lambda x: x.nsmallest(int(len(x)*0.2), 'predicted_R1M_USD_UNIFORM'))

# date_를 인덱스에서 제거합니다.
top_20 = top_20.reset_index(level=0, drop=True)
bottom_20 = bottom_20.reset_index(level=0, drop=True)

top_20_return = top_20.groupby('date_')['R1M_USD'].mean().reset_index()
bottom_20_return = bottom_20.groupby('date_')['R1M_USD'].mean().reset_index()

# 각 포트폴리오의 누적 수익률을 계산합니다.
top_20_return['cumulative_return'] = (1 + top_20_return['R1M_USD']).cumprod() - 1
bottom_20_return['cumulative_return'] = (1 + bottom_20_return['R1M_USD']).cumprod() - 1

# 날짜를 datetime 형식으로 변환합니다.
top_20_return['date_'] = pd.to_datetime(top_20_return['date_'])
bottom_20_return['date_'] = pd.to_datetime(bottom_20_return['date_'])

# 그래프 그리기
plt.figure(figsize=(10,6))

plt.plot(top_20_return['date_'], top_20_return['cumulative_return'], label='Top 20% Portfolio')
plt.plot(bottom_20_return['date_'], bottom_20_return['cumulative_return'], label='Bottom 20% Portfolio')

# 타이틀과 라벨 추가
plt.title('Cumulative Return of Portfolios')
plt.xlabel('Date')
plt.ylabel('Cumulative Return')

# 범례 추가
plt.legend()

# 그래프 보여주기
plt.grid(True)
plt.show()

```

![](/pic/2023-09-11-12-31-31.png)

*예측치 상위 20% 포트폴리오와 하위 20%포트폴리오의 누적수익률*

약 7년간 예측치가 상위값인 주식들의 수익률이 더 높았습니다.
통계적 해석력은 떨어지지만 투자관점에서는 나쁘지 않은 결과입니다.

#### 2. Tree-based methods 

회귀와 분류에 사용하는 기계학습 모델중 가장 단순하지만 유의미한 성과를 보이는 것이 의사결정 트리입니다.(랜덤포레스트 포함) 

**의사결정 트리**

의사결정 트리는 특성을 기반으로 샘플을 분류하는 트리 구조의 모델입니다. 트리의 각 노드에서는 특정 특성에 대한 테스트가 수행되며, 그 결과에 따라 샘플은 다음 노드로 분할됩니다. 마지막 노드, 즉 리프 노드에서 예측이 이루어집니다.

학습

트리를 학습시키기 위해, 가장 잘 분할할 수 있는 특성과 그 특성의 임계값을 찾는 과정을 반복하여 트리를 생성합니다.

장점으로는 해석력이 높으며 시각화가 가능하다는 것이 있고, 데이터의 누락 값을 자연스럽게 처리할 수 있어 사전 데이터 처리가 많이 필요없습니다.

단점으로는 오버피팅(overfitting) 경향이 매우 강하고, 복잡한 관계는 잘 모델링 하지 못합니다.

의사결정 트리(decision tree)는 분류(classification) 문제와 회귀(regression) 문제 두 가지 유형에 모두 사용될 수 있는 유연한 알고리즘입니다. 두 문제 유형에 따른 의사결정 트리의 차이점과 작동 방식에 대해 설명하겠습니다.

 - 분류 문제 : 특정 기준(예: 지니 불순도, 엔트로피)을 사용하여 데이터를 여러 하위 집합으로 분할합니다.
 - 회귀 문제 : 데이터를 여러 하위 집합으로 분할하여, 각 하위 집합에 대해 특정 수치값(예: 평균 목표 변수 값)을 예측합니다.

분류 트리에서는 리프 노드에 도달할 때 각 클래스의 확률을 제공할 수도 있으며, 가장 높은 확률을 가진 클래스를 예측 레이블로 선택합니다.

회귀 트리에서는 리프 노드에 도달할 때 해당 노드의 평균 목표값을 예측 값으로 제공합니다.

* 지니 불순도 (Gini Impurity)
$$
Gini(D) = 1 - \sum_{i=1}^{C}P_i^{2} \\
$$

$$
D : 데이터세트\\C : 클래스 수\\P_i : i 번째 클래스에 속하는 샘플의 비율
$$


* 엔트로피 (Entropy)

$$
Entropy(D) = -\sum_{i=1}^{C}p_i*\log_2(p_i) \\
$$

$$
\log_2(p_i)는 p_i의 2를 밑으로 하는 로그
$$

두 메트릭 모두 클래스 분포가 한쪽으로 치우칠수록 (즉, 노드가 순수해질수록) 불순도가 감소하게 됩니다. 이러한 메트릭들은 결정 트리가 노드를 분할할 때 더 순수한 하위 노드를 생성하도록 돕습니다.

**Decision Tree 실습**

한국 주식을 대상으로 예측 모델링울 수행합니다. 예측대상(종속변수)은 미래 1개월 수익률입다. 예측 분류 변수는 미래 수익률을 3개의 그룹으로 나누고 상위 1그룹인경우 1 중간 그룹인 경우 0, 하위 그룹인 경우 -1이 됩니다.
이를 잘 예측하는 모델을 구해 보겠습니다.

```python
from matplotlib import pyplot as plt
import seaborn as sns
import pandas as pd
from sklearn.preprocessing import MinMaxScaler
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score, classification_report
from sklearn.metrics import mean_squared_error, r2_score
import numpy as np

# Function to apply Min-Max Scaling for each group
def min_max_scale_group(group):
    scaler = MinMaxScaler()
    group_scaled = scaler.fit_transform(group)
    return pd.DataFrame(group_scaled, columns=group.columns, index=group.index)

# qcut을 사용하여 3개의 분위로 나누고, labels 파라미터를 사용하여 각 분위에 라벨을 지정
def assign_quantile(group):    
    group['quantile_label'] = pd.qcut(group['R1M_USD'], q=3, labels=[-1, 0, 1])
    return group

# 현재 작업중인 파일로 옮기기
data_ml = pd.read_csv('data_ml.csv')

# 'region' 열이 'KR'인 행만 추출
data = data_ml[data_ml['region'] == 'KR']

#가능한 모든 입력변수를 가져온다.
f_columns = [col for col in data.columns if col.startswith('f')]

#결측치가 있는경우 해당 날짜에서 가장 작은 숫자를 입력한다.
for col in f_columns:
    data[col].fillna(data.groupby('date_')[col].transform('mean'), inplace=True)

# Apply Min-Max Scaling for f_columns for each date_ group
data[f_columns] = data.groupby('date_')[f_columns].apply(min_max_scale_group)

# qcut을 사용하여 3개의 분위로 나누고, labels 파라미터를 사용하여 각 분위에 라벨을 지정
data['R1M_USD_TERNARY'] = data.groupby('date_')['R1M_USD'].transform(lambda x: pd.qcut(x, 3, labels=[-1, 0, 1]))


data['date_'] = pd.to_datetime(data['date_'])

# 2015년 12월 31일 이전 데이터를 train으로, 이후 데이터를 test로 나눔
train_data = data[data['date_'] <= '2015-12-31']
test_data = data[data['date_'] > '2015-12-31']

# 피처와 타겟 변수 분리
X_train = train_data[f_columns]
X_test = test_data[f_columns]
y_train = train_data['R1M_USD_TERNARY']
y_test = test_data['R1M_USD_TERNARY']

# 의사결정나무 모델 학습
tree_model = DecisionTreeClassifier(random_state=42)
tree_model.fit(X_train, y_train)

# test 데이터에 대한 예측 수행
y_pred = tree_model.predict(X_test)


# 모델 성능 평가
accuracy = accuracy_score(y_test, y_pred)
classification_rep = classification_report(y_test, y_pred)

print(f"Accuracy: {accuracy}\n")
print(f"Classification Report:\n{classification_rep}")

```

```
Accuracy: 0.338590492996581

Classification Report:
              precision    recall  f1-score   support

          -1       0.34      0.37      0.36      9085
           0       0.34      0.31      0.33      9049
           1       0.33      0.33      0.33      9067

    accuracy                           0.34     27201
   macro avg       0.34      0.34      0.34     27201
weighted avg       0.34      0.34      0.34     27201
```

모델의 정확도는 34%로, 랜덤하게 선택한 경우(33.33%)보다 약간 더 높습니다. 각 클래스에 대한 정밀도, 재현율, F1-score도 비슷한 범위에 있습니다, 이는 모델이 각 클래스를 비슷하게 잘 분류하고 있음을 나타냅니다. 그러나 성능이 높다고는 어렵게 평가되며, 모델의 개선이 필요해 보입니다.

투자전략의 관점에서 2015년까지 학습한 모델의 예측치를 사용해 보겠습니다.
1로 분류되는 종목들의 수익률과 -1로 분류되는 종목들의 수익률을 비교 평가하여 2016년 이후 (out of sample) 포트폴리오 그룹별 수익률을 확인해 보겠습니다.

```python
test_data['predicted_R1M_USD_TERNARY'] = y_pred
backtest_data = test_data[['date_','infocode','R1M_USD','predicted_R1M_USD_TERNARY']]

port_high_return = backtest_data[backtest_data['predicted_R1M_USD_TERNARY'] == 1].groupby('date_')['R1M_USD'].mean().reset_index()
port_low_return = backtest_data[backtest_data['predicted_R1M_USD_TERNARY'] == -1].groupby('date_')['R1M_USD'].mean().reset_index()

port_high_return['cumulative_return'] = (1 + port_high_return['R1M_USD']).cumprod() - 1
port_low_return['cumulative_return'] = (1 + port_low_return['R1M_USD']).cumprod() - 1


# 날짜를 datetime 형식으로 변환합니다.
port_high_return['date_'] = pd.to_datetime(port_high_return['date_'])
port_low_return['date_'] = pd.to_datetime(port_low_return['date_'])

# 그래프 그리기
plt.figure(figsize=(10,6))

plt.plot(port_high_return['date_'], port_high_return['cumulative_return'], label='Classified High Portfolio')
plt.plot(port_low_return['date_'], port_low_return['cumulative_return'], label='Classified Low Portfolio')

# 타이틀과 라벨 추가
plt.title('Cumulative Return of Portfolios')
plt.xlabel('Date')
plt.ylabel('Cumulative Return')


```

![](/pic/2023-09-11-14-39-04.png)

결과가 거의 차이가 없습니다. 모델의 유용성이 매우 떨어집니다.

이는 모델의 복잡도가 높아지면 분류를 잘 하지 못하는 트리 모델의 특성에 기인할수 있습니다. 

f1과 f1011 두개의 입력변수만을 사용하는 경우 결과는 다음과 같습니다.

![](/pic/2023-09-11-14-48-42.png)

좋은 입력변수를 아주 적은 숫자만 사용하는 경우 모델을 개선할 수 있다면 트리 모델로도 충분히 좋은 결과를 기대해 볼수 있다는 걸 확인할 수 있습니다.

**랜덤 포레스트 (Random Forest)**

랜덤 포레스트는 여러 개의 의사결정 트리를 합친 앙상블 기법입니다. 각 트리는 부트스트랩 샘플(원본 데이터에서 복원 추출로 얻은 샘플)을 사용하여 학습됩니다.

학습

여러 개의 트리를 독립적으로 학습시킨 후, 그 결과를 통합하여 예측을 수행합니다.
분류 문제의 경우에는 투표를 통해, 회귀 문제의 경우에는 평균을 통해 예측을 결정합니다.

장점은 오버피팅 문제를 상당히 완화할수 있다는 점과 병렬처리가 가능해 대용량 데이터셋에서도 빠른 학습이 가능하다는 점이 있습니다.

단점은 의사결정트리에 비해 모델에 대한 해석이 힘들다는 점과 데이터 셋이 부족한 경우 잘 작동하지 않는다는 점이 있습니다.

모델의 구현과 해석이 단순 의사결정 트리와 거의 유사합니다.

**Random Forest 실습**

```python
from matplotlib import pyplot as plt
import seaborn as sns
import pandas as pd
from sklearn.preprocessing import MinMaxScaler
from sklearn.metrics import accuracy_score, classification_report
from sklearn.metrics import mean_squared_error, r2_score
import numpy as np
from sklearn.ensemble import RandomForestClassifier

# Function to apply Min-Max Scaling for each group
def min_max_scale_group(group):
    scaler = MinMaxScaler()
    group_scaled = scaler.fit_transform(group)
    return pd.DataFrame(group_scaled, columns=group.columns, index=group.index)
# 현재 작업중인 파일로 옮기기
data_ml = pd.read_csv('data_ml.csv')

# 'region' 열이 'KR'인 행만 추출
data = data_ml[data_ml['region'] == 'KR']

#가능한 모든 입력변수를 가져온다.
f_columns = [col for col in data.columns if col.startswith('f')]
#f_columns = ['f1','f1011']

#결측치가 있는경우 해당 날짜에서 가장 작은 숫자를 입력한다.
for col in f_columns:
    data[col].fillna(data.groupby('date_')[col].transform('mean'), inplace=True)

# Apply Min-Max Scaling for f_columns for each date_ group
data[f_columns] = data.groupby('date_')[f_columns].apply(min_max_scale_group)

# qcut을 사용하여 3개의 분위로 나누고, labels 파라미터를 사용하여 각 분위에 라벨을 지정
data['R1M_USD_TERNARY'] = data.groupby('date_')['R1M_USD'].transform(lambda x: pd.qcut(x, 3, labels=[-1, 0, 1]))

data['date_'] = pd.to_datetime(data['date_'])

# 2015년 12월 31일 이전 데이터를 train으로, 이후 데이터를 test로 나눔
train_data = data[data['date_'] <= '2015-12-31']
test_data = data[data['date_'] > '2015-12-31']

# 피처와 타겟 변수 분리
X_train = train_data[f_columns]
X_test = test_data[f_columns]
y_train = train_data['R1M_USD_TERNARY']
y_test = test_data['R1M_USD_TERNARY']

# 랜덤포레스트 분류기를 사용하여 모델 학습
rf_model = RandomForestClassifier(random_state=42)
rf_model.fit(X_train, y_train)

# test 데이터에 대한 예측 수행
y_pred = rf_model.predict(X_test)

# 모델 성능 평가
accuracy = accuracy_score(y_test, y_pred)
classification_rep = classification_report(y_test, y_pred)

print(f"Accuracy: {accuracy}\n")
print(f"Classification Report:\n{classification_rep}")
```

```
Accuracy: 0.36726590934156833

Classification Report:
              precision    recall  f1-score   support

          -1       0.37      0.41      0.39      9085
           0       0.39      0.44      0.41      9049
           1       0.34      0.25      0.28      9067

    accuracy                           0.37     27201
   macro avg       0.36      0.37      0.36     27201
weighted avg       0.36      0.37      0.36     27201
```

36%가 넘어 의사결정 트리 모델의 비해 정확도가 크게 상승한 것을 확인할 수 있습니다.

앞서 단순 의사결정 트리에서 했던 투자전략 관점에서의 수익률을 확인해 보겠습니다.

![](/pic/2023-09-11-15-18-35.png)

결과적으로 성능의 개선은 없었습니다. 

f1과 f1011 두개의 팩터만 사용한 경우에는 의사결정 트리에 비해서도 더 높은 성능 개선을 보여줬습니다.

![](/pic/2023-09-11-15-20-15.png)

유의미한 지표를 사전에 먼저 찾는 작업이 잘 진행된다면 단순 의사결정 트리보다는 높은 성능을 낼 수 있을거라고 기대할 수 있습니다.

#### 3. XGBoost

XGBoost는 "eXtreme Gradient Boosting"의 약어로, 특히 대규모 데이터셋에 대한 트리 부스팅 시스템을 최적화하기 위해 개발된 오픈 소스 머신 러닝 라이브러리입니다. 이 라이브러리는 효율성, 속도 및 성능에 중점을 둔 구현을 제공하며, 아래에 몇 가지 주요 특징과 장점이 나열되어 있습니다:

주요 특징

1. Gradient Boosting 알고리즘: XGBoost는 Gradient Boosting 알고리즘을 사용하여 약한 학습자(보통 의사결정 트리)들을 순차적으로 훈련시키며, 각 단계에서 이전 단계의 오차를 최소화하려고 합니다.

2. XGBoost는 트리의 복잡성을 제어하기 위해 L1(Lasso Regression) 및 L2(Ridge Regression) 규제항을 추가하여 과적합을 방지할 수 있습니다.

3. 결측값 처리: XGBoost는 결측값을 자동으로 처리할 수 있으며, 최적의 방향으로 결측값을 할당합니다.

장점

XGBoost는 효율적인 알고리즘과 병렬 처리 덕분에 뛰어난 성능을 발휘하며, 많은 기계 학습 경쟁에서 우승한 모델을 생성했습니다.(Kaggle 최대 우승 모델)

GBoost는 회귀, 분류, 순위 및 사용자 정의 예측 문제에 사용할 수 있으며, 다양한 종류의 목적 함수와 평가 기준을 지원합니다.

XGBoost는 스파스 데이터와 매우 큰 데이터셋을 둘다 효율적으로 처리할 수 있습니다.

단점

XGBoost 모델은 랜덤 포레스트나 단일 의사 결정 트리보다 해석하기 어렵습니다.

대규모 데이터셋에서는 트레이닝 시간이 길어질 수 있습니다, 특히 트리의 깊이가 깊고, 앙상블에 사용되는 트리의 수가 많을 경우에 그러합니다.

하이퍼파라미터 튜닝: XGBoost 모델은 여러 하이퍼파라미터를 가지고 있으며, 이들을 최적화하는 것은 시간이 많이 소요되며, 실질적인 경험과 도메인 지식이 필요합니다.

```python
from xgboost import XGBClassifier  # XGBClassifier를 import합니다.
from matplotlib import pyplot as plt
import seaborn as sns
import pandas as pd
from sklearn.preprocessing import MinMaxScaler
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score, classification_report
from sklearn.metrics import mean_squared_error, r2_score
import numpy as np

# Function to apply Min-Max Scaling for each group
def min_max_scale_group(group):
    scaler = MinMaxScaler()
    group_scaled = scaler.fit_transform(group)
    return pd.DataFrame(group_scaled, columns=group.columns, index=group.index)

# 현재 작업중인 파일로 옮기기
data_ml = pd.read_csv('data_ml.csv')

# 'region' 열이 'KR'인 행만 추출
data = data_ml[data_ml['region'] == 'KR']

#가능한 모든 입력변수를 가져온다.
f_columns = [col for col in data.columns if col.startswith('f')]


#결측치가 있는경우 해당 날짜에서 가장 작은 숫자를 입력한다.
for col in f_columns:
    data[col].fillna(data.groupby('date_')[col].transform('mean'), inplace=True)

# Apply Min-Max Scaling for f_columns for each date_ group
data[f_columns] = data.groupby('date_')[f_columns].apply(min_max_scale_group)

# qcut을 사용하여 3개의 분위로 나누고, labels 파라미터를 사용하여 각 분위에 라벨을 지정
data['R1M_USD_TERNARY'] = data.groupby('date_')['R1M_USD'].transform(lambda x: pd.qcut(x, 3, labels=[0, 1, 2]))

data['date_'] = pd.to_datetime(data['date_'])

# 2015년 12월 31일 이전 데이터를 train으로, 이후 데이터를 test로 나눔
train_data = data[data['date_'] <= '2015-12-31']
test_data = data[data['date_'] > '2015-12-31']

# 피처와 타겟 변수 분리
X_train = train_data[f_columns]
X_test = test_data[f_columns]
y_train = train_data['R1M_USD_TERNARY']
y_test = test_data['R1M_USD_TERNARY']

# xgBoost 모델 학습
xgb_model = XGBClassifier(random_state=42, use_label_encoder=False, eval_metric='mlogloss')  # xgBoost 모델 객체를 생성합니다.
xgb_model.fit(X_train, y_train)  # 모델을 학습합니다.

# test 데이터에 대한 예측 수행
y_pred = xgb_model.predict(X_test)  # test 데이터에 대한 예측을 수행합니다.

# 모델 성능 평가
accuracy = accuracy_score(y_test, y_pred)  # 정확도를 계산합니다.
classification_rep = classification_report(y_test, y_pred)  # 분류 리포트를 생성합니다.

print(f"Accuracy: {accuracy}\n")
print(f"Classification Report:\n{classification_rep}")
```

```
Accuracy: 0.362780780118378

Classification Report:
              precision    recall  f1-score   support

           0       0.37      0.35      0.36      9085
           1       0.37      0.45      0.41      9049
           2       0.34      0.29      0.31      9067

    accuracy                           0.36     27201
   macro avg       0.36      0.36      0.36     27201
weighted avg       0.36      0.36      0.36     27201
```

정확도는 Random Forest의 성과와 비슷합니다.
투자전략의 관점에서 2015년까지 학습한 모델의 예측치를 사용해 보겠습니다.
0으로 분류되는 종목들의 수익률과 2로 분류되는 종목들의 수익률을 비교 평가하여 2016년 이후 (out of sample) 포트폴리오 그룹별 수익률을 확인해 보겠습니다.

```python

test_data['predicted_R1M_USD_TERNARY'] = y_pred
backtest_data = test_data[['date_','infocode','R1M_USD','predicted_R1M_USD_TERNARY']]

port_high_return = backtest_data[backtest_data['predicted_R1M_USD_TERNARY'] == 2].groupby('date_')['R1M_USD'].mean().reset_index()
port_low_return = backtest_data[backtest_data['predicted_R1M_USD_TERNARY'] == 1].groupby('date_')['R1M_USD'].mean().reset_index()

port_high_return['cumulative_return'] = (1 + port_high_return['R1M_USD']).cumprod() - 1
port_low_return['cumulative_return'] = (1 + port_low_return['R1M_USD']).cumprod() - 1


# 날짜를 datetime 형식으로 변환합니다.
port_high_return['date_'] = pd.to_datetime(port_high_return['date_'])
port_low_return['date_'] = pd.to_datetime(port_low_return['date_'])

# 그래프 그리기
plt.figure(figsize=(10,6))

plt.plot(port_high_return['date_'], port_high_return['cumulative_return'], label='Classified High Portfolio')
plt.plot(port_low_return['date_'], port_low_return['cumulative_return'], label='Classified Low Portfolio')

# 타이틀과 라벨 추가
plt.title('Cumulative Return of Portfolios')
plt.xlabel('Date')
plt.ylabel('Cumulative Return')

# 범례 추가
plt.legend()

# 그래프 보여주기
plt.grid(True)
plt.show()

```

![](/pic/2023-09-25-16-03-19.png)

수익률이 낮을 것으로 분류되는 그룹의 수익률이 더 좋아 성과가 안좋습니다.

f1과 f1011만 사용하는 경우에는 조금 나아지긴 하지만 큰 개선은 없습니다.

![](/pic/2023-09-25-16-04-05.png)

#### 4. Support vector machines

SVM은 분류 및 회귀 분석을 위한 지도 학습 모델로, 데이터를 분리하는 가장 큰 폭을 가진 초평면을 찾는 것을 목표로 합니다.

![](/pic/2023-09-12-10-48-41.png)

초평면(Hyperplane) : SVM의 핵심 아이디어는 데이터를 분류하는 최적의 결정 경계를 찾는 것입니다. 이 결정 경계를 초평면이라고 합니다. 초평면은 n-차원 공간에서 n-1 차원의 부분 공간으로 정의됩니다. 예를 들어, 2차원 공간에서는 선이 되고, 3차원에서는 평면이 됩니다.

서포트 벡터(Support Vectors) : 서포트 벡터는 초평면을 정의하는 데 사용되는 데이터 포인트들입니다. 이들은 결정 경계에 가장 가까운 데이터 포인트들로, 초평면과의 거리가 가장 작습니다.

마진(Margin) : 마진은 초평면과 서포트 벡터 사이의 거리입니다. SVM은 이 마진을 최대화하는 초평면을 찾는 것을 목표로 합니다. 즉, 마진을 최대로 하는 결정 경계를 찾는 것이 SVM의 주요 목표입니다.

**모델링 방식**

1. 데이터가 선형으로 분리 가능한 경우, SVM은 두 클래스를 구분하는 최적의 초평면을 찾습니다. 이를 "하드 마진" SVM이라고도 합니다.
2. 데이터가 완벽하게 선형으로 분리되지 않는 경우에는 "소프트 마진" SVM을 사용하여 일부 오류를 허용하며 최적의 초평면을 찾습니다.
3. 선형으로 분리할 수 없는 데이터에 대해 SVM은 커널 트릭을 사용하여 데이터를 더 높은 차원의 공간으로 매핑하고, 그 공간에서 선형 분리를 시도합니다. 

서포트 벡터 머신(SVM)에서 서포트 벡터는 결정 경계를 정의하는 데 극도로 중요한 역할을 합니다. 그러나 이것이 다른 데이터 포인트들이 전혀 중요하지 않다는 것을 의미하지는 않습니다. 

**커널 함수**

커널 함수는 머신 러닝에서 서포트 벡터 머신(SVM)을 포함한 여러 알고리즘에서 중요한 역할을 합니다. 커널 함수의 역할과 다양한 유형의 커널에 대해 알아보겠습니다.

비선형 분류 문제를 선형 분류 문제로 변환하는 것은 머신 러닝에서 매우 중요한 개념입니다. 커널 함수를 통해 더 높은 차원의 특성 공간으로 데이터를 매핑함으로써, 원래의 차원에서는 선형으로 분리할 수 없던 데이터를 선형으로 분리할 수 있게 됩니다.

커널 함수와 차원 확장

데이터를 더 높은 차원으로 매핑하는 것은 각 데이터 포인트에 대해 추가적인 특성을 생성하는 것과 같습니다. 이러한 추가 특성은 기존 특성의 다항식이나 그 외의 비선형 조합으로 형성될 수 있습니다.

예시

예를 들어, 2차원 공간에서 두 클래스의 데이터 포인트가 서로 둘러쌓여 있는 형태(원형으로 분포)로 주어진다고 가정합시다. 이 경우, 2차원 공간에서는 선형 분리가 불가능합니다. 하지만, 만약 이 데이터를 3차원 공간으로 매핑하면, 두 클래스를 선형 초평면으로 분리할 수 있는 새로운 차원이 생길 수 있습니다.

수학적으로, 2차원 공간에서의 좌표 (x, y)를 3차원 공간으로 확장하여 (x, y, x² + y²)의 형태로 변환할 수 있습니다. 이러한 변환을 통해 원형 분포의 데이터를 3차원 공간에서는 위 아래로 분리되는 형태로 만들 수 있으며, 이를 선형 초평면으로 분리할 수 있게 됩니다.

커널 함수의 종류

1. 선형 커널함수 : 선형 커널은 간단하게 데이터 포인트들의 내적을 계산하는 함수입니다. 선형 커널은 기본적으로 어떠한 추가적인 변환 없이 데이터 포인트들을 사용합니다.
$$
    K(x,y) = x^Ty
$$

2. 다항 커널 (Polynomial Kernel) : 다항 커널은 데이터를 다항식 특성 공간으로 매핑합니다. 주어진 데이터 포인트 x와 y, 그리고 특정 파라미터 d(degree)와 c(constant)를 사용합니다.

$$
    K(x,y) = (x^Ty+c)^d
$$

3. RBF(Radial Basis Function) 또는 가우시안 커널 : RBF 커널은 각 데이터 포인트 사이의 유클리드 거리에 기반하며, 그 거리를 가우시안 함수에 대입하여 계산합니다.
$$
    K(x,y) = exp(- \frac{||x=y||^2}{2\sigma^2})
$$

4. 시그모이드 커널 : 데이터 포인트들을 시그모이드 함수를 통해 매핑합니다. 이는 신경망의 활성화 함수와 관련이 있습니다.

$$
    K(x,y) = tanh(\alpha xTy + c)
$$
 
커널의 선택은 문제의 복잡성, 데이터의 분포 등 다양한 요소에 따라 달라집니다. 또한, 각 커널의 하이퍼파라미터 (예: 다항식 차수, RBF의 σ 등) 튜닝도 모델의 성능에 큰 영향을 미칩니다.

```python

from matplotlib import pyplot as plt
import pandas as pd
from sklearn.preprocessing import MinMaxScaler
from sklearn.metrics import accuracy_score, classification_report
from sklearn.metrics import mean_squared_error, r2_score
import numpy as np
from sklearn.svm import SVC
from sklearn.metrics import accuracy_score

# Function to apply Min-Max Scaling for each group
def min_max_scale_group(group):
    scaler = MinMaxScaler()
    group_scaled = scaler.fit_transform(group)
    return pd.DataFrame(group_scaled, columns=group.columns, index=group.index)

# 현재 작업중인 파일로 옮기기
data_ml = pd.read_csv('data_ml.csv')

# 'region' 열이 'KR'인 행만 추출
data = data_ml[data_ml['region'] == 'KR']

#가능한 모든 입력변수를 가져온다.
f_columns = [col for col in data.columns if col.startswith('f')]

#결측치가 있는경우 해당 날짜에서 가장 작은 숫자를 입력한다.
for col in f_columns:
    data[col].fillna(data.groupby('date_')[col].transform('mean'), inplace=True)

# Apply Min-Max Scaling for f_columns for each date_ group
data[f_columns] = data.groupby('date_')[f_columns].apply(min_max_scale_group)

# qcut을 사용하여 2개의 분위로 나누고, labels 파라미터를 사용하여 각 분위에 라벨을 지정
data['R1M_USD_BINARY'] = data.groupby('date_')['R1M_USD'].transform(lambda x: pd.qcut(x, 2, labels=[0, 1]))

data['date_'] = pd.to_datetime(data['date_'])

# 2015년 12월 31일 이전 데이터를 train으로, 이후 데이터를 test로 나눔
train_data = data[data['date_'] <= '2015-12-31']
test_data = data[data['date_'] > '2015-12-31']

# 피처와 타겟 변수 분리
X_train = train_data[f_columns]
X_test = test_data[f_columns]
y_train = train_data['R1M_USD_BINARY']
y_test = test_data['R1M_USD_BINARY']

# SVM 분류기 객체 생성
svm_classifier = SVC(kernel='rbf', random_state=42)

# 모델 훈련
svm_classifier.fit(X_train, y_train)

# 테스트 데이터에 대한 예측 수행
y_pred = svm_classifier.predict(X_test)

# 정확도 계산
accuracy = accuracy_score(y_test, y_pred)
print(f"Test Accuracy: {accuracy * 100:.2f}%")
```
'R1M_USD' 값을 2개의 분위수로 나누고, 각 분위수에 0 또는 1 라벨을 지정하여 새 변수 'R1M_USD_BINARY'를 생성합니다. (수익률 상위그룹 1,하위 그룹 0)
이 코드는 주어진 피처를 사용하여 'R1M_USD_BINARY' 변수를 예측하는 SVM 분류기 모델을 훈련시키고 테스트합니다.

```
Test Accuracy: 51.06%
```

결과는 51.06%로 정확히 50%로 되어 있는 분류 기준 보다는 높습니다만 매우 뛰어난 성능이라고 보기는 힘들겠습니다.

이전 처럼 전략으로 한번 변환해서 보겠습니다.

```python
# 전략으로 보는 평가
# Predict on the test data
test_data['predicted_R1M_USD_BINARY'] = y_pred
backtest_data = test_data[['date_','infocode','R1M_USD','predicted_R1M_USD_BINARY']]

port_high_return = backtest_data[backtest_data['predicted_R1M_USD_BINARY'] == 1].groupby('date_')['R1M_USD'].mean().reset_index()
port_low_return = backtest_data[backtest_data['predicted_R1M_USD_BINARY'] == 0].groupby('date_')['R1M_USD'].mean().reset_index()

port_high_return['cumulative_return'] = (1 + port_high_return['R1M_USD']).cumprod() - 1
port_low_return['cumulative_return'] = (1 + port_low_return['R1M_USD']).cumprod() - 1

# 날짜를 datetime 형식으로 변환합니다.
port_high_return['date_'] = pd.to_datetime(port_high_return['date_'])
port_low_return['date_'] = pd.to_datetime(port_low_return['date_'])

# 그래프 그리기
plt.figure(figsize=(10,6))

plt.plot(port_high_return['date_'], port_high_return['cumulative_return'], label='Classified High Portfolio')
plt.plot(port_low_return['date_'], port_low_return['cumulative_return'], label='Classified Low Portfolio')

# 타이틀과 라벨 추가
plt.title('Cumulative Return of Portfolios')
plt.xlabel('Date')
plt.ylabel('Cumulative Return')

# 범례 추가
plt.legend()

# 그래프 보여주기
plt.grid(True)
plt.show()
```

![](/pic/2023-09-12-14-06-58.png)

전략의 관점에서 보면 높은 1 그룹의 수익률이 0 그룹의 수익률보다 확실히 높은 것을 알 수 있습니다.

## 다음시간

오늘은 간단하게 머신러닝 모델들을 소개하고 사용하는 방법을 알아 봤습니다.
다음시간에는 인공신경망과 앙상블 모델로 모델을 개선하는 과정을 진행해 보겠습니다.

