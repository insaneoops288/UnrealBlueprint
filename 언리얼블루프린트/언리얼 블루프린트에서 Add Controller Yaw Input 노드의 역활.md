# **Add Controller Yaw Input 노드 상세 분석**

Add Controller Yaw Input은 플레이어의 입력을 받아 컨트롤러의 **Yaw(수직축 기준 회전)** 값을 변화시키는 기능을 합니다. 주로 1인칭이나 3인칭 게임에서 마우스를 좌우로 움직여 화면을 돌릴 때 사용됩니다.

## **1\. 주요 역할**

* **컨트롤 회전(Control Rotation) 수정**: 플레이어 컨트롤러가 가지고 있는 회전 값 중 'Yaw' 축에 입력된 수치를 더합니다.  
* **카메라 및 캐릭터 방향 제어**: 이 노드를 통해 회전된 컨트롤러의 방향은 카메라의 시점이나 캐릭터의 몸체 방향을 결정하는 기초 데이터가 됩니다.

## **2\. 노드 파라미터**

* **Target**: 기본적으로 Pawn 또는 Character가 대상입니다. (실제로는 내부적으로 연동된 PlayerController에 값을 전달합니다.)  
* **Val (Float)**: 더해줄 회전량입니다.  
  * 양수(+) 값: 오른쪽으로 회전  
  * 음수(-) 값: 왼쪽으로 회전  
  * 보통 마우스의 Mouse X 축 이벤트의 Axis Value를 그대로 연결합니다.

## **3\. 작동 조건 (중요 설정)**

이 노드만 사용한다고 해서 캐릭터가 무조건 회전하는 것은 아닙니다. 캐릭터 블루프린트의 **디테일(Details) 패널**에서 다음 설정을 확인해야 합니다.

### **캐릭터 본체가 함께 회전하게 하려면:**

* **Pawn** 섹션: Use Controller Rotation Yaw를 **Check(True)** 해야 합니다.  
* 이 설정이 켜져 있으면, 컨트롤러의 Yaw가 변할 때 캐릭터 모델도 그 방향을 즉시 바라보게 됩니다.

### **카메라만 회전하고 캐릭터는 나중에 회전하게 하려면 (3인칭):**

* **Character Movement** 컴포넌트: Orient Orientation to Movement를 **Check**합니다.  
* **Pawn** 섹션: Use Controller Rotation Yaw를 **Uncheck**합니다.  
* **Spring Arm** 컴포넌트: Use Pawn Control Rotation을 **Check**합니다.  
* 이렇게 하면 마우스로 화면(카메라)은 자유롭게 돌리되, 캐릭터는 이동할 때만 그 방향을 쳐다보게 됩니다.

## **4\. 수학적 개념 (Yaw, Pitch, Roll)**

언리얼 엔진의 3D 회전축은 다음과 같습니다.

* **Yaw (Z축)**: 좌우 회전 (도리도리) \-\> Add Controller Yaw Input  
* **Pitch (Y축)**: 상하 회전 (끄덕끄덕) \-\> Add Controller Pitch Input  
* **Roll (X축)**: 좌우 기울기 (갸우뚱) \-\> Add Controller Roll Input

## **5\. 블루프린트 구성 예시**

가장 일반적인 마우스 좌우 회전 구현 방법입니다.

1. **InputAxis Turn** 이벤트 생성 (Project Settings에서 설정된 마우스 X축)  
2. **Add Controller Yaw Input** 노드 배치  
3. Axis Value 핀을 Val 핀에 연결

### **요약**

Add Controller Yaw Input은 \*\*"플레이어의 컨트롤러를 좌우로 얼마나 돌릴 것인가"\*\*를 결정하는 명령입니다. 게임의 시점 처리와 캐릭터의 방향 전환을 위해 가장 빈번하게 사용되는 노드 중 하나입니다.