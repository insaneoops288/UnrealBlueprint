# **언리얼 블루프린트: Ceil 노드 이해하기**

언리얼 엔진에서 **Ceil(천장)** 노드는 수학의 '올림' 함수를 수행합니다. 입력된 값보다 크거나 같은 최소의 정수를 찾는 역할을 합니다.

## **1\. 주요 기능**

* **입력:** 실수(Float) 또는 더블(Double)  
* **출력:** 입력값보다 크거나 같은 가장 가까운 정수 (보통 정수형 Int 또는 실수형 Float로 반환 가능)  
* **수학적 기호:** ![][image1]

## **2\. 작동 예시**

Ceil 노드는 수직선 상에서 항상 \*\*오른쪽(양의 무한대 방향)\*\*으로 값을 이동시킨다고 생각하면 쉽습니다.

| 입력값 (Input) | 출력값 (Output) | 설명 |
| :---- | :---- | :---- |
| **1.1** | **2** | 1.1보다 큰 가장 가까운 정수는 2입니다. |
| **1.9** | **2** | 1.9보다 큰 가장 가까운 정수는 2입니다. |
| **2.0** | **2** | 소수점이 없으면 값은 변하지 않습니다. |
| **0.3** | **1** | 0.3을 올림하면 1이 됩니다. |
| **\-1.1** | **\-1** | \-1.1보다 큰(오른쪽) 가장 가까운 정수는 \-1입니다. |
| **\-1.9** | **\-1** | \-1.9를 올림하면 \-1이 됩니다. |

## **3\. 유사한 노드와의 비교**

블루프린트에서 수치를 조절할 때 자주 함께 쓰이는 노드들과의 차이점입니다.

1. **Ceil (올림):** 항상 소수점 첫째 자리에서 위로 올립니다. (![][image2])  
2. **Floor (내림):** 항상 소수점을 버리고 아래로 내립니다. (![][image3])  
3. **Round (반올림):** 0.5를 기준으로 가까운 정수로 이동합니다. (![][image4])  
4. **Truncate (절삭):** 소수점 이하를 단순히 제거합니다. (양수에서는 Floor와 같고, 음수에서는 Ceil과 비슷하게 작동합니다.)

## **4\. 실전 활용 사례**

### **① 인벤토리 및 슬롯 계산**

아이템 개수에 따라 필요한 슬롯 페이지 수를 계산할 때 유용합니다.

* 예: 아이템이 21개 있고, 한 페이지에 10개씩 들어간다면?  
* ![][image5]  
* 이 결과를 **Ceil** 하면 **3**이 되어, 총 3페이지가 필요함을 알 수 있습니다.

### **② UI 진행바 (Progress Bar)**

경험치나 수치를 칸 단위로 표시할 때, 아주 조금이라도 수치가 차 있다면 한 칸을 채워서 보여주고 싶을 때 사용합니다.

### **③ 게임 내 구매 로직**

특정 자원을 유료 재화로 환산할 때, 소수점 단위가 나오더라도 유저가 최소한 1단위의 재화를 지불하게끔 강제할 때 사용됩니다.

## **5\. 노드 사용 팁**

* 블루프린트에서 Ceil을 검색하면 **Math \> Float** 또는 **Math \> Double** 카테고리에서 찾을 수 있습니다.  
* 반환 타입이 정수(Int)가 필요한 경우, 출력 핀을 정수 변수에 연결하면 자동으로 형변환이 일어납니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAZCAYAAAAiwE4nAAABOElEQVR4XmNgGAWjYEgBOTm5ciD2RRcnB8jLy0sqKCgsFBUV5UGXg4NRC0kBQ89CWVlZZaDcfCB+BMT9MjIyQkBDi4H4LhBPU1JS4kdWT5GFQM2aQPEV0tLSQHtkOIHsHUCxZ0BcCmT7AOmvQByNpodsC1mAYjOA2BjGBxq2BoivSklJiQDpOUD8F2i4A7Imsi0E+Qio0RzIZAHxoZZcBQUvkMsIlOMAYgFkPSBAtoXoAChvAzTsN9CwdHQ5ZEA1C4EGFYHiDCmIsQKyLQSlPqDmtUDcCwpe5PhDkl+qqKgojqyPbAuBGj2B+D8QLwIaYACk3wPxAahBjEB2KcjXyHqg+sizEASMjY1ZlZWVxUA0jA/yET7DKLKQHDBqIRgAFRSAEgm6ODkAWAwKAx0/QVxcnBtdjq4AAH8AbLkSE+iZAAAAAElFTkSuQmCC>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAE0AAAAXCAYAAABOHMIhAAACL0lEQVR4Xu2WP0scURTFlUQwGIkal8X9M7O7hUssRFhQFLSxtjFNIH4IsYmVYG2ZYLFYmFR21qKFaCEiGEgVAoKI4CdQVJDkd3VG3tzsuE8xO1l4Bw47c98979535s3baWlxcHBwcHAIkc/nx33fP/I87yTgaqFQaNd5z4lsNvuWmptGzRPuD0ulUr/O/S9Bw1Pwk46byOVyrzBynoVV9Vg9iNZGR85n+qjoeCNB/QHWuUMvv+FesVicINyq82JNQ9yFcInfH/xey0Rcf9V5taC1NrqkTcOgYerv0+sQ14P08z1Y86zOjTVNdgjxSV4ZD/EoPLdZvEBrbXRJmibHEfXX4bdMJtMrMelF1gxPYTEiiDPNRDiBzeI1RGujS9I0dlaa+keys+CcxMrlcifXuxITjyICZ9otXlL/C7wMDUqlUq+53w6M/BjJdqbVBkdMlp6O4RUcjQw2m2nMNULuT8/4XKlHNPN6nnpANxPssmqlUmnTg01lmiwgOIP6bCkHvZ7nITC/Ty+/0G7xZ/ZGjzedaY2AmEUvG7yiPXrsFs60KGQn08uKfDbpsXs0m2nkvYPLsGpL6n/Q89SCGEYPC+YZhn4OTpt5VqYhGoMX5K1x+yKMp9PpDuJb/t2BuWJI7iFarasF39K0fwXqv4c3wVpMntFXKZIcZ5r6TvmLQVorT3GR+0vfeBoPaaVemGfCT9A09XGreUBf3RFBnGmNRpKmPRrOtCfAmfYEONPs8Ac98f7ctMx7oAAAAABJRU5ErkJggg==>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAE0AAAAXCAYAAABOHMIhAAACQklEQVR4Xu2XvUscQRjG75BAJAGFcBxyH3t3hICYQrxCtAqEFClioWJMZZkuhYLaBrnKUklxXGMhiU1AULBIIfkTUiRpFRRERFIoIgTze8nOMbzcHnsf7grOAw/jvvPM7MzvZnE3kXBycnJycjIqFAqj+K2uRy3P8xZKpdIzXb+Tyufzb/CSrtvKZrO9gF1mY1Xd10RJ8lP4Jz5m/DtqPTpkRGaNdZR1PQ6xjm3hout1BUFjk/1sZJX2B+01vuHvDZ0LUJI5FxlzgkeA/pT2l9SkT4dFcUNjb8N4i3UcyV7bgiani/pLHpk8k4zhi7DQGPeK/F9csWpLXJ/ncrnndtYobmjsN8P+XmQymSdtQ7Mlm2kFGtmaf+P6vHIfXbMVNzRbkUNLpVKPye5rQAYa3kw0eETvNTQyD8nuNoG2L2DtMaJ7DU1EtqKhMfZ9N6AxzyjZ32QPw5oxy3qeZooFGvkh8qd4ncuk/09lrxvQyuXyg2KxmCY/ENZy+vU8zSTrjByaiPwk/sPYD7Qr+LMsxusQWhSKDZrI/6UXZDyvGhM+tJrOiRw0lE6nH8mjZK4ZPy+LYY5ZO2cUFhq5QfwJV8M66J5BknV2DI1JxvEluS8J61NIwFD/JjfxrBNkQf7Oy3Gf9/9z6kJOm8loeSGhRSD5/BNoM7qjriBo9vtWI/uxJGA+cn2FJ9XYHfyV/jnaM/za9DeSFzM080PrfeIDPKDDDaF1QT2yEDwtJ1J3asUNrSXdIrSW5KC1IQetDd11aP8A39wBOYEVfaoAAAAASUVORK5CYII=>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAIoAAAAXCAYAAADUf9f5AAAEjklEQVR4Xu2YTWgdVRiGJySCUsVWjaH5uXNvIgatYCUqKCpFdJGFIra6sC4VFESkEBsRxS5ciAtFFCSKxUqNWq124x+CVRdVXLgSRBAjtLgoRShYrNDq8945c5l8vXcyZ+6kE/S88DIz5+eb833zznd+oiggICAgICAgICAgIOB/hbGxsfFGo3GHLS+KZrP5UBzHS3CjrasTzq8D/fiWAhtX4t+XXGdsXS+Mjo5eQp+X4XF4VP01JtuuLsgnvt3XjOsfeKjVat1C8cCyRjTYDN+lwRF4qmww6bfJBeG3tSKUjG+nFISyvtF3lr4fOf9Owz99hEL7Jcaxa3x8/LyRkZF1PH8AF6kasm3PNhDF9fjynWLF4yDj+kGx4vmxZQ0Z/BiFW1D4xVznywRTAXDO/72WhJL6Jr/6FMoVCih21sM3fYQyNTV1KW13z8zMnKNnjUFjgQeHh4fPt+3PJvDlXMbxIdyjrKcy+SX/4GHYsn3aoNHOMsFsJlPOs80kfVUulMnJycsZ1+ORTYcFIb/6EUoWvkJxMb0tfVZGwcYT8K6opD/dwJjug7fa8jwg/hH6/OKEu0Nl09PTF3D/TW68ygilkUw5+zTncj24GkJxc/w+OWbriqAuodBmA22/5zpp66qGxAhfiPzENxQna6e/0tgoy7nvKPFstx3a8BWK0inG3piYmLg584LKhSJgc46x3W/Li6AuoSDsq+NkAfsA/BZ+DI9h4+HI74OuCBf/vUy3l9k6H2i6jpMNyUl4g61vw1co+nA4vYvbgdUWiuZ63vW5gm/rVkJdQtFfHieL39fTNYp+Kp5PaLq27fsF77sd2+9rerN1RaFvqljBhXTMZ8BXKBjbr0Ww7ldbKALB3cz4viLY19m6PNQolPbCletNaVlmDdB7sVgeA9h8BL5NZrjIVhYBY/2Z/l+wLrzQ1nXgIxSpTX9H+uwjFO3TabNQhozv0zjZxh+wdnuhRqGkGaUTD7MGmM22z2CIvo9a3wvyNfgjPAGflC1rvBckDt772Yoi8xGKggDvgdsclbJ+gn9w/yAB3RL1GGQz2ZZt9CWONLi+AvfHHn9jjUJJt5pdhZI3Ht6z3vpfkNfAQ/Sf95mC3HpzQUcdtu4M+AhF04AMZ7g3ThZAou7nJAjbrw8orcrmPPeDtjIPdQnF7db0d3sLpQyUERjfe9i+0dblQSJhLE/T7/m0jPsd8O5suxT6EE/R4V5b0W2/beG2x5p3D6/C8bRS8atFPk43yC/3YXJ9I8jP2HqDQWy8EydpfdnH4HnWxed4dsGtMTcyuzWm66viJOu+GFW488FmC77le4hHn62xO7k2/L2R3dbLkThJj7ZhZ63hTl73wKM4em2nc9R+kdKd2tr+lZ08asDYW/S15+nbadrvtjYE/fldbIgdH3UoyPOv8BOzENQPeAy+1EjWHUsSfaEU7wHsbsfuTlueB5MALHX+s8H2WdNQasxdhVcAiZGM8pwtrwLaGUpscJsEFVWYSVJIsBVP9QHdQKa8k79ozpYHBHSgbIVIFvs91Qz4j0OLWISy1ZYHJPgXAxe43vbsNPEAAAAASUVORK5CYII=>

[image5]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGMAAAAXCAYAAAAfiPFCAAADxUlEQVR4Xu1YTWsUQRCdkAiKip9xTXazs7sRQkREWD+ISBC8eNGDiij6E3KQHNSLwYunHPQgCjGCRoJfoARPSg5BDyo5iCIogqBB2YN4NPiB6Htut1Qq3TOT4KyXefCY6aqa6u56s909GwQZMmTIkCFDhgw+dHZ2rimVSqe1PYMDxWJxPYr1MAzDX2S5XO6FuUnHEYVCYRFiTyJuSPs8aEHsMNijHfl8voC+x8A92gc0o5/DeO4dWEPMFK591Wp1gQ5sFHSdwMdRtXLBzNk13yBAsq1wPkUnm9BsRnsjO0L7mI3B/XLYBnF9get3478q0niB2G7wdmtr6xK22Q94E7aP4E/mcg0OtuPwvYL468xza9F+AvtFNFtUeOrw1OmZrpULes6u+TJoIZx3wZH29vbV1o72F/ADWGabvwYk2FWpVIqw9dA/BzH6SNtGrjye3Yk3ZFWp/gtzisH+YT8hbR0dHXthn+YYpD1t+OqE8VVtrWS8hp6za75UO4dEb1kQsN/acf/IVyQ7gCRiUETEjqKIG7SPYLF9/bjsYvJnpD1t+OrU1dW11NZKxkeBc9bzsuB6fh78KgPQnjAdH5HBxFzEQFwP4q8EnmUlRowfsO+QNiHGeC6XWyx9KcNZJy69tlYyOApRYjgR1jfNbyym9s1FDMQOIO6Qtlv4xGCh2Qf7kna0K7DXwEncr5C+/wEuP7ZW2ufDfMT4BQ65Ti5JxWCxEHcnNPuOCz4xzBs3SwzY2sD3hm3SZ4FxbYPvNZ6dSkqu4zpPEuDZo7ZW2ucD56zn6wXWR+QOx7FZL9M+ggVKIgb8uxF7Nog49nFg/1oMvkBmjWdsInKD1nniwDphfG/CiFq5kFgMJmVy/PxWap9FQjGaKAQF0Q6JNMRoBGydML77UbVyIZEYfKPQwRA4rH0SScQwa+k9eQx0wSdGUN8wp3EK2yKNYf2b5TM4Yb9bGg1ZJ54WtT8OsWKwAwQMoINBu0/gvh/cp2OTiMFNm/m0XSNCjLij7WjgWf6MYBdMwRIx6pAh4aoTEYrjbhwixUCi/aH5ElbkXxAVR/x2cBq+G2g2O/wzvrgj0IS4U+wLuQ5qJwr0ALwlJs2l7xziX/K/rhnBDUDorxNZY4zve0Tgz5xd89UPa/49PsrztKZWOVRf3Bri7Z6VKxR7Af8Gwf1zxI/xzcX9JbCGpWuzzpk2YupETjLOfOSOgJ/kOCPmnN7eZwZzDezWvvnAnIx6MZkDvLqO2hk8MG/A5cDzxZ2hgeDmlnQzzJAizNn7Oo+12pchHr8BMaSZWN1JQpEAAAAASUVORK5CYII=>