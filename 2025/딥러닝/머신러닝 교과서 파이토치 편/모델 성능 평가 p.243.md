### 홀드아웃 (holdout method)

전체 데이터를 훈련 데이터셋(Training set)과 테스트 데이터셋(Test set)으로 나눈 뒤, 훈련 데이터셋을 다시 훈련 데이터셋과 검증 데이터셋(validation set)으로 나눈다.

![[06_02.png|400]]

### k-겹 교차검증 (k-fold cross validation)

훈련 데이터셋을 $k$ 개의 폴드로 랜덤하게 나누고
$k-1$ 개의 폴드(training fold)로 모델을 훈련하고 나머지 하나의 폴드(test fold)로 성능을 평가한다.

이 과정을 $k$ 번 반복하여 $k$ 개의 모델과 성능 추정을 얻는다.

그다음 서로 다른 독립적인 폴드에서 얻은 성능 추정을 기반으로 모델의 평균 성능을 계산한다.

![[06_03.png|400]]

`sklearn.model_selection.StratifiedKFold()`
`sklearn.model_selection.cross_val_score()`
`sklearn.model_selection.cross_validate()` 등을 사용할 수 있다.

### 중첩 교차 검증 (nested cross-validation)

중첩 교차 검증은 바깥쪽 k-겹 교차 검증 루프가 데이터를 훈련 폴드와 테스트 폴드로 나누고 안쪽 루프가 훈련 폴드에서 k-겹 교차 검증을 수행하여 모델을 선택한다.

모델이 선택되면 테스트 폴드를 사용하여 모델 성능을 평가한다.

![[06_07.png|400]]