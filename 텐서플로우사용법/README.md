## 텐서플로우로 사용자 정의 모델

텐서플로우는 현제 가장 인기있는 딥러닝 라이브러리이다.

 - 텐서플로우의 장점

   1. GPU를 지원하고 분산 컴퓨팅을 지원한다.
  
      GPU는 계산을 작은 단위로 나눠 여러 스레드에 병렬로 실행하여 속도를 극적으로 향상한다.

      그리고 딥러닝 모델 훈련 및 추론을 여러 대의 컴퓨터로 분산시키면 더 큰 모델, 더 빠른 훈련 및 높은 가용성을 제공하여 효율성을 향상시킬 수 있다.

   2. JIT컴파일러를 포함
     
      이는 프로그램이 실행 중에 코드를 컴파일하므로 실행 환경과 하드웨어에 대한 정보를 활용하여 코드를 최적화할 수 있다.

      그리고 실행 패턴을 분석하고 최적화를 수행할 수 있습니다.

      이로 인해 자주 실행되는 코드 블록은 빠르게 수행되고, 사용하지 않는 노드는 가지치기 하여 메모리 사용량을 줄이고 성능을 향상시킨다.
      
   3. 플랫폼에 중립적이다.
  
      하나의 특정 플랫폼에 종속되지 않아 Windows, macOS, Linux와 같은 다양한 운영 체제에서 동작하며, x86, ARM, GPU 및 TPU와 같은 다양한 하드웨어 아키텍처에서도 지원된다.
   
   4. 자동 미분기능과 다양한 고성능 옵티마이저를 제공하여 쉽게 손실함수를 최소화 할 수 있다.

텐서플로우에는 고수준 API인 keras가 있어 딥러닝을 수행하기 아주 편하다.

다만 여기서 자신만의 손실함수, 지표, 층, 모델, 초기화, 규제, 가중치 규제등을 세부적으로 제어할 때 사용자 정의 모델을 쓴다.




