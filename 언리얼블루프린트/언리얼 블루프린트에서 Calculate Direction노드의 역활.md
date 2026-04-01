언리얼 블루프린트에서 Calculate Direction노드의 역활?

Calculate Direction 노드는 애니메이션 블루프린트(AnimBP)를 만들 때 거의 필수적으로 사용되는 노드로, **캐릭터의 이동 방향과 바라보는 방향 사이의 각도(-180 ~ 180도)**를 계산해 줍니다.

앞서 질문하신 Unrotate Vector가 벡터 자체를 변환한다면, Calculate Direction은 그 결과를 바탕으로 **하나의 숫자(각도)**를 뽑아내어 블렌드 스페이스(Blend Space)에 전달하기 위해 사용합니다.

1. 핵심 개념
이 노드는 두 가지 정보를 입력받아 결과를 계산합니다.

Velocity (벡터): 캐릭터가 현재 실제로 이동하고 있는 물리적 방향.

Base Rotation (회전값): 캐릭터가 현재 몸을 돌려 바라보고 있는 방향.

결과값: 캐릭터가 정면을 기준으로 왼쪽으로 가고 있는지, 오른쪽으로 가고 있는지를 도(Degree) 단위로 반환합니다.

2. 반환값의 의미 (범위: −180.0∼180.0)
반환된 Float 값에 따라 캐릭터의 이동 상태를 다음과 같이 판별할 수 있습니다.

값 (Degree)	의미	블렌드 스페이스 활용
0	정면 이동	전진(Run Forward) 애니메이션
90	오른쪽 이동	오른쪽 게걸음(Strafe Right)
-90	왼쪽 이동	왼쪽 게걸음(Strafe Left)
180 / -180	후방 이동	후진(Run Backward)
3. 주요 사용 사례: 8방향 이동 (Strafe)
가장 대표적인 활용처는 3D 액션 게임의 이동 애니메이션입니다.

상황: 플레이어가 키보드의 'W'와 'D'를 동시에 눌러 대각선 오른쪽 앞으로 뛰어갑니다.

노드 작동:

Velocity는 대각선 방향 벡터가 들어옵니다.

Base Rotation은 캐릭터의 현재 회전값이 들어옵니다.

Calculate Direction은 이 둘을 비교해 약 45.0이라는 값을 내뱉습니다.

결과: 이 45.0이라는 값을 Blend Space 2D의 'Direction' 축에 입력하면, 엔진이 알아서 '앞으로 뛰기'와 '오른쪽으로 뛰기' 애니메이션을 섞어서 출력합니다.

4. Unrotate Vector와의 차이점
Unrotate Vector: 월드 속도를 캐릭터 기준 로컬 속도(X, Y, Z)로 바꿉니다. (값의 뭉치인 '벡터'가 필요할 때)

Calculate Direction: 캐릭터 기준 이동 방향을 '각도' 하나로 바꿉니다. (블렌드 스페이스의 '방향' 축에 넣을 '실수'가 필요할 때)

💡 팁
이 노드는 주로 애니메이션 블루프린트의 Update Animation Event 그래프에서 사용합니다.

Base Rotation에는 보통 Get Control Rotation 또는 캐릭터의 Get Actor Rotation을 연결합니다.

