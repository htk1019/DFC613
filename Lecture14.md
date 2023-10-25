## 14강. 인공신경망 모델과 GPT

### Common supervised algorithms(Continue..)

#### 5. Neural networks

신경망은 복잡한 주제이며, 이 장에서는 가장 간단한 신경망 구조의 기본 개념에 대해 소개합니다. 신경망은 인간의 뇌에서 아이디어를 따왔다고 하지만 사실 현재는 인간의 뇌와는 거의 관련이 없다는 것에 대부분의 전문가들이 동의합니다. 이로 인해 이들은 종종 을 인공 신경망이라고 불립니다.

금융 분야에서 신경망을 사용한 초기 참고 자료로는 Bansal과 Viswanathan(1993)과 Eakins, Stansell, Buck(1998)이 있으며, 각각 가격 결정 커널의 비선형 형태를 추정하고, 주식에 대한 기관 투자와 기업의 속성 간의 관계를 식별하고 수량화하는 것을 목표로 합니다. Burrell 및 Folarin(1997)의 초기 검토에서는 1990년대 동안 신경망의 금융적 적용이 나열되어 있습니다.

최근 연구에서는 Sezer, Gudelek 및 Ozbayoglu(2019), Jiang(2020), Lim 및 Zohren(2020)이 금융 시계열 예측을 위한 딥 러닝 모델의 사용을 조사했습니다. 요즘은 신경망이라면 무조건 딥 러닝으로 치환하는 추세입니다.

Feng, Polson 및 Xu(2019)는 주식 수익률의 단면을 가장 잘 설명하는 팩터를 찾기 위해 신경망을 사용했습니다. Gu, Kelly 및 Xiu(2020b)는 기업 속성과 거시 경제 변수를 미래 수익에 매핑하여 예측 모델을 개발했습니다. Luyang Chen, Pelger 및 Zhu(2020)는 복잡한 신경망 구조를 사용하여 가격 결정 커널을 추정했으며, 이 정보를 기반으로 포트폴리오 구축에 사용하였습니다.

**퍼셉트론**

퍼셉트론은 신경망의 기원 중 하나로, Rosenblatt (1958)에 의해 처음 소개되었습니다. 이는 이진 분류를 목표로 하며, 여기서 출력은 투자하지 않음(0)과 투자함(1)으로 가정하겠습니다. 

퍼셉트론은 다수의 입력을 받아 하나의 신호를 출력합니다. 여기서 신호는 하나의 고정된 숫자가 아니라 전류나 강물처럼 흐름이 있는 것을 상상하는게 이해하는데 도움이 됩니다.
퍼셉트론의 출력은 1이나 0 두가지 값을 가질 수 있습니다.
입력값에 따라 출력값이 1이나 0이 되는 것입니다.

![](/pic/2023-09-26-08-43-19.png)

*그림 1 : 퍼셉트론*

그림 1은 입력으로 2개의 신호를 받은 퍼셉트론의 예 입니다. $x_1$, $x_2$ 등은 입력신호, y는 출력신호를 뜻하고 $w_1$, $w_2$는 가중치를 뜻합니다.

입력신호가 뉴런에 보내질 때는 각각 고유한 가중치가 곱해집니다. ($w_1*x_1 + w_2*x_2$)

뉴런에서 보내온 신호의 총합이 정해진 한계를 넘을 때 1을 출력합니다.
이를 '뉴런이 활성화 한다.' 라고 표현하기도 합니다. 그 한계를 임계값이라고 하고 기호로 $\theta$를 사용하곤 합니다.

퍼셉트론의 동작원리는 이게 다입니다. **인공신경망도 이 구조에서 크게 벗어나지 않습니다.**

$$
y= \left\{
    \begin{array}{lll}
    1&\text{if  } w_1*x_1 + w_2*x_2 > \theta\\

    0&\text{if  } w_1*x_1 + w_2*x_2 \leq \theta 
    \end{array}\right.
$$

퍼셉트론은 입력 신호 각각에 고유한 가중치를 부여합니다. 각 가중치는 신호가 결과에 주는 영향력을 조절하는 요소로 작용합니다. 즉 가중치가 클수록 해당 신호가 그만큼 더 중요함을 뜻합니다.

입력변수를 벡터로 표현하여 표현하면 다음과 같습니다.

$$
f(\mathbf{x})=\left\{\begin{array}{lll}
1 & \text{if } \mathbf{x}'\mathbf{w}+b >0\\
0  &\text{otherwise}
\end{array}\right.
$$

$x$ 는 입력벡터입니다.

$w$ 는 가중치 벡터입니다.

$b$는 편향입니다.

**논리회로를 퍼셉트론 구조로 구현하기**

컴퓨터는 기본적으로 논리회로를 기반으로 하고 있습니다. 이러한 논리 회로의 조합으로 무한한 표현이 가능한 것입니다. 퍼셉트론으로 논리회로를 구현해 보겠습니다.

<table style="text-align: center;">
  <tr>
    <th colspan="3" style="text-align: center;border: 1px solid" >AND</th>
  </tr>
  <tr>
    <td colspan="2" style="text-align: center;border: 1px solid">INPUT</td>    
    <td style="text-align: center;;border: 1px solid">OUTPUT</td>
  </tr>
  <tr>
    <td style="text-align: center;;border: 1px solid">0</td>
    <td style="text-align: center;;border: 1px solid">0</td>
    <td style="text-align: center;;border: 1px solid">0</td>    
  </tr>
  <tr>
    <td style="text-align: center;;border: 1px solid">1</td>
    <td style="text-align: center;;border: 1px solid">0</td>
    <td style="text-align: center;;border: 1px solid">0</td>    
  </tr>
  <tr>
    <td style="text-align: center;;border: 1px solid">0</td>
    <td style="text-align: center;;border: 1px solid">1</td>
    <td style="text-align: center;;border: 1px solid">0</td>    
  </tr>
  <tr>
    <td style="text-align: center;;border: 1px solid">1</td>
    <td style="text-align: center;;border: 1px solid">1</td>
    <td style="text-align: center;;border: 1px solid">1</td>    
  </tr>
</table>


이 논리회로를 퍼셉트론으로 구하려면 우리는 $w1$, $w2$, $\theta$의 값을 정하면 됩니다. 해답은 많으니 하나만 고릅시다. 가령 ($w1$, $w2$, $\theta$)가 (0.5,0.5,0.7), (1.0,1.0,1.0), (0.5,0.5,0.8)은 AND 게이트의 조건을 만족합니다.

다음으로 살펴볼 것은 NAND게이트와 OR 게이트입니다.

<table style="text-align: center;">
  <tr>
    <th colspan="3" style="text-align: center;border: 1px solid" >NAND</th>
  </tr>
  <tr>
    <td colspan="2" style="text-align: center;border: 1px solid">INPUT</td>    
    <td style="text-align: center;;border: 1px solid">OUTPUT</td>
  </tr>
  <tr>
    <td style="text-align: center;;border: 1px solid">0</td>
    <td style="text-align: center;;border: 1px solid">0</td>
    <td style="text-align: center;;border: 1px solid">1</td>    
  </tr>
  <tr>
    <td style="text-align: center;;border: 1px solid">1</td>
    <td style="text-align: center;;border: 1px solid">0</td>
    <td style="text-align: center;;border: 1px solid">1</td>    
  </tr>
  <tr>
    <td style="text-align: center;;border: 1px solid">0</td>
    <td style="text-align: center;;border: 1px solid">1</td>
    <td style="text-align: center;;border: 1px solid">1</td>    
  </tr>
  <tr>
    <td style="text-align: center;;border: 1px solid">1</td>
    <td style="text-align: center;;border: 1px solid">1</td>
    <td style="text-align: center;;border: 1px solid">0</td>    
  </tr>
</table>

<table style="text-align: center;">
  <tr>
    <th colspan="3" style="text-align: center;border: 1px solid" >OR</th>
  </tr>
  <tr>
    <td colspan="2" style="text-align: center;border: 1px solid">INPUT</td>    
    <td style="text-align: center;;border: 1px solid">OUTPUT</td>
  </tr>
  <tr>
    <td style="text-align: center;;border: 1px solid">0</td>
    <td style="text-align: center;;border: 1px solid">0</td>
    <td style="text-align: center;;border: 1px solid">0</td>    
  </tr>
  <tr>
    <td style="text-align: center;;border: 1px solid">1</td>
    <td style="text-align: center;;border: 1px solid">0</td>
    <td style="text-align: center;;border: 1px solid">1</td>    
  </tr>
  <tr>
    <td style="text-align: center;;border: 1px solid">0</td>
    <td style="text-align: center;;border: 1px solid">1</td>
    <td style="text-align: center;;border: 1px solid">1</td>    
  </tr>
  <tr>
    <td style="text-align: center;;border: 1px solid">1</td>
    <td style="text-align: center;;border: 1px solid">1</td>
    <td style="text-align: center;;border: 1px solid">1</td>    
  </tr>
</table>

NAND 게이트를 표현하려면 (-0.5,-0.5,-0.7) 조합으로 할수 있습니다. 사실 AND 게이트를 구현하는 매개 변수의 부호를 모두 반전 하면 NAND 게이트가 됩니다. OR 게이트는 (1.5,1.5,1)을 사용하면 구현 됩니다. 이들은 답이 될수 있는 수많은 조합들 중에 하나입니다.

그렇다면 퍼셉트론으로 AND, NAND, OR 논리회로를 모두 표현할 수 있음을 알았습니다.

이를 파이썬으로 구현해볼까요?

```python
def AND(x1,x2):
    w1, w2, theta = 0.5,0.5,0.7
    tmp = x1*w1 + x2*w2
    if tmp<=theta:
        return 0
    elif tmp>theta:
        return 1
```

위 공식은 이해하기는 쉽지만 신경망 구현을 위해서는 되도록 다음의 형식을 따라야 합니다. $\theta$를 -b로 치환하면 다음과 같은 공식이 됩니다.

$$
y= \left\{
    \begin{array}{lll}
    1&\text{if  } w_1*x_1 + w_2*x_2 + b > 0 \\

    0&\text{if  } w_1*x_1 + w_2*x_2 + b \leq 0
    \end{array}\right.
$$

그럼 이 공식으로 AND를 다시 구현해 보겠습니다.

```python
import np

def AND(x1,x2):
    x = np.array([x1,x2])
    w = np.array([0.5,0.5])
    b = -0.7
    tmp = np.sum(w*x) + b
    if tmp<=0:
        return 0
    else:
        return 1

```

여기서 -Θ 가 편향 b로 치환되었습니다. w는 각 입력 신호가 결과에 주는 영향력(중요도)을 조절하는 매개변수고, b(편향)는 뉴런이 얼마나 쉽게 활성과 하느냐를 조정하는 매개변수 입니다. 

다음은 NAND와 OR 게이트를 구현해 보겠습니다.

```python

def NAND(x1,x2):
    x = np.array([x1,x2])
    w = np.array([-0.5,-0.5])
    b = 0.7
    tmp = np.sum(w*x) + b
    if tmp<=0:
        return 0
    else :
        return 1

def OR(x1,x2):
    x = np.array([x1,x2])
    w = np.array([0.5,0.5])
    b = -0.2
    tmp = np.sum(w*x)+b
    if tmp<=0 :
        return 0
    else :
        return 1

```

가중치가 w와 b라면 결국 AND, NAND, OR는 모두 같은 구조의 퍼셉트론인데 가중치 매개변수의 값의 차이만 있습니다. 가중치가 변하면 AND도 되었다가 OR도 되었다고 NAND도 되는 것입니다.

하지만 XOR게이트는 좀 다릅니다.

XOR 게이트는 배타적 논리합이라는 논리 회로입니다. Input이 둘중 한개만 1일때 1을 출력합니다.

<table style="text-align: center;">
  <tr>
    <th colspan="3" style="text-align: center;border: 1px solid" >XOR</th>
  </tr>
  <tr>
    <td colspan="2" style="text-align: center;border: 1px solid">INPUT</td>    
    <td style="text-align: center;;border: 1px solid">OUTPUT</td>
  </tr>
  <tr>
    <td style="text-align: center;;border: 1px solid">0</td>
    <td style="text-align: center;;border: 1px solid">0</td>
    <td style="text-align: center;;border: 1px solid">0</td>    
  </tr>
  <tr>
    <td style="text-align: center;;border: 1px solid">1</td>
    <td style="text-align: center;;border: 1px solid">0</td>
    <td style="text-align: center;;border: 1px solid">1</td>    
  </tr>
  <tr>
    <td style="text-align: center;;border: 1px solid">0</td>
    <td style="text-align: center;;border: 1px solid">1</td>
    <td style="text-align: center;;border: 1px solid">1</td>    
  </tr>
  <tr>
    <td style="text-align: center;;border: 1px solid">1</td>
    <td style="text-align: center;;border: 1px solid">1</td>
    <td style="text-align: center;;border: 1px solid">0</td>    
  </tr>
</table>

앞에서 사용했던 방법으로 가중치 매개변수 값을 구해 구현하려고 하면 안될 겁ㄴ디ㅏ.
왜냐하면 퍼셉트론은 '선형' 영역으로 구분되는 경우만 표현할 수 있다는 한계가 있기 때문입니다.

OUTPUT값이 올라가서나 떨어지거나 한 방향으로만 향해 갔다는 뜻입니다.
(OR : 0-1-1-1), (AND : 0-0-0-1), (NAND :1-1-1-0)

그런데 XOR은 비선형입니다. (XOR : 0-1-1-0)

그래서 XOR를 표현하려면 다층 퍼셉트론이 필요합니다.

XOR 게이트를 구현하려면 NAND,OR, AND를 조합하면 됩니다.

![](/pic/2023-09-26-11-56-38.png)

```python

def XOR(x1,x2):
    s1 = NAND(x1,x2)
    s2 = OR(x1,x2)
    y = AND(s1,s2)
    return y
```

이 함수는 XOR 게이트의 결과를 출력합니다. 방금 구현한 XOR을 퍼셉트론으로 표현하면 다음과 같습니다.

![](/pic/2023-09-26-11-59-34.png)

실제로 AND, OR가 단층 퍼셉트론인 데 반해 XOR는 2층 퍼셉트론입니다. 이처럼 층이 여러개인 퍼셉트론을 다층 퍼셉트론 이라고 합니다.

다층 퍼셉트론을 사용하면 아무리 복잡한 회로도 만들 수 있습니다. 이러한 다층 퍼셉트론은 컴퓨터를 만들 정도로 복잡한 표현도 해냅니다.

인공신경망은 이 퍼셉트론의 아이디어로 부터 시작됩니다.

**활성화 함수(Activation Function)**

퍼셉트론에서 가중치를 결정할 때 우리는 자의적인 숫자를 입력했습니다. AND, OR, NAND 등의 게이트 함수를 만들 때 여러가지의 답들 중에서, 답이 잘 나오는 적절하다 싶은 아무값을 직접 입력한 것입니다.

신경망은 이 가중치를 데이터를 학습한 모델이 결정해줍니다.

퍼셉트론이 어떻게 신경망 구조로 넘어가는지 한번 배워보겠습니다.

![](/pic/2023-09-28-12-26-55.png)

순서대로 가장 왼쪽을 입력층, 가장 오른쪽을 출력층, 중간줄은 전부 은닉층이라고 합니다. 

은닉층은 신경망 모델을 사용하는 사용자에게는 보이지 않습니다. 그래서 '은닉'인 것이죠.

신경망과 퍼셉트론의 차이는 활성화 함수의 존재입니다.
다른말로 하면 퍼셉트론은 신경망에서 활성화 함수를 단순하게 결정한 하위 버전이라고 할수 있습니다.

활성화 함수는 입력 신호를 그대로 사용하지 않고 변환하는 함수입니다. 이름이 '활성화' 함수로 붙은 이유는 입력 신호들을 받아 활성화를 일으킬지 말지를 결정하는 역할을 하기 때문입니다.

구조를 자세히 그려보면 다음과 같습니다.

![](/pic/2023-09-28-12-21-26.png)

기존 입력 값의 가중합 $w_1*x_1 + w_2*x_2 + w_3*x_3$이 활성화 함수 f에 들어간 다음 z가 결정됩니다.

여기서 이 원 하나를 뉴런 또는 노드라고 부릅니다.

**많이 사용하는 활성화 함수**

퍼셉트론은 임력된 값이 특정 값을 벗어나면 활성화 되는데 이는 신경망에서 '계단 함수'를 활성화 함수로 사용한 것과 같습니다. 활성화 함수에는 계단함수 말고도 많은 종류가 있습니다.

1 ) 계단함수

계단함수를 파이썬으로 구현하면 다음과 같습니다.

```python
def step_function(x):
  y = x>0
  return y
```

입력값 x가 -5부터 5까지의 범위를 가진다면 이 함수의 그래프는 다음과 같습니다.
잘 동작하는지 살펴보겠습니다.

```python
x = np.arange(-5.0, 5.0, 0.1)
y = step_function(x)
plt.plot(x,y)
plt.ylim(-0.1,1.1)
plt.show()
```

![](/pic/2023-09-28-12-34-09.png)

이는 입력 x값이 0보다 크면 1을 반환하고 아니면 0을 반환하는 함수입니다. 

2 ) 시그모이드 함수

신경망에서 정말 자주 사용되는 활성화 함수입니다.

$$
f(x) = \frac{1}{1+e^{-x}}
$$

시그모이드 함수에 1.0과 2.0을 입력하면 출력값은 f(1)=0.731, f(2) = 0.880 이 출력됩니다.

시그모이드 함수를 구현하면 다음과 같습니다.
```python
import numpy as np

def sigmoid(x):
    return 1/(1+np.exp(-x))
```

잘 동작하는지 살펴보겠습니다.
```python
x = np.arange(-5.0, 5.0, 0.1)
y = sigmoid(x)
plt.plot(x,y)
plt.ylim(-0.1,1.1)
plt.show()
```

![](/pic/2023-10-06-12-15-23.png)

잘 작동합니다.

시그모이드 함수와 계단 함수와 눈에 띄는 차이는 결과의 '매끄러움' 입니다.
매끄럽다는 뜻은 출력값이 연속적으로 변화한다는 뜻입니다. 계단함수는 갑자기 변하기 때문에 매끄럽지 않습니다. 이 '매끈함' 이라는 속성이 신경망 학습에서 중요한 역할을 하게 됩니다.
계단함수는 0과 1중 하난의 값만 돌려주는 반면 시그모이드 함수는 실수(0.731..0.880..) 등을 돌려줍니다.

그러나 두 함수는 어떤 관점에서는 같은 모양을 하고 있습니다. 둘다 입력값이 작을때는 출력이 0에 가깝고, 입력값이 클때는 1(혹은 100%)에 가까워 지는 것입니다.

또한 두 함수는 모두 '비선형 함수' 라는 공통점을 가집니다.


 **신경망에서는 활성화 함수로 비선형 함수를 사용해야 합니다.**

선형함수를 이용하면 신경망의 층을 깊게 해도 의미가 없어지기 때문입니다. 
직관적으로 이해할만한 예시를 알려드리곘습니다.
만약 활성함수가 선형함수인 h(x) = cx 라면 어떻게 될까요?

3층 네트워크가 되면 y(x) = h(h(h(x))) 가 됩니다. 이 결과는 y(x) = c*c*c*x 입니다.
결국 c가 $c^3$이 될 뿐입니다. 즉 여러층으로 구성해도 구조가 변하지 않습니다.

하지만 꼭 선형성이 필요한 경우도 있는데 그럴때는 ReLU 유형의 함수를 사용합니다.

3 ) ReLU (Rectified Linear Unit)

ReLU (Rectified Linear Unit) 함수는 신경망, 특히 딥 러닝에서 널리 사용되는 활성화 함수입니다. ReLU 함수는 매우 간단한 비선형 함수로, 음수 값을 0으로 만들고 양수 값은 그대로 둡니다. 계단함수와 선형함수가 결합된 형태입니다. 비선형이지만 선형적인 특성을 가지고 있습니다. 수학적 표현은 단순합니다.

$$f(x) = max(0,x)$$


구현도 굉장히 쉽습니다.

```python
def relu(x):
    return np.maximum(0,x)
```

잘 동작하는지 살펴보겠습니다.
```python
x = np.arange(-5.0, 5.0, 0.1)
y = relu(x)
plt.plot(x,y)
plt.show()
```

![](/pic/2023-10-06-12-31-02.png)

**간단한 신경망 구현 예시**

다음과 같이 생긴 신경망을 패키지 없이 구현해 보겠습니다.

![](/pic/2023-10-06-12-35-40.png)


*디음 코드는 굳이 힘들게 암기할 필요가 없습니다. 생각보다 쉽게 그냥 구현이 되는구나 정도로 이해하면 됩니다.*

```python

import numpy as np

def sigmoid(x):
    return 1/ (1+np.exp(-x))

def relu(x):
    return np.maximum(0.5,x)

def init_network():
    network = {}
    network['w1'] = np.array([[0.1,0.3,0.5],[0.2,0.4,0.6]])
    network['b1'] = np.array([0.1,0.2,0.3])
    network['w2'] = np.array([[0.1,0.4],[0.2,0.5],[0.3,0.6]])
    network['b2'] = np.array([0.1,0.2])
    network['w3'] = np.array([[0.1,0.3],[0.2,0.4]])
    network['b3'] = np.array([0.1,0.2])
    
    return network

def forward(network, x):
    W1, W2, W3 = network['w1'], network['w2'], network['w3']
    b1, b2, b3 = network['b1'], network['b2'], network['b3']
    
    a1 = np.dot(x,W1)+b1
    z1 = sigmoid(a1)
    a2 = np.dot(z1,W2)+b2
    z2 = sigmoid(a2)
    a3 = np.dot(z2,W3)+b3
    y = relu(a3)

    return y

network = init_network()
x = np.array([1.0,0.5])
y = forward(network,x)

```

가중치와 Bias인 b값은 임의로 제 마음대로 넣은 값입니다.
그러면 입력에 1.0과 0.5를 입력하는 경우 네트워크를 타고 그 결과는 다음과 같습니다.
 
```python
array([0.5       , 0.69627909])
```


init_network() 와 forward() 라는 함수를 정의했습니다. init_network() 함수는 가중치와 편향을 초기화 하고 이들을 딕셔너리 변수인 network에 저장합니다.  그리고 forward() 함수는 입력 신호를 출력으로 변환하는 처리 과정을 모두 구현하고 있습니다.
함수이름이 forward() 라 한 것은 신호가 순방향으로 전달되기 때문입니다.
이로써 신경망의 순방향 구조가 구현이 되었습니다.

여기서는 sigmoid를 사용했습니다. relu를 사용해도 되고 분류에 많이 사용되는 softmax를 사용해도 됩니다.

**(기타) softmax 활성화 함수**

```python
def softmax(x):
    c = np.max(x) #연산 숫자가 너무 커지는 경우 오버플로가 발생할 수 있어서 만듦
    exp_x = np.exp(x-c) #오버플로 대책
    sum_exp_x = np.sum(exp_x)
    y = exp_x / sum_exp_a
    
    return y

```

실제 신경망은 이 구조에서 벗어나지 않습니다.
다만 현재까지는 우리가 가중치 W를 임의로 정했는데 딥러닝을 통하면 최적의 W값이 학습 데이터에 적합하게 학습을 통해 정해진다는 차이만 있습니다.

계속 반복해서 오류를 줄여나가는 방향으로 가중치를 결정하여 오류가 가장 적게 되는 W를 찾는 것이 학습이라고 이해하면 됩니다.

그럼 이제 신경망을 활용하여 주식 모델링에 활용해 보겠습니다.

keras를 사용하여 간단한 인공신경망(ANN) 모델을 구축하겠습니다.

*tensorflow 와 keras가 설치되어 있어야 합니다.*

```
> pip install tensorflow
> pip install keras
```

두개의 은닉층과 하나의 출력층으로 구성되어 있고 활성화 함수는 ReLU로 만들었고 출력층은 Softmax로 구성했습니다.

```python
from keras.models import Sequential
from keras.layers import Dense
from keras.utils import to_categorical

from matplotlib import pyplot as plt
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

# One-hot encoding the target variable
y_train_encoded = to_categorical(y_train)
y_test_encoded = to_categorical(y_test)


# 인공신경망 모델 구축
model = Sequential()
model.add(Dense(128, input_dim=X_train.shape[1], activation='relu'))
model.add(Dense(64, activation='relu'))
model.add(Dense(3, activation='softmax'))  # 3개의 클래스이므로 출력 유닛은 3개입니다.


# 모델 컴파일
model.compile(loss='categorical_crossentropy', optimizer='adam', metrics=['accuracy'])

# 모델 학습
model.fit(X_train, y_train_encoded, epochs=50, batch_size=10, verbose=1)

# test 데이터에 대한 예측 수행
y_pred_probs = model.predict(X_test)
y_pred = np.argmax(y_pred_probs, axis=1)

# 모델 성능 평가
accuracy = accuracy_score(y_test, y_pred)  # 정확도를 계산합니다.
classification_rep = classification_report(y_test, y_pred)  # 분류 리포트를 생성합니다.

print(f"Accuracy: {accuracy}\n")
print(f"Classification Report:\n{classification_rep}")
```

학습을 완료하는 데 까지 몇십분이 소요됩니다. 50으로 설정되어 있는 epochs를 아래 코드 처럼 1이나 2로 줄이면 코드가 잘 수행되는지를 좀 더 빠르게 확인해 볼 수 있습니다.

``` python
# 모델 학습
model.fit(X_train, y_train_encoded, epochs=1, batch_size=10, verbose=1)
```

결과

```
Accuracy: 0.3689570236388368

Classification Report:
              precision    recall  f1-score   support

           0       0.37      0.32      0.34      9085
           1       0.39      0.48      0.43      9049
           2       0.34      0.31      0.32      9067

    accuracy                           0.37     27201
   macro avg       0.37      0.37      0.37     27201
weighted avg       0.37      0.37      0.37     27201
```

XGBoost에 비해 미묘하지만 좀더 나은 예측률을 보여젔습니다.

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
```

투자 전략으로서의 결과는 좋지 않습니다.

![](/pic/2023-10-06-18-12-09.png)

팩터로 f1과 f1011만 사용하는 경우에는 조금 나아지긴 하지만 큰 개선은 없습니다.

![](2023-10-06-18-28-16.png)


### 인과성, 비정상성

기계 학습 (ML) 도구에 대한 주요 비판 중 하나는 기능과 레이블 간의 인과 관계를 명확히 파악할 수 없다는 것입니다. 이는 대부분의 ML 도구가 설계상 상관관계에 초점을 맞추기 때문입니다. 인과 관계는 방향성을 가지며 상관 관계보다 훨씬 강력한 관계를 나타냅니다.

최근 연구에서 주식 시장에서의 감정과 수익률 간의 관계는 이를 잘 나타내는 예시입니다. 높은 감정이 주식의 가격 상승을 유발할 수 있으며, 반대로 주식의 상승이 감정을 유발할 수도 있습니다. 최근의 연구는 감정이 수익률을 유발하는 것이 더 가능성이 높다는 결론을 내렸습니다. 반면에 뉴스와 수익률 간의 관계는 수익률이 뉴스를 만들고 뉴스가 가격 상승이나 하락을 유발한 다는게 알려져 있지만 수익률이 뉴스를 유발하는 것이 더 가능성이 높다고 나옵니다.

이런 인과관계는 명확하게 규정하기 어렵지만 데이터 수집을 다양한 시점과 소스 수행하고, 이런 다양한 환경에서 발견되는 관계가 만약 일정하게 유지된다면 이는 인과 관계를 나타낼 가능성이 높습니다. 물론 투자에서 이러한 일정한 관계가 항상 존재하는지는 확실하지 않습니다.

인과관계를 통계적으로 찾는 로직에 대표적인 것은 그레인저 인과성 (Granger causality)이 있습니다.

#### 그레인저 인과성(Granger causality)

그레인저 인과성(Granger causality)은 시계열 데이터에서 한 시계열이 다른 시계열의 미래 값을 예측하는 데 도움이 되는지를 판단하는 통계적 방법입니다.
물론 그레인저 인과성이 존재한다고 해서 실제 인과관계가 존재한다는 의미는 아닙니다. 그저 한 시계열이 다른 시계열의 정보를 포함하는 것처럼 보이는지를 나타낼 뿐입니다.
(완벽한 인과성을 통계적 기법으로 알아낼 수 있는 방법은 없습니다.)

예를들어, 두 시계열 X와 Y가 있을 때 , X가 Y의 원인이 될수 있는지 여부를 그레인저 인과성으로 판단해보고 그 결과가 나쁘지 않다면 X는 Y의 미래값을 예측하는데 도움이 될것입니다.
만약 X가 Y를 그레인저 인과한다면(X granger causes Y), Y를 예측할 때 과거의 Y값만을 사용하는 것보다 과거의 X값도 사용한 예측이 더 많은 정보를 제공하게 됩니다. 
즉, X의 변화가 Y의 변화를 예측할 수 있다는 것이고 이를 통계적인 수치로 확인해 볼 수 있습니다.

**수식적 표현**

1) $Y_t$의 예측에 과거 $Y_t$의 값을 사용 
$$Y_t = \alpha_0 + \alpha_1 Y_{t-1} + ... + \alpha_pY_{t-p} + \epsilon_t$$

2) $Y_t$의 예측에 과거 $Y_t$와 $X_t$의 과거값을 모두 사용

$$Y_t = \beta_0 + \beta_1 Y_{t-1} + ... + \beta_pY_{t-p} + \gamma_1 X_{t-1} + ... + \gamma_pX_{t-p} + \mu_t$$

이때 $\gamma_i$ 들 중에서 하나 이상이 통계적으로 유의미 하다면 그레인저 원인이 될수 있다고 봅니다.
그레인저 인과성 테스트는 F-통계량과 p-value로 주어집니다.

**주의점**
그레인저 인과성 테스트는 데이터의 정상성을 가정합니다. 따라서 테스트를 실시하기 전에 시계열 데이터가 정상성을 갖는지 확인하는 것이 중요합니다. 정상성을 갖지 않는 시계열 데이터에 그레인저 인과성 테스트를 직접 사용하는 것은 올바르지 않습니다.(예, 주식의 가격을 사용하지 말고, 수익률을 사용한다 던가, CPI를 쓰지 않고 CPI YoY를 사용하라는 것)

그레인저 인과성 테스트는 "인과성"을 전통적인 의미로 해석해서는 안 됩니다. 이 테스트는 단순히 한 시계열이 다른 시계열의 미래 값을 예측하는 데 도움이 되는지 여부만을 알려줍니다.

```python
import numpy as np
import statsmodels.api as sm
from statsmodels.tsa.stattools import grangercausalitytests
import matplotlib.pyplot as plt

# 시계열 데이터 생성
np.random.seed(123)
X = np.random.randn(100)
# Y는 X의 1시간 전 정보와 노이즈로 구성된다.
Y = np.append(np.random.randn(1), X[:-1]) + np.random.randn(100)

# 그레인저 인과성 테스트
data = np.column_stack([Y, X])
gc_test = grangercausalitytests(data, maxlag=2, verbose=True)

# 데이터 시각화
plt.figure(figsize=(12, 6))
plt.plot(X, label='X_t', color='blue')
plt.plot(Y, label='Y_t', color='red', alpha=0.7)
plt.title('Generated Time Series Data')
plt.legend()
plt.show()
```

결과
```
Granger Causality
number of lags (no zero) 1
ssr based F test:         F=122.0427, p=0.0000  , df_denom=96, df_num=1
ssr based chi2 test:   chi2=125.8565, p=0.0000  , df=1
likelihood ratio test: chi2=81.2139 , p=0.0000  , df=1
parameter F test:         F=122.0427, p=0.0000  , df_denom=96, df_num=1

Granger Causality
number of lags (no zero) 2
ssr based F test:         F=61.0114 , p=0.0000  , df_denom=93, df_num=2
ssr based chi2 test:   chi2=128.5831, p=0.0000  , df=2
likelihood ratio test: chi2=82.1382 , p=0.0000  , df=2
parameter F test:         F=61.0114 , p=0.0000  , df_denom=93, df_num=2
```

ssr based F test, ssr based chi2 test, likelihood ratio test, parameter F test의 모든 p-값이 0.0000입니다. 이는 0.05 (또는 다른 유의 수준)보다 훨씬 작습니다. 따라서, 1 타임 지연으로 그레인저 인과성이 있다고 결론을 내릴 수 있습니다. 2도 만찬가지 입니다.


### GPT

GPT (Generative Pre-trained Transformer)는 OpenAI에서 개발된 자연어 처리 모델로, Transformer 아키텍처를 기반으로 합니다. GPT에 대한 기술적인 이야기는 범위를 벗어나므로 주로 사용을 위주로 말씀드리겠습니다.

GPT의 주요 사용처는 다음과 같습니다.

1) 텍스트 생성 : GPT는 주어진 문맥을 기반으로 새로운 텍스트를 생성할 수 있습니다. 예를 들어, 시나 글쓰기, 기사 작성, 시나리오 생성 등의 작업에 활용될 수 있습니다.

2) 질의 응답 (Question Answering): GPT는 특정 질문에 대한 응답을 생성하는 데 사용될 수 있습니다. 이는 고객 지원, FAQ 시스템, 지식 검색 등에 활용될 수 있습니다.

3) 기계 번역 (Machine Translation): 텍스트를 다른 언어로 번역하는 데 GPT를 활용할 수 있습니다.

4) 요약 (Summarization): 문서나 기사를 짧게 요약하는 작업에 GPT를 사용할 수 있습니다.

5) 감정 분석 (Sentiment Analysis): 텍스트의 감정 또는 톤을 분석하는 데 GPT를 활용할 수 있습니다. 예를 들어, 상품 리뷰가 긍정적인지 부정적인지 판단하는 데 사용될 수 있습니다.

6) 프로그래밍 코드 생성 및 도움: 간단한 프로그래밍 작업에도 활용될 수 있습니다. 사용자의 설명을 바탕으로 코드 스니펫을 생성하거나 코드에 대한 설명을 제공하는 데 사용될 수 있습니다.

7) 이미지 생성 : 텍스트 설명을 기반으로 그에 해당하는 이미지를 생성할 수 있습니다. 예를 들어 "두 개의 머리를 가진 펭귄"과 같은 설명을 주면, 이에 해당하는 그림을 생성해낼 수 있습니다.

#### ChatGPT

ChatGPT는 OpenAI에서 개발된 GPT (Generative Pre-trained Transformer) 아키텍처를 기반으로 한 챗봇 모델입니다. GPT는 원래 자연어 생성 작업을 위해 설계되었지만, ChatGPT는 이를 챗봇과 대화형 응용 프로그램에 특화되게 조정한 모델입니다.

ChatGPT는 사용자의 질문과 대화의 문맥을 이해하고 그에 따라 응답을 생성합니다. 이를 통해 자연스러운 대화 흐름을 유지할 수 있습니다. 다양한 주제와 문맥에 대해 대화할 수 있습니다. 정보 제공, 스토리텔링, 문제 해결, 엔터테인먼트 등 다양한 작업도 수행할 수 있습니다.

ChatGPT는 자연어 처리 기술의 발전을 통해 사람과 기계 간의 자연스러운 대화를 가능하게 하는 한 가지 예시입니다.

#### GPT API 사용

OpenAI의 GPT를 활용하면 파이썬에서 다양한 작업을 수행할 수 있습니다. GPT는 자연어 처리를 위한 강력한 모델이므로, 텍스트와 관련된 많은 작업에 적용할 수 있습니다. OpenAI는 이를 위한 API를 제공하며, 이 API를 사용하면 사용자는 복잡한 머신 러닝 모델을 직접 구현하거나 관리할 필요 없이 GPT의 기능을 활용할 수 있습니다.

다음은 GPT API를 사용해서 문장을 생성하는 과정을 적었습니다.

**API KEY 발급**

KEY는 OPENAI 홈페이지에서 발급받을 수 있습니다. 

발급위치 : https://platform.openai.com/account/api-keys

![](/pic/2023-10-12-16-24-55.png)

Create new secret key를 눌러서 키를 발급받으시길 바랍니다.

![](/pic/2023-10-12-16-28-13.png)

처음 발급할 때 보여주는 KEY번호는 창을 닫으면 볼수 없기 때문에 메모장에 적어놓을 것을 추천합니다.

**OPEN AI 라이브러리 설치**

python에서 openai 서비스를 이용하기위해 openai 패키지를 설치해야 합니다.
command 창에서 아래 명령어로 설치할 수 있습니다.(windows 기준)

```
>> pip install openai
```

**Python에서 GPT 호출하기**

설치한 openai를 import하고 발급받은 API KEY를 사용합니다.

**잠깐!**

지금 당장은 설치한 openai와 key가 있어도 실행되지 않을 것입니다.
왜냐하면 Billing 정보를 입력해야 하기 때문입니다. 일정량 까지는 무료이지만 사용량이 늘어나면 과금을 하는 구조입니다. 개인이 공부 용도로 사용하는데는 무료로도 충분하지만 그 이상 사용할 가능성이 있기 때문에 일단 과금을 위해 지불관련 정보를 입력해야 하는데 생략했습니다.

**사용하기**

```python
import openai

OPENAI_YOUR_KEY  ='YOUR API KEY'
openai.api_key = OPENAI_YOUR_KEY

MODEL =  "gpt-3.5-turbo"

def generate_gpt_text(user_msg):
    # Define the system message
    msg= user_msg
    # Create a dataset using GPT
    response=openai.ChatCompletion.create(model="gpt-3.5-turbo", messages=[{"role": "user", "content": msg}])    
    # 응답에서 요약된 텍스트를 반환합니다.    
    return response.choices[0].message.content

user_msg = "이순신 장군에 대해서 설명해줘"

generate_gpt_text(user_msg)

```

```결과
'이순신(1545년 ~ 1598년)은 조선시대의 장군이자 민란기의 주인공으로 알려져 있습니다. 그는 조선시대 최고의 무신이자......
```

이제 원하는 질문을 user_msg에 담아서 GPT에 응답하게 할 수 있습니다.

API를 잘 활용하면 내가 보유중인 데이터에 기반하여 학습시키고 대답하게 하는 것도 가능합니다.
Function Calling이라는 기능입니다. 간단한 예시를 보여드리겠습니다.


물론 본 수업의 범위를 벗어나므로 자세한 방법은 openai의 홈페이지를 참고하시길 바랍니다.

#### 기타 LLM(llama) 사용

LLAMA는 Meta가 개발한 오픈 소스 대형 언어 모델(Large Language Model, LLM)로, OpenAI의 GPT 모델과 Google의 AI 모델인 PaLM 2에 대응하는 모델입니다. 이 모델은 거의 누구나 연구와 상업 목적으로 사용할 수 있도록 무료로 제공되는 것이 특징입니다​.
이 모델은 Hugging Face에서도 통합 지원을 받고 있습니다.
아쉽지만 llama2는 한국어를 지원하지 않습니다. 

**llama2 사용법**

설치해 사용하기 위해서는 높은 사양의 그래픽카드가 필요합니다. 또한 별도로 설치해야 하는 프로그램도 많습니다. 그래서 이 방식을 아직은 일반 사용자에게 추천하지 않습니다. 

*여기서 llama2의 설치가 가능합니다 https://ai.meta.com/llama/*

대신 허깅페이스 사이트에 접속해 설치없이 사용하는 방법이 있습니다. llama의 성능이 어느정도로 올라왔는지를 파악해 볼 수 있습니다.

https://huggingface.co/spaces/ysharma/Explore_llamav2_with_TGI

![](/pic/2023-10-17-13-22-11.png)

아직은 한글 지원도 안되고 성능도 GPT에 크게 못미치기 때문에 사용성이 떨어지지만 비용이 거의 안들기 때문에 성능이 조금이라도 더 향상되면 사용 하는 유저가 기하급수적으로 늘지 않을까 판단됩니다.


