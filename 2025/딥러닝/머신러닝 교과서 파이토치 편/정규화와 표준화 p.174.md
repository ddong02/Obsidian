대부분의 머신러닝과 최적화 알고리즘은 특성의 스케일이 같을 때 성능이 좋다.

정규화와 표준화는 데이터 값의 범위를 일정한 범주로 변환하여 변수 간 스케일 차이로 인한 왜곡을 줄이기 위해 수행한다. → **특성 스케일 조정**

### 정규화(normalization)

대부분 정규화는 특성의 스케일을 $[0,1]$ 범위에 맞춘다. (min-max scaling)

데이터를 정규화하기 위해 각 특성의 열마다 최소-최대 스케일 변환을 적용하여 샘플에서 새로운 값을 계산한다.

$$
\mathbf{x}^{(i)} = \frac
{\mathbf{x}^{(i)} - \mathbf{x}_{\text{min}}}
{\mathbf{x}_{\text{max}} - \mathbf{x}_{\text{min}}}
$$

```python
from sklearn.preprocessing import MinMaxScaler
mms = MinMaxScaler()
X_train_norm = mms.fit_transform(X_train)
X_test_norm = mms.transform(X_test)

### 테스트 데이터에는 .transform() 만 적용해야함.
### 모델이 훈련 데이터 분포를 기준으로 학습하기 때문에
### 테스트 데이터의 정규화에 fit을 하게 되면 입력 분포가 달라짐
```
### 표준화(standardization)

표준화는 특성의 평균을 0에 맞추고 표준 편차를 1로 만들어 정규 분포와 같은 특징을 가지도록 만든다.

$$
\mathbf{x}^{(i)}_{\text{std}} = \frac {\mathbf{x}^{(i)} - \micro_x} {\sigma_x}
$$
$\micro$ 는 특성의 평균, $\sigma$ 는 그에 해당하는 표준 편차이다.

```python
from sklearn.preprocessing import StandardScaler
stdsc = StandardScaler()
X_train_std = stdsc.fit_transform(X_train)
X_test_std = stdsc.transform(X_test)
```

