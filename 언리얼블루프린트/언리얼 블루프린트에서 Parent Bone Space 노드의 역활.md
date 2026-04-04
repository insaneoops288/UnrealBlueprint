# **언리얼 엔진 Parent Bone Space의 역할과 이해**

언리얼 엔진의 애니메이션 시스템에서 본의 변형(Transform)을 조작할 때, **Space** 설정은 결과값에 결정적인 영향을 미칩니다. 그중 Parent Bone Space는 부모와 자식 본 간의 계층 구조를 활용하는 방식입니다.

## **1\. 기본 정의**

**Parent Bone Space**는 선택한 본의 변형을 **부모 본(Parent Bone)의 좌표계**를 기준으로 적용하겠다는 의미입니다.

* **기준점:** 대상 본의 바로 위에 있는 부모 본의 위치와 회전값.  
* **좌표축:** 부모 본이 바라보고 있는 방향이 기준 축(![][image1])이 됩니다.

## **2\. 다른 좌표계와의 비교**

이 개념을 정확히 이해하려면 다른 Space들과 비교하는 것이 가장 빠릅니다.

| 좌표계 (Space) | 기준 (Reference) | 특징 |
| :---- | :---- | :---- |
| **World Space** | 월드 전체의 절대 좌표 | 캐릭터의 방향과 상관없이 항상 일정한 방향으로 움직임. |
| **Component Space** | 캐릭터 메시(Skeletal Mesh)의 루트 | 캐릭터의 모델링 원점을 기준으로 계산됨. 애니메이션 수정 시 가장 흔히 사용. |
| **Bone Space (Local)** | 자기 자신(Target Bone) | 본 본인의 현재 위치와 회전을 기준으로 상대적인 값을 추가함. |
| **Parent Bone Space** | **부모 본 (Parent)** | 부모 본의 움직임에 따라 기준 축이 함께 변함. |

## **3\. 작동 원리 예시 (팔 구조)**

팔의 구조(상완(Shoulder) \-\> 전완(Elbow) \-\> 손목(Wrist))를 예로 들어보겠습니다.

1. **상황:** 전완(Elbow) 본에 회전값을 주려고 합니다.  
2. **Parent Bone Space 설정 시:** \* 전완의 회전 기준은 상완(Shoulder)의 방향이 됩니다.  
   * 상완이 위로 들리면 전완의 회전 기준축도 함께 위를 향하게 됩니다.  
   * 따라서 상완의 각도와 상관없이 "상완에 대해 어느 정도 굽힐 것인가"를 제어하기에 매우 직관적입니다.

## **4\. 주요 활용 사례**

* **절차적 애니메이션 (Procedural Animation):** 부모 본의 움직임에 따라 자식 본이 자연스럽게 반응해야 하는 경우(예: 망토, 머리카락, 장비의 흔들림).  
* **IK(Inverse Kinematics) 보정:** 특정 관절을 부모 관절의 각도에 맞춰 정교하게 제한하거나 수정할 때 사용합니다.  
* **리깅 수정:** 특정 본의 기본 포즈가 부모와 일치하지 않을 때, 부모를 기준으로 오프셋(Offset)을 주어 정렬하기 쉽습니다.

## **5\. 주의사항**

* **계층 구조 의존성:** 부모 본이 급격하게 회전하거나 스케일이 변하면 자식 본의 Parent Bone Space 계산 결과도 급격하게 변할 수 있습니다.  
* **루트 본(Root Bone):** 부모가 없는 최상위 루트 본에 이 설정을 사용하면, 보통 Component Space와 동일하게 작동하거나 월드 기준을 따르게 됩니다.

## **요약**

**Parent Bone Space**는 "부모가 움직이면 나(자식)의 기준점도 같이 움직인다"는 원리를 이용해, **부모 본에 대한 상대적인 변형**을 줄 때 사용하는 가장 계층 구조 친화적인 좌표계입니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEEAAAAZCAYAAABuKkPfAAADFElEQVR4Xu2WO2gUYRDH7zCKiuDzCN5r7w4bD/HBSUBRsTCg2Ig2gSgItiISiFqJIgHBQnwUISCiYqNgCgkIijYiAUErObASUVKIBEGFKEn8TTIrX+ZWb/dur9s//Nndmfn+Ozs73yOVSpAgQYIECZqhq1gsnvI8b8Qnz8PlcnmzOLnucX3CUqm01IoEgdiNdiwcqFarS/wY0cI26Lz7MrZVrk4YWJ0gEnMNluxYQTqXy60l6BWchW/hzlqttlic6rulvrvwgIwxGoGQxOBpxszAn9wfCvjAdKFQ2I7/MwU4yn1WbCamKbz5gn+Fv+B13rOX63rlI81/xP+uQDBovyb7CZZ9O0XI8/yMBPe58WFBJ3Uzvq5J9Fs/SKN9hvffsY4oEG103ufz+Q2uXT46VAEElUplJYHjOuCk2BBcw/1jeNjGRwHjh0SXJJ+gucz4jsAxeb9rj4guNO6j32vscwXG9zC0vny8FmGcP+hJcpKkjYsKNHbAKThJu2/y7byjB9sLeZcbHxVoSLL3MpnMCsfsF2A2dAEEKibTQaZFnRY9nmphflrI35cukITQvCA2LcDLdgugWJTNZpc7z2nJHf1p+Nyxh0KaQTclWTjadA5FgLewy7ZoAXpsXBzw5qfYtKwRkYusi4hsJ5LsgtZtFyRUQXNCtb/AgzYmDogu/NFOAa7QRhe5PpVk/daNA6o/KrqyFVp/HNApNon+R3Lf6tt5PsZzyQkNBjHnCD4vyXLfp3+sjnC3jW0FzNl16L2DE9IV1t8u/AII7TTjex7wztWuLQiykg77a4B8uBRAu6HPBrcC9Heh9xuOlUKeOMNCzgbS/toFDecZ7JeszWJuK7GLoEwFbd2Gvd2Hrvq99oASBOLOancNWd+/EEZf5r0WQHaChu1c/Q2F+Qs9Et+Ab6wP2zb4HU6xQO62fgG+Af2wDySas34fzhY589+EDJrp62n2tRdQAD38nYDfZCq6vo5A/jKsWXtc6LR+25CKk+DtuBZQi07rxwLarV/a1trjQqf124auylcjnc0joNP6CRKkUn8AO1bypOScly0AAAAASUVORK5CYII=>