# **Find Look At Rotation 노드 상세 가이드**

Find Look At Rotation 노드는 3D 공간 내의 두 위치(Vector) 사이의 방향을 계산하여, '출발점'이 '목표점'을 정면으로 바라보게 만드는 **회전(Rotator)** 값을 반환합니다.

## **1\. 노드의 구성 요소**

### **입력 (Inputs)**

* **Start (Vector):** 바라보는 주체(예: 캐릭터의 눈, 카메라, 포탑)의 현재 위치입니다.  
* **Target (Vector):** 바라봐야 할 대상(예: 플레이어, 아이템, 특정 목표물)의 위치입니다.

### **출력 (Outputs)**

* **Return Value (Rotator):** Start 위치에서 Target 위치를 정확히 정면(언리얼 기준 X축)으로 향하게 하는 회전값입니다.

## **2\. 주요 역할 및 특징**

### **① 복잡한 수학 계산의 단순화**

일반적으로 두 점 사이의 회전값을 구하려면 벡터의 뺄셈(![][image1])을 수행한 후, 이를 RotationFromVector로 변환하는 등의 삼각함수 기반 계산이 필요합니다. 이 노드는 이 과정을 하나의 노드로 축약해 줍니다.

### **② 정면(Forward) 기준**

언리얼 엔진에서 모든 액터의 **기본 정면 방향은 X축**입니다. Find Look At Rotation은 결과적으로 액터의 X축이 타겟을 향하도록 하는 값을 계산합니다.

## **3\. 대표적인 활용 사례**

* **AI 캐릭터의 시선 처리:** 적 AI가 플레이어를 항상 쳐다보게 만들 때 사용합니다.  
* **포탑(Turret) 조준:** 방어 타워가 침입자를 따라가며 회전할 때 필수적입니다.  
* **카메라 추적:** 시네마틱 카메라나 3인칭 카메라가 특정 오브젝트를 화면 중심에 놓기 위해 회전할 때 사용합니다.  
* **발사체 방향 설정:** 화살이나 마법 구체가 소환될 때 목표물을 향해 날아가도록 초기 회전값을 설정할 때 유용합니다.

## **4\. 실전 사용 팁**

### **특정 축으로만 회전시키기 (예: 좌우로만 회전)**

대상이 공중에 있더라도 캐릭터가 고개는 까딱하지 않고 몸만 좌우(Yaw)로 돌리게 하고 싶을 때가 있습니다. 이럴 때는 노드의 결과값인 Rotator를 **Break Rotator**로 분해한 뒤, 필요한 축(주로 Yaw)만 사용하고 나머지(Pitch, Roll)는 0으로 고정하거나 현재 값을 유지하면 됩니다.

### **부드러운 회전 (RInterp To)**

Find Look At Rotation의 결과값을 Set Actor Rotation에 바로 연결하면 캐릭터가 즉시(순간이동하듯) 타겟을 보게 됩니다. 부드럽게 회전시키려면 이 노드의 결과값을 **RInterp To** 노드의 Target 핀에 연결하여 프레임마다 조금씩 회전하도록 구현하세요.

## **5\. 요약**

"**내가(Start) 저기(Target)를 보려면 어떻게 몸을 돌려야 하지?**"라는 질문에 대한 답을 주는 노드라고 생각하면 가장 쉽습니다.

[image1]: <data:image/png;base64,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>