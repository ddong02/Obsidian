### 로지스틱 회귀의 **활성화 함수** → **로지스틱 시그모이드 함수**

- **오즈비(odds ratio)**
특정 이벤트가 발생할 확률

$$
\frac {P}{(1-P)}
$$
여기서 $P$ 는 예측하려는 대상을 의미한다.

- **로짓(logit) 함수**
오즈비(odds ratio)에 로그 함수를 취한 함수 (로그 오즈)

$$
\text{logit}(P) = \log \frac{P}{(1-P)}
$$
여기서 $\log$는 자연 로그이다.

- **로지스틱 시그모이드 함수 (logistic sigmoid function)**
logit 함수를 거꾸로 뒤집은 함수

$$
\sigma(z) = \frac {1}{1 + e^{-z}}
$$

![[03_02 1.png|400]]

가중치와 절편 파라미터 $w$와 $b$를 활용하여 특성 $x$에 대한 시그모이드 함수의 출력을 특정 샘플이 클래스 1에 속할 확률 $\sigma(z) = p \, (y=1| \boldsymbol{x}; \mathbf{w}, b)$로 해석한다.

예측 확률은 임계 함수를 사용하여 간단하게 이진 출력으로 바꿀 수 있다.

$$
\hat{y} =
\begin {cases}
1 \quad \sigma(z) \ge 0.5 \\
0 \quad \text{else}
\end {cases}
$$

이는 아래와 동일하다

$$
\hat{y} =
\begin {cases}
1 \quad z \ge 0.0 \\
0 \quad \text{else}
\end {cases}
$$


### 이진 분류를 위한 로지스틱 회귀의 **손실함수**

$$
L(\mathbf{w}, b) = \sum_{i=1}^n [-y^{(i)} \log{(\sigma(z^{(i)})) - (1-y^{(i)}) \log({1-\sigma(z^{(i)})}})]
$$

샘플이 하나일 때

$$
L(\sigma (z),\, y;\, \mathbf{w}, b) =
\begin{cases}
-\log{(\sigma (z))} \quad \quad \;\;\, \text{y=1 일 때}\\
-\log{(1 - \sigma(z))} \quad \text{y=0 일 때}
\end{cases}
$$
