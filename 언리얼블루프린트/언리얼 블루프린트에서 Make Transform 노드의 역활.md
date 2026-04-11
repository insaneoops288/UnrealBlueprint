# **언리얼 블루프린트: Make Transform 노드 가이드**

**Make Transform** 노드는 분리된 벡터(Vector)와 로테이터(Rotator) 데이터를 언리얼 엔진의 표준 데이터 형식인 Transform 구조체로 묶어주는 유틸리티 노드입니다.

## **1\. 노드의 구성 요소 (Input Pins)**

이 노드는 세 가지 주요 입력을 받습니다:

1. **Location (Vector):**  
   * 객체가 배치될 3D 월드 또는 부모 공간의 **좌표** (X, Y, Z)입니다.  
   * 기본값은 (0, 0, 0)입니다.  
2. **Rotation (Rotator):**  
   * 객체의 **회전 값** (Roll, Pitch, Yaw)입니다.  
   * 기본값은 (0, 0, 0)입니다.  
3. **Scale (Vector):**  
   * 객체의 **크기 배율** (X, Y, Z)입니다.  
   * 기본값은 (1, 1, 1)이며, 0으로 설정할 경우 객체가 보이지 않게 되므로 주의해야 합니다.

## **2\. 주요 역할 및 용도**

### **데이터 그룹화 (Packaging)**

언리얼의 많은 함수와 노드(예: SpawnActorFromClass, SetWorldTransform)는 위치, 회전, 크기를 각각 따로 받는 대신 하나의 Transform 핀을 입력으로 요구합니다. 이때 Make Transform을 사용하여 여러 데이터를 하나로 묶어 전달합니다.

### **동적 트랜스폼 생성**

게임 실행 도중 특정 계산 결과에 따라 객체의 위치나 크기를 결정해야 할 때, 계산된 값들을 이 노드에 연결하여 실시간으로 트랜스폼을 생성할 수 있습니다.

### **주요 활용 예시**

* **SpawnActor From Class:** 새로운 액터를 월드에 생성할 때 생성 위치와 방향을 설정하기 위해 필수적으로 사용됩니다.  
* **Add Child Component:** 블루프린트 내에서 컴포넌트를 생성하고 초기 배치를 설정할 때 사용됩니다.  
* **Set Relative Transform:** 부모 객체를 기준으로 자식 객체의 상대적인 위치/회전/크기를 한꺼번에 변경할 때 유용합니다.

## **3\. 유용한 팁**

### **핀 분할 (Struct Pin Splitting)**

사실 Make Transform 노드를 명시적으로 꺼내지 않고도 동일한 작업을 수행할 수 있습니다.

* 트랜스폼 입력 핀을 가진 노드(예: Spawn Actor)의 Spawn Transform 핀 위에서 **우클릭**한 뒤 \*\*'Split Struct Pin' (구조체 핀 분할)\*\*을 선택하면, 해당 노드에서 직접 Location, Rotation, Scale을 입력할 수 있도록 핀이 확장됩니다.  
* **Make Transform** 노드는 로직이 복잡하여 트랜스폼 생성을 별도의 단계로 관리하고 싶을 때 가독성을 위해 주로 사용합니다.

### **반대 노드: Break Transform**

Make Transform의 정확히 반대 역할을 하는 노드는 **Break Transform**입니다. 이 노드는 하나의 트랜스폼 변수를 다시 위치, 회전, 스케일로 분해하여 각각의 값을 추출할 때 사용합니다.

## **4\. 요약**

| 특징 | 내용 |
| :---- | :---- |
| **목적** | 위치, 회전, 스케일 값을 하나의 트랜스폼 구조체로 결합 |
| **출력 타입** | Transform (구조체) |
| **주의사항** | Scale 값이 (0,0,0)이 되지 않도록 주의 (기본값 1 권장) |
| **연관 노드** | Break Transform, SpawnActorFromClass, SetWorldTransform |

