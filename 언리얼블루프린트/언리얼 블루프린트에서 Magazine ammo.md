# **언리얼 엔진 블루프린트: 탄창 및 탄약 시스템 구현**

총기 액터(Actor) 또는 캐릭터 블루프린트에서 탄창 시스템을 구축하기 위한 논리적 단계입니다.

## **1\. 필수 변수 생성 (Variables)**

먼저 블루프린트의 **Variables** 패널에서 다음 변수들을 생성합니다. (유형: Integer)

* **CurrentAmmo**: 현재 탄창에 들어있는 총알 수. (기본값: 30\)  
* **MaxMagSize**: 탄창의 최대 용량. (기본값: 30\)  
* **TotalAmmo**: 캐릭터가 소지한 전체 예비 탄약 수. (기본값: 90\)

## **2\. 사격 로직 (Fire Logic)**

사격 버튼을 눌렀을 때 탄고의 총알을 줄이는 로직입니다.

1. **Input Action Fire** 이벤트 노드를 배치합니다.  
2. **Branch** 노드를 사용해 CurrentAmmo \> 0인지 확인합니다.  
3. **True**라면:  
   * CurrentAmmo에서 1을 뺍니다 (**Subtract**).  
   * 실제 총알 발사 로직(Line Trace, Spawn Projectile 등)을 실행합니다.  
4. **False**라면:  
   * "딸깍" 소리를 재생하거나 자동으로 **Reload** 이벤트를 호출합니다.

## **3\. 재장전 로직 (Reload Logic)**

재장전은 예비 탄약(TotalAmmo)에서 필요한 만큼을 빼서 탄창(CurrentAmmo)으로 옮기는 과정입니다.

### **논리 단계:**

1. **필요한 양 계산**: MaxMagSize \- CurrentAmmo \= AmountNeeded  
2. **보유량 확인**: TotalAmmo가 AmountNeeded보다 많은지 확인합니다.  
3. **값 업데이트**:  
   * **충분할 때**:  
     * CurrentAmmo \= MaxMagSize  
     * TotalAmmo \= TotalAmmo \- AmountNeeded  
   * **부족할 때 (TotalAmmo \< AmountNeeded)**:  
     * CurrentAmmo \= CurrentAmmo \+ TotalAmmo  
     * TotalAmmo \= 0

### **블루프린트 노드 구성 팁:**

* Clamp (Integer) 노드를 사용하면 CurrentAmmo가 MaxMagSize를 넘지 않도록 안전하게 제한할 수 있습니다.  
* **Is Reloading?** (Boolean) 변수를 추가하여 재장전 애니메이션이 재생되는 동안 사격을 방지하는 것이 좋습니다.

## **4\. 애니메이션 연동 (AnimNotify)**

시각적으로 완벽한 시스템을 위해 다음을 추가하세요.

1. **Reload Animation** 시퀀스를 엽니다.  
2. 탄창을 빼거나 끼우는 시점에 **AnimNotify**를 추가합니다.  
3. 캐릭터 블루프린트에서 해당 노티파이가 호출될 때 위에서 만든 탄약 계산 로직을 실행합니다. (애니메이션과 수치 변경 타이밍을 맞추기 위함)

## **5\. UI 연결 (HUD)**

1. **Widget Blueprint**를 생성합니다.  
2. 텍스트 블록 두 개를 배치합니다. (하나는 CurrentAmmo, 하나는 TotalAmmo)  
3. **Bind** 기능을 사용하거나, Event Tick 대신 **Event OnAmmoChanged** 같은 커스텀 이벤트를 만들어 필요할 때만 텍스트를 업데이트하여 최적화합니다.