# **Event AnyDamage의 역할과 활용**

언리얼 엔진(Unreal Engine)의 블루프린트 시스템에서 \*\*Event AnyDamage\*\*는 액터(Actor)가 어떤 방식으로든 데미지를 받았을 때 호출되는 표준 이벤트입니다. 특정 부위나 범위를 따지지 않고 "데미지가 들어왔다"는 사실 자체에 반응할 때 사용됩니다.

### **1\. 주요 역할**

* **데미지 수신 감지:** Apply Damage 함수를 통해 전달된 데미지를 액터가 인식하게 합니다.  
* **체력(HP) 차감 로직 시작:** 들어온 데미지 값을 현재 체력에서 빼는 로직의 기점이 됩니다.  
* **사망 처리:** 체력이 ![][image1] 이하로 떨어졌는지 확인하고 캐릭터를 파괴하거나 사망 상태로 전환합니다.  
* **피격 효과 트리거:** 피격 애니메이션(Hit Montage), 사운드, 파티클 시스템 등을 실행합니다.

### **2\. 파라미터(인자) 설명**

이벤트 노드에는 다음과 같은 중요한 정보들이 포함되어 있습니다.

| 파라미터 | 데이터 타입 | 설명 |
| :---- | :---- | :---- |
| **Damage** | Float | 실제로 전달된 데미지 양입니다. |
| **Damage Type** | Class Reference | 데미지의 성격(예: 불, 독, 물리 등)을 정의하는 클래스입니다. 저항력 계산 시 유용합니다. |
| **Instigated By** | Controller | 데미지를 입히도록 명령한 컨트롤러(플레이어 또는 AI)입니다. 점수 계산이나 킬 로그에 사용됩니다. |
| **Damage Causer** | Actor | 실제로 데미지를 입힌 물리적 객체(예: 총알, 검, 함정 등)입니다. |

### **3\. 기본 구현 예시**

보통 다음과 같은 흐름으로 블루프린트를 구성합니다.

1. **이벤트 호출:** Event AnyDamage 노드를 배치합니다.  
2. **체력 계산:** Current Health \- Damage를 계산합니다.  
3. **값 적용:** 계산된 결과를 Current Health 변수에 다시 저장합니다 (Clamping을 통해 ![][image1] 이하로 내려가지 않게 조절하기도 합니다).  
4. **상태 확인:** Current Health \<= 0 인지 체크(Branch)합니다.  
5. **결과 처리:** True라면 사망 로직(Destroy Actor 또는 Ragdoll)을 수행합니다.

### **4\. 다른 데미지 이벤트와의 차이점**

| 이벤트 이름 | 특징 |
| :---- | :---- |
| **Event AnyDamage** | 가장 일반적임. 모든 데미지 소스에 반응. |
| **Event PointDamage** | 특정 위치(Hit Location)나 방향(Shot Direction) 정보가 포함됨. (예: 헤드샷 판정) |
| **Event RadialDamage** | 폭발과 같은 범위 데미지일 때 발생. 폭발 중심점과의 거리에 따른 감쇠 처리에 유리함. |

### **5\. 활용 팁**

* **데미지 제어:** 특정 상태(예: 무적, 방어 중)일 때는 AnyDamage 로직 앞에 Branch를 두어 데미지 계산을 건너뛸 수 있습니다.  
* **데미지 타입 활용:** Damage Type을 Cast To 노드로 확인하여, 불 데미지일 때는 2배의 데미지를 입히는 등의 속성 로직을 구현할 수 있습니다.  
* **AI 반응:** AI 캐릭터가 이 이벤트를 받으면 Damage Causer 방향으로 고개를 돌리게 하거나 공격자를 추적하도록 명령할 수 있습니다.

**요약:** Event AnyDamage는 액터의 "피격 시스템"을 구축하는 가장 첫 번째 단계이며, 데미지 전달 체계의 핵심 연결 고리입니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAWCAYAAAD5Jg1dAAAA4ElEQVR4XmNgGNFARkaGU15ePgqIZwFxl6Kiojq6GgYlJSV+oORuIG4WFRXlUVBQMACyrwFxMIpCOTm5cqDgaSAtCBMD8qOB+DrQZHGYIkGQIqApC+E6gUBWVtYUKP4FSPvBdGoC8Vt0hUADjIHiX4G4FSbgC+T8x6UQLg7keBKlEEMAj0IloMBzXAqBuAosAAo3IOcAEG8FKuZAUugCFPsFopF1xwDxI6CEIlSIEchuBuIToMiAKzQ2NmYFKpwOlNgPNDUAqugqKIbgipAAI1C3GlBDCDA27ECa0RUMEAAApRlB7z7RgXoAAAAASUVORK5CYII=>