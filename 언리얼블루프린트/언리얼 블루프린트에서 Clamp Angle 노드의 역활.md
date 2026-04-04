# **언리얼 엔진 블루프린트: Clamp Angle 노드 가이드**

Clamp Angle 노드는 입력된 각도(단위: Degree)가 지정된 최솟값(Min)과 최댓값(Max) 범위를 벗어나지 않도록 고정하는 역할을 합니다.

## **1\. 노드 구조 및 파라미터**

* **Value (Float):** 제한하려는 대상 각도 값입니다.  
* **Min Angle Degrees (Float):** 허용할 최소 각도입니다.  
* **Max Angle Degrees (Float):** 허용할 최대 각도입니다.  
* **Return Value (Float):** 최솟값과 최댓값 사이로 조정된 결과 각도입니다.

## **2\. Clamp (Float)와 Clamp Angle의 차이점**

가장 중요한 차이점은 **각도의 연속성 처리**입니다.

### **일반 Clamp (Float)**

단순히 숫자의 크기만 비교합니다.

* 예: Value: 370, Min: \-180, Max: 180 이라면 결과는 180이 됩니다. (단순히 최대치에서 잘림)

### **Clamp Angle**

각도의 범위(![][image1] 또는 ![][image2])를 이해하고 처리합니다.

* 언리얼의 Clamp Angle은 내부적으로 입력된 Value를 \*\*정규화(Normalize)\*\*합니다.  
* 예: Value: 370은 각도상 10도와 같습니다. 만약 범위가 \-180 \~ 180이라면, 370을 10으로 먼저 변환한 뒤 범위를 체크합니다. 결과는 10이 됩니다.

## **3\. 주요 사용 사례**

### **① 카메라 피치(Pitch) 제한**

1인칭이나 3인칭 게임에서 캐릭터가 하늘이나 땅을 무한정 바라보지 못하게 막아야 합니다.

* **Min:** \-80 (아래쪽 제한)  
* **Max:** 80 (위쪽 제한)  
* 이렇게 설정하면 마우스를 아무리 위아래로 움직여도 카메라가 뒤집히지 않습니다.

### **② 캐릭터 헤드 트래킹 (Look At)**

캐릭터가 특정 대상을 쳐다볼 때, 목이 180도 뒤로 돌아가는 기괴한 현상을 방지하기 위해 사용합니다.

* 캐릭터의 정면을 기준으로 좌우 각도를 제한할 때 유용합니다.

### **③ 차량의 스티어링 휠**

바퀴가 돌아가는 최대 각도를 제한하여 물리적인 한계를 구현할 때 사용합니다.

## **4\. 실전 팁**

1. **각도 정규화:** Clamp Angle을 사용하기 전에 Normalize Axis 노드를 사용하여 각도를 ![][image3] 범위로 먼저 맞추고 사용하면 더 예측 가능한 결과를 얻을 수 있습니다.  
2. **노드 선택:** 언리얼에는 Clamp Angle 외에도 Clamp (Float), Clamp (Integer) 등 다양한 클램프 노드가 있습니다. 회전(Rotation) 값을 다룰 때는 반드시 Clamp Angle 계열을 사용하세요.  
3. **범위 설정:** Min 값이 Max 값보다 크면 결과가 이상해질 수 있으므로 항상 최솟값이 최댓값보다 작도록 설정해야 합니다.

## **5\. 블루프린트 구성 예시 (의사 코드)**

InputAxis LookUp (마우스 상하 입력)  
    \-\> Add Controller Pitch Input  
    \-\> Get Control Rotation  
    \-\> Break Rotator (Pitch 값 추출)  
    \-\> Clamp Angle (Min: \-90, Max: 90\)  
    \-\> Set Control Rotation (수정된 Pitch 적용)

*(참고: 최신 언리얼에서는 Player Camera Manager에서 직접 피치 범위를 설정하는 것이 더 효율적일 수 있으나, 일반 액터 회전 제어에는 위 방식이 표준입니다.)*

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAHIAAAAXCAYAAADX5BuUAAAEL0lEQVR4Xu1Zy0sVURwe0cDooVJ2877O9SpJBT24tLAMoqJwUYQYSbYTMlqFQVm4sIWLiMDaRCZGiyj/gl6SQhvLVaEQPaigEJMMAlvkwr7vzjlyOHpn5sqdq9F88GPO7zu/35nzzZzX3GtZAQIECBAgQIAAAQL8N4hGoysTiUS7EKLHrCPA74e9ho3DpmB3EL/RjAPXj7pJ2HfYo2QyuUmvr6ysBC36YM2wrlQqtUKvXwq4aC/wor26unptPB7vlvXjKL+E7dFjlHbknsu5djR4DQ2/wfUPbBble2YMAf5pJBKJsiyFX0H8mOIIdhT8JRQLLfsBtMAmYDtlSBHrkbOODsqHIbZR5ecTuHepF+2xWOyoF+3wB2C9rJd5h+BPMl+G+KsdjR3ArInjprWw6YXEoL4EcXU6V15evhrxQ4hvVRzKnRASMWNgD+AW0UfMGVUPvgJ2Uvn5BB+4F+3gn3vRDv8X4lJaWAH8u+CH+fzypp2dcBBTAb7JoDnj7iPvIp1wOLwe/hg7rAexPWEvNUnL71G5SLho/+ymHfXF8L/wOelBrAc3IwdCfrQ7iZEvaVqOIC6bHNHV8N/CaumrfBSL9Fz5Imdh9fSX4x7ppB38mJt2Yc+uEbRTpufKF0ntbfR93SMVnMRY9ghkh2gDWPf34joMO886BqBcz3ojj3yXzGvW6MKqqqoNmr+kcNKOugtu2vGCtsMfMlcjapZ5XRpdiPuUan5u4SSGQF2HJoh2XR9RyD9CXs+RfHpUqmVosUD+FvTtNqyfS1Km0cy9TztgeIKTdt7Hg3bmz3uR6pks1K5vcBJDoK4PMY9xHdUE8fOihPV+vkjkN8C+om9XcT0r+zACP2HG8iQJutPkneCkPRQKrfKgPTcvkoKAG0jq8Wjp/UqHkxh2mHkchfK0x+UmfWRHuduyT2i+vUj2ifuSRhWizRa0+Q6zb5fGM/YETefc4KRd2J8Vbtpz8yItuecIe9N1NTRcbDbgJAZck/5ZQWBf2Ib4jzSUQ8ivQ3nGkvuGAribFENROp8NkN9icoSwP9Q/oX/tcuZwnx7Mdv910k59HrQnUX5RU1OzRo9De61S+6IHcdZwEdNrjjbJczP/AdssxYybcWwP/G9z5mQDtH3Q5BS4lKL+CR8Y7APut8OMcYOL9nkzTfK69jKR+fOD/WrQeV+Bm+3mA8fNH1rymK0AbquwZ8XcbJPL7TNhn97S4CGDpzrly7yJhPYhvBzhpB38Ny/aUZ5C/inlyxVyNC6XX8X7BmH/8jK7kMW15RD+T9igsH92uyzspaXDOD1y7+IobUPuaVzf43or0wlzKaH96jSnVzelXdjLtat2xB8XtnZ+bjVj8L4S2oFo2YD7KmwfOtxIkdwbzBgiHA7HEHeMxrJZ/y/Cq3b+asN6xsk/C/yfiQECBAgQIBP+Asm8xS773bi1AAAAAElFTkSuQmCC>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAFAAAAAXCAYAAACcTMh5AAAECklEQVR4Xu1XW0hUURS9gwZFTwub0pm5M+OUCNFrIogi+rDoxz4kozLoIyqjKBA0sL4SyYjAoOijIAqEoIgiektE/lR+RCD5k1BRCUWFglKJ2Vpz98k91xkfUTrFXbA5d++zzrn7rHte17I8ePDgwYMHD+OMWCw2LRQKNdi2/RnlE9hKXR+NRqej7jTiu2h81vVjiXA4vAXvfwnrgL2Df4z5u3l+v38y6g/BPoo9B3exi5aF+Cv2hXG9QbkvHo9PUPU+tNmD+BHYWdhSVedAxGmCnQsEApOCweA6vhDlBsOBvxd184yPugXmeYzhQy4nmCedSCSyHP4XWFt+fn5AExFrgTVSXIoCgerhP+Z4heJDrBp9xehAqDmsR+wM3GzpYwX8TXzmO+HXqvYO0HAzKrpAjEuIHZ/XLwOnIjc3d4ppo5/HEhDMj7z6kM8a+ignwr8J68fzbsPjYOE/MvlzbOB0w17D5jKGMgJ7a9oQnDSI9VA4+nyP0oVttpr2CagEfnVMoNFB+L0oV0nDjJiBmGWzkMsP5FVMXwvInA0PfqkWVGbgTppZoqxnO8MhlNB19CnkkDOQool4LSDmmLgI2A+rpJ9JeyBFROHjs8zINtg3DlYo2Xi+Yj5+OkDAC+D16pgSsIl7qDXcHogEFiHYBXuolyX8ctsRMPElBD4mrwcwzkjsYbYzI6vpMyiitsuHbkZ5DeUnlEfN7JMDhvt+t+4QnKjtHE5JE4rauA4XB0rxJAERL6GA/Eqa/ztAPzUyiBoRPyXAKR6q3gA5zQD3Evr7IMKUWeqDIlbEOOyBWW48KOC/B7eBXI6VY3YLaA+syKQtLS3+toCYDejGvgzbIWWnnO6DZjDqajl73PE0SKwG2cc6YVVmhqgxlWs+/EZblvo/IyD6qLGUWBBoIWIv0P8BuFkDzAT3uCXXh1GAwpxiriFZxrbMQI5NE2XP64dV/kkBzZpvLiwsnGri5oQKqZNttEDbHFi9Ox527lv3YVdhEcZYgrvWzR0JTK6wdtn/EiKkE1DGlDhoYD2aYw8s/6QJlRYcpO1cOFNdY5hUqeaPBjKQKneckCvBYVifvOe7lWJZuyH3tFuuA69U+uDAi8JytRlGQLaro685akU2WiPIJwGQl9jOL9w2+gUFBbPht4Zkw3XRxxUQYT1suyV58UMgzzsUAvEKw8vLywsiVmt4MjN53blh/mL4EdDmnjpdearzd7aVGpi+RgI2LJMvWIdOn6K8PeiXJQPAwTJP5HiSQlIA+F+R/35r8J7KremiLPFn4NyFeDM1h6cz4tfDzt8Y73kdmOXLNGfE4KmGzkog3Hwrw2aeBj8s/9eR60aW6T4073qGJ8s5SWADzM7V5LBMedfz4MHD/4qfuOVUJDWAW/kAAAAASUVORK5CYII=>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGMAAAAXCAYAAAAfiPFCAAAErUlEQVR4Xu2YTWhdRRTHb3gKET+LxsR8vHl5CYYiWEtwodaN9IMulCJ+EnHTRUUXhRZqLRUUCSgiVF0IsSilBJWK6ELEGkToptqNStOKUqilVUqppYVm0aLx98+ceQ7T924S8rh2cf/w586cc2bumTkz5573sqxEiRIlSpQoUaLEVYC+vr7+arX6BXw41RkqtVrtaefcnyJ2r3d3d1+fGmke7D7B5gz8ql6v35naFI3+/v7r8Gk7/kykOkMHuodsbX/B97HvSY2Gh4dvYt27ZMPze/hAarMk8NJ7mPwU/BvOtgoGuq1wr9pyivYkY/d3dXXdEGwGBwcRu6PIX6Jbob0RnoYrGxMVCPy4Bf7M+y9pbbT3pDbCwMDAI1qL2ha4V7Gf1sEKNhyqm5FNwd2yYcxa2mc09r+Zlggm5p19t9rJaRoM5Nrlg+jqQaZx9H+Fq4OMOV7B7rh06itQ9L+DH9G9JtgVBW0am1jl/ffBi82CYZv8LetYFWTBb+w3BRntp5BdwG7URB20P9S+aI5g1xYw8YutgiEH0P2hRQXZyMjIjfQPBPve3t7b6E9rEfFt0QY4n9YagSwa5n/TYCC/Ax7XZkdipa1J7Yk66Drpfwl/l30wsj27HAeyLcgLBvLl8Cy6E3Q7JON6PojsKBxUPywYfppFt8CCMQvXB1nRyAtGdIgu0q1Ixo0apv+Ls8PnfMAUiEPMtSyMDXsGtwRZW5AXjNHR0WvRTUivBWGzhvYR+Giwob0+6OOxyMbN4bFYXiTygpH5dLPNfJyyQ3YQbpVOBnwLV9C/4JJbrzXZuPHGbO1AXjAEVU5VnyP18n/gC5k5K2icdOmCo3nnrnwrWFGwA7vP9dR3LLUJwGZ1nj7FPMEIh22nrU18S7KgD+NdEoxWa14y5gsGL+xB9zU8EZym/V5wupVjCw0GNj/Cfc5XYHqet0qlEfDI9jVOa3cqb4X5gqGDhv4DeDiszfmyfO7DvKhg1Dzedj6VLIRX5O+8YISSFd02O8Hvylbkvc/Z+Csd8/IFBYNxT2bRxvPOuxl3hHGbM8vlAcjfzBZRneUFIypZJ1R9aY3OSmHauzKfxhYeDFAZGhq63fkPzbxkcGc8WMgLRs2XrLEjHcjWITsHpyyFraJ9GU5KH8bSf6fVvAHolmkjUjnv6GHsN/AzZ4WCntivSW3zkBeMmi9Zj4VyXLCDcEzUDWR83fkfhAdURUZjN9nacg/aotEqGHaFp5otxPmP87QqksjhZqXtDCnn3nhsDBeViykUJOfz+dyPUngpa5K68pAXDOS7U59Nro/zWbhch4XnIde8tJVPjUKmHVBd/bIm5gVPpEqdROd/ba4NMktdP8W/QNVGdk4ViY27i/7pmqWy/wv4cD+cwZ+PsyTlmY/6B2JjkFnq0o1URTUH2iud/xvkGfUtEx2uWioLdkuCnZrZJoxPgYL1PJyBY2zuGzxPMvZZ6aLpKlWfc3WitsDfqtFHvmjYL+l4TQ3GGcD5slwpV8XDDudT1M7Eb307Hnd+bePswQ8u+sgXDr0Yhx7TDclzgrQ1gLMb9Ex1Vyvwt1NrU5DyKjWV1LKxP0DbcyNKlChRohj8C6jrsL6XSME8AAAAAElFTkSuQmCC>