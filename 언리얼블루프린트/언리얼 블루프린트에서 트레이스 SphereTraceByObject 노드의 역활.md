# **SphereTraceByObject 노드 이해하기**

언리얼 엔진 블루프린트에서 SphereTraceByObject는 특정 시작점부터 끝점까지 **구체(Sphere) 형태의 광선**을 쏘아, 지정된 \*\*오브젝트 타입(Object Type)\*\*과 충돌하는지 확인하는 노드입니다.

## **1\. 주요 역할**

단순한 선(Line Trace)은 아주 가는 직선상에 있는 물체만 감지하지만, SphereTraceByObject는 부피가 있는 구체를 이동시키며 경로상에 걸리는 모든 물체를 찾습니다.

* **범위 감지**: 좁은 선이 아니라 일정한 두께를 가진 범위를 체크합니다.  
* **필터링**: 특정 채널(Visibility 등)이 아닌, 오브젝트의 속성(WorldStatic, Pawn, PhysicsBody 등)을 기준으로 충돌 여부를 결정합니다.

## **2\. 주요 입력 파라미터 (Inputs)**

| 파라미터 | 설명 |
| :---- | :---- |
| **Start** | 구체 트레이스가 시작될 위치 (![][image1]) |
| **End** | 구체 트레이스가 끝날 위치 (![][image1]) |
| **Radius** | 구체의 반지름 크기. 이 값에 따라 감지 범위의 두께가 결정됩니다. |
| **Object Types** | 감지할 오브젝트 타입 배열 (예: WorldDynamic, Pawn, Vehicle 등) |
| **Trace Complex** | 복잡한 충돌(Mesh 자체)을 체크할지, 단순한 충돌(Capsule/Box)을 체크할지 여부 |
| **Actors to Ignore** | 트레이스에서 제외할 액터 목록 (보통 '자기 자신'을 넣습니다) |
| **Draw Debug Type** | 화면에 트레이스 경로를 시각적으로 표시 (None, For One Frame, For Duration, Persistent) |

## **3\. 주요 출력 결과 (Outputs)**

* **Return Value**: 무언가에 부딪혔다면 True, 아니면 False를 반환합니다.  
* **Out Hit**: 충돌한 지점, 충돌한 액터, 노멀 값 등 상세 정보를 포함하는 Hit Result 구조체입니다.

## **4\. 활용 사례**

1. **아이템 습득 시스템**: 플레이어 앞에 대략적인 구체 범위를 쏘아 주변에 있는 'Item' 타입의 오브젝트를 찾을 때 사용합니다.  
2. **근접 공격 판정**: 칼을 휘두를 때 칼의 궤적을 따라 구체 트레이스를 생성하여 적(Pawn)이 맞았는지 판정합니다.  
3. **바닥 체크**: 캐릭터가 울퉁불퉁한 지형 위에 있는지 확인할 때, 선보다는 약간의 두께가 있는 구체 트레이스가 더 정확할 수 있습니다.

## **5\. Line Trace vs Sphere Trace**

* **Line Trace**: "저 앞에 점 하나를 찍었을 때 벽이 있는가?"를 묻는 것과 같습니다. (매우 가벼움)  
* **Sphere Trace**: "이 굵은 파이프를 밀어 넣었을 때 걸리는 게 있는가?"를 묻는 것과 같습니다. (조금 더 연산량이 많지만 정확한 범위 체크 가능)

### **팁**

Object Types 핀을 끌어서 **Make Array** 노드를 연결하면, 여러 종류의 오브젝트(예: Pawn과 PhysicsBody를 동시에)를 한 번에 감지하도록 설정할 수 있습니다.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADMAAAAWCAYAAABtwKSvAAADOElEQVR4Xu2WO2hUQRSG7xKFiIrPuG72cfcBBl/VamdhoWAEmyhYaGFnl0IwQW0CVnYiaBGEYBGCYiNREUwRtLARUSEoQiCRiIhIQEwIShK/f+/MZu54Iy6kCLI/HO7MOWfOY86ZmRsETTTRRBP/DQqFwscwDEeg/mKxeJfvOHTLysvl8ibmw9KzxPypa2NVgeA6oUUCPeHLLEi0L5fLbfX5K4VsNruNGKbxsd+XNQSSqGJohm+vLxPg74U++PyVBPYPEcNYe3v7dl/WENiVHIamoBu+rFqtrqUq95Bd9WUrCexfIKEBhilf1hDa2to2YGxUpLErg/eKZE67vCQoaYI5JeKc7fLlQqlUSquVpSN9WC2VSmWHKo+f96bdM2o5fy0xtC5n3z8eKYwMQuNy6PJVFeM4EfT4OtZdhH5gtAf9a4y/Bd4OKwDpmEumH3rD+DDfr9CiRyPuWmN/DvvdZu2cLibJtBnMb7v6yq43jM5NVXMFyfhBTMmDkjTO1YL14AnynHUmIJ+Gxu1cO8l8Ab1jRn5fdqzcwtjXLXspcOzD2w1NUEFt0OOOjo6NzrK6AwXWaeZnGY/GlDwg71Kg+Xx+n+am1bROlanBXO2Lug0tj/FOeCcZtmgeRs/BjJVbGPsLUMnl46MsfTb8ON9BzV25vU2UzBnN+Y7BOxpTimNNGO3oPDRViN4fjR+R3AGrpN2XY1vxJCD/JX8uzznHqlisZU2sdZuhKUAdMDJaiPM7SoJvMabgwehPQl2+zAITrUou9Hs6Dm2K/J53mQpQfOi7yw+WNnFIY6MbO2PKdotZ/BLDb2PCBOg9CKPqLfvQBsZxIfn9UouljJ1ZqnlQTLUpdB06EkYtNukuUkvDm3YrzfyzqxOk0+n1JhnRcEyYDAU6FCZURm1GkHmNlQg6l30deA+1VkGF5rE0N9Nz8TgPWcYT0JRdY87kgGIM4hfCH5dHjYlyT9DAw8WaT9AX1j3hOwvdhDKezjvoJ/RaOlT+WbDkQ5uiCiiJ7sBcCoK5za6E0Ruk5F+gs8fKLeDP+zz1eJ97pf4jag+fSaAeiIeU+ffK6Bz5Qnib/Q1wIdnf5EmPbBOrBb8BbgTrppf964AAAAAASUVORK5CYII=>