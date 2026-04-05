# **Blend Poses by bool 노드 상세 설명**

**Blend Poses by bool** 노드는 조건부 애니메이션 전환을 위한 가장 기본적인 노드 중 하나입니다. 주로 캐릭터의 특정 상태(예: 무기를 들었는가?, 땅에 서 있는가?)에 따라 애니메이션 세트를 통째로 바꿀 때 사용됩니다.

## **1\. 주요 입력 핀 (Input Pins)**

* **Active Value**: 전환의 기준이 되는 부울 변수입니다. 이 값이 True면 True 포즈를, False면 False 포즈를 출력합니다.  
* **True Pose**: Active Value가 참일 때 재생될 포즈(애니메이션)를 연결합니다.  
* **False Pose**: Active Value가 거짓일 때 재생될 포즈(애니메이션)를 연결합니다.

## **2\. 주요 설정값 (Details 패널)**

* **True Blend Time / False Blend Time**: 포즈가 바뀔 때 얼마나 부드럽게 전환될지를 결정하는 시간(초)입니다.  
  * 예: 0.2로 설정하면 0.2초 동안 서서히 포즈가 섞이며 바뀝니다.  
* **Transition Type**: 블렌딩 방식입니다.  
  * **Standard**: 일반적인 보간 방식입니다.  
  * **Inertialization**: 관성 블렌딩(Inertial Blending)을 사용합니다. 성능이 좋고 자연스러운 전환을 보여주지만, 뒤에 'Inertialization' 노드가 필요합니다.

## **3\. 주요 활용 사례**

* **전투/비전투 전환**: 무기를 장착한 상태(IsArmed \== True)와 비무장 상태(IsArmed \== False)의 Idle/Run 애니메이션 전환.  
* **공중/지면 상태**: 점프 중인지(IsInAir \== True) 아니면 걷고 있는지에 따른 포즈 전환.  
* **부상 상태**: 체력이 일정 수치 이하일 때(IsInjured \== True) 절뚝거리는 애니메이션으로 전환.

## **4\. 장점 및 주의사항**

* **장점**: 상태 머신(State Machine)을 복잡하게 만들지 않고도 애님 그래프 안에서 간단하게 포즈를 분기할 수 있습니다.  
* **주의**: 포즈가 전환될 때 두 포즈가 동시에 계산(Evaluate)되므로, 너무 많은 블렌드 노드를 중첩해서 사용하면 성능에 영향을 줄 수 있습니다. (Fast Path 최적화 확인 필요)