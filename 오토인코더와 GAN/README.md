## 오토인코더와 GAN을 사용한 표현 학습과 생성적 학습

오토인코더는 어떤 지도 없이 잠재 표현이라 부른 ㄴ입력 데이터의 밀집 표현을 학습 할 수 있는 인공 신경망이다.

오토인코더는 단순히 입력을 출력으로 복사하는 방법을 배우는데 

매우 긴 시퀀스를 전부 기억하기 어렵기 때문에 시퀀스의 패턴을 찾아 일부만 기억하는 것이 유용하다.

오토인코더는 이처럼 입력을 받아 인코더에서 효율적인 내부 표현으로 바꾸고 디코더에서 입력과 가장 가까운 것을 출력한다.

오토인코더가 입력을 재구성하기 때문에 출력을 재구성이라고 부르는데, 비용함수는 재구성이 입력과 다를 때 모델에 벌점을 부과하는 재구성 손실을 포함한다.

오토인코더가 선형 활성화 함수만 사용하고 비용함수를 평균제곱 오차로 적용하면 주성분 분석을 하는것과 동일하게 볼 수 있다.

오토인코더의 은닉층을 여러 개로 늘린 경우를 적층 오토인코더라고 한다.

층을 더 추가하여 오토인코더가 더 복잡한 것을 학습할 수 있지만 인코더가 너무 강력해지면 훈련 데이터는 완벽하게 재구성될지언정 유용한 데이터 표현을 학습하지 못해 일반화 성능이 떨어진다.

오토인코더가 N개의 층을 갖고 완벽하게 대칭인 경우 $W_L$이 L번째 층의 가중치를 나태낸다고 했을 때 디코더 층의 가중치는 $W_{N-L+1} = W_L^T$ 로 간단히 정의할 수 있다.

오토인코더를 한 번에 전체 훈련하는것보다 탐욕적 방식의 층별 훈련 으로 오토인코더 하나 훈련하고 이를 쌓아올려서 한 개의 적층 오토인코더를 만들 수 있다.

 - 합성곱 오토인코더

   이미지를 다루는 경우에는 합성곱 신경망에 비해 오토인코더가 좋은 성능을 내지 못한다.

   따라서 이미지에 대한 오토인코더를 만들려면 합성곱 오토인코더를 만들어야한다.

   합성곱 오토인코더에서 인코더는 합성곱 층과 풀링 층으로 구성된 일반적인 CNN으로 입력에서 공간 방향의 차원을 줄이고 깊이를 늘린다.

   디코더는 반대로 동작해야하기 때문에 이미지의 스케일을 늘리고 깊이를 원본 차원으로 돌리기 위해 전치 합성곱 층을 사용한다.

 - 순환 오토인코더

   인코더는 입력 시퀀스를 하나의 벡터로 압축하는 시퀀스 투 벡터 RNN을 사용하고 디코더는 이 반대인 벡터 투 시퀀스 RNN을 사용한다.

### Overcomplete 오토인코더

지금까지는 오토인코더가 흥미로운 특성을 학습하도록 강제하기 위해 코딩층의 크기를 제한하여 Undercomplete으로 만들었지만, 입력 크기만큼 혹은 입력보다 큰 코딩 벡터 층을 두어 과대완전 오토인코더를 만들 수 있다.

 - 잡음 제거 오토인코더

   오토인코더가 유용한 특성을 학습하도록 강제하는 방법중에서 입력에 잡음을 넣고 잡음이 없는 원본입력을 복원하도록 하는 것이다.

   이때 추가하는 잡음은 순수한 가우시안 잡음이나 드롭아웃처럼 무작위로 입력을 꺼내는 방식을 사용한다.

### 희소 오토인코더

   좋은 특성을 추출하도록 만드는 다른 제약 방식은 희소를 사용하는 것이다.

   이는 비용함수에 적절한 항을 추가하여 오토인코더가 코딩층에서 활성화되는 뉴런수를 감소시키도록 만든다.

   이렇게하면 오토인코더가 적은 수의 활성화된 뉴런을 조합하여 입력을 표현해야하기 때문에 코딩 층의 각 뉴런은 유용한 특성을 표현하게된다.

   뉴런의 활성화 정도를 조절할 때 비용함수를 사용해도 나쁘지 않지만 쿨백-라이블러 발산을 사용하면 좋다.

   쿨백-라이블러 발산 : $D_{KL}(P||Q) = \sum\limits_i P(i) \log \dfrac{P(i)}{Q(i)}$

   목표 희소 정도 p와 실제 희소 정도 q사이의 KL 발산 : $D_{KL} (p||q) = p \log \dfrac{p}{q} + (1-p) \log \dfrac{1-p}{1-q}$

   공식의 위와 같은데 두 개의 이산 확률 분포 P와 Q가 주어지면 $D_{KL}(P||Q)$를 사용하고, 코딩 층에서 뉴런이 활성화 될 때 목표 확률
   p와 실제 확률 q 사이의 발산은 $D_{KL} (p||q)$를 사용하면 된다

   그 후 이 손실들을 모두 합해서 비용 함수의 결과에 더한다.

### 변이형 오토인코더(VAE)

VAE는 확률적 오토인코더로 훈련이 끝난 후에도 출력이 부분적으로 우연에 의해 결정된다.

그리고 생성 오토인코더이기 때문에 훈련 세트에서 샘플링된 것 같은 새로운 샘플을 생성할 수 있다.

VAE는 주어진 입력에 대해 코딩을 바로 만드는 대신 인코더는 평균 코딩과 표준편차를 만들고 평균이 μ이고 표준편차가 σ인 가우시안 분포에 랜덤하게 샘플링 된다.

VAE는 훈련하는동안 비용함수가 코딩을 가우시안 샘플들의 군집처럼 보이도록 코딩 공간 안으로 점진적으로 이동시켜 입력이 매우 복잡한 분포를 가지더라도 간단한 가우시안 분포에서 샘플링 된 것처럼 보이게 만드는 경향이 있다.

따라서 훈련이 끝난뒤 새로운 샘플을 매우 쉽게 생성할 수 있게 된다.

변이형 오토인코더의 비용함수는 두 부분으로 구성되는데 첫 번째는 오토인코더가 입력을 재생산 하도록 만드는 일반적인 재구성 손실이고, 두 번째는 단순한 가우시안 분포에서 샘플된 것 같은 코딩을 가지도록 강제하는 잠재 손실이다.

변이형 오토인코더의 잠재 소실은 $\mathcal{L} = - \dfrac{1}{2} \sum\limits_{i=1}^n 1 + \log(\sigma_i^2) - \sigma_i^2 - \mu_i^2$이다.

여기서 n은 코딩의 차원이고, μ와 σ는 i번째 코딩 원소의 평균과 표준펀차이다.

그리고 표준편차는 양수여야하지만 은닉층의 출력값은 음수일 가능성이 있기 때문에 위의 식을 $\gamma = \log(\sigma_i^2)$로 로그 스케일로 변형하면 아래와 같이 식을 다시 쓸 수 있다.

$\mathcal{L} = - \dfrac{1}{2} \sum\limits_{i=1}^n 1 + \gamma_i - \exp(\gamma_i) - \mu_i^2$

변이형 오토인코더는 두 이미지가 겹쳐 보이는 것 같은 시맨틱 보간을 코딩 수준에서 수행 할 수 있다.

이는 먼저 두 이미지를 인코더에 통과시켜 두 코딩을 보간하고 보간된 코딩을 디코딩하여 최종이미지를 얻는다.

이렇게 하면 데이터의 의미적인 관련성을 유지하면서 데이터의 중간 단계 데이터가 생성되어 데이터 생성, 데이터 보강, 스타일 변환 등 다양한 곳에서 사용 가능하다.

### 생성적 적대 신경망(GAN)

GAN은 기본적으로 신경망을 서로 겨루게 하고 경쟁을 통해 신경망을향상하는 것을 기대한다.

이는 생성자와 판별자 두 개로 구성된다.

생성자는 랜덤한 분포를 입력으로 받고 이미지와 같은 데이터를 출력하여 변이형 오토인커더의 디코더와 같이 새로운 이미지를 생성할 수 있다.

판별자는 생성자에서 얻은 가짜 이미지나 훈련 세트에서 추출한 진짜 이미지를 입력으로 받아 입력된 이미지가 가짜인지 진짜인지 구분한다.

훈련하는동안 생성자와 판별자의 목표는 반대인데 판별자는 진짜와 가짜이미지를 구분하는 것이고, 생성자는 판별자를 속일 만큼 진짜 같은 이미지를 만드는 것이다.

따라서 훈련 과정에서 생성자와 판별자는 서로 끊임없이 앞서려고 노력하는데 훈련이 진행되면서 GAN은 내시 균형에 다다르게 된다.

내쉬 균형은 주어진 상황에서 각 참가자가 다른 참가자들의 전략을 고려하여 자신의 최적 전략을 선택한 상태로, 어떤 참가자도 다른 전략으로 전환할 경우 자신의 이익을 개선할 수 없는 상태를 말한다.

GAN은 생성자가 완벽하게 실제와 같은 이미지를 만들어내서 진짜가 반이고 가짜가 나머지 절반인 판별자가 추측을 할 수밖에 없는 상황 단 하나의 내시 균형에만 도달할 수 있다.

GAN의 가장 큰 어려움은 생성 모델이 특정 클래스 또는 데이터 분포의 일부에 집중하고, 다른 클래스나 분포를 생성하지 않는 모드 붕괴이다.

예를들어 생성자가 도형중에 사각형을 제일 그럴싸하게 만든다고 가정하면, 판별자를 조금 더 쉽게 속이기 위해 다른 도형인 삼각형이나 원형대신 사각형의 이미지만 많이 만들게 되어 생성자가 점진적으로 다른 이미지를 생성하는 방법을 잊게되는 상황을 말한다.

이렇게 되면 판별자도 다른 클래스의 가짜 이미지를 구별하는 방법을 잊어 버리고 사각형중에서 진짜와 가짜를 구분하는데만 특화되게 된다.

만약 판별자가 사각형도 너무 잘 구분하게 되면 생성자가 다른 클래스로 옮겨가 삼각형 이미지만 생성하게 되고, 판별자도 삼각형만 잘 구분해지는 과정을 계속 반복하여 GAN은 몇 개의 클래스 사이를 오가다가 어떤 클래스에서도 좋은 결과를 못만들게 된다.

이를 안정적으로 하기위해 경험 재생이라고 하는 기법을 사용하여 매 반복에서 생성자가 만든 이미지를 재생버퍼에 저장하고 실제 이미지와 버퍼에서 뽑은 가짜 이미지를 더해 판별자를 훈련하면 된다.

경험 재생기법은 판별자가 생성자의 가장 최근 출력에 과대적합 될 가능성을 줄인다.

다른 기법으로는 미니배치 판별이 있는데, 이는 배치간 얼마나 비슷한 이미지가 있는지 측정하여 통계를 판별자에게 제공하고, 판별자는 다양성이 부족한 가짜 이미지 배치 전체를 쉽게 거부하여 생성자가 다양한 이미지를 생성하도록 유도하여 모드 붕괴의 위험을 줄인다.

 - 심층 합성곱 GAN(DCGAN)

   판별자에 있는 풀링층을 스트라이드 합성곱으로 바꾸고, 생성자에 있는 풀링층은 전치 합성곱으로 바꾼다

   생성자의 출력층과 판별자의 입력층은 제외하고 생성자와 판별자에 배치 정규화를 사용한다.

   층을 깊게 쌓기 위해 완전 연결 은닉층을 제거하고 출력층에는 tanh 함수를 하고 출력층을 제외한 생성자의 다른 층에는 ReLU활성화 함수를 사용한다.

   판별자는 모든 층에서 LeakyReLU를 사용한다.

   다만 DCGAN은 주어진 데이터의 다양한 모드 간의 연결을 부족하게 학습할 수 있어 국부적으로는 특징이 구분되지만 전체적으로는 일관성 없는 이미지를 얻을 가능성이 높다.

 - ProGAN

   훈련 초기에 작은 이미지를 생성하고 점진적으로 생성자와 판별자에 합성곱 층을 추가해 갈수록 큰 이미지를 만드는 방법이다.

   적층 오토인코더와 유사하게 이전에 훈련된 층은 그래도 훈련 가능하도록 두고 생성자의 끝과 판별자의 시작 부분에 층을 추가한다.

   출력의 크기를 키우기 위해서는 합성곱 층에 업샘플링 층을 추가하여 크기를 2배씩 키운다.

   그리고 전 합성곱 층의 훈련된 가중치를 잃지 않기 위해 업샘플링된 특성 맵을 새로운 합성곱 층에 넣어 새로운 출력을 원래 출력층으로 줘서 새로운 출력과 원래 출력의 가중치 합으로 최종 출력을 결정한다.

    - 모드 붕괴를 막기위한 기법

       - 미니배치 표준편차 층

         판별자의 마지막 층에 추가하여 입력에 있는 모든 위치에 대해 모든 채널과 배치의 모든 샘플에 걸쳐 표준편차를 계산한다.

         만약 생성자가 만든 이미지에 다양성이 부족하면 판별자의 특성 맵 간의 표준편차가 작게된다.

         따라서 미니배치 표준편차 층을 통해 판별자가 쉽게 통계를 얻을 수 있고, 다양성이 아주 작은 이미지를 만드는 생성자에게 속을 가능성이 적어진다.

      - 픽셀별 정규화 층

        생성자의 합성곱 층 뒤에 픽셀별 정규화 층을 추가하여 동일한 이미지의 동일한 위치에 있는 모든 활성화 채널에 대해 정규화한다.

        이는 생성자와 판별자 사이의 과도한 경쟁으로 활성화 값이 폭주되는 것을 막는다.
        
