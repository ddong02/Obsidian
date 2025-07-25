### One-Hot Encoding

순서 없는 특성에 들어 있는 고유한 값마다 새로운 더미(dummy) 특성을 만드는 것.

n개의 고유한 범주가 있을 때, 각각의 범주를 길이 n인 벡터로 표현한 뒤, 해당 범주의 인덱스만 1이고, 나머지는 0으로 만든다.

예를 들어, `['apple', 'banana', 'cherry']` 와 같은 범주형 데이터가 있을 때
apple은 `[1, 0, 0]` banana는 `[0, 1, 0]`, cherry는 `[0, 0, 1]`과 같이 나타낸다.

```python
from sklearn.preprocessing import OneHotEncoder
X = df[['color', 'size', 'price']].values
color_ohe = OneHotEncoder()
color_ohe.fit_transform(X[: 0].reshape(-1, 1)).toarray()
```

One-Hot Encoding을 사용하여 특성 간의 상관관계가 높아지면 역행렬을 계산하기 어려워 수치적으로 불안정해진다.

변수 간의 상관관계를 감소하려면 One-Hot Encoding된 배열에서 특성 열 하나를 삭제한다.

`OneHotEncoder`에서 중복된 열을 삭제하려면 다음과 같이 `drop='first'`와 `categories='auto'`로 지정한다.

`color_ohe = OneHotEncoder(categories='auto', drop='first')`

예를 들어, `'apple' = [0, 0], 'banana' = [1, 0], 'cherry' = [0,1]` 과 같이 나타낼 수 있다.

