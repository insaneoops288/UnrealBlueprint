# **언리얼 블루프린트: For Loop vs For Each Loop**

언리얼 엔진의 블루프린트 시스템에서 반복 작업을 수행할 때 사용하는 두 노드는 목적과 입력 방식에서 뚜렷한 차이가 있습니다.

## **1\. For Loop (포 루프)**

**For Loop**는 특정 횟수만큼 동작을 반복하고자 할 때 사용하는 기본적인 반복문입니다. 인덱스(숫자)를 기준으로 작동합니다.

### **주요 핀(Pins) 설명**

* **First Index**: 반복을 시작할 숫자입니다. (보통 ![][image1]부터 시작)  
* **Last Index**: 반복을 끝낼 숫자입니다. (이 숫자까지 포함해서 실행됨)  
* **Loop Body**: 각 반복마다 실행될 로직을 연결합니다.  
* **Index**: 현재 몇 번째 반복인지 알려주는 정수 값입니다.  
* **Completed**: 모든 반복이 끝난 후 실행될 로직을 연결합니다.

### **사용 사례**

* 단순히 특정 액션을 10번 반복하고 싶을 때.  
* 숫자를 ![][image2]부터 ![][image3]까지 더하는 계산을 할 때.  
* 배열이 아닌, 특정 범위의 숫자를 제어해야 할 때.

## **2\. For Each Loop (포 이치 루프)**

**For Each Loop**는 \*\*배열(Array)\*\*의 요소를 하나씩 꺼내어 반복할 때 사용합니다. 배열의 길이를 자동으로 계산하므로 편리합니다.

### **주요 핀(Pins) 설명**

* **Array**: 반복할 대상이 되는 배열 변수를 입력받습니다.  
* **Loop Body**: 배열의 각 요소에 대해 실행할 로직을 연결합니다.  
* **Array Element**: 현재 순서에 해당하는 배열의 실제 데이터(객체, 숫자, 구조체 등)를 출력합니다.  
* **Array Index**: 현재 요소가 배열의 몇 번째 칸(![][image4])에 있는지 알려줍니다.  
* **Completed**: 배열의 모든 요소를 다 확인한 후 실행됩니다.

### **사용 사례**

* "레벨에 배치된 모든 적"이 담긴 배열에서 체력을 깎고 싶을 때.  
* 인벤토리 아이템 리스트를 화면에 표시할 때.  
* 특정 조건에 맞는 아이템을 배열 안에서 찾고 싶을 때.

## **3\. 핵심 차이점 비교**

| 특징 | For Loop | For Each Loop |
| :---- | :---- | :---- |
| **작동 기준** | 지정한 숫자 범위 (Index) | 배열의 요소 개수 (Array) |
| **입력 데이터** | 정수 (First/Last Index) | 배열 (Array) |
| **자동화** | 인덱스 범위를 직접 입력해야 함 | 배열 크기에 맞춰 자동으로 반복 |
| **주요 장점** | 특정 횟수 반복 시 직관적임 | 배열 데이터를 다룰 때 매우 안전하고 빠름 |

## **4\. 어떤 것을 선택해야 할까요?**

1. **배열을 다루고 있다면?** 고민하지 말고 **For Each Loop**를 사용하세요. 배열의 크기가 변하더라도 코드를 수정할 필요가 없으며, Array Element를 바로 사용할 수 있어 노드 연결이 훨씬 깔끔해집니다.  
2. **단순 횟수 반복이나 수학적 계산이라면?** **For Loop**가 적합합니다. 예를 들어 "3초 뒤에 총알을 5발 발사한다"와 같은 로직은 배열이 필요 없으므로 For Loop가 낫습니다.

### **💡 팁: For Each Loop with Break**

만약 배열을 돌다가 특정 조건을 만족했을 때 반복을 즉시 중단하고 싶다면, For Each Loop with Break 노드를 사용하세요. Break 핀에 신호를 주면 남은 요소가 있더라도 반복을 멈춥니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAWCAYAAAD5Jg1dAAAA3klEQVR4XmNgGNFARUWFT05OboK8vPw7IH0SiK3R1TAoKSnxAxXsAeI5MjIynLKysm5A9msg7YeiUEFBIQIo8QloijFUiBHIng8UOwEyBKaIAyiwFYgfArEkTDNQYTmQ/xtI24AFQJJQRaeBgoJoCv8DcRFMwBjI+QrEB0RFRXmQFPqCFAJtXEg7hUpAgedAfFhdXZ0XphCoIB2kEORWmEJBkEfksfv6PxAHwcRAPp8EMhVkOj4xkKChPCTqYkB8ZWVlMSD/CpA/AchlhCuEAWlpaWGQJ4CxoYZVwcAAAGi+RqhBZojtAAAAAElFTkSuQmCC>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAWCAYAAAD5Jg1dAAAATUlEQVR4XmNgGAVAIC0tLSMnJ+eLLg4GCgoKBkC8Ul5e/ikQ/8WpUEZGRhqo0AFomjCQrsCpEBkAFZWPKsQJiFXICIyZWqDCMHSJoQAAhxkWLjlcysgAAAAASUVORK5CYII=>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB0AAAAXCAYAAAD3CERpAAAB0ElEQVR4Xu2UPUgDQRSELwRBUVDUGM3f5QeEgEUgYqN2IjYBCzt7bVJpEQQ7C1uxt7AQFOughYWFINhZBEuNGEQhTSBt9HtJ9vI8LgELG8nAsPvmze3s3e2dZfXRx3+BLx6PZ2zbvoIz7ia9aXhBr9LmiWhuXzabHYjFYq/03+A980W3RxbLtBd7hw1YdoemUqkptEd8e5R+IfU+vAuHwxPK6iPkKBKJjEshgbIBuKQ8loUhnEgkFlhwDJ56hVLn4Yt4jUZYpH03eaNFo9E56qqpgY/6jNBrrh1SegdeoYFAYIT6VihzD73IdYOisXiBum48SqvCtNYdeIWqO+oWavzNu+oS+sWY07oDr1DMWVkI3gSDwWGjq9C6eFRdM5729Sa0oHUHvUKlp709Qsva1w914BXKPG23Tl+3g9Q8mXKCGYuyCeMRmFD621p34BUaCoUmqUs9QkviEY35cZfQBuOK1g38NM4xVJLJZEw3+OiX0T9lNBobXEOraU3+TugPTH1S89MJ4nmCB0ZrwrwzeQSarncg3+AufEbfYtyBH3brb9RZzGpucB7PIb1N2QDjJTcxqj2/Ao8xykLrLJpz/XN/gOBVPBuEzVquTfXxp/gGqme9FqJNb7sAAAAASUVORK5CYII=>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAD4AAAAXCAYAAABTYvy6AAACzklEQVR4Xu2XTWgTURDHE1RQ/KwSg+TjJZtgsBeRiOLHUYQe6sEiCPEm4smLoEJPeuihtwqioIFSL3rw4MWPQ8GCF7UnQRAKRTyooKhQVPyA1t+Y9/Tlsa27b4MEyR/+bGbezOzM27ezk1Sqhx566KGH/xDVanVdsVgcU0p94PoY7nNt4gD/fmI94Fp31/4GK5fvcAG+Qj6Vz+dXubaJEATBeoJPwqYELxQKB/n9jush13Yp4DNAgrfFF87Dz3EL17ncwe9EvV5fkcvlNiFf1htwN5PJrHF9vFEqlY4SdM5KMs3vcXSPJJE24yWA/bZyubyLeBvghE/h+ByHzyWO0Umx6KakeMnVtvcGgVbKDsOXcIvRk/A55B9c99v2UZGg8KYUCCez2exqSz+sC5+w7b0hxeqip0myz+h14ZLAads+KnwLx34Qv6/wEuJyS2/yuWXrvcGR2k6wOThlvz/IDX2jEds+KnwLXwTm1ZMnft5d9IIkJgm6heud9z5anSychlsl1mtizfCglLvuhW4vXDo7ca7Cj3bDS4wuL1yO+FnivIV73MVEIHBA0DfwYa1WW2v0JH5SCpemYttHRScKx38IPpWj7q4lBon1EXxahX/OFuBh2z4qkhaO3wF4nwEmb3S6EUu3T1um/iDYDtUaVY+JXKlUNiM/Qx5L6Ztw0yy6WdmMCJ11Gb43sf0C99oLyAN6Q7+pkOMrzQvfGW0Txoax1fKs5CayfPdVawIVfdOy+5W3Cvk0y7t0hIX3cITCnnC9Z09tMsqiuw7nsR23nQ2Kui+E0fQPYm5FfqHjDLox1J8BJoxtAxXyJ+Qr0gC1Kk3uF1RrDvh9UlUrbxnBdxpdG2QulmQkudQix4n1gOCjrj4uSKQRVnjXQv64kPQZVx8T8mQuFj3f/38O/c/pRtIuS9G7iXPNOqLdDWlqJDzk6uOATdsom9exCcwTPwH2cPVpzgVOWQAAAABJRU5ErkJggg==>
