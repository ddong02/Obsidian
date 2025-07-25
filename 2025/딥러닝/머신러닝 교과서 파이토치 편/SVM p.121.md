### SVM

**SVM**은 **마진(margin)**을 최대화 하는 것을 목표로 한다.

마진은 클래스를 구분하는 결정 경계와 이와 가장 가까운 훈련 샘플 사이의 거리로 정의한다.
이런 샘플을 **서포트 벡터(support vector)**라고 한다.

![[03_09.png]]

### 커널 기법
SVM에서 비선형 데이터를 다루기 위해서는 매핑 함수 $\phi$ 를 사용하여 원본 특성의 비선형 조합을 선형적으로 구분되는 고차원 공간에 투영하는 것이다.

이런 매핑 방식의 한 가지 문제점은 새로운 특성을 만드는 계산 손실이 매우 비싸다는 것이다.

이를 해결하기 위해서 **커널 기법(kernel trick)**이 사용된다.

점곱 $\mathbf{x}^{(i)T} \mathbf{x}^{(j)}$ 를 $\phi \left(\mathbf{x}^{(i)} \right)^T \; \phi \left(\mathbf{x}^{(j)}\right)$ 로 바꾼다.

두 포인트 사이 점곱을 계산하는 데 드는 높은 손실을 절감하기 위해 **커널 함수(kernel function)**를 정의한다.

$$
\mathcal{K}
\left (
\mathbf{x}^{(i)}, \mathbf{x}^{(j)}
\right ) =
\phi \left( x^{(i)} \right) ^ T
\phi \left( x^{(j)} \right)
$$
가장 널리 사용되는 커널 중 하나는 **방사 기저 함수(Radial Basis Function, RBF)**이다. **가우스 커널(Gaussian Kernel)**이라고 한다.

$$
\mathcal{K}
\left(
\mathbf{x}^{(i)}, \mathbf{x}^{(j)}
\right) =
\exp
\left(
- \frac{||\mathbf{x}^{(i)} - \mathbf{x}^{(j)}||^2}{2 \sigma^2}
\right) =
\exp
\left (
-\gamma
||\mathbf{x}^{(i)} - \mathbf{x}^{(j)}||^2
\right), \quad \gamma = \frac{1}{2\sigma^2}
$$

