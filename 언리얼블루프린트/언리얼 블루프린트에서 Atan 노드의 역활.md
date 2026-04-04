# **언리얼 블루프린트 Atan(Arctangent) 노드 활용 가이드**

언리얼 엔진에서 Atan 노드는 삼각함수 탄젠트(![][image1])의 역함수입니다. 즉, \*\*탄젠트 값(높이/밑변)을 입력하면 그에 해당하는 각도(Degree 또는 Radian)\*\*를 결과로 돌려줍니다.

## **1\. Atan vs Atan2: 무엇이 다른가?**

블루프린트에서 Atan을 검색하면 두 가지 주요 노드가 나옵니다.

### **Atan (Arctangent)**

* **입력:** 하나의 실수값(Float).  
* **설명:** 단일 값에 대한 아크탄젠트를 구합니다.  
* **한계:** 출력 범위가 ![][image2]로 제한됩니다. 평면상의 ![][image3] 회전을 처리하기에는 정보가 부족합니다.

### **Atan2 (Arctangent2) \- 권장 사용**

* **입력:** ![][image4]값과 ![][image5]값 두 개를 받습니다.  
* **설명:** 원점$(0,0)![][image6](X, Y)$까지의 각도를 계산합니다.  
* **장점:** \* 출력 범위가 ![][image7]로 전체 회전 범위를 커버합니다.  
  * ![][image5]가 0일 때(나누기 0 오류)도 안전하게 처리합니다.  
  * **캐릭터가 특정 방향을 바라보게 할 때 필수적입니다.**

## **2\. 주요 활용 사례**

### **① 두 지점 사이의 각도 계산 (가장 흔한 사례)**

캐릭터가 마우스 커서 방향을 바라보게 하거나, 적이 플레이어를 향해 회전해야 할 때 사용합니다.

1. **대상 위치 \- 내 위치**를 계산하여 상대적인 벡터(![][image8])를 구합니다.  
2. 이 ![][image4]와 ![][image5]값을 Atan2 노드에 연결합니다.  
3. 나온 결과값(Degree)을 **Z축 회전(Yaw)** 값으로 사용하여 Set World Rotation을 호출합니다.

### **② 조이스틱(Gamepad) 입력 처리**

조이스틱의 가로축(![][image5])과 세로축(![][image4]) 입력값을 Atan2에 넣으면 유저가 조이스틱을 어느 방향(각도)으로 밀고 있는지 정확히 알 수 있습니다.

## **3\. 블루프린트 구성 예시**

적 AI가 플레이어를 향해 회전하는 로직을 짤 때의 흐름입니다:

1. **Get Actor Location (Player)** → 플레이어 위치  
2. **Get Actor Location (Self)** → 적(본인) 위치  
3. **Subtract (Player \- Self)** → 상대 위치 벡터 추출  
4. **Break Vector** → ![][image9] 성분 분리  
5. **Atan2 (Y, X)** → 분리된 ![][image4]와 ![][image5]를 입력 (이때 반환값은 각도)  
6. **Make Rotator** → Yaw 핀에 연결  
7. **Set Actor Rotation** → 회전 적용

## **4\. 주의사항**

* **입력 순서:** Atan2 노드에서 **Y가 위쪽, X가 아래쪽**인 경우가 많습니다(수학적 정의). 순서가 바뀌면 각도가 ![][image10] 뒤틀리니 주의하세요.  
* **라디안 vs 디그리:** 블루프린트의 기본 Atan2 노드는 보통 **도(Degrees)** 단위를 반환하지만, C++이나 일부 수학 라이브러리는 \*\*라디안(Radians)\*\*을 반환할 수 있으므로 노드 설명을 확인해야 합니다. (언리얼 블루프린트 표준 노드는 Degrees 기준입니다.)

## **요약**

* **Atan:** 단순 수치 계산용.  
* **Atan2:** 게임 내 월드 좌표 기반의 **회전 및 방향 설정**용.  
* 캐릭터가 무언가를 쳐다보게 만들고 싶다면? **Atan2**를 쓰시면 됩니다\!

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAXCAYAAAAYyi9XAAABs0lEQVR4Xu2UsUvDUBDGW0RQEBS0Sts0SVsRHNQho7voIEh1c3BU3HRxcXDxL1DUTXQSdRC6OEnBTcGpLi5aEIqTICqCQ/2dydkYbNChuOTgI5fv7t737uVeYrHIIouskZmm6ViWtRHkm2aITSC6G+SbZXEEN23b3gsGmmKZTGYMwZdGgrlcrlNyOIEZ8gahWoI51HYRS8ozkUh0qB/Mk2+3Q7AWBPykxPGnwSNYBrPgDJQNw+j3rbHiqzsAJ/j74Bl/23Gc1rpi7HPRJKj81KFw/g2k0+lu3q9AEdF2zYM34O7Bu3L46+Apm82OKKeBhoIkDxNblWNVzttEReqU0zXApXLauW72y8IEPYvbrs2RtwVuQwRLPq7wZ0E6G4C/8OLzdIxrHYUIHisnQr8S5NsM4a/l8/le+DIFNyKk+WFH6t90Q8FUKtVD4JrAqQwCReNgyXT/PnJdDnXSZDrh7kCV+KgIyDXwDc25rmu5U10DhbpaPSjn/SqiAq8j+SEsgjdZyOus6N3bKniAm9LhUMAtWO4JfHGgJBv7JkpiG0J9wXsjPAVJuRK8xpUT+PMi+xf7AEeAnJygBJS7AAAAAElFTkSuQmCC>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAG4AAAAXCAYAAADqdnryAAAEkUlEQVR4Xu2YW4hVVRjH9zAKimlmjuNc15kLDGmgOCIoIiIa+mCBDVn60MM8eEF8MKgQQUkELy8KgmiCGEgGEggKKkFDQUkGEViCKFgMikiKwvgiXn7/s9eas+bDk3uaOXXC/YePvdd/fWvt77L2+tbeSZIjR44cOXLkyJEjRzWgxjn3LvI7cge5WSgUPoCvNTpLkF+RW8jt1tbW3fX19RMinYRxB+B7dEXeifv+T5Bv3s+7yI+0Z1gdUEvfDenR/yfXTd3d3WOj/hpisAH+M+RzZE7UN2LU8NCPlYgwcXNzcyf3V8SrX5xLEytHFqithNF/EsMu1NXVveJ12oK+rrS3eq5qgL3jLGfR3t7+Knaf7ezsnJSkwV8t39va2uZFasW4KVZqoDMdnYtwh2iOEUd7Pu33dI/eeNo7NXc0xz8HEy9jwsfILsN/AnevpaXlTbW9UceSUmKk0w3/AKPfD+3QJ8Avttx/DfllOQMlZD+yMBBamPjZB3dOCRCnBYn0l4YlCbF6G+6hEqa29R9+DdJQGjECMNFR5Kl1iPbKmOf+idWREcgfyCmaY+RMUuVvnPXBgreqHpuv2wVHEo7DDwSe9jrFJ9bxC3nA+ZfAVfKNY7K+OEEBIXHIiSRNgu7XxjqulLjrclhcocprnPXTIgS/TOIUg1VR+1GsEyXuG1/7K1fjmOysDLIORYnr81uFdFYandfgL7m0OLeLw9BxtBt0jXWrBdZPC2x/A/mrXOI0XklRcpCBWEcxUCwUE8Um8IqfObSMHDxk1/MSF7aCv0tc2PvlgHV0uGD8DJ55GPmK+55yjmrLUS2xfFZYPy3CYoz98YtxcIHHfsdjXWkHkoxOLSsHDJnp0k+Ag4mvTwoO/DkZKgMrnTjGr0L6CdAerhuRy8gl2gWr29TU1Ay9w/JZ8aLECcy/Hlke2jqgYc89H4PRTZycBAcYcCSjrAhjXRq4+xi1GSM7uN+JfOn+hcSpWGP38XCs9tD3US9zXo24ItBdLbF8DPont6bfYdZnyc/P4Y6gvz181uhth7vGPIup3bPoO0b7oqtE4kBtR0fHND/4hVIwNcjzHyF7lQR/tH2KHPX9ui8W5oDGxsapcL+5YRoag2ctZGyv5QX4Jdj5qS/0SuYK5Fv5aXUNangzX/c+WdGitFyD9DUuTKAYwO91aUwUr3A40YLXCfoU8rD0yFJ9dH6xx30VgQJjawoP3yJDC/4bTfd2m/FOK2nfd3V1TYz7soL5lzPvUssH0HfeB0yit2C21RkOrA9loAQOJjE6jPQ7/3nj/NlgcEQy5FRZPInHfaOO8DAC8l34xnDpX5KB+BCA3iF0fvIrswja613662tm4KodWRInHfzapnu/bWo7vaL6GnT0RuH/hWjBFz/c0bucYUcYOfx+fQb5GkM+5PqFS1/3wRooUIOmaPUrefStdemKk96Q7bPakSVxqmv4tQ/pRX5BfoiTFqC6zHyntSu59DvtFot9rtWrJGr15iE9PPgt++M4Qla9qkWWxAnyT37K32Toz/YhIMmLpKerLTc5RhGqqZbLkSPHy4BnSyGM6lYVNXAAAAAASUVORK5CYII=>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACUAAAAXCAYAAACMLIalAAACgElEQVR4Xu2Vu2sUURTGZzGCwYAgrIv7mp3dVdDGx9rYqIiFFoqoaEj+gI1oI4JCbARJISJIEAQDQoo0ESXBwoCNgoVYKIIQ8QERIhbBMk0E9fc597rHcUcXQQXZAx93zne/87h37twJgq51rWv/2Mrl8kbwIAzD92ChUCgUkxpZo9FYzvxJMCcdeFitVtcbSaZSqRyDfw3egefgsPiE5jjoZ24MbDVzsUFuAfeiKArl1+v1rGtug9VRfBXcTXCrWCyudg1eZTEz+L3SUGgIbha/Lp9xHfMv4QZ8Hp63wx118734F5Tbz3vRCPgMLhtO/rCRZRQMXuXz+ZIIEjfwF8EbFpQT1BD8WROnXMPiNS+fxncp1swPgLWtiOCr6HxKUyPGj8A8GPWcdorYU+AgbqZUKh1QHAX3e41MPvwnxj3yw052SslrtdoajfIpskJJGPd6Dc9N1+hgK/J7C+MdSWtKvN/BX5+ppCHaDa77JmUkGHdNnQC3KTDF+AGcM4vxmn2tbK2mNG95vU5bo63pVRD8FixZcTab7YO77wrOoquI12HGn6fomSBevZpatOdFltZUx6YGCN5Jkkv+i0o09e2cBfHhn1BjIPpjTXlTEpJd4TFjm0qeF/PKDv2Npj6COXarIN8XT2tKh5hxtJ3GN6V5y6eavjrET8ACB2+T57Viu2pX9IeCvinGptBOY3aqaflUc7f0IyXP5XIrHa2zopWd9jp3n9xBN+k/AnNZXsPtcffWpNUojvkZy3VkBGwm+Ysw/oUMgjG4i8kk+g1hzygy7XblKbhrLz6vATfQ9Dvt47R/6U9NDXAjbyPJkeT2W5OOwjucTq922e9ouvbf2hcBHOUWAtOh4gAAAABJRU5ErkJggg==>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAZCAYAAAA4/K6pAAABDUlEQVR4XmNgGAWDACgoKBjIy8vPQsdycnKuMDWysrKmyHJAPROBWAFmAIeioiJQXL4PiP8D8R0g3wwkDjNASUmJHyg+DYh/AXEr0HBtY2NjVpg8GAAlNIH4LRB/BSowRpaDWnAEZDCyODpgASpaA3IF0IBymCDU9q1AHIysGCsAOjsC6o0TII0gMSB7MhDnAJmMaMoxAVAhyK1PgPgnEFsChRiBdCmIRleLCzACnT8f5AqgaxpAzsYILEIAqNEDqPEfFG9HlycIpKWlZaDeuAuPa1IANNF8A+I1QC4LujxBALQ0HRQGQFyELkcMAIX6UiD+DQxMG3RJggCY0sSBmq8D8UMglkSXHwWDFQAAdrJC5sHSvLgAAAAASUVORK5CYII=>

[image5]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABIAAAAZCAYAAAA8CX6UAAABLUlEQVR4XmNgGAVDHIiKivLIycnVy8vLz0LGCgoKHNLS0sJAuhNZHKi2AygmgG4OCDArKyuLASX9gQpfAfF/ILsRJGFsbMwK1BgGFPsNxOeB7BhZWVkpoBQjmhkogBGoeArIICCehCRWCjSgDmQoimp8AKjJEoh/AvF1IJcRaEAZEM8gyRAQkJGR4QRq3AF1VTAQL1VSUuJHV0cUAGrOgRq0jmxDQABogCIQPwF5E12OJAD0nirQkLvAmGtAlyMaKCqCHCN/BIiLQAEO5IujqyEIQOEBdMUqoGYzUCIFhROQH4GuDi8AGQLUuB4UUzAxkEGgGATFJLJanACURoCaJgNtT2BASrHQAH8PTMk6CNU4ACiGgLZuBOIVQC4zmtwckKuAuJmBQJYYBUMJAAAjzEXYfNhWZQAAAABJRU5ErkJggg==>

[image6]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADgAAAAXCAYAAABefIz9AAADjklEQVR4Xu1Xv2sUQRTeIxEVRQ1ynN6v2YtnIQoSzh8gVqKFhQomVqKNRWxEEFG0Sgz+AUZJEQJqYaGCFmKjAaOChalskoAEURIhBg2KVeQuft/NTBhfsnu7e53kg4+bfe/tu/fNvJm587wVrOD/g1LqAHgO3IHHFumPAr7r+353oVDYU6lUVkl/HLCeYrG4s9k8i8jlcpuRdABcQOKr0h8F7e3tGyHwGnJUweFMJrNOxkRFqVTKsBZwSPoiwSR4Bf4Cb8DUis+t4OekAi0g8j7yjKTT6fXSF4RsNouFL2zDMGVtzMFcTthScInZMii6C8FrpN8tJqpAvHMCcX+CvjxMIHK3eY4IYzui9Kr3ub6GAhFwCJwFfyPJF3z+BI+6MUkEwn8YcTW82y19RJhACW4NxL9H/DjGedcXKpAbFAEzCOjN5/NrafP1/vgBdti4BAJTiLkDzqEzdkknEUMgc/WBVa4iBQLbWQfH+JwIFAhHD2eFe83ajIgRsN+JiyUQBZQR81Xpzd/q6SL5nsvHNqd83wViOsEqariFxxYj6gm7zdQ1GyaQAQ880e/Gzn6fMm07b4sxxYUJtDNeAy/x2djmwG/gMDgIfrI5ZQIL0+bcPowfw/NFT1xPzNFI4HNfHCx2xdgKGG+Ks4LwdSjd4rxOPoCdMoZwc0ofYcRxgs/iMWW6gvkG3I5jDhV0TSh9ac9wL1obj2LYPqKAHmuLKpB+xL6A/xn3NMb7MZ5EQftkbJhA5D4F3xj2717XbtpzVDmHDXMsV0sdvHSVbplxJuNKYvzOTUBEFJiC7Qr4GvFbXBviaxy7wUECOTGw3y6XyxtcuwVzs2bkvcBn5gBPyrhFMBEC+pXecwtI8NApsI4oApU+DN7KY5x3LOyDiK+49gCB/CFx2ROTEQbkPchrRNpjoZFA7hfEPJITY8FOkRMSIPAfoKuyiHkJTnLiOVGkeXeiqA9AXh93PX1aJ0OYQHTAahRy3N6jIYjUohKYnCLippU+meVVQ07LyYsNFkNyLAUmhRHY8Mc2vqcNcaNB3wffd/iOSXssMDkS3eTYCgRPy7g4cCctDKZreJBclz4C9il52sYGCjmvzEnlrGBTs2ZWsD5pYfD1yc67+g2+s4vEuFeZ/QjOw3ZGvpcYZkaf4m7bLX1xYFZw2R/iEvi+IYi4ZwVKohYl32kGvNv4dybRP3oLX/9CWvLXbAUR8Rcvs0VypuS6xgAAAABJRU5ErkJggg==>

[image7]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAIIAAAAXCAYAAADHqJcNAAAFP0lEQVR4Xu1ZW2hdRRQ9IVdI8dGHxtQ87px7EwxRUTH64QukL+mHIra1JfGv2Fb9ECpVCfnREqgEIQoipMXSj9LSFlQQ1Fq0UJDYflRLGkQM6IcEKa0otB8VjWvd2RN25p7cRz3H5sIs2MzZj9lnZt89M3vOjaKAgICAgICAgICAgID/BZ2dnUviOH7DGDPu64AmyFeBvgfNgC6B9sJ+pTbq6em5JZ/Pj4l+Bs/fgh7VNsVicSlk20iweZ+81jcYmjGHAczlk9bW1pt8JcGYuHiApkAbIG7yzFx8afMb/O1pa2u7UesR6xehewvtFrQPKN1/B5wug9NRtOfQXgXN4vmAb9fV1fU05Mc7Ojo6yUvSvAn7805GgD8B2ke99FsH/gL7K5uX3TPk92DSrzh+EaHZF2hg7i2Yxy7QFcYMdDIpESiD7tNI/BUKhXvB/wR6VduB3wCa4TMTADE5zHg7n9A9DNlzyn53qguIPxhesBpO83wZ6HJSIkD+Fewe0zKZ5EnYb1d2f8KuX5k1gd8P+YQbOOx3OCV9aH6xwJtDEnJI4scxrwLoF8YhKREwty2gI1rGRQH7yfb29tvIMy6MD+PkbPh+xpL9yaN9Qo8JugHQHY5PFfLyhRLhZzcoBW5nB9HvdTKxXSUMyrwBUg/ZXy6RTAPsCDUkQgmca6VEkLlPaBnm/FBsd+BSnGCzBs//uDgSyu8xsDmT9Y6gUSURzlMHGohkm8Nu0gP+Bw5SbDj4M/CzXPeVYMyCdpJvhBohrURALLdz7ur45OLZDTqE5xwFKj6Drp/yO43jpI394ixrBI1KiQDdazJY0gnZFieMPetKhQ8GfJ9JCAgnKP1GnAyBuZUUlRdN1x1yRqeSCIiJga8fob+K9gW0O0FnKHc24PcxPtA/5WRcTLQztuAuOjnfIYmRHSolQn9//w3QDcsP6ugdyp2N9C8LCCdI+yS/9UCq7yH4+5itJFIiYLOmkr4SOP60EoGArk/FbIYLRusZF+p0Irj6C3S51rGkhkqJwFUC3Yew+RztpJrYZ25rzzIRuIKA70BHQVul/UNuI/6uUtp+r3XlZJAIvBYeAv3NOBh7O3vW6VNNhNjiXXQcr5HW+z4qJYKx18Jx7gBy0+BRUbpu4nkssreDzBIB/YfQf3OkfvSCvYpN5b1ik4GAfDSSM3ghwOz+hLiM5+0t5yNfToq9gtlUSQSjroXwexf6nzY2GS6Av5tyxoWyVBIBaO7u7r5dBlaV8PIW30GVRJhGAnRomfwQ0ySuPvQv4vlUb2/vzdouloIpr6rieoB+y0F73LcJDfheCd9fggrk2UJ2HPZrfVsfTGqO248Ni2D4eNKXk/wfW+SJiaCuhXPzVt9fZo0Uz2hHhJ/bJXi1NLZAp+9srokLoUoilE1U5CwEL4L6+IMlDZyB8CdaD+gPtMuXOzC4Zv62+1JUflzUjLSOBtYCxn5XmVvphMSJheB75I0U0zphlN+yhZU58NJHQFcwoMOR92UN8l9BWyMVYMl4rkbeHJzdJfR/3vGyS03m5fhw8sWMehJBPsQxNt/wB/bUPC75uX3edwRjj4uz6kqZg90HPDacDZ53GPupuXR8ZA51Fs0mkctmPK8H/Q762thibcjYY2HYuzlsMnaH4HY3KGfiXEHZCKg1EdzZ7pNe2fLNhJ+L32Y8jC1yWWz3aV/Y1VbA7guxYewYw2vaQTMHJtMS20+dG5kgC1XlvLZRTzsE4s6oQXYCh1oToQ40IW7PMB645TyoF46HZrFZ5/3hFHCdkNN/pAUEBAQ0Hv4F+xrpudXSlfUAAAAASUVORK5CYII=>

[image8]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACoAAAAYCAYAAACMcW/9AAACRElEQVR4Xu2WvWtUQRTFd8EExSJBhYX9eruL1eIHsiIoahVBSROihRAL/wARWTBaSSwCARELQYJg4z9gIQHBQBqRVFrJthZCioBpREiExN/ROzpcH9m3UWEhe+Cw8+65c+bOzJt5m8sNMMAAuxt7qtXqzSRJngbyPF+v149J5Pd8rIm1Wm2vN0kDuU98X9huNpvDIUdexG5HY88RG419AvKlUukgSW/hFnwPz7RarSGJpj0z7Tm8pD7OIxX0LTPoLfpswq+0J1KKyFcqlZMalyKv0S4q5nJ+A4OLZvgJ1kNcg/G8iMlYnJ8V7EiB/h2b6JTXQR7v6UKhsN8LqWg0GiMYLZvhDcXK5fIB2i/hpM/vBfSflS8FvcJzn9Muw4U41hUq0ApdZiUSGcjI5/UKPE7DdbjG1h4JccY4RWxJY8X5XUEnVaet1yvQ4XW4ntvufckIraJWU4uA54xiVuSbnosUdICSn6dvy8/+b0GhDTxXzHsVjvucTLAiHzDj+/y+jmf/L2D+L+Sr0+31zKCouxjckyHtqzbzjk6tz90JisXiIfw+aFW1ul7PCl0R8+HujK8UFe2TdwL8z+L3DS7okvd6Fvy4x0KRAdp226Y/rpQAOyQX+D3sNQ/y7tguzXqtK+yr8xi+8xqxE/ALXOdQnfO6gNa2wT9SbMnrAdGp3+zpw0HyFRvgF1V00Hl+6HVRXy/nM0Z8A36m3Yo1gVfouDTvA9co/qjP/+/QtqYV2ldgdaZg28f7CjpErOQj/U/w2gD9iO/ESK/g24/f8AAAAABJRU5ErkJggg==>

[image9]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEEAAAAZCAYAAABuKkPfAAADFElEQVR4Xu2WO2gUYRDH7zCKiuDzCN5r7w4bD/HBSUBRsTCg2Ig2gSgItiISiFqJIgHBQnwUISCiYqNgCgkIijYiAUErObASUVKIBEGFKEn8TTIrX+ZWb/dur9s//Nndmfn+Ozs73yOVSpAgQYIECZqhq1gsnvI8b8Qnz8PlcnmzOLnucX3CUqm01IoEgdiNdiwcqFarS/wY0cI26Lz7MrZVrk4YWJ0gEnMNluxYQTqXy60l6BWchW/hzlqttlic6rulvrvwgIwxGoGQxOBpxszAn9wfCvjAdKFQ2I7/MwU4yn1WbCamKbz5gn+Fv+B13rOX63rlI81/xP+uQDBovyb7CZZ9O0XI8/yMBPe58WFBJ3Uzvq5J9Fs/SKN9hvffsY4oEG103ufz+Q2uXT46VAEElUplJYHjOuCk2BBcw/1jeNjGRwHjh0SXJJ+gucz4jsAxeb9rj4guNO6j32vscwXG9zC0vny8FmGcP+hJcpKkjYsKNHbAKThJu2/y7byjB9sLeZcbHxVoSLL3MpnMCsfsF2A2dAEEKibTQaZFnRY9nmphflrI35cukITQvCA2LcDLdgugWJTNZpc7z2nJHf1p+Nyxh0KaQTclWTjadA5FgLewy7ZoAXpsXBzw5qfYtKwRkYusi4hsJ5LsgtZtFyRUQXNCtb/AgzYmDogu/NFOAa7QRhe5PpVk/daNA6o/KrqyFVp/HNApNon+R3Lf6tt5PsZzyQkNBjHnCD4vyXLfp3+sjnC3jW0FzNl16L2DE9IV1t8u/AII7TTjex7wztWuLQiykg77a4B8uBRAu6HPBrcC9Heh9xuOlUKeOMNCzgbS/toFDecZ7JeszWJuK7GLoEwFbd2Gvd2Hrvq99oASBOLOancNWd+/EEZf5r0WQHaChu1c/Q2F+Qs9Et+Ab6wP2zb4HU6xQO62fgG+Af2wDySas34fzhY589+EDJrp62n2tRdQAD38nYDfZCq6vo5A/jKsWXtc6LR+25CKk+DtuBZQi07rxwLarV/a1trjQqf124auylcjnc0joNP6CRKkUn8AO1bypOScly0AAAAASUVORK5CYII=>

[image10]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABsAAAAXCAYAAAD6FjQuAAAB7ElEQVR4Xu2UwStEURTG3zQUIcIYZt6bN/OmTEpZDDYsxYKRhYX4A2wkZTFlK8lGsrAhOzZjq6ykFGWppJQFTVmxmLLF74x3x/UyjwVlMadO99xzvnO+c+599xlGRSry7yQej7fbtr2NPqEFdNOLSafT1fgXY7HYPWsePcce0DH4dtEZ6i2wrkiOHjcSiYRN0g3BdQmaplmLvRMKheo1WADMBv5j4s3iECIhRgddTFU0Gm1RCRCOEJtU+yJACqMPBBzltCyrD/CUtu8G88g6rnxIAN8eeUfSoKc5mbIDnS456MR0j+ROgh7gAWaV7CmYZf/Mmi4lf/gf0S7B+k4myVKkDNlVJBJpNdwJfMheWTNuXvk7+4YsL5PL8WCfoAXut6eU/J6vyLKuK5hMJtsga9JxRZHOZQIvmRRVk2hknzAiX5D5C+BF6Vo/IvZzf0LmOE4jCTn0TCYicQx7XxX/VTJXgiSNolvoNMkO62kqlWrg/GuwD22fDwTMrO73k0A4HK7THe6dlf4iYvuQvbAO6f6y4haS7oqPWP4oyDVmQGHk/RC/wL+s/ODCgtN93wpJ/RS6JGmJDudZb9mveXH8PXqJ5cGs2u9vSchzcuderK9I5xTJUGCC52B540rkuCEdBjsJSafx04kq8lvyBlq2nFoxNnVNAAAAAElFTkSuQmCC>