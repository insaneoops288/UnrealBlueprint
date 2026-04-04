# **언리얼 엔진 블루프린트: 쿼터니언(Quaternion)의 역할**

언리얼 엔진에서 회전을 다룰 때 가장 익숙한 것은 \*\*Rotator(Pitch, Yaw, Roll)\*\*입니다. 하지만 복잡한 회전이나 부드러운 애니메이션을 구현할 때는 **Quaternion(사원수)** 노드와 개념이 필수적입니다.

## **1\. 쿼터니언이란?**

쿼터니언은 4개의 숫자(![][image1])로 이루어진 복소수 체계입니다. 로테이터가 세 개의 축을 기준으로 순차적으로 회전하는 방식이라면, 쿼터니언은 **하나의 회전축과 그 축을 중심으로 회전한 각도**를 하나의 단위로 표현합니다.

## **2\. 쿼터니언을 사용하는 이유 (역할)**

### **① 짐벌 락(Gimbal Lock) 방지**

로테이터(Euler Angles) 방식의 가장 큰 단점은 두 회전축이 겹쳤을 때 한 축의 자유도를 잃어버리는 **짐벌 락** 현상입니다. 쿼터니언은 네 개의 성분을 사용하여 수학적으로 계산하므로 짐벌 락이 발생하지 않습니다.

* **예시:** 비행기 시뮬레이션이나 우주 공간에서의 자유로운 회전 구현 시 필수.

### **② 부드러운 회전 보간 (SLERP)**

두 회전 값 사이를 부드럽게 연결할 때 로테이터는 최단 경로를 찾지 못하고 떨리는 현상이 생길 수 있습니다. 쿼터니언은 \*\*구면 선형 보간(SLERP, Spherical Linear Interpolation)\*\*을 사용하여 두 회전 사이의 가장 짧은 경로로 매끄럽게 회전시킵니다.

* **주요 노드:** Lerp (Quat) 또는 Slerp.

### **③ 복합 회전의 정확한 계산**

여러 회전 값을 합치거나 특정 축을 기준으로 회전시킬 때 로테이터 연산보다 정밀도가 높고 계산 속도가 빠릅니다.

## **3\. 블루프린트에서의 주요 관련 노드**

블루프린트에서 쿼터니언은 변수 타입으로 직접 노출되지 않는 경우가 많지만, 아래 노드들을 통해 내부적으로 쿼터니언 연산을 수행합니다.

| 노드 이름 | 역할 |
| :---- | :---- |
| **Combine Rotators** | 내부적으로 두 로테이터를 쿼터니언으로 변환하여 합친 후 다시 로테이터로 반환합니다. |
| **Make Quat** | 로테이터 값을 쿼터니언 구조체로 변환합니다. |
| **Break Quat** | 쿼터니언의 ![][image1] 성분을 분리합니다. |
| **Lerp (Quat)** | 두 회전 사이를 쿼터니언 방식으로 보간합니다. Shortest Path 옵션을 체크하면 가장 빠른 회전 경로를 선택합니다. |
| **Rotate Vector** | 특정 벡터를 쿼터니언(또는 로테이터) 값만큼 회전시킨 새로운 벡터를 구합니다. |
| **Quat Rotation** | 쿼터니언을 로테이터로, 또는 그 반대로 변환할 때 사용됩니다. |

## **4\. 실무 활용 팁**

1. **일반적인 배치는 Rotator:** 캐릭터를 바닥에 세우거나 단순히 좌우로 돌리는 작업은 로테이터로 충분합니다.  
2. **카메라 및 복합 회전은 Quat:** 카메라 쉐이크, VR 헤드트래킹 보정, 캐릭터의 복잡한 절차적 애니메이션(IK 등)에서는 쿼터니언 보간을 사용하는 것이 훨씬 자연스럽습니다.  
3. **최단 경로 회전:** 캐릭터가 350도에서 10도로 회전할 때, 로테이터는 거꾸로 340도를 돌 수 있지만, 쿼터니언 Lerp는 자동으로 20도만 회전하는 최단 경로를 선택합니다.

## **5\. 요약**

* **로테이터:** 사람이 이해하기 쉬움 (0\~360도), 하지만 짐벌 락 위험 있음.  
* **쿼터니언:** 수학적으로 완벽함, 짐벌 락 없음, 부드러운 보간 가능, 하지만 직관적인 수치 수정이 어려움.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAD4AAAAWCAYAAACYPi8fAAADI0lEQVR4Xu2Wv2tUQRDH32GE+At/xjO5y+3dBQ1BQeUQEbURBUW0MIhCwCaFrQgiWIgi/gGJEQuRkCJgYVARUVMFLAz2oghBEcVColhEUMH4+d7bPTbP93LPYCO8Lwy7OzszOzs7O7tBkCFDhgwZMmTIkOE/RLlcbjXGtNthrlAorO3q6lpPf5Ev1wy1Wm1xpVLJY29VdG4h8Oy1Ol5bW9tyX2Y+OP04nVypVDrFpiehW9AF6Dq8+7QTtDeQaYkqxYFgFZF/jN4d6GtnZ+dR8bUojo/DGw1S2hLQ2Ya9d+jNqsXeDngbGI9FZZPg+fOJ/n5/4gDMa4qMxloEusIiWyQMTSliDYVktODUAHp70emBphlf1gRr1BjPQFejSklQsNB7It+gfmgU+gB9gXqj8gloifOnnt7QiDbpJBH4psiy2a30P1pnm56SjOPosL0yfdAs/ZOaoz2tscuANCgWi0uq1eomujmNdTA2CGk3XffJ98dfP9fd3b1CrQZW6EVHR8e6hnZKyLF8Pr/M2ngIvcX5guYUEAWRthrVSwMFAf1B9I8H1tc0kE+ePy8TM1eOycngL4xHoShD074dBROaiCswzcCprzRh7TkcnUuDOH/+gC6/0tKN3Sn6Ms1gbFpBxzye7vegL5cGnPQafLqN7i6PvQjeam88L+L8UWofFJN2nDQwtM9ViDTHohuZu2ufNGfkrDXyHqo0DHmw9/kndvZoLLtgGupxMvQPWTvfzdxN1eG9Aipkqsb3TPjajJmw7mxPY0fQvWbul7vfyqD686PNmrDcPzVhdJ7BG6F9QFv2jWgzJjy9zy5AUdjUfAS9kh3kXptImqtowXsjh5g/4usLJgzwObo5E6bqlAk3qFdmn5NrZkewRXHIhFVd+5psTPgfBH1cREHSfQjqATiftHGLxgcIud3YvhQVEHCiL8Zh/SuUyo2Pk3yTj+7JjSLBzhwo8Mi1L6TO1GHf12FXsX3g4BlFH+q3LJ3YEPKb5wiGyCE/0CSAafCv7MwPFjnBIheDSEbwBC4thR+OHyX7Q6LfC834cg7Y2cnczaRTTIuF2PkNPrHevkTrwIMAAAAASUVORK5CYII=>