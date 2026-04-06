# **언리얼 엔진: Switch on Physical Surface 노드 분석**

Switch on Physical Surface 노드는 트레이스(Trace) 작업 후 충돌한 물체의 물리적 특성을 판단하여 로직을 분기하는 **플로우 컨트롤(Flow Control)** 노드입니다.

## **1\. 주요 역할 (Core Role)**

이 노드의 핵심 역할은 \*\*"무엇을 맞췄는가?"\*\*에 따라 게임의 반응을 다르게 만드는 것입니다.

* **실행 흐름 분기:** 입력받은 EPhysicalSurface 이늄(Enum) 값에 따라 일치하는 출력 핀(Pin)을 실행합니다.  
* **가독성 향상:** 여러 개의 Branch(If문)를 중첩해서 사용하는 대신, 하나의 노드로 깔끔하게 다양한 표면 처리를 관리할 수 있습니다.

## **2\. 데이터 흐름 (Workflow)**

일반적으로 다음과 같은 순서로 데이터가 전달됩니다.

1. **Line Trace / Sweep Trace:** 광선을 쏘아 물체와 충돌시킵니다.  
2. **Break Hit Result:** 충돌 결과 구조체를 분해합니다.  
3. **Get Surface Type:** Phys Mat (Physical Material) 핀에서 표면 타입을 추출합니다.  
4. **Switch on Physical Surface:** 추출된 타입을 입력으로 받아 해당 핀의 로직을 수행합니다.

## **3\. 설정 방법 (Setup Steps)**

이 노드를 제대로 사용하려면 사전에 프로젝트 설정이 필요합니다.

1. **Project Settings:** Project Settings \> Physics \> Physical Surface 섹션에서 새로운 표면 이름(예: Grass, Wood, Metal, Water)을 정의합니다.  
2. **Physical Material 생성:** 콘텐츠 브라우저에서 Physical Material 에셋을 생성하고, 위에서 정의한 Surface Type을 할당합니다.  
3. **Material/Mesh 할당:** 해당 물리 재질을 실제 머티리얼의 Phys Material 슬롯이나 스태틱 메시의 콜리전 설정에 할당합니다.

## **4\. 실제 활용 사례 (Use Cases)**

| 사례 | 설명 |
| :---- | :---- |
| **발소리 (Footsteps)** | 캐릭터가 걷는 바닥 재질에 따라 풀 밟는 소리, 콘크리트 걷는 소리 등을 다르게 재생. |
| **피격 효과 (Impact FX)** | 총알이 박힐 때 나무는 구멍과 파편, 금속은 스파크, 물은 물보라 효과를 생성. |
| **탈것 마찰력** | 자동차 타이어가 닿는 지면이 진흙인지 아스팔트인지에 따라 미끄러짐 정도를 조절. |
| **발자국 데칼** | 눈 위를 걸을 땐 발자국 데칼을 남기고, 바위 위에서는 남기지 않음. |

## **5\. 블루프린트 구성 예시**

\[Line Trace By Channel\]   
       |  
\[Break Hit Result\]   
       |  
       \+-- \[Phys Mat\] \--\> \[Get Surface Type\]   
                               |  
                   \[Switch on Physical Surface\]  
                               |  
                \+-------+------+------+-------+  
                |       |      |      |       |  
             (Default) (Grass) (Wood) (Metal) (Water)  
                |       |      |      |       |  
              \[FX A\]  \[FX B\] \[FX C\] \[FX D\]  \[FX E\]

## **6\. 주의사항**

* **Default 핀:** 일치하는 표면 타입이 없거나 설정되지 않은 경우 Default 핀이 실행됩니다. 항상 예외 상황을 위해 연결해 두는 것이 좋습니다.  
* **물리 재질 누락:** 충돌한 물체에 Physical Material이 할당되어 있지 않으면 무조건 Default로 판정됩니다.  
* **Complex Collision:** 트레이스 설정에서 Trace Complex를 체크해야 머티리얼 슬롯별로 설정된 물리 재질을 정확히 가져올 수 있는 경우가 있습니다.

이 노드는 언리얼 엔진의 환경 상호작용 시스템에서 **'인지(Perception)'와 '반응(Response)'을 연결하는 핵심 연결고리**입니다.