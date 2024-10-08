# 🕹️DQN 학습 - 도립진자의 제어
: Pendulum System 환경의 DQN을 활용한 제어기 강화 학습

## 🧩 Requirements
- gym
- pytorch

## 📁 Reference Website
https://gymnasium.farama.org/environments/classic_control/pendulum/

## 🧩 Problem Solving Process
#### Cartpole system의 DQN학습 코드에 대한 이해

<img src=https://gymnasium.farama.org/_images/cart_pole.gif width="400" height="300"/>  <----- DQN학습의 CartPole

Cartpole System은 pole이 수직상태를 유지하도록 cart를 밀거나 당겨야 한다. 목표는 pole을 쓰러트리지 않고 오래 유지하는 것이다. OpenAI Gym에서 제공한 Cartpole 예제 코드는 에이전트가 최적의 정책을 학습하는데 사용된다. 에이전트는 현재 state를 관찰하고 action을 선택하며, 그에 따른 다음 상태와 보상을 받는다. 이 과정을 반복하며 더 좋은 정책을 학습하게 된다.


여기서 사용된 DQN(Deep Q Nextwork)은 딥러닝과 강화학습을 결합한 알고리즘이다. 딥러닝 신경망을 사용하여 강화학습 에이전트를 학습시키고, 최적의 행동을 결정하는 Q함수를 근사하는 방식을 가진다.


🔹Q러닝 학습에 사용되는 행동가치함수 Q를 개선하는 공식 : **Q(s,a)=Q(s,a)+α((r^'+γ〖max〗_(a^' ) Q(s^',a^' ))-Q(s,a))**


DQN이전에 Q함수는 상태-행동 쌍으로 이루어진 가치함수로, 테이블 환경에서 상태-행동에 대한 결과 중 가장 최대 값을 가지는 행동을 최적행동으로 선택한다. 이러한 Q함수 추정을 위해 off-policy 시간차 학습 알고리즘인 Q러닝을 사용하는데...


다만, 이전의 학습까지 적용했던 환경은 제한적인 상태와 행동을 가졌기 때문에 모든 상태-행동 쌍을 비교했다. 하지만, Cartpole의 경우, 셀 수 없는 상태와 행동이 존재하는 환경이기 때문에 모두 비교하는 것은 불가능에 가깝다. 그렇기 때문에 필요한 것이 Q함수의 근사화이다. 가중치가 존재하는 신경망(θ)을 사용해서 각 상태에서 모든 작업에 대한 Q값을 근사화 할 수 있다.


그러므로, Q네트워크에서 신경망을 통해 Q함수를 근사화 하는 것은 손실함수를 정의하여 Q-value의 목표 값과 예측 값 사이의 차이를 가중치를 업데이트하고 경사 하강법을 통해 최소화한다. 

## 🧩 DQN Pendulum design process
<img src=https://gymnasium.farama.org/_images/pendulum.gif width="300" height="300"/>  <----- DQN학습으로 Pendulum을 위로 고정해야한다.

Pendulum 이산적이지 않다. 상태 공간은 3차원으로 구성되어 있으며, 이 3차원은 각도, 각속도, 그리고 진자의 끝점 위치로 이루어져 있다. 행동 공간은 연속적으로 [-2,2]사이로 이루어져 있다. 기본적으로 DQN은 cartpole같은 이산적 행동 공간에 적합하다. Pendulum 공간을 이산적 행동 공간으로 변환해야 한다.
그리고 e-greedy정책을 사용해서 학습된 정책을 더 많이 활용할 수 있도록 해야 한다. (학습이 진행됨에 따라서 엡실론의 값을 줄여가면서 탐색의 비율을 줄인다.)

(1) 기본적인 매개변수를 설정 : 학습률, 할인률, 엡실론의 감소율, 메모리 사이즈..


(2) Q네트워크 함수 설계(Qnetwork) : 기존 딥러닝에서 쓰이는 인공신경망과 같은 구조


(3) Agent 구현 : Q네트워크 학습의 과정인 sample생성, 타겟 모델 업데이트 등 


(4) DQN 학습 알고리즘 : 리플레이 메모리 저장

## 🧩 Result
