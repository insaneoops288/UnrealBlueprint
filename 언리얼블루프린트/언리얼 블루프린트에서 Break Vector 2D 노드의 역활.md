# **Break Vector 2D 노드 상세 설명**

언리얼 엔진 블루프린트에서 **Break Vector 2D** 노드는 데이터 구조를 해체하여 특정 축의 값에만 접근하거나 개별적인 계산을 수행할 때 필수적으로 사용되는 노드입니다.

## **1\. 주요 역할 (Core Function)**

Vector 2D 데이터 타입은 내부적으로 두 개의 소수점 숫자(![][image1])를 가지고 있습니다. **Break Vector 2D** 노드는 이 묶여 있는 데이터를 다시 풀어헤쳐서, 사용자가 ![][image2] 값이나 ![][image3] 값에 개별적으로 핀을 연결할 수 있도록 만들어줍니다.

* **입력 (Input):** Vector 2D 구조체 (예: 화면 좌표, 입력 축 값)  
* **출력 (Output):** \- **X:** 가로축 또는 첫 번째 성분 (Float)  
  * **Y:** 세로축 또는 두 번째 성분 (Float)

## **2\. 왜 사용하는가? (Use Cases)**

### **① 특정 축의 값만 필요할 때**

예를 들어, 캐릭터의 이동 입력에서 '앞/뒤' 입력값만 추출하여 특정 로직에 사용하고 싶을 때, Vector 2D 형태의 입력 데이터에서 ![][image3] 값만 뽑아낼 수 있습니다.

### **② 데이터 가공 및 계산**

Vector 2D 상태에서는 두 값이 한꺼번에 계산되지만, ![][image2]에는 ![][image4]을 더하고 ![][image3]에는 ![][image5]를 곱하는 식의 **개별적인 수학 연산**이 필요할 때 데이터를 분리해야 합니다.

### **③ 다른 데이터 타입으로의 변환**

2D 좌표의 ![][image2] 값을 3D 벡터(![][image6])의 ![][image7] 값(높이)으로 옮기고 싶을 때처럼, 데이터를 재조합할 때 중간 단계로 사용합니다.

## **3\. 실전 예시 (Example Scenarios)**

| 상황 | 설명 |
| :---- | :---- |
| **마우스 좌표** | 마우스의 화면 위치(Vector 2D)에서 ![][image2] 좌표만 확인하여 화면 왼쪽/오른쪽 클릭 여부를 판별할 때 |
| **UI 슬라이더** | 위젯에서 핸들의 위치 데이터를 가져와 특정 축의 비율만 계산할 때 |
| **입력 시스템** | Enhanced Input에서 전달받는 IA\_Move (Vector 2D) 값을 분리해 Add Movement Input 노드의 Scale 값으로 전달할 때 |

## **4\. 팁: 노드 생성 및 대체 방법**

1. **검색으로 생성:** 블루프린트 그래프에서 우클릭 후 Break Vector 2D를 검색합니다.  
2. **구조체 핀 분할 (Pin Splitting):** \- 굳이 이 노드를 꺼내지 않아도 됩니다.  
   * Vector 2D 타입의 변수 핀 위에 **마우스 우클릭**을 한 뒤 \*\*'구조체 핀 분할(Split Struct Pin)'\*\*을 선택하면 노드 없이도 그 자리에서 바로 ![][image2]와 ![][image3]로 나뉩니다. (기능적으로는 동일합니다.)  
3. **반대 노드:** 분리된 값을 다시 하나로 합치고 싶을 때는 **Make Vector 2D** 노드를 사용합니다.

## **요약**

**Break Vector 2D**는 "좌표(뭉친 데이터)를 숫자(개별 데이터)로 쪼개는 가위"와 같습니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACoAAAAYCAYAAACMcW/9AAACRElEQVR4Xu2WvWtUQRTFd8EExSJBhYX9eruL1eIHsiIoahVBSROihRAL/wARWTBaSSwCARELQYJg4z9gIQHBQBqRVFrJthZCioBpREiExN/ROzpcH9m3UWEhe+Cw8+65c+bOzJt5m8sNMMAAuxt7qtXqzSRJngbyPF+v149J5Pd8rIm1Wm2vN0kDuU98X9huNpvDIUdexG5HY88RG419AvKlUukgSW/hFnwPz7RarSGJpj0z7Tm8pD7OIxX0LTPoLfpswq+0J1KKyFcqlZMalyKv0S4q5nJ+A4OLZvgJ1kNcg/G8iMlYnJ8V7EiB/h2b6JTXQR7v6UKhsN8LqWg0GiMYLZvhDcXK5fIB2i/hpM/vBfSflS8FvcJzn9Muw4U41hUq0ApdZiUSGcjI5/UKPE7DdbjG1h4JccY4RWxJY8X5XUEnVaet1yvQ4XW4ntvufckIraJWU4uA54xiVuSbnosUdICSn6dvy8/+b0GhDTxXzHsVjvucTLAiHzDj+/y+jmf/L2D+L+Sr0+31zKCouxjckyHtqzbzjk6tz90JisXiIfw+aFW1ul7PCl0R8+HujK8UFe2TdwL8z+L3DS7okvd6Fvy4x0KRAdp226Y/rpQAOyQX+D3sNQ/y7tguzXqtK+yr8xi+8xqxE/ALXOdQnfO6gNa2wT9SbMnrAdGp3+zpw0HyFRvgF1V00Hl+6HVRXy/nM0Z8A36m3Yo1gVfouDTvA9co/qjP/+/QtqYV2ldgdaZg28f7CjpErOQj/U/w2gD9iO/ESK/g24/f8AAAAABJRU5ErkJggg==>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABIAAAAZCAYAAAA8CX6UAAABLUlEQVR4XmNgGAVDHIiKivLIycnVy8vLz0LGCgoKHNLS0sJAuhNZHKi2AygmgG4OCDArKyuLASX9gQpfAfF/ILsRJGFsbMwK1BgGFPsNxOeB7BhZWVkpoBQjmhkogBGoeArIICCehCRWCjSgDmQoimp8AKjJEoh/AvF1IJcRaEAZEM8gyRAQkJGR4QRq3AF1VTAQL1VSUuJHV0cUAGrOgRq0jmxDQABogCIQPwF5E12OJAD0nirQkLvAmGtAlyMaKCqCHCN/BIiLQAEO5IujqyEIQOEBdMUqoGYzUCIFhROQH4GuDi8AGQLUuB4UUzAxkEGgGATFJLJanACURoCaJgNtT2BASrHQAH8PTMk6CNU4ACiGgLZuBOIVQC4zmtwckKuAuJmBQJYYBUMJAAAjzEXYfNhWZQAAAABJRU5ErkJggg==>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAZCAYAAAA4/K6pAAABDUlEQVR4XmNgGAWDACgoKBjIy8vPQsdycnKuMDWysrKmyHJAPROBWAFmAIeioiJQXL4PiP8D8R0g3wwkDjNASUmJHyg+DYh/AXEr0HBtY2NjVpg8GAAlNIH4LRB/BSowRpaDWnAEZDCyODpgASpaA3IF0IBymCDU9q1AHIysGCsAOjsC6o0TII0gMSB7MhDnAJmMaMoxAVAhyK1PgPgnEFsChRiBdCmIRleLCzACnT8f5AqgaxpAzsYILEIAqNEDqPEfFG9HlycIpKWlZaDeuAuPa1IANNF8A+I1QC4LujxBALQ0HRQGQFyELkcMAIX6UiD+DQxMG3RJggCY0sSBmq8D8UMglkSXHwWDFQAAdrJC5sHSvLgAAAAASUVORK5CYII=>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABMAAAAYCAYAAAAYl8YPAAABLklEQVR4Xu2Tu0rEQBiFN2xjYSG48ZL7pbJbWLCztxF8CisrLRbBVxGx0wcQWwtfwGJLUcFFFGyEtOr3j8nyO2uGbcU9cEj+c86cmYSk05ljjpngZVnWT9P0Cq7bJt4avMAb1zwRzQ7169Az/ICPdllZlitot+SOGLtC5mN4E4bh8iQYRVGY5/kmwSV49lsZ8z58kGyjURKhPYmnsxM4yq6Fvu8vNprc1/ol6xZ03sBRJidoK5vKGzjKKvG0psqqJEkG2jP4k2VvsrDlnYm3ofMGjrKRo2wUBEFP5w3ayniMU3sR3+Yq2p14jJ6KG3QxzgmMi6JItBHH8Rb6q1wbjY230d61JrsOECv4aRNvWMc85kN4j7bH9QC+pN9f/9SpZgKPGXOiXQp3fvyT/wdf0Pl28kfmDLEAAAAASUVORK5CYII=>

[image5]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAWCAYAAAD5Jg1dAAAA6UlEQVR4XmNgGNFATk5OS0FB4ZC8vPx/ID6uqKhoBxRmRFEEFDQDKjwJVGgA5DIDFZ4HaQDyC+CKgBwOoOB6IF4kJSUlAhIDajIG8r8C8RMgVoSZJg7k3IVaWQQSU1dX5wWyD4PEgJp8YYayAAWmAPEPmKCoqCgPkH8AqjkaphADADXEQBXNMjY2ZkWXhwOgwltARXuUlJT40eXgACQJVLhTRkZGCF0ODkDWgKwDKuJEl4MDkCKgSXVAhd0wMVAoAHEQsjpGoEApSCGS40GhMU9WVtYUrgooEAzEf6E+RcbPgZqV4AoHOQAAAGk6B+rLd+sAAAAASUVORK5CYII=>

[image6]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADsAAAAZCAYAAACPQVaOAAADoElEQVR4Xu2WS0iVQRTHr2Rg9NLK7Pq4371qibWouIS0aBFUBD0IhQqUFrWoRRuTgkSiAqGCFklS2DsIgmopRLmQWrppkQmB9EAQigiFghC1399vRsfxqovuIuH7w2Fm/ufMmTnnOzPzxWIRIkSIEOE/QDKZzAuC4CzS4Qp8vmsHd8LT30CSrs1CQE5JScnqsrKygwTxCxlBjsMvcm0UGPxdY9OMfYVn889Ip9OL8V1bXl6+wddlFalUqoiF+hVMIpFI+3qzkcdIna/LFlh3F/7HkHpfl1UUFhYuY5FuZJxFD/h6uAZ07XRzfV22gP/W2ZKdbeSw0BMFS8WedBWmhDv5+oHLZxNOsnsItsDXZx0s1KZgkWbLmfJtJ94jrq0PbOLYXKJ9x2a/0t5hvM4z09mvQf/U2B2m3ReEl54S/Qf5ZMYtRUVFS+1EEl2F/e0gPGr9+LlaWVm5Qjqzx8tIL9KE3RXa98gL7qPSydVdoDwThF/2keV0cWkROXRtPagqhpjXaOwU1Hm4NmugjePnAdwHdFuQU/SH4fZrQ7QXtDbtMdq4Lk350VzGLcggut3iSktLl9B/CNfFZbZSlSjR8YMbM5etkjikmOwepsEYj8uJxtoETl7hvNK3dYF9nYKLmc0Z7jRyXX2TeX2t76yxSRztR8Y/ka3GXlX1A6m2PgyUyBET6CTgarVXVYeSyB5LFLA46fXB6I8qcHfeJFBsw+A30sswl7YdRw2+nQtzi/ch25UcfOwJwhJ+Y0uI/lFtArkZMwmBy5eozxoF6HqQbp1dx710aeSlvqbLY1tvfLbgp0YJVdCMB6WHy3OrYwYwrA7C7A4ok7TP/EV8JKaeiw7651jkEJKMTQWln5ZO2ch2+uwQJGwz+mGk1dfBtSW9C9PyQRjsxDNVXFy8JgjPbKdvmxEYxpEvQfjj8NqW3FxImNL3eQvn/R6Y7bLQhuXDLzmdc/guVZzLO5XgHou02feMhGWE+9YiTb4+E+yX9XmVlb4wlbHKbOxtVVXVctcGLq4LxpYfbbl4gtvB+BpdnfXnsnPn4XYv3KgqKTZVQTqvs1ZPJuicyvm4f3ZmQ0VFxVrs38ecs6EnAe4+G2gUT3uR8TcCX29tzDFR9WwMwgRPnFeTnHsp86Ynw/O+087TZcm4j3m3nBfC/iN81kVlbecFE6pZoMbn54MuA+bG50qSqRz7rMzQKXGxDP/b5txPe458JMMLL8/nI0SIEGFB4C+mdAdZmgvwVgAAAABJRU5ErkJggg==>

[image7]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAZCAYAAADuWXTMAAABFUlEQVR4XmNgGAUDCOTl5SWBuBeIZ+HCcnJy1uj6wEBWVtYPqOA/ED8EKsqDGgbCKUD8F4hPS0tLy6DrAwFGoOQUIF4sLi7ODRNUVFQ0A4q9Bxp2C8iWR9YAB1JSUiJABTtkZGRUYGIgxSBNIM0gQ5DVowCgIhugom4gkxEmBnImEP8C4iAkpZjA2NiYVUtLiw3GV1JS4of6MxhZHUEAMkgeErJlDEguIQigGnuA+B+IjS6PDzCCbJOHRNcsmCCQrQmMRjdkhRgA5D+oP/eA/AwTV1BQaAAa6oKsFgWAJIGa3gPxFeT4BLLFgWJbgVEojaweDvAkBEagrZVA8fkgNpI4BOBJCMxAsUWggAMaEIEkPgroDgDCC0YGIF/cVgAAAABJRU5ErkJggg==>