# Warming up

- Tokens

    언어를 구성하는 요소 중 **가장 작은 단위**, 공백이나 구두점으로 분리할 수 없는 것을 말함

    ```swift
    2 + 3; // Token은 4개
    2[ ]+[ ]3[ ]; // [공백]을 두어도 의미가 변하지 않는다

    if // 올바른 Token
    i[ ]f // 로 띄워쓰기 한다면 올바르지 않은 Token, 의미가 달라지기 때문에

    [ ] // 공백은 말그대로 빈칸, Token을 구분하는 역할, 연산자와 피연산자를 처리하는 방식을 결정하기도 함
    // 탭도 마찬가지로 공백으로 인식, 연속적인 공백은 하나의 공백으로 처리
    ```

- Expression

    하나의 값으로 표현되는 코드, **표현식**이라고 부름

- Evaluate

    표현식을 통해 하나의 **결과값을 도출해 내는 것**을 평가한다고 표현함.

    코드를 실행해서 값을 얻는 행위라고 한다

    ```swift
    let num = 10
    num // 가장 간단한 표현 식, 평가하면 7이 됨.

    num + 1 // 3개의 Token이 모여서 하나의 표현식을 나타냄, 평가시 값은 8
    // 산술 표현식

    num < 10 // 평가하면 거짓, false
    // 논리 표현식, Boolean Expressions
    ```

- Statements

    하나이상 표현식이 모이면 **문장 또는 구문**, 즉 문이라고 부른다.

- Literals

    ```swift
    let num = 7 // 7이 숫자 Literal
    let num = 6 + 7 // 6, 7이 숫자 Literal, +는 연산자
    let num2 = num > 7 // 숫자 Literal은 7 하나이다.
    ```

    이름에 숫자가 들어있는것은 숫자 Literal이 **아님**⚠️

    위치에 따라 달라진다

    `Integer`, `Floating-points`, `String`, `Boolean`, `nil`등 다양한 Literal이 존재

- Identifiers

    코드에서 이름으로 사용되는것은 모두 식별자, 즉 Identifier다

    ```swift
    let num = 7 // num이 식별자이다.
    let Num = 7 // 위의 num과 다른 식별자이다. swift는 엄격한 대소문자 구분
    let _Num = 7
    let 1num = 7 // 숫자가 맨 처음에 들어가는것은 불가!
    ```

    귀찮더라도 **두개이상의 단어**를 조합하여 식별자 만드는것을 권장⚠️

- Keywords

    프로그래밍 기능을 위한 예약되있는 단어를 뜻함

    ```swift
    let num = 7 // let이 바로 Keyword이다. 상수를 선언하는 키워드
    var num = 7 // 상수가 아니라 변수로 변경. var는 변수를 선언하는 키워드

    let let = 7 // 식별자를 상수의 이름으로 사용불가, 사용할 수 있지만 굳이?
    ```

- Complie

    코드를 이진코드로 변경하는 과정을 컴파일이라고 함.

    Xcode 컴파일러는 소스코드를 분석할때

    - Warning: ⚠️, 노란색 삼각형으로 알려줌.
    - Error: 🔴, 빨간색 원으로 알려줌.

    로 구분하여 알려줌.

- Link

    코드, 프레임워크들을 연결하는 과정을 Link라고 함.

    `Link`를 담당하는 친구는 `Linker`가 대신 해줌 😂

    위의 절차를 완료하면 실행파일이 생기는데 이런일을 **Build라고 한다**

- Run
    - Debug Mode

    > 앱을 만들때 사용

    - Release Mode

    > 앱스토어에 올릴 때 사용

    `Compile`단계와 `Runtime`단계가 나누어져 있으므로 잘 알아야할 것.

    예를들어 `is`연산자의 경우 `Runtime`단계에서 확인하기 때문에 실행해야지 알 수 있다

- Special Characters
    - `!` Exclamation Mark, 논리 부정, 옵셔널에서는 값을 강제로 꺼낼 때 사용
    - `~` 비트 연산에서 사용
    - `~` 밑에있는 `는 Back Tick이라고 부름. Keyword를 Identifier로 바꿀때 사용
    - `@` At Symbol: 코드 자체의 특성을 지정할 때
    - `#` Hashtag: Swift가 지정한 특별한 명령어를 사용할 때
    - `$` Dollar Sign: Closure에서 파라미터 이름을 대체할 때 사용
    - `%` Percent Sign: 나머지를 구할 때 사용
    - `^` Caret: 비트 연산에서 사용
    - `&` Ampersand: 메모리 주소를 얻거나 참조를 전달할 때 사용
    - `*` Asterisk: 곱하기 연산할 때 사용
    - `()` Parentheses: 함수 호출, 계산 순서 지정할때도 사용
    - `-` Hyphen
    - `_` Underscore: Wildcard 패턴 지정할 때 사용
    - `+` Plus Sign: 덧셈할 때 사용
    - `=` Equal Sign: 변수를 저장할때 사용, `==` 값을 비교할 때 사용
    - `[]` Square Bracket: Collection에 저장된 값에 접근할 때 사용
    - `{}` Curly Bracket, Brace: Code Block에 범위를 지정할 때 사용
    - `\` Backslash: Keypath 문법에 사용
    - `|` Vertical Bar, Pipe: 논리 연산, 비트연산에서 사용
    - `;` Semicolon: 문장의 끝을 파악할 ㄸ
    - `:` Colon: 자료형 지정, 딕셔너리 키와 값 구분할 때 사용
    - `,` Comma: 함수 값, 배열 값 나열
    - `.` Period: 메소드 호출, 속성에 접근
    - `<>` Angle Bracket: 크기 비교, 형식 파라미터 지정할 때 사용
    - `/` Slash: 경로를 지정할 때 사용
    - `?` Question Mark: 옵셔널에서 주로 사용
- First Class Citizen

    3가지 특징이 있다!!!

    - 상수와 변수에 저장 가능
    - 파라미터로 전달 가능
    - 함수에서 리턴 가능

    위의 **3가지만** 알고있다면 충분