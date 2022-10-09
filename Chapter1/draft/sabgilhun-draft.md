---
layout: default
nav_exclude: true
---

# Introduction

도입 챕터에서는 로우 레벨 랭귀지, 하이 레벨 랭귀지 차이를 살펴보며 시작한다.
그런 다음 '랭귀지 프로세서'가 무엇인지 알아 본다.
그리고 다양한 프로그램 시스템 예시를 본다.
구문, 의미론의 명세를 살펴 본다.
마지막으로 이책 전반에서 사용될 트라이앵글 언어를 알아볼 것 이다.

## 1.1 Levels of programming language

프로그래밍 언어는 모든 프로그래머에게 기본이 되는 도구이다. 언어는 알고리즘을 표현하는 기본적인 표현법이다. 알고리즘 자체는 표기법에 독립적이지만, 표기법 없이는 표현하기 힘들다.

실무 프로그래머는 알고리즘을 표현하는 것에 관심이 있을뿐만 아니라 효율적으로 동작하게 소프트웨어를 구축하는 것에도 관심이 있다. 이런 목적을 위해서 프로그래머는 머신에 프로그램에 입력, 수정, 번역 및 해석할 수 있는 기능이 필요하다. 이런 것들을 할 수 있는 툴들을 프로그래밍 랭귀지 프로세서 라고 말하며, 이것이 이 책의 주제이다.

머신은 기계어로 표현된 프로그램으로 구동된다. 기계어 프로그램은 명령어의 시퀀스로 이뤄져 있고, 각 명령어는 특정 동작을 수행하도록 머신에 의해 해석되어지며, 그냥 bit 열이다. 일반적으로 기계어 명령어는 아래와 같은 기본적인 동작을 한다.

- 336 주소 메모리에서 데이터를 Load
- 레지스터 1, 2가 갖고 있는 수를 Add
- 이전 오퍼레이션 결과가 0이면 13 instruction으로 Jump

컴퓨팅 초기에는 프로그램들을 기계어로 직접 작성하곤 했다. 위 명령어들은 아래와 같이 표현될 수 있다.

- 0000 0001 0110 1110
- 0100 0001 0001 0010
- 1100 0000 0000 1101

작성된 프로그램은 간단하게 머신에 로드되고 동작될 수 있다.

기계어는 명확하기 읽거나 작성하거나 수정하기 힘들다. 프로그래머는 각 항목, 각 명령어의 주소를 유의하며 작성해야하고, 모든 명령어를 기계어로 다시 인코딩해야한다. 작은 프로그램에서도 힘들고 큰 프로그램에선 거의 불가능하다.

프로그래머는 기계어보다 쉽게 코드를 작성하기 위해 심볼릭 표기법을 만들어냈다. 이 표기법에선 위의 명령어들이 아래와 같이 표현 될 수 있다.

- LOAD x
- ADD R1 R2
- JUMPZ h

여기서 LOAD, ADD, JUMPZ 심볼을 동작을, R1, R2는 레지스터, x는 데이터의 주소를 h는 명령어의 주소를 나타낸다. 그 시절의 프로그래머는 이렇게 명령어를 종이에 적고, 수동으로 기계어로 변환을 했다. 이 프로세스를 assembling이라 부른다.

그 후 프로그래머들은 수동으로 aseemble 하던 작업을 기계 자체가 assemble 하는 프로그램을 만들었다. 이 작업을 하기위해 심볼을 표준화할 필요성이 생겼다. symbolic notation은 표준화 되었고 현재 이를 assembly language라 부른다.

assembly language을 사용하여 프로그램을 작성하던 프로그래머들도 여전히 기계어를 염두하여 작업을 하였다. 프로그램은 아주 많은 수의 명령어로 이뤄져 있고. 각 명령어 개별적이고 올바른 순서대로 작성되어져야 한다. 또한 이런 명령어셋을 사용하면, 프로그래머는 알고리즘에서 레지스터, 커서 점프 등, 세부적인 부분에 빠져들게 되는 경향이 생기게 된다. 예를들어 아래와 같이 면적을 구하는 공식을 봐보자.

sqrt(s x (s - a) x (s - b) x (s - c))
where s = (a + b + c) / 2

assembly language로 작성해보면, 프로그램은 개별적인 산술동작 그리고 중간결과가 저장되어 있는 레지스터들로 표현되어진다.

LOAD R1 a; ADD R1 b; ADD R1 c; DIV R1 #2;
LOAD R2 R1;
LOAD R3 R1; SUB R3 a; MULT R2 R3;
LOAD R3 R1; SUB R3 b; MULT R2 R3;
LOAD R3 R1; SUB R3 c; MULT R2 R3;
LOAD RO R2; CALL sqrt

만약 아래처럼 우리에게 친숙한 수학 표기법을 사용했다면, 프로그래밍 훨씬 쉬워질 것 이다.

let s = (a + b + c) / 2
in sqrt(s _ (s - a) _ (s - b) \* (s - c)

오늘날 대부분의 프로그램은 이러한 종류의 프로그래밍 언어로 작성되어진다. 이러한 언어들을 high-level languages 라 불르고 이와 대조 되는 기계어, aassembly languages를 low-level languages라고 부른다. low-level languages는 기계에서 직접 동작될 수 있기 때문에 그렇게 불리며, high-level languages는 우리 머리속 개념들과 더 가깝게 표현할 수 있기 때문에 그렇게 불리고 있다. 다음은 고수전 언어에서는 지원되지만, 저수준 언어에는 기본 형식으로만 지원되거나 아예 지원되지 않는 개념들이다.

- Expressions: expression은 값을 계산하는 규칙이다. 고수준 언어에서는 보통의 수학적 표기법과 비슷하게 작성할 수 있다. (+, -, \*)
- Data types: 프로그램은 많은 데이터 타입을 생성해 내는데, primitive 타입(boolean, characters, integers), composite 타입(arrays, records) 등등이 있다. 그리고 고수준 언어에서는 프로그래머가 직접 타입의 정의할수도 있다.
- Control structures: Control structures은 고수준 언어 프로그래머에게 선택적 혹은 반복적 계산을 가능하게 한다.
- Declarations: Declarations은 고수준 언어 프로그래머에게 식별자를 도입하여 함수, 상수, 변수 타입과 같은 엔티티들을 나타낼수 있게 한다.
- Encapsulation: 패지키, 클래스를 통해 프로그래머가 Declarations을 선택적으로 노출할 수 있게 한다.

## 1.2 Programming language processors

프로그래밍 랭귀지 프로세서는 특정 언어로 표현된 프로그램을 만들어내는 시스템이다. 랭귀지 프로세서의 도움을 통해 우리는 프로그램을 동작시키거나 실행준비 시킬 수 있다.

이러한 랭귀지 프로세서의 정의는 일반적인 것이다. 좀더 구체적으로 아래와 같은 시스템들이 랭귀지 프로세서에 포함된다.

- Editors: 에디터는 프로그램 텍스트를 입력, 수정, 저장 할 수 있게한다. 일반 텍스트 에디터도 포함되며 프로그래밍을 작성하기 쉽게 맞춤화된 에디터들도 있다.
- Translators, Compilers: Translators는 어떤 언어로 작성된 것을 다른 언어로 변환하는 시스템을 말한다. 여기서 특히 컴파일러는 고수준 언어를 저수준 언어로 변환하는 것을 말한다. 이러한 동작을 하기 앞서 컴파일러는 프로그램의 구문 및 문맥 오류를 검사한다.
- Interpreters: Interpreters는 특정 언어로 작성된 프로그램을 즉시 실행시킨다. 이러한 실행방식은 컴파일과정을 생략하고 바로 응답하기 때문에, Command Language, DataBase 에서 많이 사용된다.

실제로 우리는 위와 같은 랭귀지 프로세스를 모두 사용하고 있다. 예전에는 이러한 시스템들이 모두 분리되어 있었는데, 요즘에는 통합하여 하나의 시스템에서 위의 기능들을 제공한다. 다음 예시는 이러한 2가지 접근 방식을 비교한다.

### Example 1.1 Language processors as software tools

case1:
유닉스 유저인 Java를 이용해 체스게임을 개발하는 개발자.

개발자는 editor를 실행하고 Chess.java 파일을 만든다.
'vi Chess.java'

그리고 Java Compiler를 실행한다.
'javac Chess.java'
(이때 object code로 변환되고 Chess.class 파일로 저장된다.)

그러고 나서 프로그램을 실행한다.
'java Chess'

만약 컴파일을 실패하거나 프로그램이 잘못 동작하면 개발자는 다시 위의 동작을 반복한다. (edit -> compile - run)

이 방식엔 랭귀지 프로세서간 커뮤니케이션이 없다. 컴파일러가 에러를 내면 그것들을 기록하고 다시 에디터로 가서 찾아야 한다. 이는 비효율적이다.

소프트웨어 툴 철학의 근본은 소수의 공통적인 그리고 간단한 툴을 제공하고 그툴을 조합하여 여러가지 작업을 할 수 있게하는 것이다.

위의 철학은 순수한 형태이며, 실제론 절충안이다. 에디터는 컴파일을 해주고, 컴파일이 실패하면 에디터를 자동으로 실패한 곳으로 위치시킨다.

### Example 1.2 Integrated languageprocessor

Borland JBuilder는 자바를 위한 전체 통홥된 랭귀지 프로세서이다. 에디터, 컴파일러 다른 기능을 포함하고 있다. 유저는 프로그램을 열거나 수정, 컴파일, 실행하는 명령을 내릴 수 있다. 이런 명령들은 메뉴에서 선택하여 쉽게 가능하다.

이 에디터는 자바에 특화되어 있다. 들여쓰기, 자바 키워드 리터럴 하이라이팅 등을 지원한다.

컴파일러는 에디터랑 통합된다. 컴파일 에러가 발생되면 문제가 있는 구문을 포커싱하고 개발자는 쉽게 그곳을 수정할 수 있다. 만약 프로그램이 여러 에러를 포함하면 컴파일러는 리스트로 노출하며 에러 메시지를 선택하여 볼 수 있는 기능을 제공한다.

The object program??

## 1.3 Specification of programming languages

언어 디자이너, 랭귀지 프로세서 개발자, 일반 프로그래머 각 그룹의 사람들은 언어에 대한 일반적인 이해에 의존한다. 이를 위해 서로 동의된 언어 명세를 참조해야한다.

- Syntax는 프로그램의 형태와 관련 있다. 언어에서 Syntax는 프로그램에서 어떤 토큰이 사용되는지, 어떤 구문이 토큰과 하위구문의 조합으로 구성되는지에 대한 방법을 정의한다.
- Context constraints 아래와 같은 규칙들을 말한다. Scope rules, 범위 규칙은 각 선언의 범위를 결정하고 그 범위내에서 각 식별자의 선언을 찾을 수 있도록하는 것이다. Type rules, 타입 규칙은 각 표현식의 타입을 유추하고 각 연산에 올바른 피연산자가 제공되는지 확인할 수 있도록 하는 것이다. 이러한 규칙을 Context constraints(문맥 제한)으로 불리는 이유는 이러한 규칙들이 잘 작성되었는지는 문맥에서 알 수 있기 때문이다.
- Semantics은 프로그램의 의미와 관련 있다. 의미론의 지정하는 방식에는 여러가지가 있다. 그 중 하나는 프로그램의 입력을 출력에 매핑하는 수학 함수로 의미론을 지정하는 방식이다. (이는 denotational 의미론의 기초이다.) 또 하나는 프로그램이 기계에서 실행될 때의 행동으로 프로그램의 의미를 취할 수 있다. (이는 perational 의미론의 기초이다.) 이 책의 경우 언어 처리기에 대한 내용이기 때문에 후자의 의미론 관점을 선호한다.

프로그래밍 언어 명세를 정할때, formal specification, informal specification 사양중에 선택할 수 있다.

- informal specification은 영어 혹은 다른 자연어들로 쓰여진 명세를 말한다. 명세가 잘 작성되면, 프로그래밍 언어의 모든 사용자가 쉽게 이해할 수 있지만 실제론 힘들며, 제대로 잘못해석될 여지가 많다.

- formal specification은 정확한 명세로 작성된 사양을 말한다. 이 경우 모호하지 않고 일관성과 완경성이 있기 때문에 잘못 해석될 여지가 적다. 하지만 이 경우 공식 명세를 알고 있는 사람만 이해할 수 있다.

실제 프로그래밍 언어에서는 위의 2개를 섞어서 사용한다. Syntax는 BNF 혹은 그 변형을 이용한 formal한 명세를 사용한다. 왜냐하면 BNF(or 변형) 표기법은 쉽고 많이 사용되기 때문이다. contextual constraints과 semantics은 informal 명세를 사용한다. 이들의 formal 명세가 좀 더 어렵고, 가능한 표기법이 아직 널리 사용되고 있지 않기 때문이다.

### 1.3.1 Syntax

Syntax는 프로그램 형태와 관련있다. 우리는 context-free grammar을 통해 formal한 명세를 지정할 수 있다. 이는 아래의 요소들로 구성된다.

- terminal symbols: 실제 프로그래밍 언어를 구성하는 원자적 심볼들을 말한다. 'while', '>=', ';' 과 같은 키워드들을 말한다.
- nonterminal symbols: 언어에서 특정 구문 종류를 말한다. 'Program', 'Command', 'Expressions', 'Declaration' 등을 말한다.
- start symbol: 이는 nonterminals의 하나이며, 이는 프로그램에서 주요 구문 종류이다.
- production rules: 구문이 어떠한 terminals 와 하위 구문으로 구성되는지 정의한 것을 말한다.

문법은 보통 BNF로 작성된다. BNF로 production rules을 표현하면 아래와 같다.

N ::= a
N ::= b

여기서 N은 nonterminals이며 a, b는 terminal의 연속 혹은 nonterminals이다.

::= 은 왼쪽이 오른쪽으로 구성되는 것을 의미하고, | 은 대체될 수 있음을 말한다.

### Example 1.3 Mini-Triangle syntax

Mini-Triangle은 학습용 toy 프로그래밍 언어이다.

! 여긴 주석
let
const m ~ 7;
var n: Integer
in
begin
n := 2 _ m _ m;
putint(n)
end

(프로덕션 룰의 통해 문법 맞는지 확인)

10p. Syntax Tree, phrase, sentance 설명... 이해 안감

### Example 1.4 Mini-Triangle syntax trees

d + 10 _ n은 프로덕션 룰을 통해 sytax trees를 그려보면 해당 구문이 Expression nonterminal symbol인걸 알 수 있다. 그리고 Mini-Triangle에서 모든 이진 연산이 동일 우선순위를 갖기 때문에 (d + 10) _ n 으로 계산 된다는 것도 알 수 있다.

그외 다른 문구도 syntax trees 그려 분석하면 어떤 구문인지 분석 할 수 있다.

예제 1.3과 같은 문법에는 2가지 역할이 있다.

- 구문의 형태와 어떤 것이 하위 구문인지 나태낸다. 예시로 V-name := Expression 구문에서 V-name, Expression 각각 하위 구문이 된다. 이렇게 프로그램이 구문과 하위 구문으로 이뤄지는 방식을 '구문구조'라 부른다.
- 그리고 구문간 순서와 구분자를 나타낸다. 예시로 V-name := Expression 에서 Expression 은 V-name := 이후에만 입력 될 수 있고, V-name, Expression 사이에는 := 구분자가 필요하다.

(nonterminal symbol은 구문으로 보면 되는 걸까?)

11p
(concrete syntax 에 대한 설명 이해 못함)

concrete syntax은 의미론적으론 영향이 없다. V := E, V assign E 등등 여러 문법으로 쓰여지더라도 해당 구문이 실행되면 동일한 의미일 것이기 때문이다.
이러한 컨셉은 처음에 구문구조를 잡고 추후에 의미를 정하는 방식으로 각 단계에 집중할 수 있게 한다. 이것을 AST라 부른다. AST는 terminal, non-terminal 노드들로 이뤄지고 또 non-terminal 하나의 subtree를 갖는 형태로 구성된다. terminal는 의미를 갖지 않는 토큰으로 지정된다.

### Example 1.5 Mini-Triangle abstract syntax

AST를 지정해보자. 각 구문의 의미는 신경 쓰지 않는다.

... 예시들

프로그램의 AST는 구문 구조를 명시적으로 나타내며, AST는 문젝 제약 및 의미를 지정하기 편리한 구조이다. 이러한 편리함은 컴파일러에도 동일하다.

### 1.3.2 Contextual constraints

Contextual constraints은 스코프룰 or 타입룰 같은 것이다. 이것들이 잘 구성되어 있는지 아닌지는 문맥에서 알 수 있다.
모든 프로그래밍 언어는 식별자 선언을 허용하고 사용할 수 있다.

선언을 위해 식별자 발생하는 경우 => binding occurrence
사용을 위해 식별자 발생하는 경우 => applied occurrence

위의 binding occurrence, applied occurrence에 대한 언어에서의 규칙을 scope rule 이라 한다.

컴파일 타임에 결정된다면, static binding
런 타임에 결정된다면, dynamic binding

### Example 1.6 Triangle scope rules
