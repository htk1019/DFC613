## 3강 팩터투자연구 <a name = "factor"></a>

### CAPM 이후 팩터이론의 발전 ###

오랜 기간 동안 주식 수익률을 설명하는 데 있어서 베타가 결정적 요인이었던 CAPM 모형은 금융학에서의 기본 원칙처럼 여겨졌습니다.

Black·Jensen·Scholes(1972)와 Fama·MacBeth(1973) 연구에 따르시면 1969년 이전의 기간 동안 평균 주가 수익률과 베타 사이에는 양의 관계가 있었다고 합니다.

그런데 시간이 흘러가면서 이런 관점을 반박하는 논문들이 계속 나오기 시작합니다.

1. Banz(1981) 연구에서는 시장 가치(시가 총액)가 작은 주식의 경우, 수익률이 더 높다는 소형주 효과를 발견하였습니다. 그렇다면 소형주의 평균 수익률은 해당 주식의 베타를 고려했을 때 예상보다 높고, 대형주의 평균 수익률은 그와 반대로 예상보다 낮게 나타났습니다.

2. Stattman(1980), Rosenburg·Reid·Lanstein(1985) 연구에서는 미국 주식의 평균 수익률이 장부 가치 대 시장 가치(BE/ME, PBR의 역수)와 양의 상관관계를 가지고 있다고 합니다.

3. Basu(1983)는 earnings-price ratios(E/P)가 미국 주식의 수익률을 설명하는 데 중요한 변수라고 주장하였습니다.

1963년부터 1990년까지의 기간을 연구한 결과들은 대부분 1970년대 이전의 연구와는 다르게 평균 주가 수익률과 베타 사이에는 더 이상 양의 관계가 나타나지 않았다고 합니다. 즉 주식의 크기, 장부 가치와 시장 가치의 비율, 그리고 주식의 수익률을 예측하는 데 있어 다른 중요한 변수들을 이 존재한다는 것이며. 이후 이러한 새로운 연구들은 기존의 베타만을 사용하는 모형의 한계를 지적하면서 현재까지도 발전해 오고 있습니다.

다음은 시장수익률과 주식수익률과의 관계를 보여주는 코드입니다.


#### 1969년 이전까지의 시장수익률과 주식수익률의 선형관계
```python
import pandas as pd
import numpy as np
import requests
from io import BytesIO, StringIO
from zipfile import ZipFile
import matplotlib.pyplot as plt
import seaborn as sns

# Download the 'Portfolios Formed on Market Beta' data
url = 'http://mba.tuck.dartmouth.edu/pages/faculty/ken.french/ftp/Portfolios_Formed_on_BETA_CSV.zip'
response = requests.get(url)

# Try to unzip the content
content = None
try:
    with ZipFile(BytesIO(response.content)) as zf:
        # Assuming the first file in the ZIP is the one we want
        with zf.open(zf.namelist()[0]) as file:
            content = file.read().decode('utf-8')
except Exception as e:
    print(f"Error encountered: {e}")

# Convert the content to a StringIO object so it can be read like a file by pandas
data_io = StringIO(content)
# Read the data into a pandas DataFrame, skipping the additional header information
beta_data = pd.read_csv(data_io, skiprows=15)
beta_data=beta_data.iloc[:1500, [0] + list(range(6, 16))]
beta_data = beta_data.rename(columns={beta_data.columns[0]: 'Date'})
# Filter out rows where 'Date' column is not numeric
beta_data = beta_data[beta_data['Date'].astype(str).str.isnumeric()]
# Convert 'Date' column to datetime format
beta_data['Date'] = pd.to_datetime(beta_data['Date'], format='%Y%m')

beta_data.set_index('Date', inplace=True)
beta_data = beta_data.apply(pd.to_numeric, errors='coerce')
beta_data = beta_data.iloc[:,:] / 100

# Filter data
filtered_data = beta_data[beta_data.index <='1968-12-31']

# Pivot data
beta_long = filtered_data.reset_index().melt(id_vars='Date', var_name='name', value_name='value')

# Calculate mean and standard deviation
grouped_data = beta_long.groupby('name').agg(
    mu=('value', lambda x: x.mean() * 12),
    sd=('value', lambda x: x.std() * np.sqrt(12))
)

# Plot data
plt.figure(figsize=(10, 6))
sns.scatterplot(data=grouped_data, x='sd', y='mu', hue='name', s=200)
for idx, row in grouped_data.iterrows():
    plt.text(row['sd'], row['mu'], idx, verticalalignment='bottom', fontsize=10)
plt.legend().set_visible(False)
plt.xlabel("Annualized Sd")
plt.ylabel("Annualized Mean")
plt.title("Risk Return Profile Before 1969")
plt.show()


```
![](/pic/2023-08-12-15-05-00.png)

1969년 이전까지의 데이터를 보면 시장에 존재하는 모든 주식을 베타에 따라 10분위로 나눈 후 수익률을 살펴 봤을 때 베타가 높을 수록 수익률과 위험이 함께 증가하는 경향이 보입니다.

```python
# Filter data
filtered_data = beta_data[beta_data.index > '1968-12-31']
filtered_data = beta_data[beta_data.index <= '1990-12-31']
```

반면에 데이터를 1969년부터 1990년대로 확대하면 그 경향성이 사라집니다.
![](/pic/2023-08-12-15-09-34.png)

이는 더 이상 CAPM이 유의미하지 않다는 뜻으로 해석되고 주식을 설명하는 다른 요인들의 존재를 찾도록 유도합니다.

#### 사이즈 팩터 ####

![](/pic/2023-08-12-15-11-22.png)

*Table I 사이즈-베타 포트폴리오의 평균수익률, post-ranking 베타, 평균 사이즈: 주식을 ME(수직방향)로 정렬한 뒤 pre-ranking 베타(수평방향)로 정렬. 1963년 7월 ~ 1990년 12월 기간*

패널 A 분석에서는 1990년대까지 소형주(Small-ME)의 수익률이 대형주(Large-ME)의 수익률보다 높았습니다. 그러나 베타를 기준으로 정렬한 포트폴리오 간의 수익률 차이는 미미하며, 이는 베타의 설명력이 제한적임을 나타냅니다.

패널 B에서는 과거 수익률로 추정한 베타가 지속적으로 유지되는 모습을 볼 수 있습니다. 이는 과거에 높은 베타를 가진 주식이 나중에도 높은 베타를 유지하는 경향이 있음을 의미합니다.

패널 A와 B의 종합적인 해석을 통해, 주식의 크기가 클수록 그 이후의 베타와 수익률이 감소하는 경향이 있음을 알 수 있습니다.

최종적으로, 주식의 사이즈와 평균 수익률 사이에는 상관관계가 있으며, 특히 소형주의 수익률이 대형주보다 우수하다는 것을 확인할 수 있습니다. 하지만 주식의 사이즈를 고정하면, 베타만을 고려했을 때 평균 수익률과의 관계는 발견되지 않습니다. 이는 주식의 **사이즈를 통제하면 베타와 평균 수익률 사이에는 연관성이 없다는 것을 의미합니다.**

```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
import requests
from io import BytesIO
from zipfile import ZipFile

# 1. Loading necessary libraries
# (Already included above)

# 2. Downloading and reading the data
url = "https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/ftp/25_Portfolios_ME_BETA_5x5_CSV.zip"
response = requests.get(url)
zip_file = ZipFile(BytesIO(response.content))
csv_file = zip_file.open("25_Portfolios_ME_BETA_5x5.csv")

# Skip the initial description lines and read the CSV data into a pandas DataFrame
data = pd.read_csv(csv_file, skiprows=16)
data = data.rename(columns={"Unnamed: 0": "date"})
index_of_value = data[data['date'].str.contains("Average Equal Weighted Returns -- Monthly", na=False)].index[0]
data = data.iloc[:index_of_value,]

# Rename columns
data.columns = data.columns.str.replace('SMALL', 'ME1')
data.columns = data.columns.str.replace('LoBETA', 'BETA1')
data.columns = data.columns.str.replace('HiBETA', 'BETA5')
data.columns = data.columns.str.replace('BIG', 'ME5')

# Convert columns to numeric, coercing invalid parsing to NaN
for col in data.columns:
    if col != 'date':
        data[col] = pd.to_numeric(data[col], errors='coerce')

# 3. Data manipulation
mean_values = data.drop('date', axis=1).mean(numeric_only=True)

# Convert series to DataFrame for plotting
df_mean = mean_values.reset_index()
df_mean.columns = ['name', 'value']
df_mean[['size', 'beta']] = df_mean['name'].str.split(' ', expand=True)

# 4. Plotting
plt.figure(figsize=(10, 10))
pivot_table = df_mean.pivot(index='size', columns='beta', values='value')
sns.heatmap(pivot_table, annot=True, fmt=".2f", cmap='viridis', cbar=False, linewidths=.5)
plt.xlabel('')
plt.ylabel('')
plt.show()
```
![](/pic/2023-08-12-16-28-22.png)

*2023년까지의 Size와 Beta 그룹별 월간 수익률*


다음은 사이즈와 베타 각각을 단독으로 봤을 때의 표 입니다.
![](/pic/2023-08-12-16-31-55.png)

1. 포트폴리오 사이즈로 봤을때 월간 평균 수익률은 강한 역상관성이 존재합니다.
2. 포트폴리오 사이즈로 봤을때 베타는 강한 역상관성이 존재합니다.
3. 그래서 얼핏 보면 수익률과 베타가 관계가 있는 것처럼 보입니다.
4. 그러나 사이즈와 무관하게 베타를 단독으로 봤을 때는 평균 수익률과 뚜렷한 관계가 없습니다.

그러므로 주식 수익률은 1990년대 까지의 조사로 봤을 때 (비교적 최근까지) 사이즈와는 강한 상관관계가 있지만 시장 베타간에는 상관성이 없다는 것을 알수 있습니다.

그런데 사이즈외에 다른 설명력이 있는 팩터들은 없을까요?

#### 여러 팩터들을 사용한 결과

![](/pic/2023-08-12-16-57-58.png)

*Table Ⅲ 주식의 월수익률을 베타, 사이즈, BE/ME, 레버리지, E/P 4개의 팩터와 회귀분석 한 결과의 기울기 평균값과 t-통계값: 1963년 7월 ~ 1990년 12월*

표의 각 행은 해당 열에 있는 변수를 설명변수로, 수익률을 종속변수로 하여 파마-맥베스 회귀분석을 실시했을 때 나오는 기울기 및 t-통계량을 표시한 것입니다. 단일 변수가 아닌 여러 가지 팩터를 한꺼번에 사용한 결과를 보여주는 것입니다. 예를 들어, 3번째 행은 수익률을 베타와 ln(ME) 기준으로 파마-맥베스 회귀분석 방식의 회귀분석을 실시한 결과입니다. '파마-맥베스 회귀분석'에 대한 자세한 설명은 나중에 하도록 하겠습니다.

표의 결론을 해석해 보면 다음과 같습니다.

1.  ln(ME)는 사이즈입니다. 수익률 만으로 회귀분석 하는 경우 기울기는 -0.15%이며, t-통계값 -2.58입니다. 통계적으로 유의미한 결과를 보여줍니다.
2. 시장 베타는 앞에서 살펴본 것처럼 통계적 유의성이 없습니다.
3. 수익률을 가치평가지표인 ln(BE/ME)로 회귀분석했을 때 평균 기울기는 0.5%이고 t-통계값은 5.71로 이는 유의성이 높습니다.
4. ln(ME)와 ln(BE/ME)를 동시에 입력변수로 사용하는 경우에 ln(ME)의 통계적 유의성은 -1.99로 줄었지만 여전히 높은 수준입니다.
5. 즉 BE/ME의 설명력이 강력하지만 ln(ME)도 설명력을 가지므로 두 변수를 모두 회귀분석에 포함시키는 것이 좋은 모델이라는 것을 확인할 수 있습니다.

### 파마 프렌치 3 팩터 모델

파마-프렌치 3 팩터 모델은 주식의 수익률을 예측하기 위한 금융 모델입니다. 1992년에 킨스 프렌치와 유진 파마에 의해 제안되었습니다. 이 모델은 CAPM (자본 자산 가격 모델)을 확장하여 주식의 수익률을 더 정확하게 설명하려고 합니다.

모델은 세 가지 "팩터" 또는 위험 요소를 사용합니다:

**시장 위험 프리미엄:** 이것은 CAPM에서도 사용되는 팩터로, 시장 전체의 기대 수익률과 무위험 수익률 간의 차이입니다.

**사이즈 팩터 (Size Factor, SMB: Small Minus Big):** 소형주가 대형주에 비해 더 높은 수익률을 보이는 경향을 반영합니다. SMB 값은 소형주와 대형주의 수익률 차이를 나타냅니다.

**가치 팩터 (Value Factor, HML: High Minus Low):** 가치주 (저평가된 주식 또는 높은 책값 대 시장가치 비율을 가진 주식)가 성장주에 비해 더 높은 수익률을 보이는 경향을 반영합니다. HML 값은 높은 장부가치 대 시장가치 비율을 가진 주식과 낮은 비율을 가진 주식의 수익률 차이를 나타냅니다.

파마-프렌치 3 팩터 모델은 주식의 수익률을 설명하는 데 있어 단순한 CAPM보다 더 효과적이라는 연구 결과가 있습니다.

#### 밸류 팩터 : BE/ME, E/P

다음의 테이블은 대표적인 가치지표인 BE/ME, 와 PER의 역수인 E/P를 비교해 봅니다.

![](/pic/2023-08-12-17-19-27.png)
*Table Ⅳ BE/ME 혹은 E/P로만 구성한 포트폴리오의 특성: 1963년 7월 ~ 1990년 12월 기간*

1. 평균수익률과 E/P 지표는 강력한 선형관계가 아닌 U자형을 보여줍니다.
2. 평균수익률과 BE/ME 지표는 강한 양의 상관관계를 보여줍니다.
3. BE/ME와 E/P는 선형성이 높은 상관관계를 보여줍니다.

상관관계가 높은 비슷한 성격의 두 지표들 중에서 BE/ME가 더 설명력이 높습니다.

정리하자면  베타만 보고 주식의 평균 수익률을 예측하는 것은 믿을 만한 방법이 아닙니다.
1963년부터 1990년까지 주요 주식 시장의 평균 수익률은 베타로는 설명할 수 없습니다. 반면에 사이즈와 BE/ME는 수익률 변동을 잘 나타내 줍니다.

이 이론을 기반으로 설명한 것이 파마 프렌치의 3 팩터 모델입니다.

그렇다면 사이즈와 밸류팩터를 모두 활용한 경우의 성과는 어떠했을까요?

![](/pic/2023-08-12-17-27-25.png)
*Table Ⅴ 사이즈-BE/ME 포트폴리오의 월평균수익률; 주식을 ME(수직방향)로 정렬한 뒤 BE/ME(수평방향)로 정렬*

- 동일한 사이즈 안에서는, BE/ME가 커짐에 따라 수익률이 보통 강하게 증가합니다.
- 평균수익률과 사이즈 간에 음(-)의 상관관계가 있습니다.
- **사이즈를 통제하면 BE/ME가 평균 수익률의 강한 변동을 포착하게 되고, BE/ME를 통제하면 수익률에 사이즈 효과가 남습니다. 둘은 모두 설명력이 있습니다.**

논문에 포함된 시점인 1990년이 아니라 최근(2023년)까지도 그 이론이 유지되고 있는지를 보기 위해 다음의 코드로 확인해 보겠습니다.

```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
import requests
from io import BytesIO
from zipfile import ZipFile

# 1. Loading necessary libraries
# (Already included above)

# 2. Downloading and reading the data
url = "https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/ftp/25_Portfolios_5x5_CSV.zip"
response = requests.get(url)
zip_file = ZipFile(BytesIO(response.content))
csv_file = zip_file.open("25_Portfolios_5x5.CSV")

# Skip the initial description lines and read the CSV data into a pandas DataFrame
data = pd.read_csv(csv_file, skiprows=15)
data = data.rename(columns={"Unnamed: 0": "date"})
index_of_value = data[data['date'].str.contains("Average Equal Weighted Returns -- Monthly", na=False)].index[0]
data = data.iloc[:index_of_value,]

# Convert columns to numeric, coercing invalid parsing to NaN
for col in data.columns:
    if col != 'date':
        data[col] = pd.to_numeric(data[col], errors='coerce')

# 2. Filter and transform data
mean_values = data.drop('date', axis=1).mean()

mean_values.index = mean_values.index.str.replace('SMALL', 'ME1')
mean_values.index = mean_values.index.str.replace('BIG', 'ME5')
mean_values.index = mean_values.index.str.replace('LoBM', 'BM1')
mean_values.index = mean_values.index.str.replace('HiBM', 'BM5')

df_mean = mean_values.reset_index()
df_mean.columns = ['name', 'value']
df_mean[['size', 'BtoM']] = df_mean['name'].str.split(' ', expand=True)

# 3. Visualization
g = sns.catplot(data=df_mean, x='BtoM', y='value', col='size', kind='bar',
                col_wrap=1, height=4, aspect=2, sharey=False, order=['BM1', 'BM2', 'BM3', 'BM4', 'BM5'])

g.set_axis_labels('Book-to-Market', 'Mean Value')
g.set_titles("{col_name}")
g.set_xticklabels(rotation=90)
plt.show()

```

결과는 다음과 같습니다. 
![](/pic/2023-08-12-17-40-18.png)

*대형주를 제외하면 여전히 밸류지표는 유효성을 지니고 있습니다.*

#### 사이즈와 밸류간의 상호작용이 발생하는 이유

Table III에서 살펴봤듯이 ln(ME)와 ln(BE/ME) 모두는 주식의 평균 수익률을 설명하는데 필요한 요인입니다.

Table V에서 살펴본 것처럼 10x10 평균 수익률 행렬은 사이즈를 통제하면 BE/ME가 유의미하고 BE/ME를 통제하면 사이즈가 유의미하다는 것이 구체적인 증거입니다.

이제 왜 그런 상황이 생기는지에 대한 설명은 다음과 같은 것들이 있습니다.

* 시가총액이 작은 기업일수록 전망이 어두울 확률이 높고, 그 결과 주가가 낮아져 BE/ME가 커진다.
* 시가총액이 큰 기업들은 전망이 밝고, 더 높은 주가 상승률을 보였으며 그래서 더 낮은 BE/ME를 보인다.

![](/pic/2023-08-12-18-25-43.png)

투자자들의 정보에 대한 과잉반응에 대한 이론을 이용한 설명입니다.

다음은 기간별로 살펴본 3팩터 모델의 유의미성입니다.

![](/pic/2023-08-12-18-28-58.png)
*Table Ⅵ NYSE 주식으로 구성된 시총가중, 동일가중 포트폴리오의 세부기간 월평균수익률과, 수익률을 독립변수 (a) ln(ME), ln(BE/ME)로, 또는 (b) 베타, ln(ME), ln(BE/ME)로 월별 횡단면 파마-맥베스 회귀분석한 결과 도출된 절편과 기울기의 세부기간 평균*

표를 통해 도출한 결론은 다음과 같습니다.

* 시장 베타는 사이즈와 밸류가 입력변수로 포함되면 더욱 유의미 하지 않게 됩니다.
* 사이즈 효과는 존재합니다. 다만 그 유의미성이 뒤로 갈수록 약화됩니다.
* BE/ME와 수익률 간의 관계는 강합니다. 

때문에 파마프렌치는 사이즈와, BE/ME가 횡단면 평균주가수익률을 설명한다고 주장합니다.

#### 가치팩터(HML)과 사이즈팩터(SMB)의 수익률 분석

무엇이 좋은 투자 팩터인지를 결정하기 위해 고려해야 하는 기준들을 나열하였습니다. 이 기준들을 간단하고 이해하기 쉽게 풀어보겠습니다

1. 지속성: 팩터가 짧은 시간 동안만 효과를 보이지 않고, 오랜 기간 동안 계속해서 효과가 나타나야 합니다. 여러 경제 상황을 포함해서도 꾸준히 작동해야 합니다.
2. 범용성 : 다양한 국가, 지역, 섹터, 자산군에서도 작동해야 합니다. 특정 국가에서만 작동하는 전략이라면 이 역시 우연일 가능성이 크큽니다.
3. 강건성 : 팩터의 정의나 방식이 조금 바뀌더라도, 그 효과는 여전히 나타나야 합니다. 전략이 작동하는 이유가 명확하다면 정의가 약간씩 달라도 당연히 작동해야 하며, 결과 역시 비슷해야 합니다.
4. 투자가능성 : 이론적인 성과만이 아닌, 실제로 투자를 통해 그 효과를 누릴 수 있어야 합니다. 아무리 효과가 좋아도 실제로 투자할 때 여러 제약사항으로 인해 이익을 얻을 수 없다면 무용합니다.

HML 팩터와 SMB 팩터가 현재까지도 잘 작동하고 있는지 살펴보겠습니다.

```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
import requests
from io import BytesIO
from zipfile import ZipFile

# 2. Downloading and reading the data
url = "https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/ftp/F-F_Research_Data_Factors_CSV.zip"

# Download the content and read the first few rows to check structure
content = requests.get(url).content
csv_file = ZipFile(BytesIO(content)).open("F-F_Research_Data_Factors.CSV")
data = pd.read_csv(csv_file, skiprows=3, skipfooter=2, engine='python', delimiter=',')

# Rename the columns
data.columns = ['date', 'Mkt', 'SMB', 'HML', 'RF']
index_of_value = data[data['date'].str.contains("Annual", na=False)].index[0]
data = data.iloc[:index_of_value,]

# Convert columns to numeric, coercing invalid parsing to NaN
for col in data.columns:
    if col != 'date':
        data[col] = pd.to_numeric(data[col], errors='coerce')

# Convert returns from percentage to decimals
data[['Mkt', 'SMB', 'HML']] = data[['Mkt', 'SMB', 'HML']] / 100

# Convert cumulative returns to log scale
data['Log Cumulative SMB'] = np.log(1 + data['Cumulative SMB'])
data['Log Cumulative HML'] = np.log(1 + data['Cumulative HML'])

# Plotting
fig, axes = plt.subplots(nrows=2, ncols=1, figsize=(14, 10))

# Plot SMB with date as x-label
data['Log Cumulative SMB'].plot(ax=axes[0], title='Log Cumulative Returns of SMB')
axes[0].set_ylabel('Log Cumulative Return')
axes[0].set_xlabel('Date')
axes[0].grid(True)

# Plot HML with date as x-label
data['Log Cumulative HML'].plot(ax=axes[1], title='Log Cumulative Returns of HML', color='orange')
axes[1].set_ylabel('Log Cumulative Return')
axes[1].set_xlabel('Date')
axes[1].grid(True)

plt.tight_layout()
plt.show()

```

![](/pic/2023-08-12-21-52-25.png)
*log 스케일로 그려본 사이즈(SMB), 밸류(HML)의 누적수익률*

SMB의 경우 1980년대 이후로 크게 변동이 없어 성과를 내지 못했습니다. 이는 SMB가 그 기간 동안 거의 수익을 창출하지 못했다는 것을 의미합니다.

HML의 누적 수익률은 2000년대 중반까지 지속적으로 상승세를 보였습니다. 그러나 그 이후로는 감소하는 추세를 보였으며, 이런 하락세가 지속되었습니다. 그럼에도 불구하고, 2020년을 기점으로 다시 반등하는 모습을 보이기 시작했습니다.

HML의 팩터로의 생명력은 아직 살아있는듯 한데 사이즈 효과는 거의 사라진 것으로 판단됩니다. 왜일까요? 다음 섹션에서 설명합니다.

#### 사이즈 팩터(SMB)에 대한 분석

https://www.aqr.com/-/media/AQR/Documents/Whitepapers/Fact-Fiction-and-the-Size-Effect.pdf?sc_lang=en

소형주 효과란 가장 널리 알려진 시장 이상현상(Market Anomay)입니다.
그러나 현실은 가장 약한 이상 현상이 되었습니다. 투자에 있어서는 유의미성이 매우 떨어집니다.

논문에서 밝힌 이유는 다음과 같습니다.

- 사이즈 효과가 처음 발표된 1981년 논문에 따르면 소형주가 대형주 대비 성과가 높다.
    - 해당 논문에서는 1936년부터 1975년까지를 대상으로 했다.
    - 해당 기간을 대상으로 다시 분석해보면 성과가 매우 저조하다.
    - 수익률이나 알파는 통계적 유의성 조차 없다.
- 이유는 데이터 오류 때문이다.
  - 주식 수익률에 가장 대표적으로 사용되는 DB는 CRSP
  - 초기 연구들에서 발생하는 오류는 ‘상장폐지 편향’
  - 사이즈 효과에 대해 연구한 1980년대 논문들은 상장폐지가 된 주식 데이터가 없음
  - 상장폐지는 수익률에 악영향을 미치며, 소형주가 상장폐지가 되는 경우가 많음
  - 따라서 과거 연구에서는 소형주 수익률이 과대평가 되었음    

파마 프렌치 3 팩터 데이터에서 주석으로 다음과 같은 내용이 있습니다.
```
Please note that in 2005, CRSP completed the compilation and merging of daily data between 1925 and 1962 for securities that traded on NYSE in that period.
```
2005년 이전의 데이터는 생존편향의 오류가 있었음을 인정하는 것입니다.

그럼 보완 이후의 데이터로 사이즈 효과를 검증하면 어떻게 될까요?

![](/pic/2023-08-17-10-46-51.png)

1926년부터 2017년까지로 보면 소형주 효과가 존재합니다. 그러나 다른 알려진 팩터들과 비교해 보면 수익률의 크기가 가장 낮은 편입니다.

![](/pic/2023-08-17-10-50-18.png)
위 차트에서 보면 SMB의 수익률이 2.5%로 가장 낮고 통계적 유의성고 가장 떨어지는 것을 확인해 볼 수 있습니다.
```
BAB: 저베타 Long - 고베타 Short
HML: 가치주 Long - 성장주 Short
MOM: 고모멘텀 Long - 저모멘텀 Short
RMW: 우량주 Long - 불량주 Short
SMB: 소형주 Long - 대형주 Short
```

SMB 팩터를 1월에만 투자하는 경우와 그외의 기간에 투자하는 경우의 수익률은 다음과 같습니다. 이를 보면 소형주는 1월에 투자할 경우 수익률이 높지만 그 외의 월에는 마이너스입니다. 소형주 효과라는게 사실은 1월 효과로 대부분 설명이 되는 것입니다.

![](/pic/2023-08-17-10-53-46.png)
*January vs. Non-January Cumulative Return*

![](/pic/2023-08-17-10-55-58.png)
*SMB Performance in January and Non-January Months (full sample)*

**대부분의 소형주 효과는 초소형주로부터 발생**

시가총액 하위 1%만 제외해도 SMB 팩터의 수익률은 10% 가까이 감소합니다.
시가총액 하위 10%를 제외하면 오히려 수익률이 마이너스를 기록합니다.
결국 소형주 효과는 시가총액 하위 10%에서 발생한다고 말할 수 있습니다.

![](/pic/2023-08-17-10-59-03.png)
*Exhibit 18 Size Decile Returns for Data Subsamples*

만약 소형주 효과가 대부분 거래가 힘든 초소형주에서 나온다면 이 전략은 상당한 비용이 발생하는 전략이라고 볼 수 있습니다.
규모가 작은 소형주 일수록 거래비용이 비싸고, 시장 충격 비용이크고, 매수-매도 호가차이가 크며 거래가 거의 없는 날도 많기 때문입니다. 결국 비용까지 감안하면 수익성이 없는 전략일 수도 있습니다.

다음은 국내에서 거래되는 소형주의 거래대금 분포입니다.
거래 데이터는 거래소에서 받아왔습니다.

```python
import requests
import json
import pandas as pd
import numpy as np
from datetime import datetime

# URL and query parameters
url = 'http://data.krx.co.kr/comm/bldAttendant/getJsonData.cmd'
query = {
    'bld': 'dbms/MDC/STAT/standard/MDCSTAT01501',
    'mktId': 'ALL',
    'trdDd': datetime.now().strftime('%Y%m%d'),
    'share': 1,
    'money': 1,
    'csvxls_isNo': 'false'
}
headers = {'referer': 'http://data.krx.co.kr/contents/MDC/MDI/mdiLoader/index.cmd'}

# Send POST request
response = requests.post(url, data=query, headers=headers)

# Parse JSON data
data_json = json.loads(response.text)
data_df = pd.DataFrame(data_json['OutBlock_1'])

# Filter and transform data
data_df_filter = data_df[
    (data_df['MKT_ID'].isin(['STK', 'KSQ'])) &
    (data_df['ISU_SRT_CD'].str[-1] == '0') &
    (~data_df['ISU_ABBRV'].str.contains('스팩'))
].copy()

data_df_filter['MKTCAP'] = data_df_filter['MKTCAP'].str.replace(',', '').astype(float)
data_df_filter['ACC_TRDVAL'] = data_df_filter['ACC_TRDVAL'].str.replace(',', '').astype(float)

# Calculate percent rank and groupings
data_df_filter['p'] = data_df_filter['MKTCAP'].rank(pct=True).round(2)
data_df_filter['g_01'] = np.where(data_df_filter['p'] <= 0.01, 'Y', 'N')
data_df_filter['g_05'] = np.where(data_df_filter['p'] <= 0.05, 'Y', 'N')
data_df_filter['g_10'] = np.where(data_df_filter['p'] <= 0.10, 'Y', 'N')


# Summarize data
pivot_df = pd.melt(data_df_filter[data_df_filter['ACC_TRDVAL'] != 0],
                   id_vars=['MKTCAP', 'ACC_TRDVAL'], value_vars=['g_01', 'g_05', 'g_10'])
pivot_group = pivot_df.groupby(['variable', 'value']).agg(
    max_mktcap=pd.NamedAgg(column='MKTCAP', aggfunc='max'),
    median_trdval=pd.NamedAgg(column='ACC_TRDVAL', aggfunc='median'),
    max_trdval=pd.NamedAgg(column='ACC_TRDVAL', aggfunc='max')
)
pivot_group['max_mktcap'] /= 100000000
pivot_group['median_trdval'] /= 100000000
pivot_group['max_trdval'] /= 100000000

# Filter and display results
pivot_group = pivot_group[pivot_group.index.get_level_values('value') == 'Y']
pivot_group.reset_index(inplace=True)
pivot_group.drop(columns='value', inplace=True)
print(pivot_group)

```

결과
```
  variable  max_mktcap  median_trdval  max_trdval
0     g_01  258.458092       1.110167   16.789271
1     g_05  358.698555       1.144940  273.439670
2     g_10  441.830054       1.429848  273.439670
```
국내 상장 주식들 중에서 시가총액 하위 10%이하 종목의 일평균 거래대금은 1억남짓입니다. 이정도면 개인투자자가 투자 하기에도 규모가 부족합니다. 하루 총 거래대금이 1억이면 호가에 몇십만원 정도밖에 걸려있지 않다는 뜻이이 때문입니다.

#### 가치투자팩터(HML)에 대한 분석

밸류란 싼 주식이 비싼 주식 대비 성과가 좋은 현상을 말합니다. 따라서 밸류 프리미엄은 싼 자산을 매수하고 비싼 자산을 공매도 함으로써 얻을 수 있습니다.
지난 87년간의 주식 연구에 의하면 밸류 프리미엄은 미국 뿐만 아니라 40여개 국가의 주식시장에 존재합니다.

다음은 HML 팩터가 아닌 공매도 없이 상위 10%의 BookToMarket 팩터값을 가지는 종목들에 투자했을 경우의 로그 수익률 입니다.

```python
import pandas as pd
import matplotlib.pyplot as plt
import zipfile
import requests
from io import BytesIO
import re

# Define the function to download and process the Fama-French data
def download_french_data(url):
    response = requests.get(url)
    with zipfile.ZipFile(BytesIO(response.content)) as z:
        file_name = z.namelist()[0]
        with z.open(file_name) as f:            
            lines = f.readlines()
            skiprows = 0
            for line in lines:
                if re.match(b'^[0-9]{6}', line):
                    break
                skiprows += 1
            
            nrows = 0
            for line in lines[skiprows:]:
                if line.strip():
                    nrows += 1
                else:
                    break
        data = pd.read_csv(z.open(file_name), skiprows=skiprows-1, nrows=nrows-1, index_col=0, parse_dates=True)
    data.index = pd.to_datetime(data.index, format='%Y%m')
    return data

# Download the Fama-French data
url = "https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/ftp/Portfolios_Formed_on_BE-ME_CSV.zip"
BtoM = download_french_data(url)
BtoM = BtoM[['Hi 10']]

Mkt_url = "https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/ftp/F-F_Research_Data_Factors_CSV.zip"
Mkt = download_french_data(Mkt_url)
Mkt['Mkt'] = Mkt['Mkt-RF'] - Mkt['RF']
Mkt = Mkt[['Mkt']]

# Join the dataframes
merged_df = pd.merge(BtoM, Mkt, left_index=True, right_index=True, how='left')

# Transform the data
merged_df = merged_df / 100
merged_df = merged_df.cumsum()

# Plot the data
plt.figure(figsize=(12,6))
for column in merged_df.columns:
    plt.plot(merged_df.index, merged_df[column], label=column, linewidth=1.4)
plt.legend(loc='lower right')
plt.xlabel('')
plt.ylabel('누적 수익률(로그)')
plt.show()

```

![](/pic/2023-08-17-14-56-46.png)
*상위 10%의 BookToMarket 주식으로 포트폴리오의 시장대비 우수한 성과*

다만 밸류 팩터는 대형주에서 그 성과가 약합니다.(사라지지는 않음)

![](/pic/2023-08-17-15-03-28.png)

소형주를 대상으로한 밸류 팩터의 성과는 연간 5.5%로 유의하지만 대형주는 1.7%이고 유의미하지도 않습니다.

워런 버핏과 같은 가치투자자들은, 실질적으로 우량성 등을 함께 고려합니다.
밸류 효과 자체는 High-distressed firm에서 주로 발생하기 때문에, 수익성과 같은 우량함을 지칭하는 팩터들을 추가하는 것은 좋은 방법입니다.


![](/pic/2023-08-17-15-00-35.png)

표에서 보듯이 단순히 밸류와 수익성 각각을 보는 것보다 두개를 같이 보는 경우의 샤프지수가 증가하는 것을 알 수 있습니다. 모멘텀을 함께 보는 경우도 수익률을 증가시킵니다.

![](/pic/2023-08-17-15-04-36.png)

밸류 팩터에 모멘텀을 추가하는 경우 성과가 훨씬 좋아집니다.

즉 다양한 팩터를 섞어서 사용하는 것이 좋은 전략이 될 수 있다는 뜻입니다.


## 마치며
다음 강의에는 본격적으로 팩터를 발굴하고 조합하여 포트폴리오룰 구성하는 방법을 배워보겠습니다.