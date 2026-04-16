# **Get Rotation X Vector 노드 상세 가이드**

언리얼 엔진에서 **Get Rotation X Vector** 노드는 회전(Rotator) 데이터를 방향(Vector) 데이터로 변환하는 가장 핵심적인 노드 중 하나입니다.

## **1\. 핵심 역할**

이 노드는 입력받은 **Rotator(Pitch, Yaw, Roll)** 값을 사용하여, 해당 회전 상태에서 \*\*X축(정면)\*\*이 어느 방향을 향하고 있는지를 나타내는 \*\*길이가 1인 방향 벡터(Forward Vector)\*\*를 반환합니다.

* **입력:** Rotator (회전값)  
* **출력:** Vector (정면 방향을 나타내는 단위 벡터)

## **2\. 왜 X축인가요?**

언리얼 엔진의 좌표계(Coordinate System)에서 각 축은 다음과 같은 기본 의미를 가집니다:

* **X축:** 정면 (Forward)  
* **Y축:** 오른쪽 (Right)  
* **Z축:** 위쪽 (Up)

따라서 "Rotation X Vector"를 가져온다는 것은 "해당 회전 방향의 **정면 벡터**를 가져오겠다"는 뜻과 같습니다.

## **3\. 주요 활용 사례**

### **① 캐릭터 이동 (Forward Movement)**

캐릭터가 바라보는 방향으로 이동시키고 싶을 때 사용합니다.

* Get Control Rotation (컨트롤러가 바라보는 회전) → Get Rotation X Vector → Add Movement Input의 'World Direction'에 연결.

### **② 라인 트레이스 (Line Tracing / Raycasting)**

총기 발사나 상호작용을 위해 정면으로 레이저를 쏠 때 필요합니다.

* 시작 지점: Get Actor Location  
* 끝 지점: Get Actor Location \+ (Get Rotation X Vector \* 사거리)

### **③ 발사체 생성 (Projectile Spawning)**

총알이나 마법 구체를 발사할 때 진행 방향을 설정합니다.

* Spawn Actor from Class 노드의 'Spawn Transform'을 설정할 때, 회전값으로부터 진행 방향 벡터를 계산하여 발사체에 속도(Velocity)를 부여합니다.

## **4\. 관련 노드와 비교**

| 노드 이름 | 역할 | 결과값의 의미 |
| :---- | :---- | :---- |
| **Get Rotation X Vector** | 회전값의 정면 벡터 추출 | **Forward Vector** |
| **Get Actor Forward Vector** | 액터의 현재 정면 벡터 추출 | 현재 액터가 보는 방향 |
| **Get Forward Vector (라이브러리)** | Get Rotation X Vector와 동일함 | 회전 기반 정면 벡터 |
| **Get Rotation Y Vector** | 회전값의 오른쪽 벡터 추출 | **Right Vector** |
| **Get Rotation Z Vector** | 회전값의 위쪽 벡터 추출 | **Up Vector** |

## **5\. 수학적 배경 (참고)**

수학적으로 이 노드는 회전 행렬(Rotation Matrix)의 첫 번째 행(Row) 혹은 열(Column) 벡터를 추출하는 것과 같습니다. 결과값은 항상 \*\*단위 벡터(노멀라이즈된 상태)\*\*이므로, 여기에 원하는 수치(속도, 거리 등)를 곱해서 바로 사용할 수 있어 매우 편리합니다.

### **요약**

**Get Rotation X Vector**는 "내가 이만큼 회전해 있다면, 나의 '앞'은 정확히 어느 쪽인가?"라는 질문에 대한 답(벡터)을 주는 노드입니다.