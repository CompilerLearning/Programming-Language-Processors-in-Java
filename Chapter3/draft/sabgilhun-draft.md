# Compilation
컴파일러 내부 구조 공부, 컴파일러의 기본 기능은 고수준 언어를 저수준 언어로 바꾸는 것, 하지만 이 작업 전에 꼭 소스 코드가 제대로 작성됐는지 확인 필요. 컴필레이션은 3가지 단계로 구성됨, '구문분석', '문맥 분석' '코드 제너레이션'

이 장에선 각 단계의 관계를 공부할 것, 그리고 컴파일러에서 채택할 수 있는 디자인들을 확인할 것이다. 각 디자인들은 몇번 소스 코드를 훑는지에 따라 특성이 갈린다. 그리고 어떤 디자인을 선택할지에 대해 논의한다.

## 3.1 Phases
컴파일러 내부에서, 오브젝트 프로그램이 생성되기까지, 소스 프로그램은 여러가지 변형을 거친다. 이러한 변형을 'phases'라고 한다. 3가지 주요한 컴파일러 단계는 아래와 같다.

Syntactic analysis: 소스프로그램 파싱, 문법이 맞는지 확인하기 위해, 그리고 구문 구조를 결정

Contextual analysis: 파싱된 프로그램을 분석, 문맥적 제한이 맞는지 확인하기 위해

Code generation: 확인된 코드를 변환, 오브젝트 프로그램으로, 소스언어 타겟언어의 의미에 따라

각 단계는 이전에 다뤘던 언어의 명세 3가지 부분과 각각 대응된다.

syntax - syntactic analysis
contextual constraint - contextual analysis
semantics - code generation

어떤 언어의 경우 코드 최적화 단계도 포함된다.

각 단계 사이에서 이미 적용된 것을 나타내야 한다. 이때 적절한 방법이 AST이다. AST는 명시적으로 소스 프로그램의 구문 구조를 나타낸다. 

AST의 서브트리는 각 구문에 대응된다. 그리고 리프 노드는 식별자, 리터럴, 오퍼레이터에 대응된다. 그러고 나서 모든 다른 터미널 심볼은 문법 분석 후 버려진다. (??? 주석인가)

### 3.1.1 Syntactic analysis
구문 분석의 목적은 소스의 구문 구조를 결정하는 것이다. 이 과정을 파싱이라 부른다. 다음 단계에서 어떻게 프로그램이 구성 되어 있는지 알아야 하기 때문에, 이 단계는 중요하다.

문법이 잘 적용되었는지 확인하기 위해 소스는 파싱된다. 그리고 적절한 구문 구조의 표현을 구성한다. 이 책에선 AST로 구성한다.

AST의 터미널 노드는 식별자, 리터럴, 오퍼레이터에 상응되고, 서브 트리는 구문을 나타낸다. 블랭크, 주석 등은 AST에서 제거 된다. 구두점, 괄호 역시 각 구문의 분리를 나타내기 위한 역할이기 때문에 AST에선 사라진다.  이 단계에서 만약 적절한 구문 구조가 없어 에러가 발생하면 AST대신 에러리포트를 낸다.

3.1.2 Contextual analysis
문맥 분석에선 파싱된 프로그램을 더 분석한다. 문맥 제한을 잘 따랐는지 확인하기 위해

scope rule은 컴파일 타임에 식별자의 사용이 상응되는 정의를 찾고 정의 되지 않은 식별자를 찾게 해준다. 

type rule은 컴파일 타임에 각 표현의 타입을 추론 하게 한다.

만약 파싱된 프로그램이 AST로 나타내진다면, 문맥 분석을 통해 decorated AST를 만들어내질 것 이다. 이것은 문맥 분석을 통해 얻어진 정보가 더해진 형태이다.

스코프 룰 적용 결과로 식별자 사용처는 선언부와 연결된다. 그리고 그 것을 다이어그램에서 대시 화살표로 표현할 것이다.

타입 룰 적용 결과로 각 expression은 타입 값으로 꾸며질 것이다. 그리고 이것을 다이어그램에서 표현식 루트 노드에 ':T'로 마킹 할 것 이다.

3.1.3 Code generation
앞 단계들을 거치면 소스 코드가 잘 작성됐는지 확인은 끝난다. 코드 제너레이션은 마지막으로 오브젝트 프로그램으로 변환하는 것이다. 각 언어의 의미에 맞게

코드 제네레이션 단계에서 제일 많이 다뤄지는 이슈는 소스 코드에서 선언된 식별자를 다루는 것이다. 의미론적 관점에선 선언은 식별자를 일종의 엔티티에 바인딩 하는 것 이다.

ex)
const m ~ 7 = 식별자 m과 7 값을 바인딩

var b : Boolean = 식별자 b와 변수를 담을 어떤 주소와 연결 (이것은 제네레이터가 결정)
이렇게 되면 코드제너레이터는 각 b 사용처를 주소로 교체한다.

컴파일러 디자이너에게 타겟 랭귀지의 특성은 이슈가 될 수 있다. 그러니까, 어떤 기계 언어 혹은 어셈블리 랭귀지를 만들지에 대한 이슈. 하지만 우리가 학습을 할땐 기계어보단 mnemonically한 코드를 다루는 것이 훨씬 가독성이 좋다.

```
MNEMONIC: English word MNEMONIC means "A device such as a pattern of letters, ideas, or associations that assists in remembering something.". So, its usually used by assembly language programmers to remember the "OPERATIONS" a machine can do, like "ADD" and "MUL" and "MOV" etc. This is assembler specific.

Assembly: There are two "assemblies" - one assembly program is a sequence of mnemonics and operands that are fed to an "assembler" which "assembles" the mnemonics and operands into executable machine code. Optionally a "linker" links the assemblies and produces an executable file.

the second "assembly" in "CLR" based languages(.NET languages) is a sequence of CLR code infused with metadata information, sort of a library of executable code, but not directly executable.
```
'Assembly vs Mnemonic(니모닉)'??

## 3.2 Passes
여러 컴파일 디자인 들을 알아볼 예정

컴파일러 디자인에서, 각 단계를 책임지는 모둘로 나누기를 원한다. (실제로 그렇게 하는 경우도 있다.) 컴파일러 디자인은 컴파일러의 모듈성, 시/공간 요구치 프로그램에 대한 pass 수에 영향을 준다. 

pass란? 소스 코드를 완벽하게 훑는 것 혹은 소스 프로그램의 대체 표현(AST)을 훑는 것을 말한다.

one-pass는 한번 훑는 것, multi-pass 는 여러번 훑는 것

이 장에선 one, mulit의 장단점을 비교할 것이다.

### 3.2.1 Multi-pass compilation
기본적인 형태인 S.A., C.A., C.G.를 차례대로 하는 경우 3번의 pass가 발생되므로 멀티이며, 이는 일반적인  디자인이다.

### 3.2.2 One-pass compilation
위 디자인의 대안은 구문 분석기가 다른 컴파일 단계를 컨트롤 하는 디자인이다. 

문맥 분석, 그리고 코드 제너레이션은 구문 분석 하는 동안 즉시 이뤄진다. 구문이 파싱 되자마자, 구문 분석기는 문맥 분석기를 호출하여 확인한다. 그리고 나서 코드 제네레이터로 오브젝트 코드를 생성한다. 그러고 나서 구문 분석은 소스프로그램을 다시 분석한다.

### Compiler design issues
one, multi pass 디자인 중 선택은 제일 먼저 정해져야 하는 중요한 것 이다. 둘다 장단이 있기 때문에 쉬운 선택은 아니다. 주요한 장단들을 아래와 같이 정리 해보자.

* 속도는 one, 여러번 순회
* 공간은 multi, 한번에 하나의 동작만 하기 때문
* 모듈성은 multi
* 유연성 mulit
* optimize를 위해선 multi 필요
* 소스 언어의 특성에 따라 디자인이 강제 될 수 있다.
    java와 같이 정의보다 사용 하는 곳이 먼저 나와도 되는 언어는 multi

