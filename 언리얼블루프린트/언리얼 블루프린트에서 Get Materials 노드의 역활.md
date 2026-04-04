# **Get Materials 노드 상세 가이드**

언리얼 엔진 블루프린트에서 **Get Materials** 노드는 특정 메쉬 컴포넌트에 할당된 모든 머티리얼 인터페이스를 **배열(Array)** 형태로 반환하는 함수입니다.

## **1\. 주요 역할**

* **전체 머티리얼 추출**: 메쉬에 설정된 여러 개의 머티리얼 슬롯(Element Index) 정보를 한 번에 가져옵니다.  
* **일괄 처리**: For Each Loop 노드와 결합하여, 메쉬에 사용된 모든 머티리얼의 파라미터를 동시에 수정하거나 정보를 확인할 때 사용합니다.

## **2\. 노드 구성**

* **Target (입력)**: 보통 Static Mesh Component나 Skeletal Mesh Component를 연결합니다.  
* **Return Value (출력)**: Material Interface Object Reference 타입의 \*\*배열(Array)\*\*을 반환합니다.

## **3\. Get Material (단수) vs Get Materials (복수)**

| 구분 | Get Material (단수) | Get Materials (복수) |
| :---- | :---- | :---- |
| **반환 타입** | 단일 객체 (Object Reference) | 배열 (Array) |
| **매개변수** | Element Index (특정 슬롯 번호) 필요 | 매개변수 없음 (전체 반환) |
| **용도** | 특정 슬롯(예: 0번)의 머티리얼만 가져올 때 | 메쉬에 몇 개의 머티리얼이 있든 모두 처리할 때 |

## **4\. 주요 활용 사례**

### **① 모든 머티리얼의 파라미터 일괄 변경**

게임 실행 중 캐릭터나 사물의 색상을 한꺼번에 바꾸고 싶을 때 사용합니다.

1. Get Materials로 배열을 가져옵니다.  
2. For Each Loop를 돌립니다.  
3. 각 요소(Array Element)를 Create Dynamic Material Instance로 바꿉니다.  
4. 동적 머티리얼의 파라미터(Vector, Scalar 등)를 조절합니다.

### **② 특정 머티리얼 포함 여부 확인**

메쉬에 특정 머티리얼(예: 'M\_Highlight')이 사용되고 있는지 검사하여 로직을 분기할 수 있습니다.

### **③ 머티리얼 개수 파악**

Get Materials \-\> Length 노드를 연결하면 해당 메쉬가 총 몇 개의 머티리얼 슬롯을 가지고 있는지 알 수 있습니다.

## **5\. 주의사항**

* **Read-Only**: 이 노드는 머티리얼을 **가져오는(Get)** 역할만 합니다. 머티리얼을 바꾸고 싶다면 Set Material 노드를 사용해야 합니다.  
* **인스턴스 확인**: 반환되는 값은 Material Interface이므로, 실제 부모 머티리얼인지 머티리얼 인스턴스인지는 추가적인 확인(Casting)이 필요할 수 있습니다.

**요약하자면:** Get Materials는 "이 메쉬에 붙어있는 모든 머티리얼 리스트를 배열로 보여줘\!"라고 요청하는 노드입니다.