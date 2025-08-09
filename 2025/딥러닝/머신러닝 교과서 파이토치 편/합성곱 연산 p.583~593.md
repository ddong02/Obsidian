
### 1D 합성곱

두 개의 벡터 $\boldsymbol{x}$와 $\mathbf{w}$에 대한 이산 합성곱은 $\boldsymbol{y} = \boldsymbol{x} * \mathbf{w}$ 처럼 나타낸다.
$\boldsymbol{x}$는 입력, $\boldsymbol{w}$는 필터 또는 커널이라고 부른다.

이산 합성곱의 수학적 정의는 다음과 같다.

$$
\boldsymbol{y} = \boldsymbol{x} * \mathbf{w} \rightarrow
\boldsymbol{y}[i] = \sum^{+\infty}_{k=-\infty} {\boldsymbol{x}[i-k]\mathbf{w}[k]}
$$

원본 입력 $\boldsymbol{x}$와 필터 $\mathbf{w}$가 각각 $n$개, $m$개의 원소를 가지고 $m \le n$ 이라고 할 때, 패딩된 벡터 $\boldsymbol{x}^p$의 크기는 $n+2p$이다. ($p$는 추가된 패딩 수)

$$
\boldsymbol{y} = \boldsymbol{x} * \mathbf{w} \rightarrow
\boldsymbol{y}[i] = \sum^{k=m-1}_{k=0} {\boldsymbol{x}^p[i + (m-1) - k]\mathbf{w}[k]}
$$

필터를 뒤집느냐의 차이점이 있는 합성곱과 교차상관(cross-correlation)은 서로 비슷하다.
사실 대부분의 딥러닝 프레임워크에서는 합성곱이 아닌 교차상관을 수행하지만 관례상 합성곱이라고 부른다.

$$
\boldsymbol{y} = \boldsymbol{x} * \mathbf{w} \rightarrow
\boldsymbol{y}[i] = \sum^{+\infty}_{k=-\infty} {\boldsymbol{x}[i+k]\mathbf{w}[k]}
$$

**합성곱 출력 크기 계산**
합성곱 출력 크기는 입력 벡터 위를 필터 $\mathbf{w}$가 이동하는 전체 횟수로 결정된다.

입력 벡터의 크기를 $n$, 필터 크기를 $m$이라고 하고, 패딩이 $p$, 스트라이드가 $s$인 $\boldsymbol{x} * \mathbf{w}$ 출력 크기는 다음과 같이 계산된다. $\lfloor \cdot \rfloor$ 는 버림연산

$$
o =
\bigg \lfloor
\frac {n+2p-m} {s}
\bigg \rfloor
+1
$$

### 2D 이산 합성곱

$m_1 \le n_1$이고 $m_2 \le n_2$ 인 행렬 $\boldsymbol{X}_{n_1 \times n_2}$ 와 필터 행렬 $\boldsymbol{W}_{m_1 \times m_2}$ 같은 2차원 입력을 다룰 때, $\boldsymbol{X}$ 와 $\boldsymbol{W}$ 의 2D 합성곱 결과는 행렬 $\boldsymbol{Y} = \boldsymbol{X} * \boldsymbol{W}$ 가 된다.

$$
\boldsymbol{Y} = \boldsymbol{X} * \boldsymbol{W} \rightarrow
Y[i, j] = \sum^{+\infty}_{k_1=-\infty} \sum^{+\infty}_{k_2=-\infty}
X[i-k_1, j-k_2] W[k_1, k_2]
$$

