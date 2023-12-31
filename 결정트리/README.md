## 결정 트리

결정트리는 먼저 루트노드에서 시작한다.

그리고 각 노드에서 조건을 만족하면 왼쪽 자식노드로 만족하지 않으면 오른쪽 자식노드로 이동한다.

이때 자식노드를 가지지 않는 리프노드라면 추가적인 검사를 하지 않는다.

### 노드 속성

- sample : 얼마나 많은 데이터를 해당 노드에서 분류를 할건지
  
- value : 각 데이터들이 해당 노드에서 어떤 클래스에 속해있는지

- gini : 불순도로 아래와 같은 공식의 값에 따라 표기한다.

$G_i = 1 - \sum\limits_{k=1}^n p_{i,k}^2 \quad$  ($p_{i,k}$는 i번째 노드에있는 샘플중 클래스 k에 속한 샘플의 비율이다.)

결정트리에서 특정 샘플이 어떤 클래스에 속하는지 추정하려면 해당 데이터의 값을 트리에 넣었을 때 나오는 최종 노드의 각 클래스별 확률을 비교하여 가장 확률이 높은 클래스로 예측하면 된다.

### cart훈련 알고리즘

사이킷런에서 결정트리를 훈련시키기 위해 cart알고리즘을 사용한다.

이는 훈련세트를 가장 순수한 서브셋으로 나눌 수 있는 특성 $k$ 와 임곗값 $t_k$ 를 찾아서 두 개의 서브셋으로 나눈다.

이때 비용함수는 $j(k,t_k) = \frac{m_{left}}{m} G_{left} + \frac{m_{right}}{m} G_{right}$이다.

이러한 방식으로 계속 서브셋을 나눠서 최대 깊이가 될때까지 분할을 하거나, 불순도를 줄일 수 없으면 멈추게 된다.

### 탐욕적 알고리즘

cart알고리즘은 탐욕적 알고리즘이다.

탐욕적 알고리즘은 각 단계에서 가장 최선의 선택을 하며, 이 선택이 전체 문제의 최적해로 이어질 것이라고 기대하는 것이다.

하지만 이러한 선택이 반듯이 전체 문제의 최적해를 보장하는 것은 아니다.

### 결정 트리의 계산 복잡도

예측을 하기 위해서는 루트 노드에서 리프 노드까지 탐색해야한다.

결정트리의 경우 일반적으로 균형을 이루고 있으므로 복잡도는 $O(log_2(m))$ (m:데이터의 수)이다.

여기서 각 노드에서는 하나의 특성만 비교하기 때문에 특성의 수가 많아도 복잡도가 달라지지 않아 속도가 빠른 장점이 있다.

다만, 훈련 알고리즘의 경우 각 노드에서 모든 훈련샘플의 특성을 비교해야하기 때문에 $O(n m log_2(m))$이다.

### 불순도

기본적으로 결정 트리에서 gini분순도를 사용하지만, 엔트로피 불순도를 사용할 수도 있다.

엔트로피 : $H_i = - \sum\limits_{k=1}^n p_{i,k} log_2(p_{i,k}) \quad 단, P_{i,k} \neq 0$

지니 불순도와 엔트로피의 실제값은 차이가 거의 없어 둘다 비슷한 트리를 만들어 낸다.

다만, 지니 불순도는 로그를 계산할 필요가 없어서 계산이 조금 더 빠르고 가장 빈도가 높은 클래스를 한쪽 가지로 고립시키는 경향이 있다.

반면에 엔트로피는 균형잡힌 트리를 만든다.

### 결정트리 규제

결정트리의 경우 제한을 두지 않으면 트리가 훈련 데이터에 아주 가깝게 맞추려고 하여 과대적합하기 쉽다.

따라서 훈련 데이터에 과대적합을 피하기 위해서 자유도를 제한할 필요가 있다.

일반적으로 결정 트리의 최대 깊이를 제어하는데 max_depth을 줄이면 과대적합할 위험을 감소시킨다.

이 외에도 분할되기 위해 가져야하는 최소 샘플수, 리프 노드의 최소 샘플의 개수, 리프 노드의 최대 수등의 다양한 규제를 할 수가 있다.

### 결정트리 - 회귀

결정트리의 분류모델과 차이점은 각 노드에서 클래스를 예측하는 대신 어떤 값을 예측하는 것이다.

그리고 예측값은 트리를 순회하여 가장 많은 샘플이 있는 리프 노드를 훈련 샘플의 평균 타깃값으로 예측한다.

결정 트리의 경우 이해하기 쉽고 해석하기 쉬워 여러 용도로 사용할 수 있다는 장점이 있다.

하지만 결정트리는 계단 모양으로 결정 경계를 만들기 때문에 데이터의 회전에 민감하여 같은 데이터이더라도 회전에 따라 일반화가 잘 안될수도 있다.

이러한 문제를 해결하기 위해서 pca기법을 사용하면 된다. 

 - Pca : 데이터를 주성분 분석으로 주성분을 사용하여 새로운 기저를 찾고 데이터를 회전시키면 결정 트리가 데이터의 회전에 민감하지 않도록 할 수 있다.

이뿐만 아니라 결정 트리는 훈련데이터의 노이즈에 민감하게 반응한다는 단점이 있다.

결정 트리는 훈련 데이터에 맞추려는 경향이 있으며, 노이즈가 포함된 데이터까지 학습하면 모델이 훈련 데이터에 과적합될 가능성이 높아진다.

이러한 과대적합은 새로운 데이터에 대한 일반화 성능을 저하시킬 수 있다.
