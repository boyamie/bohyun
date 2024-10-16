---
{"dg-publish":true,"permalink":"/3-d/kalman-filter/"}
---

스터디 회고록: 대민님이 발표를 너무 잘해주셔서 감동받았습니다! 대민님의 발표를 듣고 내가 혼자 공부하며 틀렸던 부분도 바로잡고 대민님의 훌륭한 인사이트를 엿볼 수 있는 소중한 기회였습니다.

![](https://i.imgur.com/zqpiCYf.png)
대민님이 설명해주신 판서입니다!

1. **관측값과 예측값의 차이 (Innovation term)**
   - `Zₜ - H * Xₜ` : 여기서 `Zₜ`는 실제 관측된 값, `H * Xₜ`는 예측된 관측값입니다. 이 둘의 차이를 **Innovation term**이라고 부르며, 이는 현재 예측이 얼마나 실제 관측과 차이가 있는지를 나타냅니다.

2. **잔영 비율 (Kalman Gain, Kₜ)**
   - 이 차이에 **Kalman Gain (Kₜ)** 을 곱하여, 예측값을 얼마나 조정할지를 결정합니다. **Kalman Gain**은 예측값을 업데이트하는데 있어서 얼마나 신뢰를 줄지 계산한 가중치입니다.

3. **예측값 업데이트**
   - 최종적으로 `Xₜ = Xₜ⁻ + Kₜ * (Zₜ - H * Xₜ⁻)` : 여기서 `Xₜ⁻`는 이전에 예측된 값이고, **Innovation term**에 잔영 비율을 곱한 값을 더해 업데이트된 예측값 `Xₜ`를 계산합니다.

4. **닷 H 행렬**
   - `H` 행렬은 시스템을 모델링한 값과 물리량 사이의 차이를 맞추기 위한 변환 행렬입니다. 물리적으로 관측할 수 있는 값과 예측된 상태를 연결해주는 역할을 합니다. 관측 시스템과 상태 시스템 간의 차이를 줄이기 위해 `H`를 사용합니다.

![](https://i.imgur.com/3xZMnAp.png)
대민님이 설명해주신 판서입니다!

1. **State prediction (상태 예측)**
   - `Xₖ⁻`는 현재 상태를 예측한 값입니다. 이는 이전 시간에서 예측한 값과 제어 변수 (`Uₖ`)를 바탕으로 예측됩니다. 이 단계는 미래의 상태를 미리 계산하는 과정입니다.

2. **Measurement process (측정 과정)**
   - `Zₖ`는 실제 관측된 측정값입니다. 이 값은 시스템에서 얻어진 실제 데이터로, 상태 예측과 비교하여 차이를 분석합니다.

3. **A priori & A posteriori update (사전/사후 업데이트)**
   - 사전 값인 `Xₖ⁻`에서 측정값 `Zₖ`와 비교하여 차이를 이용해 사후 값 `Xₖ`를 계산합니다. 여기서 Kalman Gain을 활용해 관측값이 예측값을 얼마나 수정해야 할지를 결정하게 됩니다.
   
4. **Next state prediction (다음 상태 예측)**
   - 다음 상태인 `Xₖ₊₁⁻`를 다시 예측하는 단계로 넘어가며, 이 과정을 반복합니다. 즉, 예측된 값과 실제 측정된 값의 차이를 반영하여 다음 예측값을 개선해 나가는 방식입니다.
![](https://i.imgur.com/FvXdGkv.png)
대민님이 설명해주신 판서입니다.
1. **Markov assumption**
   - Markov assumption은 현재 상태가 이전의 모든 상태 정보를 포함하고 있다고 가정합니다. 즉, 어떤 시간 `t=k`에서의 상태는 이전 상태인 `t=k-1`의 정보만 가지고 있으면 충분하며, 그 이전의 모든 정보는 `t=k-1`에 압축되어 있다고 볼 수 있습니다. 이는 시스템이 필요한 메모리 상태를 줄여주는 효과를 줍니다.

2. **트래킹 모델링**
   - 트래킹하는 물체가 시간이 지남에 따라 계속 이동하는 경우, 상위 그림에서 설명된 것처럼, `t=K`에 도달할 때 이전의 모든 상태(`t=1`부터 `t=K-1`까지)의 정보를 메모리 상태로 가지게 됩니다.
   - 그러나 마코프 가정을 적용하면, 하단 그림처럼 `t=K` 상태에 도달할 때 단순히 `t=K-1`의 정보만 가지고 있어도 충분하다는 결론을 얻게 됩니다. 이로써 연산 및 메모리 사용량을 줄이는 효과를 얻을 수 있습니다.
![](https://i.imgur.com/hHNTWG4.png)
대민님의 판서입니다.
1. **노이즈의 의미**
   - 시스템에서 관측되는 값에는 항상 일정량의 불확실성, 즉 **노이즈**가 존재합니다. 이는 측정 기기의 한계나 외부 요인들로 인해 발생합니다. Kalman 필터는 이러한 노이즈를 모델링하여 상태를 추정합니다.

2. **노이즈의 범위**
   - Kalman 필터에서는 노이즈의 영향을 일정한 범위로 가정하고, 그 범위를 원형이나 타원형의 형태로 추정합니다. 즉, 위치나 속도와 같은 상태 변수에서 노이즈가 있을 경우, 이 노이즈는 정규 분포를 따르며, 노이즈가 존재하는 범위는 **Gaussian noise**로 나타낼 수 있습니다. 그림에서 나타난 것처럼 노이즈는 각 변수에서 불확실성을 반영하여 원이나 타원의 형태로 가정됩니다.

3. **Gaussian Noise와 AWGN 가정**
   - Kalman 필터는 주로 **가우시안 노이즈**를 가정하며, 이는 평균이 0이고 분산이 일정한 노이즈 모델입니다. 이를 **AWGN (Additive White Gaussian Noise)** 가정이라 부르기도 하며, 노이즈가 특정 주파수 대역에 상관없이 일정하게 존재하는 경우를 나타냅니다.

4. **다른 필터와 노이즈 모델링**
   - Kalman 필터 외에도 다른 필터들이 존재하며, 각 필터는 다른 방식으로 노이즈를 모델링합니다. 예를 들어, 입자 필터(Particle Filter)는 비선형 시스템에서 복잡한 노이즈 모델링을 적용하는 경우에 사용됩니다.
   여기서 **H**는 Kalman 필터에서 중요한 역할을 하는 **관측 행렬 (Observation Matrix)**입니다. 이를 더 자세히 설명하면 다음과 같습니다.

1. **H의 역할**
   - Kalman 필터는 시스템의 상태를 추정할 때, 상태 변수와 실제 관측값 사이의 관계를 설정해야 합니다. 이때 **H** 행렬은 **시스템 상태**와 **관측된 값** 사이의 변환을 정의해주는 행렬입니다.
   - 상태 벡터 `Xₜ`는 위치, 속도 등의 물리적인 상태를 나타내고, 실제로 우리가 측정하는 값 `Zₜ`는 관측된 물리량입니다. 하지만 이 둘의 단위나 값의 범위가 다를 수 있습니다. 예를 들어, 위치는 미터 단위로 측정되고 속도는 미터/초로 측정되기 때문에 서로 다른 물리적 단위를 가질 수 있습니다.

2. **H의 필요성**
   - **H 행렬**은 이러한 단위 차이를 조정하여 예측된 상태와 관측된 값을 비교할 수 있게 만듭니다. 즉, **H는 관측값의 단위를 상태 변수와 일치시키는 역할**을 합니다.
   - 예를 들어, 시스템 상태에서 위치는 km 단위로 나타나지만, 센서는 미터 단위로 값을 측정할 수 있습니다. 이 경우, **H** 행렬을 사용하여 예측된 위치 값을 센서의 단위에 맞춰 변환해야 합니다.
   
3. **분포의 평균과 H의 관계**
   - H 행렬은 단순히 단위를 맞추는 것뿐만 아니라, **관측된 값의 평균 위치를 예측된 상태로 변환**하는 역할도 합니다. 즉, 관측된 값이 특정한 분포를 따를 때, 그 분포의 평균을 Kalman 필터에서 예측된 상태와 비교하여 더 정확한 추정치를 만들 수 있습니다.
![](https://i.imgur.com/YAZ1C41.png)
대민님의 판서입니다.
### 1. **System modeling for state space (상태 공간을 위한 시스템 모델링)**
   - **알고 싶은 정보**: 시스템의 상태 공간에 대한 변수들(예: 위치, 속도)을 설명하며, 이는 상태 벡터 (State Vector)로 표현됩니다.
   - **알고 있는 정보**: 시스템에 영향을 주는 입력 신호 `Uₜ`, 즉 제어 벡터 (Control Vector)입니다.
   - **모르는 정보**: 시스템의 오차를 나타내는 **Q 행렬**로, 이는 정규 분포 `N(0,1)`을 따른다고 가정됩니다. 이 오차는 시스템의 불확실성을 반영하는 랜덤한 요소입니다.

### 2. **Sensor/Observer modeling for measurement (관측/센서 모델링)**
   - **알고 싶은 정보**: 관측을 통해 얻고자 하는 정확한 값입니다.
   - **알고 있는 정보**: 센서에서 관측된 값 `Zₜ`이며, 이는 센서의 확률 밀도 함수(PDF)를 따릅니다.
   - **모르는 정보**: 관측 오차로, 이는 **R 행렬**로 나타내며, 마찬가지로 정규 분포 `N(0,1)`을 따른다고 가정됩니다.

### 3. **Update process: "prediction vs. measurement" (업데이트 과정: 예측과 측정의 비교)**
   - Kalman 필터에서 상태는 **예측된 값**과 **실제 측정된 값**의 차이로 업데이트됩니다.
   - 업데이트는 **Kalman Gain** `Kₜ`을 통해 이루어지며, 이는 예측값과 측정값의 신뢰도를 조정하는 역할을 합니다.
   - 업데이트 공식은 다음과 같습니다:
     $Xₜ = \hat{X}ₜ⁻ + Kₜ \cdot (Zₜ - H \hat{X}ₜ⁻)$
     - `Zₜ - H \hat{X}ₜ⁻`는 **Innovation term**으로, 예측값과 실제 관측값의 차이를 의미합니다.
     - `Kₜ`는 **Kalman Gain**이며, **업데이트 비율**은 대략적으로 `R/Q`로 계산됩니다.

1. **센서 관측과 측정 오차**
   - Kalman 필터에서 **센서**는 시스템의 상태를 추적하기 위한 도구로 사용됩니다. 시스템이 수학적으로 모델링되었을 때, 이를 잘 작동하고 있는지 확인하는 것이 바로 **센서**를 통해 이루어집니다. 즉, 센서가 측정한 값 `Zₜ`는 상태를 반영한 값으로, 이 값은 **확률 밀도 함수(PDF)**에 따라 수학적으로 복잡해집니다.
   - 센서로부터 얻은 값의 정확성을 평가하려는 목적에서 **측정 오차(Measurement Error)** 가 등장합니다. 이 측정 오차는 관측된 값이 실제 상태와 얼마나 차이가 나는지를 표현한 것입니다.

2. **업데이트 과정 (Update Process)**
   - Kalman 필터의 핵심 과정은 **Innovation term**을 기반으로 합니다. Innovation term은 예측된 상태와 관측된 상태 간의 차이로, 이는 필터가 측정된 값을 얼마나 반영할지를 결정하는 요소입니다.
   - 이 Innovation term에 **Kalman Gain**을 곱하여 예측된 값에 반영하는 방식으로 업데이트를 진행합니다. Kalman Gain은 시스템이 얼마나 빠르게 관측값을 반영할지를 결정하는 가중치로, **업데이트 비율**은 노이즈의 분산에 따라 결정됩니다.

3. **칼만 게인의 수렴**
   - 시간이 지나면서 Kalman Gain은 수렴하게 됩니다. 즉, 예측과 측정이 일치해질수록 Kalman Gain의 변화가 줄어들게 되고, 최종적으로 **노이즈 분산**에 대한 비율로 안정적인 값을 얻게 됩니다.
   
4. **Particle Filter로의 확장**
   - Kalman 필터는 선형 가우시안 시스템을 가정하고 있으나, 비선형 시스템에서 더 복잡한 상황을 다루기 위해 **Particle Filter**가 사용됩니다. Particle Filter는 Kalman 필터와 달리, 상태의 공분산을 모델링할 때 **Innovation term의 공분산**을 사용합니다. 즉, 상태 추정을 위한 확률 분포의 변화가 더 복잡한 공분산 형태로 표현됩니다.

![](https://i.imgur.com/8bpagjb.png)
대민님의 판서입니다.
### 1. Kalman 필터의 수식 (Equations)
   - **예측 과정**
     $Xₜ = A Xₜ⁻₁ + B Uₜ + Wₜ$
     여기서 `A`는 상태 전이 행렬, `B`는 제어 입력, `Wₜ`는 시스템 잡음(노이즈)을 의미합니다. 이 식은 현재 상태를 예측하는 과정입니다.

   - **오차 공분산 행렬**
     $Pₜ⁻ = A Pₜ⁻₁ A^T + Qₜ$
     여기서 `Pₜ`는 상태 추정의 공분산 행렬이며, 이는 시스템의 불확실성을 나타냅니다. `Qₜ`는 시스템 노이즈의 공분산 행렬입니다.

   - **Innovation term**
     $Zₜ - H Xₜ⁻$
     여기서 `H`는 관측 행렬로, 상태 벡터와 측정값의 물리적 단위를 맞춰주는 역할을 합니다. **Innovation term**은 예측된 값과 실제 관측값 사이의 차이를 나타냅니다.

   - **업데이트 과정**
     $Xₜ = Xₜ⁻ + K (Zₜ - H Xₜ⁻)$
     여기서 `K`는 **Kalman Gain**으로, Innovation term의 중요성을 반영해 예측된 상태를 업데이트하는 가중치입니다.
![](https://i.imgur.com/dZIO13k.png)
### 2. 확장 Kalman 필터 (EKF) 알고리즘
   - EKF는 비선형 시스템에서 Kalman 필터를 확장한 버전으로, 비선형 상태 전이 모델과 측정 모델을 다룹니다.
   
   - **예측 과정 (Predict)**
     $\muₜ = g(uₜ, \muₜ⁻₁)$
     여기서 `g(uₜ, \muₜ⁻₁)`는 비선형 함수로, 현재 상태를 예측하는 함수입니다. Kalman 필터에서의 선형 상태 전이를 대신해 비선형 함수로 예측합니다.

   - **오차 공분산 업데이트**
     $\Sigmaₜ = Gₜ \Sigmaₜ⁻₁ Gₜ^T + Rₜ$
     여기서 `Gₜ`는 상태 전이에 대한 Jacobian 행렬이며, `Rₜ`는 시스템 노이즈의 공분산입니다. 이는 예측된 상태의 공분산을 업데이트하는 과정입니다.

   - **업데이트 과정 (Update)**
     $\muₜ = \muₜ⁻ + Kₜ (zₜ - h(\muₜ⁻))$
     여기서 `h(\muₜ⁻)`는 측정 모델의 비선형 함수입니다. 관측값과 예측값의 차이를 계산하여 상태를 업데이트합니다.

   - **공분산 업데이트**
     $\Sigmaₜ = (I - Kₜ Hₜ) \Sigmaₜ$
     여기서 `\Sigmaₜ`는 상태 추정의 공분산 행렬로, Kalman Gain `Kₜ`을 사용하여 업데이트됩니다.

### 3. Sigma 행렬의 역할
   - **Sigma**는 상태 벡터의 분산과 공분산을 나타내며, 이는 상태의 불확실성을 반영합니다. Kalman 필터와 EKF에서 이 공분산을 기반으로 시스템의 상태를 예측하고 업데이트하는 과정이 이루어집니다.
![](https://i.imgur.com/o5zETmo.png)

Particle Filter는 베이지안 필터의 일종으로, 많은 입자를 활용하여 상태를 추정하는 확률적인 방법입니다. 
1. **입자 샘플링 (Importance Sampling)**
   - 처음에 많은 수의 입자를 무작위로 생성하고, 그 입자들을 통해 상태 공간을 탐색합니다. 예를 들어, 초기값 `X₀`에서 100개의 입자를 생성한 후, 제어값 `Uₜ`이 들어왔을 때 상태 `Xₜ`가 얼마만큼의 값을 가질지를 샘플링합니다.
   - 각 입자는 확률적으로 특정 상태를 나타내고, 각 입자가 현재 관측값에 얼마나 적합한지를 계산하여 가중치를 부여합니다. 이 과정에서 입자들이 실제로 얼마나 의미 있는지를 평가합니다.

2. **입자 필터링**
   - 다음 단계에서는 각 입자의 가중치에 따라 의미 있는 입자들만 남기고 나머지 입자는 제거합니다. 즉, 가중치가 높은 입자들은 남기고, 낮은 입자들은 탈락시킵니다. 이는 상태 추정을 더 정확하게 하기 위해 입자 수를 줄이는 과정입니다.
   - **Endfor 루프**에서 중요한 점은, 이 과정에서 남은 입자들만 가지고 다음 단계로 넘어간다는 점입니다. 새로운 관측값이 들어올 때마다 샘플링과 필터링 과정을 반복하며, 관련된 입자들만 남깁니다.

3. **상태 추정**
   - 남은 입자들로 다음 상태를 예측하고, 그 상태에서 다시 새로운 입자들을 생성하여 이 과정을 반복합니다. 이를 통해 시간이 지남에 따라 상태 추정을 점차 더 정확하게 해나갑니다.

4. **Stochastic한 과정**
   - Particle Filter는 매우 **확률적(Stochastic)** 인 과정입니다. 즉, 입자들은 랜덤하게 샘플링되고, 그 중 일부가 남겨지며, 남은 입자들이 다음 상태로 이어지게 됩니다. 이 과정은 완전히 결정적이지 않으며, 많은 입자를 활용해 여러 가능성을 추정하는 방식입니다.
![](https://i.imgur.com/By3kuVU.png)

Particle Filter는 다수의 입자를 통해 상태를 샘플링하고, 확률적으로 가장 적합한 입자들만 남기면서 상태 추정을 점진적으로 개선해 나가는 방법입니다.
![](https://i.imgur.com/Sbb8ult.png)

1. **적자생존 (Survival of the fittest)**
   - Particle Filter는 많은 입자를 생성한 후, 그중에서 가장 적합한 입자들만 남기는 방식으로 동작합니다. 즉, 상태 추정에 적합하지 않은 입자들은 제거되고, 적합한 입자들만 다음 단계로 이어지는 방식입니다. 이러한 특성은 **자연 선택(Natural Selection)**, 즉 적합한 개체만이 생존하는 개념과 유사합니다.
   
2. **가중치에 따른 입자의 생존**
   - 각 입자는 가중치가 부여되고, 이 가중치에 따라 생존 여부가 결정됩니다. 가중치가 높은 입자들은 더 많은 확률로 다음 상태에서 선택되고, 가중치가 낮은 입자들은 제거됩니다.

3. **유전 알고리즘과의 유사성**
   - 이 과정은 **유전 알고리즘(Genetic Algorithm)** 과 유사한 특성을 보입니다. 유전 알고리즘에서도 적합도가 높은 개체들이 선택되고, 다음 세대에 유전자가 전달되는 방식으로 최적해를 찾아가는 과정이 반복됩니다.
   
4. **상태 추정 (Predict step)**:
   - 그래프에서는 `Predict step 4`가 나타나 있습니다. 이는 Particle Filter가 상태를 예측하는 과정에서 입자들이 어떻게 분포하고 있는지를 보여줍니다. 여러 입자가 흩어져 있지만, 다음 단계로 넘어갈 때는 가장 적합한 입자들만이 남게 됩니다.
![](https://i.imgur.com/9EXqc6i.png)

1. **Kalman 필터**:
   - **가우시안 분포**: Kalman 필터는 상태 변수와 측정 값이 모두 **가우시안 분포**를 따른다고 가정합니다. 즉, 노이즈 분포인 `Q`와 `R`도 가우시안 노이즈로 가정됩니다.
   - **선형 모델**: 시스템이 선형일 때 Kalman 필터가 적합합니다. 시스템 모델과 측정 모델이 모두 선형인 경우 상태를 효과적으로 추정할 수 있습니다.

2. **Extended Kalman 필터 (EKF)**:
   - **비선형 모델**: EKF는 시스템이 **비선형**일 때 사용됩니다. 그러나 EKF는 비선형 시스템을 선형으로 근사하는 방식으로 작동합니다. 즉, 비선형 함수를 선형화하여 필터링을 수행합니다.
   - **가우시안 분포**: Kalman 필터와 마찬가지로 EKF도 노이즈가 가우시안 분포를 따른다는 가정을 유지합니다.

3. **Particle 필터**:
   - **임의의 모델 및 분포**: Particle 필터는 선형, 비선형, 그리고 임의의 분포 모델에도 적용할 수 있습니다. 특정 분포에 의존하지 않기 때문에 매우 유연합니다.
   - **연산량**: Particle 필터는 많은 입자를 사용하기 때문에 연산 비용이 큽니다. 따라서 **최적의 효율**을 낸다고 보기 어렵습니다. 특히 고차원의 상태 공간에서는 연산 부담이 커집니다.

4. **Bayes 필터**:
   - **일반적 프레임워크**: Bayes 필터는 매우 일반적인 개념으로, 구체적인 구현은 없지만 확률적 추정의 기초가 되는 프레임워크입니다. Kalman 필터와 Particle 필터는 Bayes 필터의 구체적인 구현입니다.

1. **공분산 업데이트 (Covariance Update)**:
   - Kalman 필터는 **예측**과 **관측** 간의 불확실성을 고려하여 상태를 추정하는데, 이를 수학적으로 공분산 행렬 `P`로 표현합니다.
   - **공분산 행렬 `Pₜ`** 은 시스템 상태의 불확실성을 나타내며, 필터의 각 단계마다 이 공분산을 업데이트합니다. 이를 통해 시스템 상태에 대한 불확실성이 어떻게 변화하는지를 추적할 수 있습니다.

2. **Kalman 필터에서 공분산의 역할**:
   - Kalman 필터는 예측 단계에서 `Pₜ`를 업데이트하고, 새로운 관측값이 들어오면 이를 기반으로 공분산을 조정합니다. 이는 관측값과 예측값의 차이, 즉 **Innovation term**을 반영해 상태를 업데이트할 때 중요한 역할을 합니다.
   - **Kalman Gain** `Kₜ` 역시 공분산 행렬에 의해 계산되므로, 공분산의 업데이트는 상태 업데이트의 비율을 결정하는 핵심 요소입니다. `Kₜ`가 클수록 관측값에 더 많은 가중치가 부여되고, 작을수록 예측값에 더 많은 가중치가 부여됩니다.

3. **상태의 정확성 향상**:
   - Kalman 필터는 각 단계에서 상태의 불확실성을 감소시키기 때문에 시간이 지남에 따라 상태 추정이 점점 더 정확해집니다. 이는 **공분산 행렬이 점점 작아지거나 수렴**하면서 상태에 대한 확신이 커지는 것을 의미합니다.

4. **완전 파생 모델**:
   - Kalman 필터는 이러한 공분산 업데이트 덕분에 단순한 상태 예측 알고리즘이 아니라 **확률적 추정**을 가능하게 해주는 고도화된 필터입니다. 이는 단순한 선형 필터링 방식에서 파생된 완전한 형태의 확률 기반 필터라고 볼 수 있습니다.
   - 이 때문에 Kalman 필터는 **베이지안 필터**의 구체적인 구현이자, 상태 추정의 정확성을 점진적으로 향상시키는 강력한 방법론으로 자리잡고 있습니다.
![](https://i.imgur.com/dIydMN9.png)

1. **상태 (State)와 제어 입력 (Control Input)**:
   - `u(t-1), u(t)` 등은 **제어 입력**으로, 시스템에 들어가는 입력 값을 의미합니다. 이러한 제어 입력은 시스템 상태에 영향을 미치며, 각 시점의 상태는 이전 시점의 제어 입력에 의해 결정됩니다.
   - `x(t-1), x(t)` 등은 시스템의 **상태**로, 이 상태는 시스템 내부의 현재 상황을 나타냅니다. 이 상태는 이전 상태 `x(t-1)`과 제어 입력 `u(t-1)`에 의해 결정됩니다.

2. **관측 (Observation)**:
   - `z(t-1), z(t)` 등은 **관측값**을 의미합니다. 관측값은 상태에서 직접적으로 얻어지며, 시스템 상태 `x(t)`는 관측 모델 `h(x(t))`을 통해 관측값 `z(t)`로 변환됩니다. 즉, 관측값은 상태로부터 유도된 값입니다.

3. **생성 모델 (Generative Model)**:
   - 관측값 `z(t)`는 상태 `x(t)`를 기반으로 생성됩니다. 수식으로는 `z(t) = h(x(t))`로 표현되며, 이는 상태에서 관측값을 추정하는 방식입니다.

4. **베이지안 필터 (Bayes Filter)**:
   - 하단에 있는 베이지안 필터 알고리즘은 상태 추정 과정에서 어떻게 확률적으로 상태를 업데이트하는지를 보여줍니다.
     - 먼저, 이전 상태 `x(t-1)`에 대한 정보를 바탕으로 현재 상태에 대한 믿음(belief)을 계산합니다.
     - 제어 입력 `u(t)`과 관측값 `z(t)`를 고려하여 상태 `x(t)`에 대한 새로운 확률 분포를 계산합니다.
     - 마지막으로, 이 분포를 사용하여 현재 상태에 대한 믿음을 업데이트하고 반환합니다.
![](https://i.imgur.com/XWUoZ7D.png)

1. **Point Cloud와 3D 객체 포즈 추정**:
   - **Point Cloud**는 3D 객체의 형상과 위치 정보를 포함한 데이터로, 3D 객체를 관측할 때 사용됩니다. 이 포인트 클라우드를 통해 **6자유도(6DoF) 객체 포즈**를 추정하게 됩니다. 6DoF는 3D 공간에서 위치(3차원)와 회전(3차원)을 의미합니다.
   - Particle Filter를 사용하여, 여러 개의 **입자(Particles)** 가 다양한 포즈를 가정하고 생성됩니다. 이러한 입자들은 객체가 어느 위치와 자세를 가질 수 있는지를 나타냅니다.

2. **입자 샘플링과 최적 포즈 찾기**:
   - 각 입자는 다른 포즈(자세)를 나타내며, 이 입자들은 다양한 위치에 분포되어 있습니다. **Importance Sampling**을 통해 각 입자가 얼마나 관측된 Point Cloud와 일치하는지 평가됩니다.
   - 평가된 결과로 입자들 중에서 가장 적합한(가장 Point Cloud와 일치하는) 입자들이 선택됩니다. 선택된 입자들의 포즈가 현재 3D 객체의 최적 포즈를 나타냅니다.

3. **Rendered Particles**:
   - 그림에서 보이는 여러 개의 렌더된 입자는 객체의 다양한 포즈를 나타내며, 이를 통해 최적의 포즈를 찾는 과정입니다. 입자들 중에서 가장 적합한 포즈가 남고, 이 최적 포즈가 객체의 현재 위치와 자세를 나타냅니다.
![](https://i.imgur.com/TwuiDn0.png)

1. **I(t)와 0과 1로 이루어진 매트릭스**:
   - 일반적으로 `I(t)`는 **입력 이미지(input image)** 를 의미합니다. 이 이미지는 **object detection**에 사용되는 데이터로, 2차원 또는 3차원일 수 있습니다. 2차원 이미지는 픽셀 값을 가지며, 이 값들은 일반적으로 0에서 255 사이의 값을 가지거나, 특정 조건에 따라 0과 1로 표현될 수 있습니다.
   - 만약 이 이미지가 바이너리로 표현된 경우, 0과 1은 **특정 객체의 존재 여부**를 나타낼 수 있습니다. 예를 들어, 1은 객체가 있는 픽셀, 0은 없는 픽셀을 나타낼 수 있습니다.

2. **3차원 정의와 관측**:
   - 3차원 데이터는 일반적으로 2차원 이미지 + 색상 채널(RGB) 또는 깊이 정보 등으로 이루어진 경우가 많습니다. 따라서 `I(t)`는 3차원 데이터로 볼 수 있고, 여기서 object detection이 이루어질 수 있습니다.

3. **Observation과 Detection**:
   - **Observation `z(t)`** 는 **상태 `x(t)`** 로부터 생성된 값입니다. 즉, 상태는 실제 시스템의 상태(위치, 속도 등)이고, 관측 값은 이 상태를 기반으로 측정된 정보입니다. 여기서 관측값은 2차원 이미지로 표현될 수 있습니다.
   - **Detection**은 `g(I(t))`로 표현되며, 이는 **Object Detector**를 통해 이미지 데이터 `I(t)`에서 객체를 감지하는 과정을 의미합니다.

4. **j에 대한 의문**:
   - `g(I(t))`에서의 **j**는 여기서 나타나지 않기 때문에, 특별한 역할이 없을 가능성이 높습니다. 만약 j가 특정한 객체 또는 클래스에 대한 인덱스라면, 각각의 객체가 감지될 때마다 해당 객체를 가리키는 인덱스로 j를 사용할 수 있지만, 이 그림에서는 언급되지 않은 것 같습니다.
![](https://i.imgur.com/FpQndLu.png)

1. **Tracking ID의 역할**:
   - **Tracking ID**는 객체마다 고유한 ID를 부여함으로써, 각 객체가 여러 프레임에서 식별 가능하도록 합니다. 즉, 동일한 객체가 연속된 프레임에서 추적될 수 있으며, 추적 중 객체가 다른 객체와 혼동되지 않도록 고유 ID로 식별합니다.
   - 예를 들어, 하나의 프레임에서 자동차와 보행자를 동시에 감지하고, 다음 프레임에서도 두 객체를 각각 추적할 수 있습니다. 이때 **Tracking ID**가 객체 간 혼동을 방지하고 추적을 이어나갈 수 있게 합니다.

2. **Threshold를 통한 제거**:
   - **Threshold**는 객체가 추적에서 제외될 조건을 의미할 수 있습니다. 객체가 감지되지 않거나 신뢰도(Confidence Score)가 특정 기준 이하일 때 추적에서 제외됩니다. 이는 트래킹 시스템에서 노이즈를 제거하거나 불필요한 객체를 배제하는 데 사용됩니다.
   - 예를 들어, 감지된 객체의 신뢰도가 너무 낮다면, 그 객체는 제거되고 더 이상 추적되지 않게 됩니다.
![](https://i.imgur.com/2uYKQID.png)

3. **색상과 의미**:
   - 색상은 일반적으로 각 객체의 **Tracking ID**와 연결되어 있습니다. 즉, 객체마다 고유한 색상으로 시각화되며, 이를 통해 여러 객체를 쉽게 구분할 수 있습니다. 예를 들어, 빨간색은 자동차, 파란색은 보행자 등의 색상으로 표시될 수 있습니다.
   - 각 객체에 부여된 **Tracking ID**와 색상이 일치하게 설정되며, 이를 통해 프레임 간 동일 객체를 쉽게 시각적으로 추적할 수 있습니다.
![](https://i.imgur.com/zmLRhEr.png)

1. **3D 객체 검출 (3D Object Detection)**:
   - `t-1`, `t`, `t+1` 등 각 시간 단계에서 **센서 입력**이 들어오고, 이를 통해 **3D Object Detector**가 동작하여 객체들을 검출합니다. 이 검출된 결과가 **Detection**으로 표현되며, 각 객체의 위치와 자세(포즈), 그리고 속성들이 포함된 정보를 얻게 됩니다.
   - 이 과정에서 7DOF(7 Degrees of Freedom) 객체의 위치, 크기, 회전 각도 등 모든 정보를 포함한 **검출값**이 생성됩니다.

2. **Mahalanobis 거리 기반 매칭**:
   - 각 시간 단계에서 검출된 객체들과 이전 시간의 예측값을 **Mahalanobis 거리**를 이용해 매칭합니다. Mahalanobis 거리는 두 분포 간의 차이를 계산할 때 유용한 방법으로, 객체가 같은 객체인지 여부를 판단하는 데 사용됩니다.
   - 즉, 이전 시간 `t-1`에서 예측된 객체와 현재 시간 `t`에서 검출된 객체를 비교하여, 같은 객체인지 확인하는 매칭 작업을 진행합니다.

3. **Kalman 필터의 예측과 업데이트**:
   - **Kalman Filter**는 매칭된 결과를 바탕으로 예측값을 계산하고, 이를 업데이트합니다. 
   - 먼저, 이전 상태를 기반으로 **예측(Predict)** 단계를 수행하여 각 객체의 현재 위치를 추정하고, 이후 매칭된 검출값을 이용해 그 추정을 **업데이트(Update)**합니다.
   - 이 과정을 반복함으로써 객체의 상태(위치, 속도, 포즈 등)를 지속적으로 추적합니다.

4. **Tracking ID와 결과**:
   - 각 객체는 고유한 **Tracking ID**를 가지고 추적됩니다. 예를 들어, Tracking ID 1, 2, 3 등 각 객체가 다양한 시간 단계에서 정확하게 추적될 수 있습니다.
   - **Tracking Result**는 매칭된 결과를 바탕으로 각 시간 단계에서 객체들이 잘 추적되고 있는지, 그 추적 결과를 시각적으로 보여줍니다. 여기서 Tracking ID에 따라 색상이 다르게 표현되며, 각 객체가 어떻게 움직이고 있는지 쉽게 확인할 수 있습니다.

5. **시스템 매칭과 예측**:
   - 이 과정은 **시스템 매칭**의 핵심입니다. 검출된 객체들과 이전 예측값을 매칭하고, Kalman Filter를 통해 예측된 값들을 업데이트하는 방식으로, 객체의 상태를 추정하는 시스템입니다.
![](https://i.imgur.com/sho0sQL.png)

1. **상태 벡터 (`s_t`)**:
   - 상태 벡터는 객체의 **위치(Position)**, **회전(Rotation)**, **크기(Scale)**, **선형 속도(Linear Velocity)**, **각속도(Angular Velocity)** 등을 포함하는 변수들로 정의됩니다.
   - 여기서 상태 벡터는 다음과 같습니다:
     $s_t = (x, y, z, a, l, w, h, d_x, d_y, d_z, d_a)^T$
     - `x, y, z`: 객체의 위치 (3차원 공간에서의 위치).
     - `a`: 객체의 회전 각도 (보통 Z축을 기준으로 회전).
     - `l, w, h`: 객체의 크기 (길이, 너비, 높이).
     - `d_x, d_y, d_z`: 객체의 선형 속도 (X, Y, Z 방향).
     - `d_a`: 객체의 각속도 (회전 속도).

2. **프로세스 모델 (Process Model)**:
   - 프로세스 모델은 **현재 상태**에서 **다음 상태**로 예측하는 모델로, 일정한 **속도 운동(Constant Velocity Motion)**을 가정한 상태에서 다음 상태를 예측합니다.
   - 각 변수는 예측을 통해 시간이 지나면서 변화하며, 이는 Kalman 필터가 지속적으로 상태를 업데이트하는 방식으로 사용됩니다.
     $\hat{x}_{t+1} = x_t + d_x + q_x$
     - `q_x`: 잡음 요소를 의미하며, 상태의 불확실성을 반영합니다. 다른 변수도 동일한 방식으로 선형 속도와 잡음을 더하여 다음 상태를 예측합니다.

3. **도메인마다 다른 프로세스 모델**:
   - 실제로 프로세스 모델을 잡기가 어려운 이유는 각 **도메인**에서 객체의 상태 변화를 정확히 모델링해야 하기 때문입니다. 예를 들어, 자동차를 추적하는 경우와 사람을 추적하는 경우, 물체의 회전이나 속도 변화는 다를 수 있습니다. 이러한 특성에 따라 프로세스 모델은 달라져야 합니다.
   - 여기서는 객체의 **위치(Position)** 와 **회전(Rotation)**, **크기(Scale)** 를 다루며, 속도(`d_x, d_y, d_z`)와 각속도(`d_a`)는 각각 **선형 속도**와 **각속도**로 정의됩니다.
![](https://i.imgur.com/SOWN5zF.png)

1. **Mahalanobis Distance (마할라노비스 거리)**:
   - 수식에서 `z_t`는 **관측값**을 의미하고, `Cμ_t`는 **예측값**입니다. 이 둘의 차이 `z_t - Cμ_t`는 **Innovation term**(차이값)입니다.
   - 이 차이에 대해 **공분산 행렬** `S_t`를 사용하여 거리를 계산합니다. 즉, **차이값을 공분산 행렬로 스케일링**하여 측정하는 것입니다.
   - 수식에서 공분산 행렬의 역행렬을 곱하고, 차이값 벡터에 대한 내적을 계산한 후, 마지막으로 제곱근을 취해 **Mahalanobis 거리**를 계산합니다.

2. **공분산과 차이값 (Delta)**:
   - `S_t`는 **Innovation covariance**로, 예측값과 관측값 사이의 불확실성을 나타냅니다. 이 공분산 행렬을 사용하여 두 값의 차이가 얼마나 신뢰할 수 있는지를 반영합니다.
   - 두 값의 차이에 대해 공분산을 곱해주는 방식으로, 공분산에 따라 차이값의 중요도가 달라집니다. 공분산이 작을수록 차이값이 더 중요한 영향을 미치게 됩니다.

3. **루트를 씌워서 손실 함수로 사용**:
   - 최종적으로 제곱근을 취한 값이 Mahalanobis 거리 `m`로 계산됩니다. 이 거리를 손실 함수로 사용하여 추적 과정에서 **예측값과 관측값 간의 차이를 최소화**하려고 합니다.
   - Kalman 필터의 예측값과 실제 감지된 객체 사이의 차이를 줄여가며 객체를 정확하게 추적하는 데 사용됩니다.
![](https://i.imgur.com/jdQXDPq.png)

1. **Mahalanobis 거리와 아웃라이어 제거**:
   - **Mahalanobis 거리 `m`** 는 관측된 값과 예측된 값 사이의 거리(차이)를 공분산으로 스케일링한 값입니다. 
   - 이 거리가 3표준편차 이상(`m > 3 * σ`)인 경우, **99.7%의 값이 3σ 이내**에 들어오기 때문에, 이를 **아웃라이어로 판단**하고 제거할 수 있습니다.
   - 즉, 예측된 객체의 위치와 관측된 객체 위치 사이의 차이가 너무 클 경우, 그 값을 신뢰하지 않고 제외합니다. 이는 추적 정확도를 높이기 위한 중요한 검증 과정입니다.

2. **데이터 연관(Data Association)과 그리디 매칭**:
   - **Data Association**은 예측된 객체 상태와 실제 검출된 객체를 서로 매칭하는 과정입니다. 이 과정에서는 **Mahalanobis 거리**를 사용하여 두 값이 얼마나 가까운지 평가하고, 가장 가까운 값들을 서로 매칭합니다.
   - **Greedy 매칭**은 가장 가까운 객체부터 순차적으로 매칭해 나가는 방식입니다. 이 방식은 단순하지만, 성능이 충분히 좋을 수 있습니다.
     - 예를 들어, `Kalman Filter Predictions`에서 예측된 객체와 `Detections`에서 실제 감지된 객체 사이의 거리를 계산한 후, 가장 작은 거리부터 매칭하여 추적을 이어갑니다.
     - 이 과정에서, 앞서 언급한 **Mahalanobis 거리**가 일정 값 이상인 경우 아웃라이어로 판단하고 매칭에서 제외할 수 있습니다.

3. **검증된 알고리즘의 흐름**:
   - Kalman 필터의 예측값과 실제 감지값을 매칭한 후, 잘못된 매칭이나 오류를 최소화하기 위해 거리 기반의 검증 절차를 거치게 됩니다. 이때 Mahalanobis 거리를 기준으로 하여 매칭 여부를 결정하고, 잘못된 매칭(아웃라이어)을 제거합니다.
   - 이 과정이 반복되며 각 객체의 상태가 정확하게 추적됩니다. 특히 객체가 가려지거나 일시적으로 감지되지 않는 경우에도 이전 프레임의 예측값을 기반으로 추적을 이어나갈 수 있게 됩니다.

### 1. **불확실성의 의미**:
   - 어떤 값을 측정하거나 예측할 때, 우리는 그 값이 정확히 맞다고 확신할 수 없습니다. 실제 값과 오차가 있을 수 있으며, 이 오차의 크기를 **불확실성(uncertainty)** 라고 합니다.
   - 예를 들어, 자동차의 위치를 추적한다고 가정할 때, 센서가 정확하게 위치를 측정하지 못하고 약간의 오차가 발생합니다. 이 오차의 범위를 **불확실성**으로 표현합니다.

### 2. **Kalman 필터에서 불확실성**:
   - Kalman 필터에서는 불확실성을 **공분산 행렬(covariance matrix)** 로 나타냅니다. 이 행렬은 예측값과 측정값에 대한 신뢰도를 수치로 나타내는 도구입니다.
     - 예를 들어, 공분산 값이 크면 그 값에 대한 신뢰도가 낮다는 것을 의미합니다. 즉, 불확실성이 크다는 뜻입니다.
     - 반대로 공분산 값이 작으면 그 값에 대한 신뢰도가 높다는 것을 의미합니다. 즉, 불확실성이 작습니다.

### 3. **불확실성 정보의 활용**:
   - Kalman 필터는 불확실성을 계산하여 예측값과 측정값을 **어떻게 조합할지** 결정합니다. 
   - 만약 예측값에 대한 불확실성이 크고, 측정값에 대한 불확실성이 작다면, Kalman 필터는 **측정값을 더 신뢰**하게 됩니다.
   - 반대로 측정값에 대한 불확실성이 크다면, **예측값을 더 신뢰**하게 됩니다. 이렇게 불확실성을 기반으로 예측과 측정의 **균형을 맞춰** 추적 성능을 높입니다.

### 4. **Mahalanobis 거리에서의 불확실성**:
   - Mahalanobis 거리에서도 불확실성은 중요한 역할을 합니다. 두 값의 차이를 단순히 계산하는 것이 아니라, 그 차이에 대한 **불확실성**(공분산 행렬)을 고려하여 거리 값을 산출합니다.
   - 예를 들어, 두 값이 매우 차이가 나더라도, 그 차이에 대한 불확실성이 크다면 그 값을 아웃라이어로 판단하지 않을 수 있습니다. 반대로 차이가 작더라도 불확실성이 작으면 그 값을 신뢰할 수 있습니다.

### 5. **실생활 예시로 비유**:
   - 불확실성은 실제 상황에서도 많이 접할 수 있는 개념입니다. 예를 들어, 우리는 날씨 예보를 볼 때 "강수 확률"이라는 정보를 봅니다. 오늘 비가 내릴 확률이 50%라고 하면, 비가 올 가능성에 대한 **불확실성**이 큰 상황입니다. 
   - 만약 강수 확률이 90%라면, 불확실성이 매우 작아지고, 비가 올 가능성이 높아지므로 우리는 우산을 챙길 확률이 높아집니다.

**불확실성(uncertainty)** 는 우리가 예측하거나 측정할 때 그 값이 얼마나 신뢰할 수 있는지를 나타내는 중요한 개념입니다. Kalman 필터는 이 불확실성을 수학적으로 계산하여, 예측과 측정값을 조합해 최적의 추적 성능을 유지합니다. 불확실성을 제대로 이해하고 사용하는 것이 추적 알고리즘의 핵심입니다.
![](https://i.imgur.com/Ceaq9eU.png)

### 1. **우리가 알고 있는 정보**:
   - 필터링 과정에서 우리는 **제한된 정보**를 가지고 문제를 해결해야 합니다. 일반적으로 **두 가지 정보**만을 가지고 있습니다:
     - **상태(State)**: 시스템의 현재 상태, 예를 들어 위치, 속도, 각도 등입니다.
     - **센서 측정값(Measurement)**: 센서를 통해 직접 관측된 값입니다. 하지만 이 값은 노이즈가 섞여 있거나 완벽하지 않기 때문에 불확실성이 존재합니다.

### 2. **확률적 필터링의 원리**:
   - Kalman 필터와 같은 확률적 필터는 이러한 제한된 정보를 기반으로 **시스템의 상태를 추정**합니다.
   - 필터는 **확률 분포**를 사용해 현재 상태에 대한 신뢰도를 평가합니다. 예를 들어, 현재 위치에 대한 확률 분포는 그 위치가 얼마나 신뢰할 수 있는지를 나타냅니다.

### 3. **Stochastic한 방법**:
   - Stochastic(확률론적)한 방법은 필터가 **예측과 관측 사이의 불확실성을 확률적으로 처리**하는 것을 의미합니다. 즉, 필터는 단일한 값이 아니라, **확률 분포**를 기반으로 상태를 추정하고 업데이트합니다.
   - 이는 예측값과 관측값의 불확실성을 고려하여 **최적의 상태를 추정**하는 과정입니다.

### 4. **Process Noise와 Measurement Noise**:
   - **Process Noise(프로세스 노이즈)**: 시스템 자체에서 발생하는 불확실성입니다. 예를 들어, 시스템의 모델이 완벽하지 않거나 환경적인 요인으로 인해 발생하는 오차를 의미합니다.
   - **Measurement Noise(측정 노이즈)**: 센서에서 발생하는 불확실성입니다. 센서가 정확하지 않거나 노이즈가 섞여 있을 때 발생합니다.
   - 필터는 이 두 가지 노이즈를 모두 고려하여 상태를 추정합니다.

### 5. **확률적 특성을 이용한 문제 해결**:
   - 필터는 현재 상태를 **예측(Prediction)** 한 다음, 새로운 **측정값(Measurement)** 을 기반으로 그 예측을 **업데이트(Update)**합니다.
   - 이 과정에서 예측값과 측정값의 불확실성을 확률적으로 처리하여 **최종 상태를 추정**하게 됩니다. 필터는 여러 번의 반복을 통해 점점 더 정확한 상태 추정에 가까워집니다.

   - 필터는 제한된 정보와 **불확실성**을 다루기 위해 확률적 방법을 사용합니다. 즉, 시스템의 상태와 측정값을 기반으로 확률 분포를 만들어 **최적의 추정값**을 도출합니다.
   - 이 과정에서 **Process Noise**와 **Measurement Noise**를 고려하고, 반복적인 업데이트를 통해 **불확실성을 줄이면서** 상태를 추적합니다.
![](https://i.imgur.com/oEf4wdQ.png)

1. **Differentiable Kalman Filter**:
   - **칼만 필터의 미분 가능 버전**으로, **CNN** 등과 결합하여 이미지를 입력으로 받아 상태를 직접 추정합니다.
   - 이미지 시퀀스(비디오)를 관측값으로 사용하며, 각 프레임에서 객체(여기서는 빨간 원)의 위치를 추적합니다.

2. **가려짐(Occlusion)과 불확실성**:
   - **가려짐이 적은 시퀀스**에서는 상태를 쉽게 추적할 수 있지만, **가려짐이 많은 시퀀스**에서는 객체의 위치를 정확하게 추적하기 어렵습니다.
   - 가려짐이 많아질수록 **측정 노이즈(R)** 값이 커지며, 이는 **이노베이션(측정값과 예측값의 차이)** 이 커지고 **칼만 게인**이 커지면서 추적 상태가 불안정해질 수 있습니다.
   - 이 불안정성은 예측값에 대한 신뢰도가 줄어들어 **불확실성**이 커지는 상황을 나타냅니다.

3. **Loss 계산 및 학습**:
   - **Loss 함수**는 **MSE(Mean Squared Error)**로 정의되어 있으며, 각 배치와 시퀀스에 대해 추적 성능을 평가합니다.
   - Loss는 **fully connected layer**를 통해 전달되며, 필터의 **파라미터(가중치)** 를 업데이트하는 데 사용됩니다. 이 과정에서 필터는 오차를 줄이기 위해 점진적으로 학습하게 됩니다.
   - 즉, **예측값과 실제값의 차이**(이노베이션)를 계산하여 필터의 성능을 개선하는 방식입니다.
![](https://i.imgur.com/bnOLMpu.png)


4. **PDF값과 업데이트**:
   - **출력은 PDF(확률 밀도 함수)** 형태로 제공되며, 이는 상태(위치, 속도 등)에 대한 **평균값**과 **공분산**을 포함합니다.
   - 추적 과정에서 **예측값과 관측값 간의 불확실성**을 반영한 PDF 값을 바탕으로, 필터는 학습을 통해 점진적으로 정확도를 높여갑니다.
![](https://i.imgur.com/Otlnq6E.png)

1. **Negative Log Likelihood (음의 로그 가능도)**:
   - 수식에서 \(\log(|Σ_t|)\)는 **공분산 행렬 Σ**의 **결정값(determinant)** 을 로그로 변환한 것입니다. 이 값은 상태 추정에서의 **불확실성**을 나타내며, 추정된 상태가 얼마나 신뢰할 수 있는지를 측정합니다.
   - **로그 가능도**는 확률 분포에서 발생한 데이터에 대한 불확실성을 처리할 때 자주 사용됩니다. **음의 로그 가능도**를 최소화하는 것이 **확률적으로 더 정확한 상태 추정**을 의미합니다.
   - 또한, **\(\log(|Σ_t|)\)** 를 포함하는 이유는 **확률 밀도 함수(pdf)** 에서 **정규 분포**의 특성에 따라 **정규화**를 위한 항목이 필요하기 때문입니다. 로그 함수는 큰 값을 다룰 때 계산적으로 안정적인 값을 제공합니다.

2. **공분산의 로그 사용 목적**:
   - **공분산 행렬 Σ**의 로그값을 포함시키는 것은 시스템의 상태 추정에서 **불확실성을 직접적으로 반영**하기 위한 방법입니다. Σ가 크면, 불확실성이 높아 상태에 대한 신뢰도가 떨어지며, 이를 로그 값으로 다루어 **불확실성을 최적화**할 수 있습니다.
![](https://i.imgur.com/zeJOQmM.png)

### 1. **Prediction (예측 단계)**:
   - **Action (액션) 및 Dynamics Model (다이나믹 모델)**: 
     - 여기서 **Action Sampler $f_{\theta}$** 는 로봇이 수행할 액션을 의미합니다. 예를 들어, "로봇이 1cm 이동해라" 같은 명령을 말합니다.
     - **Dynamics Model $g_{\theta}$** 은 이 액션에 의해 예측되는 로봇의 상태 변화를 모델링합니다. 즉, 주어진 액션에 따라 로봇이 어떻게 움직일지를 나타내는 수학적 모델입니다.
     - **Noisy Actions (잡음이 포함된 액션)**: 실제로 로봇은 이상적으로 움직이지 않을 수 있기 때문에, 잡음이 포함된 액션이 고려됩니다. 이는 현실 세계에서의 불확실성을 반영하는 부분입니다.
   - 결과적으로 **예측된 상태(Particle Poses)**들이 만들어지게 됩니다.

### 2. **Resampling (재샘플링 단계)**:
   - **Resample (재샘플링)**: 
     - 예측된 상태들 중에서 **중요한 입자들(Particles)**을 선택하고, 덜 중요하다고 판단된 입자들은 제거됩니다.
     - 이것은 로봇의 상태를 가장 잘 반영하는 입자들을 유지하고, 나머지를 무시하는 과정입니다.

### 3. **Measurement Update (측정값 업데이트 단계)**:
   - **Observation (관측값)**: 
     - 로봇의 센서가 현재의 환경을 관측하게 되고, 그 결과 관측값 $O_t$  가 들어옵니다.
   - **Observation Encoder $h_{\theta}$
     - 관측된 데이터를 적절한 형식으로 변환합니다. 예를 들어, 카메라로 촬영된 이미지를 처리해 로봇의 위치 추정에 사용할 수 있는 정보로 변환하는 역할을 합니다.
   - **Observation Likelihood Estimator \( l_{\theta} \)**: 
     - 각 입자가 현재 관측된 데이터와 얼마나 일치하는지를 추정합니다. 즉, 각 입자가 관측된 데이터에 얼마나 잘 맞는지를 평가해 **입자의 가중치(weight)**를 설정합니다.
   - **Set Weights (가중치 설정)**: 
     - 관측값에 얼마나 잘 맞는지에 따라 **입자의 중요도**를 결정합니다.
   - **Insert Particles (입자 삽입)**: 
     - 새로운 입자들이 추가되고, 관측값을 바탕으로 입자들의 위치를 다시 정리하여 **갱신된 상태**를 만듭니다.

### 4. **입자 기반 상태 추정 반복**:
   - 위 과정을 반복하면서 로봇의 상태를 계속해서 업데이트합니다. 매 시점마다 액션과 관측값을 반영해 입자들의 상태를 갱신하고, 그 중 가장 중요한 입자들을 기반으로 로봇의 위치를 추정하게 됩니다.

### 1. **동적 모델을 미분해서 구하는 차이점**:
   - 이전의 전통적인 **파티클 필터(Particle Filter)** 는 동적 모델을 고정된 수식으로 정의해 사용했습니다. 그러나 이 **차별화된 파티클 필터(Differentiable Particle Filter)** 에서는 **동적 모델을 학습 가능한 파라미터**로 정의하여 미분할 수 있습니다.
   - 이 말은 **로봇의 동작을 설명하는 수학적 모델**이 고정된 것이 아니라, **학습**을 통해 더 나은 동적 모델을 찾는다는 의미입니다. 
   - 즉, 주어진 환경이나 상황에 맞게 **로봇의 움직임을 더 정확하게 반영하는 모델**을 학습해가면서 사용합니다. 그래서 **딥러닝 기반의 미분 가능 구조**가 포함된 것입니다.

### 2. **입자의 추가와 제거**:
   - 기존의 파티클 필터에서는 처음에 설정한 **고정된 수의 입자**(Particles)를 이용해 상태를 추정했지만, 여기서는 **입자의 추가 및 제거**가 동적으로 이루어집니다.
   - **입자의 추가(Insert Particles)** 는 관측값을 기반으로 새로운 입자들을 넣어서 상태 추정을 더 정확하게 만듭니다. 관측 결과에 더 잘 맞는 입자들을 추가하는 것이죠.
   - 반면에, **입자의 제거(Resample)** 는 불필요한 입자나 현재 관측값과 일치하지 않는 입자들을 제거하는 과정입니다. 즉, 의미 없는 정보가 들어있는 입자들을 버리고 중요한 정보만 남기는 과정입니다.
   - 이로 인해 **입자의 수**는 시점마다 유동적으로 변할 수 있습니다. 이를 통해 효율적인 상태 추정이 가능해집니다.

### 핵심 차이점:
- **미분 가능 동적 모델**: 기존의 고정된 모델 대신 학습을 통해 동작을 설명하는 모델을 점진적으로 개선할 수 있습니다.
- **입자의 유동적인 추가 및 제거**: 입자 필터 내에서 상태 추정에 필요한 입자를 추가하고 불필요한 입자는 제거하는 동적인 과정이 포함됩니다.
