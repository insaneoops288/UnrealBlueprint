# **언리얼 엔진 블루프린트: Safe Divide 노드 분석**

## **1\. 개요 (Overview)**

수학에서 어떤 숫자를 **0**으로 나누는 것은 정의되지 않은 연산입니다. 일반적인 프로그래밍이나 블루프린트의 기본 나눗셈(/) 노드에서 분모(Divisor)가 0이 되면, 결과값으로 NaN(Not a Number) 혹은 Infinity(무한대)가 반환되어 게임 로직에 치명적인 오류(캐릭터 위치 증발, UI 깨짐 등)를 일으킬 수 있습니다.

**Safe Divide** 노드는 이러한 상황을 자동으로 감지하여 안전한 결과값을 반환하도록 설계된 노드입니다.

## **2\. 노드의 구조 및 작동 방식**

### **입력 항목 (Inputs)**

* **A (Dividend):** 나누어지는 수 (분자)  
* **B (Divisor):** 나누는 수 (분모)

### **출력 항목 (Outputs)**

* **Return Value:** 나눗셈의 결과값

### **작동 로직**

1. **B가 0이 아닌 경우:** 일반적인 나눗셈(![][image1])을 수행합니다.  
2. **B가 0인 경우:** 오류를 발생시키지 않고 **0.0**을 결과값으로 반환합니다.  
   * *참고: 엔진 버전에 따라 매우 작은 값(Epsilon)을 기준으로 0 여부를 판단하기도 합니다.*

## **3\. 왜 사용하는가? (주요 이점)**

1. **런타임 에러 방지:** NaN 값이 발생하면 해당 값을 사용하는 모든 후속 계산이 오염됩니다. Safe Divide는 이를 원천 차단합니다.  
2. **가독성 및 편의성:** 일반 나눗셈 노드를 쓸 때는 Branch 노드나 Select 노드를 이용해 "분모가 0인지"를 매번 확인해야 하지만, Safe Divide는 이 과정을 노드 하나로 해결합니다.  
3. **UI 제작 시 필수:** 진행률 바(Progress Bar)를 만들 때 현재 체력 / 최대 체력 계산을 자주 합니다. 이때 데이터 로딩 중 최대 체력이 일시적으로 0이 될 수 있는데, Safe Divide를 쓰면 UI가 깨지지 않습니다.

## **4\. 실전 활용 예시**

### **① 체력 바(Health Bar) 업데이트**

* **상황:** 캐릭터의 현재 체력을 최대 체력으로 나누어 0.0 \~ 1.0 사이의 값을 구함.  
* **위험:** 캐릭터가 생성되는 순간이나 데이터 오차로 MaxHealth가 0일 경우 에러 발생.  
* **해결:** CurrentHealth (A)와 MaxHealth (B)를 **Safe Divide**에 연결.

### **② 거리 기반 효과 (Distance Scaling)**

* **상황:** 두 포인트 사이의 거리에 반비례하여 사운드 볼륨이나 이펙트 크기를 조절.  
* **위험:** 두 포인트가 정확히 겹치면 거리가 0이 되어 값이 무한대가 됨.  
* **해결:** 거리를 분모(B)에 넣고 Safe Divide 사용.

## **5\. 주의사항**

* **결과값 0의 의미:** 분모가 0일 때 무조건 0을 반환하므로, 어떤 상황에서는 0이 아닌 1이나 다른 기본값이 필요할 수 있습니다. 이럴 때는 Select 노드와 Nearly Equal 노드를 조합한 커스텀 로직이 더 적합할 수 있습니다.  
* **정밀도:** 매우 정밀한 물리 계산에서는 0에 가까운 아주 작은 값으로 나눌 때의 처리가 중요할 수 있으므로, 단순 0 반환이 정답인지 검토가 필요합니다.

## **요약**

**Safe Divide**는 \*\*"분모가 0이 될 가능성이 있는 모든 나눗셈"\*\*에 사용하는 보험과 같은 노드입니다. 안정적인 게임 로직과 디버깅 시간 단축을 위해 적극적으로 활용하는 것이 권장됩니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADYAAAAZCAYAAAB6v90+AAACY0lEQVR4Xu2Wu4vUUBTGM7CCoqKC47DzSOahDIK4RYqt7FbEQgsfWGgrK/4Dop2IYKPFIiKyhRYiworForYLNoLtiJWgIoqFzcJaKOP4+0yiyZmHGddJlQ8+bu79zsk95+TeQxwnR44cOXKMCdd1z3iet8roWy0L1Ov17ex9jRjuDOEt9JOlUmmz9R2KSqVSxfE17MHDVs8Cvu9vaDQaJYI/QgzfYYf5fsbpkFdgN1z3rP8gFDC+AX8oMb3YGmQJYjgWFnjBSIrz5hCtH7Va7QCGj0nobpjYBWuTAtr0OFyxwrhQ0IqDuI4O0K5K49jes1oCzWZzG4bLGM4qobAal6zd36AjhN8b+ZfL5Z1WT4tisbhFxYFf4N641mq1drHWgd1BSSdAQufgZR4LjPOpqjEAuh9ecMF7TKesnhZKJkxqRUka7T78Bs8zLcS1BKrV6m6MnqkSmoeXtgeXnHUEtx6w9+kwhqfEc0JUIhT7IeN1OG19+oDT7fgnZT7nBQ2kr1pZgb0XFYPaupKISJz7GD+T4Cln1NcSMHzAHXMjZ5wOMX6Fr0bdE7XaqJppmaY9Y7eDvV/CTzw3rc76C9hFO2i13wgv4geM3kdk/tELjsE7JWp9IkwwMZ991+ATirzR6rr7is8d0bXVKNQwElAyYVKrBDJj9Ukjal7ekK7sBd1SzW3ear+AMIvBsl13/xyFNVXP6hPGFPsuecH9mrOiIA12omaXAJ1wj4LH+azV2u32VrTnqoo75t8H792E3yP5Ov/QUfGv4PtW1HNc078hH+Oi4masx7Us8N/+PHLkyJFj4vgJMbe8MzNYvcoAAAAASUVORK5CYII=>