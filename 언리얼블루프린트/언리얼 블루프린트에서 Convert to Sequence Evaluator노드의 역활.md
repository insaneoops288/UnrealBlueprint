# **Convert to Sequence Evaluator 노드의 역할과 활용**

언리얼 엔진의 애니메이션 블루프린트에서 **Sequence Evaluator**는 애니메이션 시퀀스를 재생하는 방식에 있어 '자동 제어'에서 '수동 제어'로 전환하는 핵심 노드입니다.

## **1\. 핵심 역할: 수동 시간 제어**

일반적인 **Sequence Player** 노드는 게임이 실행되면 내부 타이머에 따라 애니메이션을 0초 \-\> 1초 \-\> 2초... 순으로 자동 재생합니다. 반면, **Sequence Evaluator**는 다음과 같은 특징을 갖습니다.

* **자동 재생 중지**: 노드가 호출되어도 애니메이션이 스스로 흐르지 않습니다.  
* **포즈 추출 (Evaluation)**: 사용자가 입력한 Explicit Time(정확한 시간 값)에 해당하는 애니메이션의 한 프레임(포즈)을 즉시 결과로 출력합니다.  
* **데이터 기반 제어**: 블루프린트 변수나 계산된 로직을 통해 애니메이션의 재생 위치를 실시간으로 결정할 수 있습니다.

## **2\. Sequence Player와의 차이점**

| 특징 | Sequence Player | Sequence Evaluator |
| :---- | :---- | :---- |
| **재생 방식** | 시간의 흐름에 따라 자동으로 진행 | 입력된 Explicit Time에 고정됨 |
| **주요 입력** | Play Rate, Loop 여부 등 | **Explicit Time**, Teleport To Explicit Time |
| **제어 주체** | 엔진 시스템 (자동) | 사용자 로직/블루프린트 (수동) |
| **용도** | 일반적인 걷기, 달리기, 공격 등 | 특정 포즈 고정, 커스텀 블렌딩, 스크러빙 |

## **3\. 주요 설정 항목 (Details Panel)**

* **Explicit Time**: 애니메이션의 어느 시점(초 단위) 포즈를 보여줄지 결정하는 float 값입니다.  
* **Should Loop**: (일반적으로 Evaluator에서는 큰 의미가 없으나) 재생 범위를 넘었을 때의 처리를 결정합니다.  
* **Teleport to Explicit Time**: true일 경우, 이전 프레임과의 보간 없이 해당 시간으로 즉시 이동합니다.

## **4\. 주요 활용 사례**

### **① 애니메이션 스크러빙 (Scrubbing)**

UI 슬라이더를 움직여서 캐릭터의 애니메이션을 앞뒤로 돌려보거나, 특정 프레임에 멈춰있게 해야 하는 설정창/커스터마이징 화면에서 필수적입니다.

### **② 특정 변수에 따른 포즈 결정**

예를 들어, 캐릭터의 '기울기' 변수(![][image1] \~ ![][image2])를 애니메이션의 시간(![][image3]초 \~ ![][image4]초)으로 매핑하여, 캐릭터가 왼쪽으로 갈 때는 왼쪽 린(Lean) 애니메이션의 시작 부분을, 오른쪽으로 갈 때는 끝 부분을 보여주는 식으로 활용할 수 있습니다.

### **③ 복잡한 동기화 (Syncing)**

두 개 이상의 애니메이션을 매우 정밀하게 일치시켜야 할 때, 엔진의 자동 재생 기능 대신 블루프린트에서 하나의 타임라인을 계산하여 양쪽 Evaluator에 동일한 Explicit Time을 넣어줌으로써 완벽한 동기화를 구현할 수 있습니다.

### **④ 성능 최적화**

매 프레임 애니메이션을 업데이트할 필요 없이 특정 상태에서 고정된 포즈만 필요한 경우, Evaluator를 사용하여 불필요한 재생 로직 소모를 줄일 수 있습니다.

## **5\. 변환 방법**

1. 애니메이션 블루프린트의 **AnimGraph**로 이동합니다.  
2. 배치된 **Sequence Player** 노드(예: Play ThirdPersonIdle) 위에서 **마우스 우클릭**을 합니다.  
3. 메뉴에서 **Convert to Sequence Evaluator**를 선택합니다.  
   * *반대로 Evaluator를 Player로 되돌릴 수도 있습니다.*

**요약하자면:** Sequence Evaluator는 애니메이션을 \*\*"플레이어(재생기)"\*\*가 아니라 \*\*"검색기(포즈 추출기)"\*\*로 사용하는 것입니다. 애니메이션의 진행 상태를 블루프린트 변수로 직접 주무르고 싶을 때 이 노드를 사용하세요.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABkAAAAXCAYAAAD+4+QTAAAAjklEQVR4XmNgGAWjYCgARnl5+e1ALIkuQTFQUFAwAOKVQMOfA/FDmlgiIyMjraioaAa0SIBmliCDUUtIAgNvCVBCE4inAfEsInApMCVxoJsBAngtMTY2ZgUmQ3GQAkIYlFTR9cMAXkuoBehhCTPQgqdKSkpy6BIUAzk5OWOg4V+B+D8yBoqXo6sdBYMHAAD1aDHi7/u9ZwAAAABJRU5ErkJggg==>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABkAAAAXCAYAAAD+4+QTAAAAu0lEQVR4XmNgGAWjYCgARnl5+e1ALIkugROIi4tzGxsbs6KLowMFBQUDIF4JNPw5ED8kyRKg4slycnLG6OLoQEZGRlpRUdEMaJEAzSxBBqOWDCFLgKmCAyhRCsSz0PANIF6PRRyErdDNAQGcloAANPlJouG5QHF3dHFgchXHlX/wWoINyFMzuHABMixhBup5qqSkJIcugRMQawlIDVDtVyD+j4yB4uXoajEAsZZQBICRHi4rK6uMLj6yAQCJLEVtExxqJQAAAABJRU5ErkJggg==>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAWCAYAAAD5Jg1dAAAA3klEQVR4XmNgGNFARUWFT05OboK8vPw7IH0SiK3R1TAoKSnxAxXsAeI5MjIynLKysm5A9msg7YeiUEFBIQIo8QloijFUiBHIng8UOwEyBKaIAyiwFYgfArEkTDNQYTmQ/xtI24AFQJJQRaeBgoJoCv8DcRFMwBjI+QrEB0RFRXmQFPqCFAJtXEg7hUpAgedAfFhdXZ0XphCoIB2kEORWmEJBkEfksfv6PxAHwcRAPp8EMhVkOj4xkKChPCTqYkB8ZWVlMSD/CpA/AchlhCuEAWlpaWGQJ4CxoYZVwcAAAGi+RqhBZojtAAAAAElFTkSuQmCC>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAWCAYAAAD5Jg1dAAAATUlEQVR4XmNgGAVAIC0tLSMnJ+eLLg4GCgoKBkC8Ul5e/ikQ/8WpUEZGRhqo0AFomjCQrsCpEBkAFZWPKsQJiFXICIyZWqDCMHSJoQAAhxkWLjlcysgAAAAASUVORK5CYII=>