# **Get World Location 노드 완벽 이해하기**

언리얼 엔진 블루프린트에서 **Get World Location** 노드는 씬 컴포넌트(Scene Component)의 절대적인 3D 좌표를 추출할 때 사용됩니다.

## **1\. 핵심 역할**

이 노드의 주요 목적은 \*\*"이 객체가 게임 세상의 중심(0, 0, 0)을 기준으로 지금 어디에 있는가?"\*\*에 대한 답을 찾는 것입니다. 결과값은 **Vector(벡터)** 형식으로 반환되며, 각각 ![][image1] 축의 값을 가집니다.

## **2\. World Location vs Relative Location**

가장 많이 혼동하는 개념이 '상대 좌표(Relative)'와 '월드 좌표(World)'의 차이입니다.

| 구분 | Get World Location | Get Relative Location |
| :---- | :---- | :---- |
| **기준** | 게임 월드의 원점 ![][image2] | 부모(Parent) 컴포넌트의 위치 |
| **영향** | 부모가 움직여도 월드상의 실제 위치를 반환 | 부모가 움직여도 부모와의 거리만 같다면 값은 일정 |
| **비유** | "서울시청의 GPS 좌표" | "내 방 문에서 침대까지의 거리" |

## **3\. 노드 구조**

* **Target (입력):** 위치를 알고 싶은 **Scene Component** (Static Mesh, Camera, Collision Box 등)를 연결합니다.  
* **Return Value (출력):** 해당 컴포넌트의 현재 월드 좌표인 **Vector** 값을 반환합니다.

## **4\. 주요 사용 사례**

### **① 라인 트레이스(Line Trace) 수행**

공격이나 상호작용을 위해 광선을 쏠 때, 시작 지점을 정하기 위해 사용합니다.

* 예: "캐릭터의 카메라 위치(Get World Location)에서 앞방향으로 광선을 발사하라."

### **② 액터 스폰(Spawn Actor)**

특정 아이템이나 이펙트를 특정 위치에 생성할 때 사용합니다.

* 예: "몬스터가 죽은 위치(Get World Location)에 전리품 상자를 생성하라."

### **③ 거리 계산 (Distance)**

두 객체 사이의 실제 거리를 잴 때 유용합니다.

* Get World Location (A객체)와 Get World Location (B객체) 사이의 거리를 **Distance (Vector)** 노드로 계산합니다.

### **④ UI 표시**

3D 세상의 특정 위치를 2D 화면(HUD)에 표시하고 싶을 때, 월드 좌표를 가져와서 Project World to Screen 노드와 조합하여 사용합니다.

## **5\. 주의사항**

* **Actor vs Component:** Get Actor Location은 액터 자체의 루트(Root) 위치를 가져오고, Get World Location은 액터 내부의 **특정 컴포넌트** 위치를 가져올 때 사용합니다.  
* **Tick에서의 사용:** 매 프레임 위치를 체크해야 하는 경우(예: 추적 미사일) Event Tick에 연결하여 실시간 위치를 갱신할 수 있습니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEEAAAAZCAYAAABuKkPfAAADFElEQVR4Xu2WO2gUYRDH7zCKiuDzCN5r7w4bD/HBSUBRsTCg2Ig2gSgItiISiFqJIgHBQnwUISCiYqNgCgkIijYiAUErObASUVKIBEGFKEn8TTIrX+ZWb/dur9s//Nndmfn+Ozs73yOVSpAgQYIECZqhq1gsnvI8b8Qnz8PlcnmzOLnucX3CUqm01IoEgdiNdiwcqFarS/wY0cI26Lz7MrZVrk4YWJ0gEnMNluxYQTqXy60l6BWchW/hzlqttlic6rulvrvwgIwxGoGQxOBpxszAn9wfCvjAdKFQ2I7/MwU4yn1WbCamKbz5gn+Fv+B13rOX63rlI81/xP+uQDBovyb7CZZ9O0XI8/yMBPe58WFBJ3Uzvq5J9Fs/SKN9hvffsY4oEG103ufz+Q2uXT46VAEElUplJYHjOuCk2BBcw/1jeNjGRwHjh0SXJJ+gucz4jsAxeb9rj4guNO6j32vscwXG9zC0vny8FmGcP+hJcpKkjYsKNHbAKThJu2/y7byjB9sLeZcbHxVoSLL3MpnMCsfsF2A2dAEEKibTQaZFnRY9nmphflrI35cukITQvCA2LcDLdgugWJTNZpc7z2nJHf1p+Nyxh0KaQTclWTjadA5FgLewy7ZoAXpsXBzw5qfYtKwRkYusi4hsJ5LsgtZtFyRUQXNCtb/AgzYmDogu/NFOAa7QRhe5PpVk/daNA6o/KrqyFVp/HNApNon+R3Lf6tt5PsZzyQkNBjHnCD4vyXLfp3+sjnC3jW0FzNl16L2DE9IV1t8u/AII7TTjex7wztWuLQiykg77a4B8uBRAu6HPBrcC9Heh9xuOlUKeOMNCzgbS/toFDecZ7JeszWJuK7GLoEwFbd2Gvd2Hrvq99oASBOLOancNWd+/EEZf5r0WQHaChu1c/Q2F+Qs9Et+Ab6wP2zb4HU6xQO62fgG+Af2wDySas34fzhY589+EDJrp62n2tRdQAD38nYDfZCq6vo5A/jKsWXtc6LR+25CKk+DtuBZQi07rxwLarV/a1trjQqf124auylcjnc0joNP6CRKkUn8AO1bypOScly0AAAAASUVORK5CYII=>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADYAAAAUCAYAAADGIc7gAAADE0lEQVR4Xu2WP2gUQRjF94iCohhEzyO5P3N3OQSxkHAqSNRKxBSKhYKSdOKfwkqIQSsbC7tgkyYQFEQUmxQiSArLqJUgWIgYIRiiGCFgmkDO38vurLOTvUsCggj34HE7b97Mft/M7NwXBG200UYbfwOVSsXAw77+nyGTaBUKha2lUmkiIYJarbYDfcQYMw9neX4N+3zfWsjn84VyufyEOb7Db/BFtVrd6/vWgQ7mucj4acUDr9fr9c2+KQbBDmKacjVe3Ik2CceUuLRisXhSwfF7xvWuBcZ8IKBbPHbADO1LcA72+t4WyBDnTc1FPDUJihltlMdNnjdOYErZuzqBXEBbYGDdkTX5uL8IawH/NMHkbTubzW5HewUfB2lBpQBvBc7w/mGraYHRFuER17sCjEe1em4CJLUF7Tn8Ars8/zDakqu1Qnd3924loWRcnXc8MOHxrrp6M+C/ir+B/7TVFDPaL3jX9a5AInyLaaejdUVJJXQhSqzhaq0QvfxZ4O1MlFgD9rt6M0T+JW2E1ZzEJnO53DbXbHcm8WJuxwNoCyZlpdEGNpKYAldQKboWVIkN+H0+FLSCVxLuydJum/ASSW6AGhLhWCyGul2JVYnpKGwkMfnTErM7r1+/z4fzTSYSM39OVvKTsR3+5O3E1oF/mhi8HYtB4og2uxU3kpi+g1ULFF0Gi1zZh1y9GUz0TWqhrOZswKPArTrsSqStKPp9k3IdW9226e+jPZS4lRxEF1Ti43YurVhXBcHz5cAviyLQdwL/snEumzQtBuKY8a/LUO+F8wwetFpPT88etPdoI2pze+Zof4INgr0TD/ZA/0925phtM34/2hxjrjmefs1j0v5sg3AT8L+ET20ZpTgUj+Ly/Vo9/fHNqJ7zulRlnKfvhwmPwQDeNyas8zplUKlF+yFcxjvujY9RCkshzXOD5yv8fuR31K3zVDuify45R82HSik87/BMlMPKaJYFO+j7VkDnPvgV4ym/TyDhXXoZPBcVrquOCn1Vxt/zdRdUIEU8Z0U9+/0WrRITtBiclOOKJ60A/g23CgoH076K3AAAAABJRU5ErkJggg==>