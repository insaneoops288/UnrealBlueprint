# **Set Playrate to Match Speed 노드 가이드**

언리얼 엔진(특히 UE5)에서 캐릭터의 움직임이 어색해 보이는 가장 큰 이유 중 하나는 '발 미끄러짐(Foot Sliding)' 현상입니다. **Set Playrate to Match Speed**는 이를 해결하기 위한 핵심적인 노드입니다.

## **1\. 주요 역할**

이 노드는 캐릭터의 \*\*현재 속도(Velocity)\*\*를 입력받아, 애니메이션이 의도된 지면 속도와 일치하도록 \*\*재생률(Play Rate)\*\*을 실시간으로 계산하고 설정합니다.

* **목적**: 캐릭터가 이동하는 물리적인 속도와 발을 내딛는 애니메이션 속도를 1:1로 매칭시킵니다.  
* **결과**: 캐릭터가 너무 빨리 걷거나, 반대로 미끄러지듯 이동하는 현상을 방지하여 시각적 몰입감을 높입니다.

## **2\. 주요 파라미터 (Inputs)**

| 파라미터 | 설명 |
| :---- | :---- |
| **Target** | 속도를 조절할 대상 (보통 시퀀스 플레이어나 블렌드 스페이스). |
| **Speed** | 캐릭터의 현재 실제 이동 속도 (일반적으로 Get Velocity의 벡터 길이를 사용). |
| **Basis Speed** | 애니메이션의 재생률이 1.0일 때 기준이 되는 이동 속도 (예: 걷기 애니메이션이 초당 300cm 이동하도록 제작되었다면 300을 입력). |

## **3\. 작동 원리 (내부 계산식)**

이 노드는 내부적으로 다음과 같은 아주 단순하지만 강력한 공식으로 작동합니다.

* ![][image1]예: 캐릭터가 현재 **150**의 속도로 걷고 있고, 애니메이션 원본의 기준 속도가 **300**이라면, 재생률은 **0.5**가 되어 느리게 재생됩니다.  
* 반대로 실제 속도가 **450**이라면 재생률은 **1.5**가 되어 빠르게 재생됩니다.

## **4\. 사용 방법 예시**

보통 애니메이션 블루프린트(AnimBP)의 **Update Animation** 이벤트나 **Thread Safe** 업데이트 함수 내에서 다음과 같이 구성합니다.

1. **Get Velocity**: 캐릭터 무브먼트 컴포넌트에서 속도를 가져옵니다.  
2. **Vector Length**: 벡터를 스칼라(속력) 값으로 변환합니다.  
3. **Set Playrate to Match Speed**: 변환된 속도 값을 노드에 전달하고, 해당 애니메이션 자산의 기준 속도를 입력합니다.

## **5\. 장점 및 주의사항**

### **장점**

* **발 미끄러짐 방지**: 지면과 발이 밀착되어 움직이는 듯한 정교한 연출이 가능합니다.  
* **가변 속도 대응**: 가속/감속 구간이나 조이스틱의 기울기에 따른 미세한 속도 변화에 애니메이션이 즉각 반응합니다.

### **주의사항**

* **Basis Speed 파악**: 애니메이션 제작자가 의도한 이동 속도를 정확히 알고 있어야 합니다. (루트 모션을 사용하는 애니메이션의 경우, 루트 본의 이동 거리를 통해 파악 가능)  
* **최소/최대 제한**: 속도가 0에 가깝거나 너무 빠를 경우 애니메이션이 멈추거나 지나치게 빨라질 수 있으므로, Clamp 노드를 사용해 재생률의 범위를 제한하는 것이 좋습니다.

[image1]: <data:image/png;base64,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>