# Create More Features Also With Illegal Flying (1.20.1)

## 들어가기 전
본 저장소는 [Create: More Features](https://www.curseforge.com/minecraft/mc-mods/create-more-features) 을 수정한 버전을 다룹니다.

create_more_features-0.8.0-forge-1.20.1.jar 파일에 대응합니다.

작성자도 모더나 초고급 개발자가 아니기 때문에 현 문서에서 제공하는 모드 파일과 정보 이외의 추가적인 버그패치, 기술지원 등은 제공하기 어렵습니다.

문제가 있는 경우 이슈를 등록해서 다른 사람도 알 수 있도록 해주세요.

## 이게 무엇인가요?
원본 모드(Create: More Features) 의 아이템 중 그래비트론 이라는 아이템이 크리에이티브 무중력 비행을 차단하는 문제를 수정한 변형본 입니다.

## 무슨 문제가 있나요?
그래비트론으로 날던 중 그래비트론이 손에서 없어지는 경우 비행 기능이 꺼져야 합니다.

위 기능을 구현하기 위해 매 틱마다 손에 그래비트론이 있는지 검사하고, 없는 경우 비행 모드를 해제하는 코드가 있습니다.

따라서 손에 그래비트론이 없다면 메커니즘 메카슈트 중력 유닛 등 크리에이티브 모드 비행과 유사한 비행 기능을 제공하는 아이템들이 비행 모드를 활성화 해도 곧바로 해제되기 때문에 작동하지 않습니다.

config 로 이 기능을 끌 수도 없습니다.

## 무엇을 해야 하나요?
그냥 Create: More Features 모드를 제거하거나, [릴리즈](https://github.com/QyuBot/create-more-features-also-with-illegal-flying/releases/latest) 에 있는 수정된 모드 파일 중 하나로 원본 모드를 교체하세요.

2가지 버전이 있으니 필요한 것 하나를 받아서 교체하면 됩니다.
### OPG(Over Powered Gravitron) 버전
- 매 틱마다 손에 그래비트론이 없으면 비행모드를 해제하는 코드만 제거
- 그래비트론으로 한 번 난 뒤에는 손에서 그래비트론이 제거되어도 메인메뉴로 나갔다 올 때까지 계속 날 수 있는 문제가 있음
- 다른 아이템들은 잘 작동함

### ULG(Useless Gravitron) 버전
- 그래비트론의 코드를 모두 제거
- 아이템은 무용지물이 됨

## 어떻게 수정했나요?
[Recaf](https://recaf.coley.software/home.html) 의 바이트코드 수정 기능을 이용하여 GravitronprocedureProcedure.class 파일을 직접 수정했습니다.

## 직접 수정하기
마인크래프트 모딩 지식과 프로그래밍(어셈블리) 지식이 있으면 [수정 방법](/docs/ko/howTo.md)를 참고하여 직접 해볼 수 있습니다.
