# **언리얼 엔진 Leg IK 노드의 역할과 기능**

**Leg IK (Leg Inverse Kinematics)** 노드는 캐릭터 애니메이션의 사실감을 높이기 위해 다리 관절(허벅지, 종아리, 발)의 움직임을 역운동학(IK) 방식으로 계산하여 제어하는 애니메이션 노드입니다.

## **1\. 핵심 역할**

### **① 지형 적응 (Terrain Adaptation)**

가장 주요한 역할입니다. 기본 애니메이션은 평지를 걷는 것을 기준으로 제작되지만, 게임 내 월드에는 경사로, 계단, 울퉁불퉁한 바닥이 존재합니다. Leg IK는 각 발 아래의 지면 높이를 감지하여, 발이 공중에 뜨거나 땅에 파묻히지 않도록 다리를 더 굽히거나 펴서 지면에 밀착시킵니다.

### **② 발 밀림 방지 (Foot Sliding Prevention)**

캐릭터가 이동할 때 발이 지면에 고정되지 않고 미끄러지는 현상을 방지합니다. 특정 프레임에서 발이 지면에 닿아 있어야 한다면, IK를 통해 그 위치를 고정시켜 시각적인 안정감을 줍니다.

### **③ 골반 높이 조정 (Pelvis Adjustment)**

한쪽 다리가 너무 낮거나 높은 곳을 딛게 될 경우, 다리 길이의 한계로 인해 부자연스러운 자세가 나올 수 있습니다. Leg IK 노드는 다리뿐만 아니라 골반(Pelvis)의 높이를 함께 조절하여 전체적인 신체 균형을 유지합니다.

## **2\. 노드의 주요 입력 파라미터**

Leg IK 노드를 제대로 활용하기 위해서는 다음과 같은 데이터가 필요합니다.

| 항목 | 설명 |
| :---- | :---- |
| **Leg Definitions** | IK를 적용할 다리의 뼈 구조(허벅지, 종아리, 발가락 등)를 지정합니다. |
| **Foot Target** | 트레이스(Trace) 등을 통해 계산된 실제 발이 닿아야 할 월드 좌표 혹은 오프셋 값입니다. |
| **Alpha** | IK 효과의 강도를 조절합니다. (![][image1]은 애니메이션 원본 유지, ![][image2]은 100% IK 적용) |
| **Min/Max Extension** | 다리가 펴질 수 있는 최소/최대 길이를 제한하여 관절이 꺾이는 현상을 방지합니다. |

## **3\. 작동 원리 (Workflow)**

일반적으로 다음과 같은 단계를 거쳐 동작합니다.

1. **Trace 실행:** 캐릭터의 캡슐 컴포넌트 위치에서 아래 방향으로 라인 트레이스(Line Trace)를 쏴서 실제 지면의 높이(![][image3]값)와 경사면의 법선(Normal) 데이터를 가져옵니다.  
2. **오프셋 계산:** 원래 애니메이션상의 발 위치와 실제 지면 사이의 거리 차이(Offset)를 계산합니다.  
3. **IK 노드 적용:** 애니메이션 블루프린트의 AnimGraph에서 Leg IK 노드에 위에서 계산한 오프셋 값을 전달합니다.  
4. **관절 보정:** 노드가 내부적으로 허벅지와 무릎 관절의 회전 값을 역으로 계산하여 발을 목표 지점에 위치시킵니다.

## **4\. 사용 시 장점**

* **몰입감 향상:** 캐릭터가 환경과 상호작용하고 있다는 느낌을 강하게 줍니다.  
* **애니메이션 재사용성:** 하나의 걷기 애니메이션으로 다양한 지형에서 대응이 가능하므로 제작 비용이 절감됩니다.  
* **물리적 정확도:** 계단이나 바위 위를 걸을 때 캐릭터의 자세가 물리적으로 타당해 보입니다.

## **5\. UE5에서의 변화**

언리얼 엔진 5에서는 **IK Rig**와 **FBIK(Full Body IK)** 시스템이 도입되면서 더욱 정교한 Leg IK 구현이 가능해졌습니다. 단순한 다리 보정을 넘어, 발의 각도에 따른 발목 회전이나 전신 균형까지 통합적으로 처리하는 것이 최신 추세입니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAgAAAAVCAYAAAB7R6/OAAAAyklEQVR4XmNgGFZARkaGU15ePgqIZwFxl6KiojpcUklJiR8ouBuIm0VFRXkUFBQMgOxrQBwMViAnJ1cO5JwG0oIwTUB+NBBfB0kKgiSBuhbCjQQCWVlZU6D4F8IKgIQmEL9FVwDUaAwU/wpn4FPgC2T8x6kASHjiVUCMFUpAxnOcCkAhB2QcAOKtQEUcSApcgGK/YJwYIH4EFFCEyjMC2c1AfALMMzY2ZgUqmA4U2A80JQAqeRUUJ1ANEF3ASFMDKgwBxqQdSBNIEAB3CEQxM7lC8AAAAABJRU5ErkJggg==>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAgAAAAVCAYAAAB7R6/OAAAAVUlEQVR4XmNgGJZAAQIiUATl5eU1gTgLiPcB8V+ggoUYCoCCAUDaCoifYCiAAaCkJBA/HFVApAIgXgrkMsIl5OTkXICCT4D4LxD/h+IvQJMuIbTjAACrhy2TitRVNwAAAABJRU5ErkJggg==>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAXCAYAAADUUxW8AAABFUlEQVR4XmNgGAUDCBQUFCLk5eX/I+G/QPwEikHs/3JycjPQ9YEAI1BiPhA/AmJXIJ8ZJqGoqGgG1PgeiPcoKSnxI+mBABkZGWmg5HGgRm1kcaCYJhDfBYrfAhoijywHB0BJF6CiamQxkGKQJqD4a1lZWRNkORQAlHQD2QLjg5wHcibIuSBnI6vFC4Be4ARqmgMNpGB0eVwAFGhlQA3/QDSIDxVnAcaEgbGxMSuyYmTACFQQDrVtFrJCoEFKQLHJQCYLQjkSADkPqhEjOoBiOUCD05HF4AAWj0AFh4BYAk0OKCV/BBigOsjicElQdKDHI8jZQIPsgRqvA+V2gAIRWR+DuLg4N1ByuzxqksSGc1A0jgLSAQCMDE2L987FsgAAAABJRU5ErkJggg==>