
Unity Game Development Documentation
목차
Equipment와 EquipTool 기능 분석
Resource 기능 분석
AI 네비게이션 시스템 분석
NPC 기능 분석
보간에 대한 학습
근사값(Mathf.Approximately) 사용 이유
확장 기능 구현
개선 기능 구현
1. Equipment와 EquipTool 기능 분석
Equipment
역할: 플레이어가 장착한 아이템의 정보를 저장하는 역할을 수행합니다.
핵심 로직: 장비 해제 시, curEquip에 저장된 장비 정보를 null로 설정하며, 장비에 대한 오브젝트를 파괴합니다.
주요 함수
UnEquip: 장착 해제 로직. curEquip의 정보를 null로 설정하고, 오브젝트를 파괴하여 장비가 해제되도록 구현합니다.
EquipTool
역할: 플레이어가 장착할 아이템의 공격력, 딜레이, 거리, 스태미나 사용량 등의 속성을 설정합니다.
핵심 로직: 무기의 속성을 정의하고, 공격 또는 자원을 캐는 등의 행동을 무기 자체에 부여하여 구현합니다.
2. Resource 기능 분석
역할
Resource는 ScriptableObject에 저장된 아이템 데이터를 참조하여, 해당 프리팹을 지정된 위치에 생성하는 역할을 수행합니다.
핵심 로직
Gather(Vector3 hitPoint, Vector3 hitNormal):
quantityPerHit 횟수만큼 반복하여 아이템을 생성합니다.
capacy 값이 0보다 작거나 같으면 생성이 중지됩니다.
Instantiate 함수로 itemToGive.dropPrefab을 지정된 위치와 회전 방향에 맞게 생성합니다.
3. AI 네비게이션 시스템 분석
Navigation Mesh (네비게이션 매쉬)
기능: 3D 공간을 이동 가능한 영역과 장애물 영역으로 나눠주는 매쉬입니다.
사용 이유: 캐릭터가 이동할 수 있는 경로와 장애물을 인식하고, 이를 바탕으로 이동 경로를 계산합니다.
Pathfinding (경로 탐색)
기능: A* 알고리즘 등을 활용하여 목표 위치까지의 최적 경로를 계산합니다.
Obstacle Avoidance (장애물 피하기)
기능: 경로 중 장애물과 충돌하지 않도록 피하는 기술을 적용합니다.
Steering Behavior (스티어링 동작)와 Local Avoidance (근접 회피)
기능: 다수의 NPC나 캐릭터가 서로 충돌하지 않도록 자연스럽게 회피하는 동작을 구현합니다.
4. NPC 기능 분석
주요 기능
Stats: NPC의 체력, 이동 속도, 전리품 정보를 설정합니다.
AI: 플레이어와의 거리를 기반으로 행동을 결정합니다.
Wandering: NPC가 자연스럽게 이동하거나 멈추는 행동을 정의합니다.
Combat: NPC의 공격력, 공격 딜레이, 공격 사거리를 설정합니다.
핵심 로직
SetState 함수: NPC의 상태를 설정하여 행동을 정의합니다.
PassiveUpdate와 AttackingUpdate: 플레이어와의 거리와 시야에 따라 상태를 전환하며, 상태에 따른 행동을 수행하도록 합니다.
5. 보간에 대한 학습
선형 보간(Lerp)
Lerp: 두 점 사이의 값을 선형적으로 계산하여 부드러운 전환을 제공합니다.
예시 코드:
csharp
코드 복사
float value = Mathf.Lerp(a, b, t);
t의 값에 따라 a와 b 사이의 중간값을 반환합니다.
구면선형보간(Slerp)
Slerp: 두 점 사이를 곡선 형태로 보간하여 부드럽게 회전하거나 곡면 상에서 이동하는 효과를 제공합니다.
예시 코드:
csharp
코드 복사
Quaternion rotation = Quaternion.Slerp(startRotation, endRotation, t);
3D 회전 등을 구현할 때 유용합니다.
6. 근사값(Mathf.Approximately) 사용 이유
부동소수점 문제 해결
부동소수점 연산 특성상 작은 오차가 발생할 수 있습니다. 따라서 == 연산자로 비교하면 정확하지 않은 경우가 많습니다.
Mathf.Approximately 함수를 통해 두 부동소수점을 비교하여 근사값을 판단할 수 있습니다.
csharp
코드 복사
if (Mathf.Approximately(a, b)) {
    // 두 값이 유사하다고 판단됨
}
7. 확장 기능 구현
새로운 자원 및 보상 아이템 추가
새로운 자원 - Mushroom
ItemData에 Mushroom 데이터를 추가하여 새로운 아이템과 프리팹을 생성합니다.
Resource_Mushroom을 추가하고, Resource.cs 스크립트를 통해 채집 가능하게 합니다.
몬스터 공격 효과음 추가
Attack 시 효과음: NPC가 공격을 할 때, 사운드 클립을 재생하도록 설정하여 몰입감을 높입니다.
csharp
코드 복사
AudioSource.PlayOneShot(attackSoundClip);
새로운 조명 효과 추가
게임에 새로운 종류의 조명을 배치하여 특별한 장면에서 분위기를 강조합니다. 예를 들어, 중요한 장소에 은은한 조명을 배치하여 시각적 효과를 극대화할 수 있습니다.
