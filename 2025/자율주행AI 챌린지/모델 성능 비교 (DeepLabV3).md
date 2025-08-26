
# 성능 계산 공식

$$
Score = mIoU \cdot \max(0, 1- \frac{max(0, T_{infer} - 3ms)}{7ms})
$$

공식에 의하면 $Score$ 최댓값은 $T_{infer} \le 3ms$ 일 때의 값이므로 $mIoU$ 임

https://arxiv.org/pdf/2101.06085
![[Pasted image 20250826104652.png]]

세그멘테이션 baseline 모델인 DDRNet의 **DDRNet-39**를 기준으로 잡음
위의 기준 모델의 $Score$ 점수를 10점이라고 가정하고 다른 모델의 $Score$를 구하는 것이 목표
(50점을 기준으로 했더니 $W$값이 0이 되는 경우가 많았음)

$$
W = \max(0, 1- \frac{max(0, T_{infer} - 3ms)}{7ms})
$$
$W$를 위와 같이 정의할 때, $mIoU = 81.9$ 인 DDRNet-39의  $W$ 값은 0.1221가 돼야 함.

$$
Score = 10 = 81.9 \cdot W \quad \therefore W = \frac {10} {81.9} = 0.1221
$$

$1 - \frac {T_{infer}-3ms} {7ms} = 0.1221$ 이므로 $T_{infer} \eqsim 9.1453(ms)$ 가 된다.

다른 모델들과 DDRNet-39와의 GFLOPs를 비교하여 $T_{infer}$를 구하고, $mIoU \times W$ 로  $Score$를 구한다.
예를 들어, GFLOPs값이 72.1인 DeepLabV3-ResNet101의 $T_{infer}$ 값은 $9.1453 \times \frac{72.1}{140.6} \eqsim 4.69ms$ 가 된다.
왜냐하면 GFLOPs 값은 $T_{infer}$ 과 **비례**하므로.

---

# Pascal VOC 데이터셋
**Batch Size:16**

|        **Model**        | **GFLOPs** | **mIoU(%)** | $T_{infer}(ms)$ | $W$  |   **Score**   |
| :---------------------: | :--------: | :---------: | :-------------: | :--: | :-----------: |
|   DeepLabV3-MobileNet   |    6.0     |    70.1     |      0.39       |  1   | 70.1($=mIoU$) |
|   DeepLabV3-ResNet50    |    51.4    |    76.9     |      3.34       | 0.95 |     73.1      |
|   DeepLabV3-ResNet101   |    72.1    |    77.3     |      4.69       | 0.76 |     58.6      |
| DeepLabV3Plus-MobileNet |    17.0    |    71.1     |       1.1       |  1   | 71.1($=mIoU$) |
| DeepLabV3Plus-ResNet50  |    62.7    |    77.2     |      4.08       | 0.85 |     65.3      |
| DeepLabV3Plus-ResNet101 |    83.4    |    78.3     |      5.42       | 0.65 |     51.2      |

# Cityscapes 데이터셋
**Batch Size:16**

|        **Model**        | **GFLOPs** | **mIoU(%)** | $T_{infer}(ms)$ | $W$  | **Score** |
| :---------------------: | :--------: | :---------: | :-------------: | :--: | :-------: |
| DeepLabV3Plus-MobileNet |    135     |    72.1     |      8.78       | 0.17 |   12.5    |
| DeepLabV3Plus-ResNet101 |    N/A     |    76.2     |        -        |  -   |     -     |

**따라서 후보 모델은**
1. DeepLabV3 ResNet50 (Score 73.1)
2. DeepLabV3+ MobileNet (Score 71.1)
3.  DeepLabV3 MobileNet (Score 70.1)

https://github.com/VainF/DeepLabV3Plus-Pytorch
![[Pasted image 20250826110843.png]]
