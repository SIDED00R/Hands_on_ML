## 신경망

인공신경망은1943년에 처음 소개되었다.

초기에 기계와 대화가 가능할 것이라고 생각하였지만 생각보다 기술 발전이 많이 없었고, 그 결과 오랫동안 침체기에 있었다.

하지만 최근들어 신경망 훈련을 위한 데이터의 수가 많아지고, 90년대 이후 컴퓨터 하드웨어가 크게 발하였고, 훈련 알고리즘이 향상되었다.

이러한 이유들로 기술의 진보와 기대에 대한 투자의 선순환이 이뤄져 빠른 발전이 있었다.

### 페셉트론

페셉트론은 가장 간단한 인공 신경망 구조중 하나로 1957년 프랑크 로젠블라트가 제안했다.

페셉트론에서 입출력이 숫자이고, 각각의 입력 연결은 가중치와 연관되어있다.

TLU라는 형태의 인공뉴런을 기반으로 하는 퍼셉트론은 입력의 가중치 합을 계산한 뒤, 계산된 합에 계단함수를 적용하여 결과를 출력한다.

퍼헵트론에서 가장 많이 사용되는 계단함수는 헤비사이드 계단함수로 값이 음수일 때 0을 가지고 양수일때는 1을 갖는 함수이다.

$$
\mbox{heaviside}(z)=
\begin{cases}
0 & \mbox{if }z < 0 \\
1 & \mbox{if }z \geq 0
\end{cases}
$$

퍼셉트론은 층이 1개인 TLU로 구성되는데 각 TLU는 모든 입력에 연결되어있다.

이렇게 한 층의 모든 뉴런이 이전 층의 모든 뉴런과 연결되어있을 때 완전 연결층 또는 밀집층이라고 한다.

퍼셉트론의 입력은 입력 뉴런을 통해서 주입될 때 입력층에서 편향 특성이 더해져 입력이 된다.

퍼셉트론 학습규칙은 오차가 감소하도록 연결을 강화시킨다. 

즉, 한 번에 한 개의 샘플이 주입되면 각 샘플에 대해 예측을 만든다.

이때 잘못된 예측을 하는 출력 뉴런에 대해 올바른 예측을 하도록 입력에 연결된 가중치를 강화한다.

다만 퍼셉트론은 심각한 약점이 있는데, 실제로 XOR분류 문제와 같은 간단한 문제를 해결 할 수 없다.

하지만 퍼셉트론을 여러 개 쌓아 올려 다층 퍼셉트론을 만들면 해결 할 수 있다.

### 다층 퍼셉트론

다층 퍼셉트론은 입력층 하나와 은닉층이라고 불리는 하나 이상의 TLU층과 마지막 출력층으로구성된다.

여기서 은닉층을 여러 개 쌓아 올린 신경망을 심층 신경망이라고 한다.

초기에는 다층 퍼셉트론을 훈련할 방법을 찾기 위해 노력했지만 성공하지 못하였다. 

그러던 도중 역전파 훈련 알고리즘을 통해 그레디언트를 자동으로 계산하는 경사하강법으로 다층 퍼셉트론의 훈련에 성공하였다.

역전파 알고리즘은 후진 모드 자동 미분기법을 사용하는데 미니배치씩 학습을 에포크 수만큼 반복해서 진행하여 전체 훈련 세트를 처리한다.

각 미니배치는 입력층을 전달되어 첫 번째 은닉층으로 간다.

그 후 해당 층의 모든 뉴런에 대한 출력을 계산하고 결과를 다음 층으로 전달한다.

이와 같은 방법으로 출력층에서 출력할 때 까지 반복한다.

이는 정방향 계산으로 역방향 계산과의 차이는 역방향에서는 계산을 위해 중간 계산값을 모두 저장해야한다는 점이다.

그 후 알고리즘이 손실함수를 사용하여 출력 오차를 측정하고, 각 출력이 오차에 기여하는 정도를 계산한다.

입력층에 도달할 때까지 역방향으로 계속 이전층의 연결 가중치의 오차 기여도를 측정한다.

그러면 오차 그레디언트를 거꾸로 전파하면서 효율적으로 모든 연결 가중치에 대한 오차 그레디언트를 측정할 수 있다.

마지막으로는 경사 하강법을 수행하여 계산 오차 그레디언트를 사용해 가중치를 수정한다.

 - 자주쓰이는 활성화 함수
   
     - 시그모이드 함수 : $\sigma(z) = \frac{1}{1 + \exp(-z)}$
       
       계단 함수의 경우 수평선만 존재 함으로 그레디언트를 계산할게 없다.      
       따라서 어디서든 0이 아닌 그레디언트가 잘 정의되어있는 시그모이드 함수로 계단함수를 대체하였다.
  
     - 하이퍼볼릭 탄젠트 함수 : $\tanh(z) = 2\sigma(2z) - 1$
       
       로지스틱 함수와 비슷하지만 출력 범위가 -1 ~ 1로 차이가 있다.
       이는 훈련 초기에 각 충의 출력을 원점 근처로 모으는 경향이 있어 빠르게 수렴되도록 도와준다.
       
     - ReLU함수 : $\mbox{ReLU}(z) = \max(0, z)$
       
       연속이지만 $z=0$에서 미분이 가능하지 않다.
       출력에 최댓값이 없다는 점에서 경사 하강법의 출력 범위 양 끝단에서 발생하는 문제를 완화해준다. 
       
출력이 항상 양수여야 한다면 relu사용할 수 있다.

활성화 함수를 쓰는 이유는 선형 변환을 여러 개 연결하기만 하면 계속 선형 변환만 나오게 된다.

따라서 층 사이에 비선형 변환을 추가하지 않으면 많은 층을 쌓아도 하나의 층과 똑같아지게 된다.

훈련에 사용하는 손실함수는 전형적으로 평균 제곱 오차이다.

하지만 이상치가 많을 때는 평균 절댓값 오차를 사용할 수도 있고, 둘을 조합한 후버 손실을 사용할 수도 있다.

### 분류 - 다층 퍼셉트론

로지스틱 활성화 함수의 출력은 0과 1사이의 실수이기 때문에 이를 양성 클래스에 대한 예측 확률로 사용하면 이진 분류 문제를 쉽게 할 수 있다.

그리고 만약 각 샘플이 3개 이상의 클래스 중 하나의 클래스에만 속할 수 있다면 다중 분류를 해야한다.

따라서 출력층에 모든 예측확률의 합이 1이 되는 소프트맥스 활성화 함수를 사용해야한다.

손실함수에는 확률 분포를 예측해야함으로 일반적으로 크로스 엔트로피 손실함수가 사용된다.

### 신경망 하이퍼파라미터 튜닝

신경망은 은닉층의 수, 뉴런의 개수, 학습률등 조절해야하는 하이퍼파라미터가 많기 때문에 유연함이 때로는 단점이 되기도 한다.

하이퍼파라미터중 최적값을 찾는 가장 간단한 방법은 많은 조합을 검증세트로 평가를 해 통해 가장 좋은 결과를 내는 것을 찾아내는것이다.

이에 대해서는 대표적으로 그리드 탐색이나 랜덤탐색법을 사용해 볼 수 있다.

이때 하이퍼파라미터의 수가 많기 때문에 랜덤탐색을 사용하는 것이 더 좋을 수 있다.

하지만 이들은 훈련에 시간이 많이 걸리면 탐색할 수 있는 하이퍼파라미터 공간에 제약이 생긴다.

따라서 처음에 큰 범위로 탐색하여 대략적인 위치를 찾고, 찾은 값을 바탕으로 더 좁은 범위를 탐색하는 방식으로 진행하면 좋은 하이퍼파라미터를 찾을 수 있다.

하지만 이러한 방식은 시간이 많이 걸리므로 최상의 방법이 아니다.

따라서 Hyperopt, hyperas, 케라스 튜너, scikit-optimize 등과 같은 파이썬의 최적화 라이브러리를 사용하면 더 나은 결과를 만들 수 있다.

사실 하이퍼 파라미터 튜닝은 지금 활발히 연구되는 분야로 점점 더 발전된 튜닝기법이 나오고 있다.

 - 은닉층의 개수
   
   은닉층이 하나이더라도 나쁘지 않은 결과를 얻을 수 있다.
   
   이론상으로도 뉴런의 개수가 충분히 많다면 아주 복잡한 함수도 하나의 은닉층으로 풀 수 있다.

   하지만 심층신경망이 파라미터 효율성이 훨씬 좋기 때문에 은닉층이 충분히 있는 것이 더 좋다.

   그리고 계층 구조는 심층 신경망이 좋은 솔루션으로 빨리 수렴하게끔 도와주고, 일반화 능력도 향상시킨다.

   예를 들어 비슷한 모델을 학습한다면 기존의 모델의 하위 계층을 새로운 모델에 사용할 수 있다.

   그러면 처음 몇 개의 층의 가중치와 편향을 난수로 초기화 하는 대신 이미 구한 신경망의 가중치와 편향값으로 초기화 하여 저수준 구조에 대한 학습을 줄이는 전이학습을 사용할 수 있다.

 - 은닉층의 뉴런 개수

   과대적합 하기 전까지 점진적으로 뉴런 수를 늘릴수도 있지만, 차라리 많은 층과 뉴런을 가진 모델을 선택하고 과대적합하지 않도록 조기 종료나 규제 기법을 사용하는 것이 더 간단하고 효과적이다.

 - 학습률

   학습률을 매우 낮은 학습률에서 시작해 점진적으로 큰 학습률까지 키우게 되면 최적의 학습률을 얻을 수 있다.

   하지만 이보다 학습률에 대한 손실을 그래프로 그려 최적의 학습률을 찾는 것이 더 좋다.

 - 배치크기

   큰 배치를 사용하면 GPU와 같이 하드웨어 가속기를 효율적으로 사용할 수 있다.

   하지만 큰 배치를 사용할 경우 초기에 불안정하게 훈련이 되어 작은 배치 크기로 훈련된 모델만큼 일반화 성능이 안날수있다.

