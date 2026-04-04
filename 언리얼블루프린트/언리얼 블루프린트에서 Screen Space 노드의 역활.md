# **언리얼 엔진: 스크린 공간(Screen Space) 노드의 역할과 활용**

언리얼 엔진에서 \*\*스크린 공간(Screen Space)\*\*은 플레이어의 모니터 화면상에서의 2D 좌표계를 의미합니다. 블루프린트에서 제공되는 관련 노드들은 3D 월드와 2D 화면 사이의 가교 역할을 합니다.

## **1\. 핵심 개념: 월드 공간 vs 스크린 공간**

* **월드 공간 (World Space):** 게임 속 3D 좌표 (![][image1]). 끝없이 넓은 가상의 공간입니다.  
* **스크린 공간 (Screen Space):** 플레이어의 모니터 화면 좌표 (![][image2]). 왼쪽 상단이 $(0, 0)$이며, 해상도에 따라 끝 좌표가 결정됩니다.

## **2\. 주요 블루프린트 노드 및 역할**

### **① Project World to Screen (월드 좌표를 스크린으로)**

* **역할:** 3D 세상에 있는 특정 위치(![][image3])가 현재 카메라 시점에서 화면의 어느 지점(![][image4])에 맺히는지 계산합니다.  
* **주요 활용:**  
  * 적 캐릭터 머리 위에 **체력바(HP Bar)** 띄우기.  
  * 아이템 위치에 **상호작용 아이콘** 표시하기.  
  * 특정 지점에 2D 텍스트 레이블 붙이기.

### **② Deproject Screen to World (스크린 좌표를 월드로)**

* **역할:** 화면의 특정 2D 위치(예: 마우스 커서)를 3D 월드의 위치와 방향으로 변환합니다.  
* **주요 활용:**  
  * **마우스 클릭**으로 캐릭터 이동시키기 (RTS 게임).  
  * 화면 중앙 조준점을 기준으로 **총알 발사 방향** 계산하기.  
  * 화면상의 드래그로 3D 물체 선택하기.

### **③ Get Mouse Position / Get Viewport Size**

* **역할:** 현재 마우스 커서의 위치나 전체 게임 화면의 해상도 크기를 가져옵니다.  
* **주요 활용:** 스크린 공간 노드들의 입력값으로 주로 사용됩니다.

## **3\. 머티리얼에서의 스크린 공간 (Screen Position)**

머티리얼 에디터에도 **ScreenPosition** 노드가 존재합니다. 이는 텍스처를 입힐 때 물체의 표면 좌표(UV)가 아닌, 화면 전체를 기준으로 텍스처를 입히는 역할을 합니다.

* **포스트 프로세스:** 화면 전체에 노이즈를 넣거나 색감을 조절할 때 사용.  
* **투명 재질 (굴절):** 물체 뒤에 보이는 배경 화면을 왜곡시킬 때 화면 좌표를 참조합니다.  
* **UI 이펙트:** 위젯 머티리얼에서 화면 해상도에 맞게 이펙트를 배치할 때 유용합니다.

## **4\. 실전 활용 팁: DPI Scaling (해상도 대응)**

스크린 공간 좌표를 다룰 때 가장 주의해야 할 점은 **DPI 스케일링**입니다.

* 언리얼의 UMG(UI)는 해상도에 따라 크기가 자동으로 조절되지만, 절대적인 픽셀 좌표는 해상도마다 다를 수 있습니다.  
* Get Mouse Position Scaled by DPI와 같은 노드를 사용하여 다양한 모니터 환경에서도 UI 위치가 어긋나지 않도록 처리해야 합니다.

## **요약**

스크린 공간 노드는 \*\*"3D 세상의 위치를 2D 화면 UI로 연결"\*\*하거나, \*\*"2D 화면의 입력(클릭)을 3D 세상의 동작으로 변환"\*\*할 때 반드시 필요한 도구입니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEEAAAAZCAYAAABuKkPfAAADFElEQVR4Xu2WO2gUYRDH7zCKiuDzCN5r7w4bD/HBSUBRsTCg2Ig2gSgItiISiFqJIgHBQnwUISCiYqNgCgkIijYiAUErObASUVKIBEGFKEn8TTIrX+ZWb/dur9s//Nndmfn+Ozs73yOVSpAgQYIECZqhq1gsnvI8b8Qnz8PlcnmzOLnucX3CUqm01IoEgdiNdiwcqFarS/wY0cI26Lz7MrZVrk4YWJ0gEnMNluxYQTqXy60l6BWchW/hzlqttlic6rulvrvwgIwxGoGQxOBpxszAn9wfCvjAdKFQ2I7/MwU4yn1WbCamKbz5gn+Fv+B13rOX63rlI81/xP+uQDBovyb7CZZ9O0XI8/yMBPe58WFBJ3Uzvq5J9Fs/SKN9hvffsY4oEG103ufz+Q2uXT46VAEElUplJYHjOuCk2BBcw/1jeNjGRwHjh0SXJJ+gucz4jsAxeb9rj4guNO6j32vscwXG9zC0vny8FmGcP+hJcpKkjYsKNHbAKThJu2/y7byjB9sLeZcbHxVoSLL3MpnMCsfsF2A2dAEEKibTQaZFnRY9nmphflrI35cukITQvCA2LcDLdgugWJTNZpc7z2nJHf1p+Nyxh0KaQTclWTjadA5FgLewy7ZoAXpsXBzw5qfYtKwRkYusi4hsJ5LsgtZtFyRUQXNCtb/AgzYmDogu/NFOAa7QRhe5PpVk/daNA6o/KrqyFVp/HNApNon+R3Lf6tt5PsZzyQkNBjHnCD4vyXLfp3+sjnC3jW0FzNl16L2DE9IV1t8u/AII7TTjex7wztWuLQiykg77a4B8uBRAu6HPBrcC9Heh9xuOlUKeOMNCzgbS/toFDecZ7JeszWJuK7GLoEwFbd2Gvd2Hrvq99oASBOLOancNWd+/EEZf5r0WQHaChu1c/Q2F+Qs9Et+Ab6wP2zb4HU6xQO62fgG+Af2wDySas34fzhY589+EDJrp62n2tRdQAD38nYDfZCq6vo5A/jKsWXtc6LR+25CKk+DtuBZQi07rxwLarV/a1trjQqf124auylcjnc0joNP6CRKkUn8AO1bypOScly0AAAAASUVORK5CYII=>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACoAAAAYCAYAAACMcW/9AAACRElEQVR4Xu2WvWtUQRTFd8EExSJBhYX9eruL1eIHsiIoahVBSROihRAL/wARWTBaSSwCARELQYJg4z9gIQHBQBqRVFrJthZCioBpREiExN/ROzpcH9m3UWEhe+Cw8+65c+bOzJt5m8sNMMAAuxt7qtXqzSRJngbyPF+v149J5Pd8rIm1Wm2vN0kDuU98X9huNpvDIUdexG5HY88RG419AvKlUukgSW/hFnwPz7RarSGJpj0z7Tm8pD7OIxX0LTPoLfpswq+0J1KKyFcqlZMalyKv0S4q5nJ+A4OLZvgJ1kNcg/G8iMlYnJ8V7EiB/h2b6JTXQR7v6UKhsN8LqWg0GiMYLZvhDcXK5fIB2i/hpM/vBfSflS8FvcJzn9Muw4U41hUq0ApdZiUSGcjI5/UKPE7DdbjG1h4JccY4RWxJY8X5XUEnVaet1yvQ4XW4ntvufckIraJWU4uA54xiVuSbnosUdICSn6dvy8/+b0GhDTxXzHsVjvucTLAiHzDj+/y+jmf/L2D+L+Sr0+31zKCouxjckyHtqzbzjk6tz90JisXiIfw+aFW1ul7PCl0R8+HujK8UFe2TdwL8z+L3DS7okvd6Fvy4x0KRAdp226Y/rpQAOyQX+D3sNQ/y7tguzXqtK+yr8xi+8xqxE/ALXOdQnfO6gNa2wT9SbMnrAdGp3+zpw0HyFRvgF1V00Hl+6HVRXy/nM0Z8A36m3Yo1gVfouDTvA9co/qjP/+/QtqYV2ldgdaZg28f7CjpErOQj/U/w2gD9iO/ESK/g24/f8AAAAABJRU5ErkJggg==>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADsAAAAZCAYAAACPQVaOAAADoElEQVR4Xu2WS0iVQRTHr2Rg9NLK7Pq4371qibWouIS0aBFUBD0IhQqUFrWoRRuTgkSiAqGCFklS2DsIgmopRLmQWrppkQmB9EAQigiFghC1399vRsfxqovuIuH7w2Fm/ufMmTnnOzPzxWIRIkSIEOE/QDKZzAuC4CzS4Qp8vmsHd8LT30CSrs1CQE5JScnqsrKygwTxCxlBjsMvcm0UGPxdY9OMfYVn889Ip9OL8V1bXl6+wddlFalUqoiF+hVMIpFI+3qzkcdIna/LFlh3F/7HkHpfl1UUFhYuY5FuZJxFD/h6uAZ07XRzfV22gP/W2ZKdbeSw0BMFS8WedBWmhDv5+oHLZxNOsnsItsDXZx0s1KZgkWbLmfJtJ94jrq0PbOLYXKJ9x2a/0t5hvM4z09mvQf/U2B2m3ReEl54S/Qf5ZMYtRUVFS+1EEl2F/e0gPGr9+LlaWVm5Qjqzx8tIL9KE3RXa98gL7qPSydVdoDwThF/2keV0cWkROXRtPagqhpjXaOwU1Hm4NmugjePnAdwHdFuQU/SH4fZrQ7QXtDbtMdq4Lk350VzGLcggut3iSktLl9B/CNfFZbZSlSjR8YMbM5etkjikmOwepsEYj8uJxtoETl7hvNK3dYF9nYKLmc0Z7jRyXX2TeX2t76yxSRztR8Y/ka3GXlX1A6m2PgyUyBET6CTgarVXVYeSyB5LFLA46fXB6I8qcHfeJFBsw+A30sswl7YdRw2+nQtzi/ch25UcfOwJwhJ+Y0uI/lFtArkZMwmBy5eozxoF6HqQbp1dx710aeSlvqbLY1tvfLbgp0YJVdCMB6WHy3OrYwYwrA7C7A4ok7TP/EV8JKaeiw7651jkEJKMTQWln5ZO2ch2+uwQJGwz+mGk1dfBtSW9C9PyQRjsxDNVXFy8JgjPbKdvmxEYxpEvQfjj8NqW3FxImNL3eQvn/R6Y7bLQhuXDLzmdc/guVZzLO5XgHou02feMhGWE+9YiTb4+E+yX9XmVlb4wlbHKbOxtVVXVctcGLq4LxpYfbbl4gtvB+BpdnfXnsnPn4XYv3KgqKTZVQTqvs1ZPJuicyvm4f3ZmQ0VFxVrs38ecs6EnAe4+G2gUT3uR8TcCX29tzDFR9WwMwgRPnFeTnHsp86Ynw/O+087TZcm4j3m3nBfC/iN81kVlbecFE6pZoMbn54MuA+bG50qSqRz7rMzQKXGxDP/b5txPe458JMMLL8/nI0SIEGFB4C+mdAdZmgvwVgAAAABJRU5ErkJggg==>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAFUAAAAZCAYAAABAb2JNAAAFB0lEQVR4Xu2YTWhdRRTHb0gFv7FqjM3Hm5cmErV+gFGh2IUUK3YRkWah0uLGhS50UUtTmoXWRVGxghWLYotaRbpoXdWCxiyCgosGiouKIhSxBEottrRgIcQm/v73nklOJvc9F00X4v3DYe6cc+bMnDMz58x7WVahQoUKFSpU+J+hXq9fHULYCn0UqVarvQn/Jq8H/3mvg3w3VPc6/wXIL9b/QFdX1x3yPZVHdHd3P+n9LSPGv56Oi2jp7Oy8xYz8BU339vbeBr/V6yiAyPaZzgj6vYnOkgC7D9K0pPzLRXt7+3Wsey80gSv7af+ALnGA3hgYGLgq1VfwiUsXOoehWfReo10hEp/+IDqj9HeVjc/R09PTjsIJBS2VCRqI7DNoKJUtFTo6Om7F/k98Lktllwvs7iEQw5ltGCf1GvqfWsDejXyP/v7+G5B/r5igM5DKQYvGE9wXU0GOtra261EYl1IqEzC6SQvLroDDEcyxhjmmU/5SALunoDMcnvsjT4FSwKA/obu8viCeySbQXZ7KBbPbUK6of1EWVLv6R1hQSGVLCRa27QoG9bR8g9Y7nq7z79AlXHzUqUf5RhvzXiqLCMVJvuA3awE0WEY8z679HiZ92vNTaIFK3LQ/EpyTtHvp3+51mLgf2YehSDO/8P0yOndCu0OR/H+DpjSfdP3Yvr6+G+Fvcfa/or1bMr7XQ8egA+TkDtohbB4V0X9cOlz3e3UTMlcH6K8MxUkrDUqweEAbUllEKDalUXrIFV6REc9TAVMgGiZjTjjy5xh3Hic2m54K23YtKirZ5pyC/wTd1lAE4qzyUShOjPrnoYPK734+eGs1lnleVS40+8/COw1vFe0+Fc5QOKjrukvVnfYHaExFKtryqBUpbRY6bHa9bDn8CbO3KDVEILvQVAdDg5okLkJVjsWPMmFfqhuB/lAors/2zCV7eC9B71hXgR+G1qlDQbqW7280TpsmHv3H6M9g54VoQyDAD8M/V0uKiVXmSdm1l0ue/+h/bUUo94X2g6ykDtj4n5H/WpbWavP5dlz1JpVHaI7QJKfqVD6EwkVVYbrLQlExN6V6HlqYnIFWa6G6bqG4+t+pbzqroSl/GrRQPeUyCxTynXJCzngdOQWdw+49kS/U7OrSrsHuzRZYObjRVFrlR9kNs+fV59BEXGMKba7Z25nKHBQjVf8dqWAOwXbb3mHr+D6YXosU6MyEIhfqR8MwEzwF1bOFp3ZRrvYwJ8fkpN/xeHqhI/XkoR5lmZ1Cm6PxNTRYGlL+/lKbkcoNc0U7uMKWQhuN/IT5W44wXw2V375l4atSnRQ2ccPddAG7mMoi3Kn7JFu4GXmOh0acepTpZOcb5U5042uYzQX0bVGSs0f8DYnvZVvTysj3cJvT/N3uFqcrvSWVlwG9GSbelvI1qe2gJj8ETaY6Oin69ZZeXYbtV7FSX3zsD/pxJtMax9QP8+/Jhk+frDh9ej1szdwLwHw+4IOnlAJvOpTcEMF82yGdzB2CRlCOOMSA0WbJ2QP949D7mTNuz5+PsbNZfNpn6E/NDcrywNwHb1w/S7UpYT6f6mWQ7z5tDzTJ+LdkRzylo1D8XD4eCwzfG6DZWPTKgO1hdP6WPb5PRqJ/NixOO1rPbEhuiIKJ7BHWcxTZGWitlzdE+Jec1AD5/weMXdFsMyQv+V8hhxasE1h2MkCrxkleVng0pp78AVShQoUKFSpUaIB/APWxk23IeyCaAAAAAElFTkSuQmCC>