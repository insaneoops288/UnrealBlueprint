# **언리얼 엔진 블루프린트: Is Input Key Down 노드 상세 분석**

Is Input Key Down 노드는 언리얼 엔진의 블루프린트 시스템에서 \*\*특정 시점에 사용자가 특정 키를 누르고 있는지 여부를 즉시 확인(Polling)\*\*할 때 사용하는 함수입니다.

## **1\. 노드의 핵심 역할**

이 노드는 "지금 이 순간, 이 키가 눌려 있는가?"라는 질문에 대해 **부울(Boolean: True/False)** 값으로 답해주는 역할을 합니다.

* **방식:** 상태 확인(Polling) 방식  
* **특징:** 입력 이벤트(Input Action/Axis)처럼 키를 누르는 순간 실행되는 것이 아니라, 로직이 흐르는 도중에 해당 키의 상태만 체크합니다.

## **2\. 입력 및 출력 파라미터**

### **입력 (Inputs)**

* **Target:** 보통 Player Controller를 연결합니다. 어떤 플레이어의 입력을 확인할지 결정합니다.  
* **Key:** 확인하고자 하는 특정 키(예: ![][image1], ![][image2], ![][image3] 등)를 선택합니다.

### **출력 (Outputs)**

* **Return Value:** 키가 눌려 있으면 True, 눌려 있지 않으면 False를 반환합니다.

## **3\. 주요 활용 사례**

### **① 매 프레임 연속적인 체크 (Tick 이벤트와 조합)**

플레이어가 특정 키를 누르고 있는 동안 캐릭터의 속도를 높이거나, 에너지를 충전하는 등의 로직을 구현할 때 유용합니다.

* 예: "Shift 키를 누르고 있는 동안 스테미너 소모 및 이동 속도 증가"

### **② 조건문(Branch)에서의 활용**

특정 액션을 수행하기 전, 보조 키가 눌려 있는지 확인할 때 사용합니다.

* 예: "마우스 왼쪽 버튼 클릭 시, Ctrl 키가 함께 눌려 있다면 특수 공격 수행"

### **③ 상태 머신 또는 복잡한 콤보 시스템**

단순한 이벤트 트리거만으로 부족한 복잡한 입력 조합을 판단할 때 현재 키 상태를 확인하여 로직을 분기시킵니다.

## **4\. Input Action 이벤트와의 차이점**

| 구분 | Input Action (Event) | Is Input Key Down (Function) |
| :---- | :---- | :---- |
| **작동 방식** | 이벤트 기반 (Pressed / Released) | 상태 확인 기반 (Boolean 반환) |
| **실행 시점** | 키 상태가 변할 때 자동으로 호출됨 | 노드에 실행 핀이 도달했을 때만 확인 |
| **장점** | 효율적임 (변화가 있을 때만 작동) | 실시간 흐름 제어에 유연함 |
| **단점** | 매 순간의 상태를 알기 위해 변수 저장이 필요함 | 매 프레임 호출 시 성능 비용 발생 (매우 미비함) |

## **5\. 사용 시 주의사항**

1. **Player Controller 연결:** 이 노드는 Player Controller를 타겟으로 합니다. 캐릭터(Pawn) 블루프린트 내에서 사용할 경우 Get Player Controller 노드를 연결해야 합니다.  
2. **입력 모드 영향:** UI 전용 모드(UI Only Mode)인 경우 특정 키 입력이 무시될 수 있으므로 게임 모드 설정을 확인해야 합니다.  
3. **하드코딩 지양:** 특정 키(![][image4])를 직접 지정하기보다는, 언리얼의 **Input Action/Mapping** 시스템을 사용하는 것이 유지보수와 멀티플랫폼 대응에 더 유리합니다. (하지만 디버깅이나 특수한 프로토타이핑 시에는 매우 유용합니다.)

## **6\. 블루프린트 구성 예시**

1. Event Tick 노드 배치  
2. Is Input Key Down 노드 배치 (Key를 'Left Shift'로 설정)  
3. Branch 노드에 연결  
4. True 핀에 'Sprint' 로직, False 핀에 'Walk' 로직 연결

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABUAAAAYCAYAAAAVibZIAAABiklEQVR4Xu2UL0hDURTGN5hBtCjO4fbe5vZEEMHgyzYXLQPT7BaDbThEg4Ji0iAWEUxa7WMIBoPBLhhMJuOCZfN38L5xdvamzeI+ONx7v++d75z7h5dIDPGPkc1mpwqFwiPRUfGez+fDXC7nMX/WGvxLqVSal1zmG0a7y2QyY11ziJoTt7qkA1zdaRWrFYvFJfjmLLCaJFZdtVqMFplWjZSE2yWnbPhvIKy5xLrmpQO417iCrBfhT8IwHNF8F3ywygdtPK4VLZ0cop3GmKZYH4mx4nohF0NiS5sKRxzDV8RUa77vr7DeYZqMuD5EpkRDblC2RNKZ53lztmA6nR6Hu+Dl+NanByTNEG/EvUsqY7Itmi3IWEXbtB59UKZP0h3jVRAE005bID6kINsOGM+lsPXoA91MiKEYM98n1iNNFXyA35Nd6NyBkMrSCdEm6ZZuRyNNmXaIm4FPyMKdVYP4lJvVmirYkvPV2q+Q2yXxkmlK88r0IPHTE4qDdCE/GMsLMFzmSCYtP8Tf4Au7K3kTffl4bgAAAABJRU5ErkJggg==>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADEAAAAYCAYAAABTPxXiAAADp0lEQVR4Xu2Wb2jNYRTH7+2ODGGYsX/P7raUUdIV0ZKE7AX5s6JI4gUhZUj+JG8kZWIUyVpeiDL/EjXEjLLsjdWUxmokXkle7IXEfL739zy3x69fE6+u7NS38/zOOc95zjnPOc+9sdggDdIg/T+Un58/sqysbH5JScni8vLy0ZIVFRWNq6yszA/bZh2lUqkhxpiDoKe0tHQHfCvoAMdJqg0+Jbwn2yiHwM+CKwUFBSOcUDdAAs9JoFU35G/IOiLIOeAtLTQtQrcfNITlWUcEeQS8p/LFEbo9JLcsLM86omUuEmw/fB+fCV9XXFxcCcayjCeTyelgnuZH3wz+ZFpwqdrO32MpoTnSA+G3aBThs0B+PN+/EHEN02MzwFlpozVKwuI7eAg28SKNcjY4WITsBPwWaAFNJngItrHuJoBZzlbJIW9HfhSsZ90KPoA6ZyPi3InImsENxYDtIfg99yoqeXQN4IXisb6ewqf6ftKk7HWgCRLo99CpFlMl0J/jO2mdfgZz7fY460vgjuxIxigpe6txGaBbK39+W8oO6gLndb4CZ90OPoEpekjgd/HT5pKyvs6AGvcdRekWYOMx+BcdzHpzRUXFBFUJ5JmgqumA7Z4cE1Szh7Yrgl8A70HSOWVd54Lz9oTtJNuI3+Ws4/AtfP/QDUlpC6mbuO4nJUqo57XJF4rYtERO2LjXyViXI/voywoLC8cjewme4HymDbYZVY41cUlmnmklE2GXIdtGD0zQDWrDV6CRc2uJN/cXYxvU6ViEI3QpdH1grZOxrgFf0VV7dtXIvoEG1kt1sG7P6TW0yHqk9/ak7UxoRhx5hfn975N6FMPbXmtkiIPWgW4cljiZCZ7iXrWNk7H3sAlmZIYXXKZnvSRXgjnY75RedrJ3do44bzg2Y9A/gV8M6zU/VVVVQzMCG1QfxrM9u/SzirwTrHIyO2iah0wfy45A3vG9i8+4fixZf3IDbIdVbdFnb/YAfKEKw/o1525w/kXIFoDr9p+CitOBfZ7T66nnuynTCTaoa2C3Ca5Oaz1jJ+G9OFkd82al1M6DHIN2VQn+JmSnl2q7CQrQCO6DFaCL/S3wevc7YILbUEGugvOgFZuj7lm3BbiM7LH08JvwR3q+XUzKKtcOdfqKqN5MDGsJan5Ue9nW+65KSq8XKxb6YXSkAvl6+Vefu29HkmtmBvJlW2tSVEx/TCZiHv4lSnALFSZoo2daR/01yGoqC+iU7VuhXtcbthukv6Sf/nsb3hpG2SgAAAAASUVORK5CYII=>

[image3]: <data:image/png;base64,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>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAD8AAAAYCAYAAABN9iVRAAADlUlEQVR4Xu2XS2hTQRSGE1JfINpN1TaPSZqINIKCEVyoIGJFFz4QFaQVBBGhgohQH92qCx+IoCBUXEjBIlhciSJduNOdDwoupKilWFRUEAWp2Pqd3rnt5PQmTWJFwfzwM5M5jzlz7pyZSShURRVVVFHFBIwx9bAjmUzWatk/ikg8Hk8T73bajY2NjfN9AetIZTKZWa5yMYQxOAUHo9FoTAtLBYHswccnOOpQfr+3/WF4Db1F2rZUxGKxOfhoh1/hE3gOdsDHsC2RSOyi7WGO2do2EBjkMPgmlL6WlwOZFD93TUAi8b1a5kDnQV1d3VxXVgqwbYIvZNH4yrqyXC43A7+3kI1IMlxZQdhMduFsgPYH7RqtUw7wF8XPa0mAzr4smPGHlSSZrb0Suw+w193iLvC5AfmwtFoWCJR3wPMEeoJ2FMMtWqccSPJsEo9rmZOYQZjS8kJIp9ML0O+D/cSZ1HIfklDEz/WOC4R12tPQ0BCXYGXxsEXrlQPrZyQg+2ECOylz0B5RsmLwzyP5MJMS6kIWj95NveMCgWI7Bq3Sly9eygRToAYft+EQXGG8G6SeL76YthO+hOvRC2vDQkA/ZbydMkRsjVruQuq+UEnkAUdLcdjlHzz+4uEZrVsqnG39yi52jHyJG8Y7qA7JGaPtioFa32rjmnSGVATJEM6u4HitP+bXqgTq6paDYvWO31XGu1F6ykmAU44Vf5Q84LAZZz+tU82KM1yk3t2T/ksqlVqu5YXgLz4ooS6MV2Kns9nsTC0bh9QESt1kP+OOW+M3EmAld7AkTBJnAu53AQteaLyt/xE2aXkhoNtS4uJbiOGgHs8DSkeDlJzF90ugrgz9WqE7plHsfhcQfKtdxFV+1jiiiNw6QTYCYlmC3Tvs7hcqF2yT6HQWO+zCtiafopTQQrsjHtkE1Pvj/uRmirtZtroJeF3J+YLsgPHK7J4OkLH9khTYHcpPig+J+5jxzqN98tsVEt8yZHdojTs+DjnYUPhsJxHKtTH+POT3ReO9u3259K9L/cgWpv/MeLU86QHE2F4z8XYfs2VsQEj/rfEW3cfv3ahHAuzllvkO+42TdIUIeoetnjxtJRltsJf+Jf7EzNMG0wr79Tbr8emAPS8u63LTED24jlh2SlvJ2VQJ5PFywRTZ9r8D8Sv+Q8Hb/u+CLG+CZ0NlvMzKQEQWztds1oJ/ATVSr/qgmi5InZPYbaE/k9j/G78ANCUXUphzoqIAAAAASUVORK5CYII=>