# **Combine Rotators 노드 상세 가이드**

언리얼 엔진의 블루프린트에서 **Combine Rotators** 노드는 두 회전(Rotator) 값을 결합하여 새로운 회전값을 계산하는 데 사용됩니다. 단순히 두 각도를 더하는 것 이상의 복잡한 수학적 계산을 내부적으로 처리합니다.

## **1\. 주요 역할 (Role)**

이 노드의 핵심 역할은 \*\*"A라는 회전 상태에서 B만큼 더 회전했을 때의 최종 회전값"\*\*을 구하는 것입니다.

* **계층적 회전:** 어떤 물체가 이미 특정 방향을 보고 있을 때, 그 방향을 기준으로 상대적인 회전을 추가하고 싶을 때 사용합니다.  
* **쿼터니언(Quaternion) 계산:** 내부적으로는 단순히 ![][image1]를 하는 것이 아니라, 회전 행렬이나 쿼터니언 곱셈을 통해 계산됩니다. 덕분에 **짐벌 락(Gimbal Lock)** 문제를 방지하며 정확한 3D 회전을 구현할 수 있습니다.

## **2\. 단순 덧셈(Add)과의 차이점**

초보자들이 흔히 하는 실수는 Rotator \+ Rotator 노드(단순 산술 더하기)를 사용하는 것입니다.

| 구분 | Rotator \+ Rotator (단순 합) | Combine Rotators |
| :---- | :---- | :---- |
| **계산 방식** | 각 축의 수치를 산술적으로 더함 | 첫 번째 회전 좌표계를 기준으로 두 번째 회전을 적용 |
| **결과** | 월드 좌표계 기준으로만 회전이 누적됨 | **상대적(Relative)** 회전 처리가 가능함 |
| **정확도** | 특정 각도에서 회전이 꼬이는 짐벌 락 발생 위험 | 수학적으로 정확한 회전 결과 제공 |

## **3\. 사용 예시 (Use Cases)**

### **① FPS 게임의 반동(Recoil) 구현**

총을 쏠 때 카메라가 위로 튀는 반동을 구현한다고 가정합시다.

* 현재 카메라의 회전값(A)에 반동만큼의 회전값(B)을 **Combine** 하면, 플레이어가 어느 방향을 보고 있더라도 정확하게 카메라의 '위쪽' 방향으로 반동이 적용됩니다.

### **② 소켓(Socket) 및 부착물 회전**

캐릭터의 손에 칼을 들려줄 때, 캐릭터 손의 회전값에 칼의 상대적 오프셋 회전값을 결합하여 칼이 항상 손의 방향을 올바르게 따르도록 할 수 있습니다.

### **③ 로컬 축 기준 회전**

물체를 자기 자신의 로컬 축(Local Axis)을 기준으로 회전시키고 싶을 때, 현재 월드 회전값에 회전시키고 싶은 양을 결합하면 됩니다.

## **4\. 주의사항: 순서의 중요성**

Combine Rotators 노드에는 **A**와 **B** 두 개의 입력 핀이 있습니다. 3D 회전 수학에서 곱셈 순서는 결과에 영향을 미칩니다.

* **A (Base):** 기준이 되는 현재의 회전값.  
* **B (Add):** 추가하고자 하는 상대적인 회전값.

일반적으로 \*\*\[현재 월드 회전\]\*\*을 A에 넣고, \*\*\[추가할 로컬 회전\]\*\*을 B에 넣어야 우리가 의도한 "자신을 기준으로 더 회전하기"가 정상적으로 작동합니다.

## **5\. 요약**

* **Combine Rotators**는 두 회전을 합성하는 노드이다.  
* 단순 덧셈보다 수학적으로 정교하며(쿼터니언 기반), 짐벌 락을 방지한다.  
* \*\*"상대적인 회전"\*\*을 적용할 때 반드시 사용해야 하는 필수 노드이다.

[image1]: <data:image/png;base64,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>