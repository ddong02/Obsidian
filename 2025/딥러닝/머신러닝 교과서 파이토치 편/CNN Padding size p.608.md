
### 출력 특성 맵의 공간 방향 차원

$$
o =
\bigg \lfloor
\frac {n+2p-m} {s}
\bigg \rfloor
+1
$$

$o$ 는 출력 특성 맵의 크기
$n$ 은 입력 특성 맵의 크기
$p$ 는 패딩 사이즈
$m$ 은 커널 크기
$s$ 는 스트라이드

![[14_12.png]]

위와 같은 구조(입력28, 커널5, 스트라이드1)에서 입력 특성 맵의 크기 = 출력 특성 맵의 크기 를 위한 패딩사이즈는

$$
28 =
\bigg \lfloor
\frac {28 + 2p - 5} {1}
\bigg \rfloor
+ 1
\quad \therefore p=2
$$
