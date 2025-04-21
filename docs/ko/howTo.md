# Recaf 을 이용한 모드 수정 방법

## 왜 Recaf 씀?
모드는 mCreator 로 작성된 것으로 추정되고, Forge 개발환경을 구축하고 디컴파일 코드를 전환하는데 오랜 시간이 걸릴 것으로 예상됨

recaf 을 이용하면 디컴파일 후 재컴파일을 할 필요 없이 코드를 수정할 수 있다. 

하지만 어셈블리와 유사한 바이트코드를 이해하고 다룰 수 있어야 한다.

## 환경 구축
- [Recaf Repository](https://github.com/Col-E/Recaf-Launcher/releases) 에서 recaf-gui 를 받는다.
- recaf-gui.jar 을 자바로 실행
- 요구 파일들을 업데이트 눌러서 설치하고 하고 설치된 jdk 를 선택한 뒤 Launch 를 눌러 실행
![image](/asset/image/01_recap-gui.png)
- 실행된 Recaf 런처에서 File > Open workspace 새 창에서 Open - Files 버튼을 누르고 모드 파일(`create_more_features-0.8.0-forge-1.20.1.jar`)을 선택한 뒤 Next 버튼 클릭
![image](/asset/image/03_select-jar.png)

## 코드 확인 및 수정 테스트
- 아래 사진과 같이 모드 내 클래스 파일의 java 코드를 확인할 수 있음
- 클래스 이름 텍스트를 우클릭 하고 Edit > Edit class in assembler 를 클릭하면 바이트 코드 수정 화면을 볼 수 있음
![image](/asset/image/11_code-editor.png)
- Assembler 에디터 에서 코드를 문법 오류 없이 적절히 수정한 뒤에 Ctrl+S 로 저장하면 GravtironprocedureProcedure.class 의 java 코드가 수정된 것을 확인할 수 있음
- 코드 수정은 Java 에디터가 아닌 Assembler 에디터 에서 해야 한다.

## 바이트코드 분석 및 수정
- bytecode 의 모든 OP 코드는 [이 페이지](https://www.coley.software/Recaf-documentation/use-bytecode-list.html)에 있음
- `A:` `B:` 등은 GOTO 로 이동 가능한 코드의 특정 포인트
- `line nn` 은 참고용 으로 그냥 지워도 됨

java 코드 에디터에서 `f_35936_` 를 검색하면 아래와 같은 코드들이 나온다.
```java
if (entity instanceof Player) {
    _player = (Player)entity;
    _player.m_150110_().f_35936_ = true;
    _player.m_6885_();
}
```
Player 의 특정 필드의 true/false 를 변경하고, Player 의 다른 메소드를 호출하고 있음

코드의 위 조건문들을 을 참고한다면 이 코드는 플레이어의 비행 모드를 변경하는 것으로 추측 가능하다.

다음으로 GravtironprocedureProcedure 의 아래 바이트코드 중 `f_35936_` 가 등장하는 부분을 보자
```bytecode
.method private static execute (Lnet/minecraftforge/eventbus/api/Event;Lnet/minecraft/world/level/LevelAccessor;DDDLnet/minecraft/world/entity/Entity;)V {
        parameters: { event, world, x, y, z, entity }
    // ...
    AK: 
        line 98
        aload _player
        invokevirtual net/minecraft/world/entity/player/Player.m_150110_ ()Lnet/minecraft/world/entity/player/Abilities;
        iconst_0 
        putfield net/minecraft/world/entity/player/Abilities.f_35936_ Z
    AL: 
        line 99
        aload _player
        invokevirtual net/minecraft/world/entity/player/Player.m_6885_ ()V
```
위 코드는 아래와 같이 추측할 수 있음
- `aload _player` : `_player` 변수를 스택에 올린다
- `invokevirtual net/minecraft/world/entity/player/Player.m_150110_ ()Lnet/minecraft/world/entity/player/Abilities;` : `Player.?` 클래스의 `Abilities` 메소드를 호출한 결과를 스택에 넣는다
- `iconst_0` : 스택에 `0` 을 넣는다
- `putfield net/minecraft/world/entity/player/Abilities.f_35936_ Z` : 스택 최상층의 값을 `Abilities.f_35936_` 필드에 넣는다
- `aload _player` : `_player` 변수를 스택에 올린다
- `invokevirtual net/minecraft/world/entity/player/Player.m_6885_ ()V` 어떤 메소드를 호출하고 있다

코드의 314 줄부터 329 줄까지인 아래 내용을 지우고 Ctrl+S 로 저장해보자
```bytecode
    aload entity
    instanceof net/minecraft/world/entity/player/Player
    ifeq AM
    aload entity
    checkcast net/minecraft/world/entity/player/Player
    astore _player
AK: 
    line 98
    aload _player
    invokevirtual net/minecraft/world/entity/player/Player.m_150110_ ()Lnet/minecraft/world/entity/player/Abilities;
    iconst_0 
    putfield net/minecraft/world/entity/player/Abilities.f_35936_ Z
AL: 
    line 99
    aload _player
    invokevirtual net/minecraft/world/entity/player/Player.m_6885_ ()V
```
그런 뒤 Java 에디터로 돌아와 보면 다음과 같이 수정된 것을 볼 수 있다(왼쪽이 전, 오른쪽이 후).
![image](/asset/image/12_edit-diff.jpg)

이런 방식으로 원하는 부분의 코드를 제거하거나, 어셈블러에 능통하다면 기능을 추가하는 것도 가능할 것이다.

수정이 완료됐다면 File > Export application 에서 .jar 파일을 내보낼 수 있다.

## 본 글에서 수정한 모드 부분은
수정하고 배포한 .jar 파일을 Recaf 로 열고 원본 모드와 bytecode 를 비교해 보면 어느 부분이 변경되었는지 찾을 수 있다. 

