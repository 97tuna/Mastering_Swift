# 05. Operator

- Operator Basics

    ```swift
    // 피연산자 개수에 따라 분류

    + a // Unary Operator
    a + b // Binary Operator
    a ? b : c // Ternary Operator
    ```

    ```swift
    +a 🅾️, + a ❎
    a + b 🅾️, a+b 🅾️

    // 연산자 양쪽에 공백을 추가하는것을 추천, 동일하게 공백줘야지 굳
    a+ b ❎, a +b ❎ // 이항 연산자일경우 양쪽에!
    ```

    ```swift
    +a // Prefix Operator, 전치 연산자
    a+ // Posfix Operator, 후치 연산자
    a + b // Infux Operator, 피연산자 사이에 있음
    ```

    Precedence

    > a + (b * c)

    ```swift
    ((a + b) * c) - d) * e
    a + b + c // 연산자 우선순위 동일, 왼쪽에서 오른쪽 방향으로 진행
    ```

    **Associativity, 연산자 우선순위**

    - → Left Associativity
    - ← Right Associativity

    괄호만 있으면, 연산자 우선순위를 고려하지 않아도 됨.

- Arithmetic Operators
    - Unary, Subtraction, Addition Operator

        ```swift
        let a = 12
        let b = 34
        +a, +b
        // a = 12, b = 34
        // 값을 다시 돌려줌

        a + b // 46

        -a, -b // 단항은 부호를 바꿔줌
        // a = -12, b = -34

        a - b // -22
        ```

    - Multiplication, Division, Remainder Operator

        ```swift
        a * b // 408

        a / b // = 0
        b / a // = 12

        let c = Double(a)
        let d = Double(b)

        c / d // = 0.359~~
        d / c // 2.83333~~

        a % b // 12, 나머지 연산자
        c % d // 에러 → 정수만 지원하기 때문에 실수 나누기는 불가, truncatingRemainder사용
        c.truncatingRemainder(dividingBy: d) // 성공
        ```

    - Overflow

        ```swift
        let num: Int8 = 10 * 10 // 가능
        let num: Int8 = 10 * 10 * 10 // Overflow
        // Swift는 Overflow를 허용하지 않습니다
        ```

- Overflow Operators

    ```swift
    Int8.min // -128
    Int8.max // 127

    let num: Int8 = Int8.max // 성공
    num + 1 // Overflow, 127 + 1은 불가
    ```

    - Overflow Addition Operator

        ```swift
        a &+ b // Overflow 허용

        let a: Int8 = Int8.max

        let b: Int8 = a + 1 // 오류
        let b: Int8 = a &+ 1 // 성공m, -128출력

        // -128이면 Int8에 저장할 수 있는 가장 작은 값.
        ```

    - Overflow Multiplication Operator

        ```swift
        let e: Int8 = Int8.max &* 2 // -2
        ```

- Comparison Operators

    **비교 연산자의 return 값은 Boolean값**

    ```swift
    a == b // Equel to 연산자

    "swift" == "swift" // True
    "swift" == "Swift" // False

    // 피연산자끼리 자료형을 일치시켜야함.

    let c = 123.3

    a == c // False, 정수 실수 비교 X
    ```

    ```swift
    a != b // 반대!
    ```

    ```swift
    a > b // a가 b보다 큰지? 확인
    "swift" > "Swift" // 문자에 할당된 아스키 또는 유니코드로 확인, 소문자 s가 더 큼
    ```

    ```swift
    a >= b // 같거나 클때!
    7 >= 7 // True
    ```

    ```swift
    a <= b // 같거나 작을 때!
    ```

- Logical Operators
    - Logical NOT Operators

        ```swift
        !a // 참은 거짓으로, 거짓은 참으로

        let a = 13
        let b = 33

        a < b // True
        !(a < b) // False
        ```

    - Logical AND Operators

        ```swift
        a && b // a도 Boolean, b도 Boolean표현식이어야 함.

        a > 30 && b % 2 = 0 // False

        true && true // True
        false && true // false
        true && false // false
        false && false // false
        ```

    - Logical OR Operators

        ```swift
        a || b // a도 Boolean, b도 Boolean표현식이어야 함.

        a > 30 && b % 2 = 0 // True

        true && true // True
        false && true // True
        true && false // True
        false && false // false
        ```

- Ternary Conditional Operator

    ```swift
    Condition ? true : false
    // Condition은 Boolean표현식

    let hour = 10

    hour < 12 ? "AM" : "PM" // AM 출력

    if hour < 12 {
        "AM"
    } else {
        "PM"
    }
    // 과 동일

    // hour < 11 "Morning"
    // hour < 17 "Afternoon"
    // "Evening"

    hour < 11 ? "Morning" : hour < 17 ? "Afternoon" : "Evening"
    ```

- Short-circuit Evaluation, Side Effect 🥕
    - Short-circuit Evaluation

        ```swift
        false && ~ // 첫번째 연산이 무조건 false면 뒤를 더 보지 않고 false return
        true || // 위와 동일, 이미 true라 검사하지 않고 true return

        var a = 4
        var b = 4

        func leftUpdate() -> Bool {
            a += 1
            return true
        }

        func rightUpdate() -> Bool {
            b += 1
            return true
        }

        if leftUpdate() || rightUpdate() {
            // leftUpdate가 먼저 실행 -> true return됨
            // 전체 결과를 true로 인식하고, rightUpdate는 실행하지 않음
        }
        ```

        a = 5, b = 4로 출력 → 단락평가 → 이미 결과를 얻으면 다음을 실행하지 않음

    - Side Effect

        프로그래밍에서는 반드시 필요, 고려하고 코드를 작성해야 함.
            → 표현식 값, 상태 변경을 뜻함

        출력은 Side Effect 아님

        a = 5는 Side Effect

        ```swift
        let resA = leftUpdate()
        let resB = rightUpdate()

        if resA || resB {
            // leftUpdate()와 rightUpdate()는 항상 실행
            // Side Effect가 일어나지 않음
            // Side Effect가 일어나게 되면 논리적으로 오류가 있을 수 있으므로 코드 작성시 피해주어야 함.
        }
        ```

- Bitwise Operators 🥕

    **연산속도가 매우 빠름**

    1. 하드웨어 다룰 때
    2. 그래픽을 처리할 때
    3. 암호화 코드를 수행할 때
    - Bitwise NOT Operators

        ```swift
        ~a // 1은 0으로, 0은 1로 변경

        let a: Int8 = 0b0000_0010 // 2
        ~a // 253 출력
        ```

    - Bitwise AND Operators

        ```swift
        a & b

        00001000
        00001000
        --------
        00001000

        // 비교하는 비트가 둘다 1이어야지 1 return
        ```

    - Bitwise OR Operators

        ```swift
        a | b

        00001000
        00001010
        --------
        00001010

        // 비교하는 비트가 어느거라도 1이어야지 1 return
        ```

    - Bitwise XOR Operators

        ```swift
        a | b

        00001000
        00001010
        --------
        11111101

        // 비교하는 비트가 달라야지 1 return
        ```

    - Bitwise Left Shift Operators

        ```swift
        a << n

        a << 1일 경우
        00001000 -> 00010000 // 왼쪽으로 옮기고 새로운 0비트가 생성
        // 값이 2^n배가 됨
        ```

    - Bitwise Right Shift Operators

        ```swift
        a >> n

        a >> 1일 경우
        00001000 -> 00000100 // 왼쪽으로 옮기고 새로운 0비트가 생성
        // 값이 (1/2)^n배가 됨
        ```

    - Arithmetic Shift

        ```swift
        Signed Shift // 논리가 아닌 산술 Shift

        새로운비트가 0이 아닌 기존의 SignBit가 채워짐
        ```

- Assignment Operators

    ```swift
    var a = 1000 // 할당 연산자, 값을 저장하는 역할을 함 -> =
    var b = 1
    a = b

    if a == 1 {
        // a와 1을 비교
    }

    if a = 1 {
        // 에러, 할당 연산자는 값을 return하지 않음
    }

    LeftValue = RightValue
    ```

    - Compound Assignment Opeartor
        - Additional Assignment Operator

            ```swift
            a += b
            a = a + b

            var a = 5
            var b = 10
            a = a + b // a = 15
            a += b // a = 25

            a -= b
            a *= b
            a /= b
            a %= b
            // 등등 다양하게 사용 가능
            ```

- Range Operators

    범위를 나타낼때는 대부분 ~기호를 사용함

    그러나 Swift에서는 그렇게 사용하지 않음

    - Closed Range Operator

        ```swift
        a ... b
        a...
        ...a

        1 ... 10 // 1에서 10까지의 범위
        Upperbound가 10에 포함됨
        // 1, 2, 3, 4, 5, 6, 7, 8, 9, 10

        10 ... 1 // 에러, Upperbound가 lowerbound보다 작을 수 없음, 오름차순만 가능
        (1 ... 10).reversed() // 로 사용

        12.12 ... 34.34 // 가능

        var sum = 0
        for num in 1 ... 10 {
            sum += num
        }
        // sum은 55임

        let list = ["A", "B", "C", "D", "E", "F"]
        list[2...] // C부터 마지막까지 나옴, C, D, E
        list[...2] // 처음부터 2 index까지 나옴, A, B, C
        ```

    - Half-Open Range Operator

        ```swift
        a ..< b // Upperbound가 포함되지 않음

        list[..<2] // 처음부터 1 index까지 나옴, A, B

        let numarr = 0 ... 5
        numarr.contains(7) // False
        numarr.contains(1) // True
        ```

- Operator Methods 🥕

    ```swift
    "a" == "a" // True

    struct Point {
        var x = 0.0
        var y = 0.0
    }

    let p1 = point(x: 12, y: 12)
    let p2 = point(x: 34, y: 34)
    p1 == p2 // 에러, 모름
    ```

    구현시 주의해야할 점

    1. 원래 기능과 동일하도록 구현
    2. 이미 존재하는 연산자의 파라미터와 리턴형을 일치 시켜야함

    ```swift
    extension point: Equatable {
        static func ==(lhs: point, rhs: point) -> Bool {
            return (lhs.x == rhs.x) && (lhs.y == rhs.y)
        }
    }

    p1 == p2 // False
    p1 != 2 // True

    extension Point {
        static prefix func -(pt: point) -> point {
            return point(x: pt.x, y: pt.y)
        }
    }

    let p3 = -p1
    p3.x // -12
    p3.y // -34

    extension Point {
        static prefix func ++(pt: inout point) -> point {
            let ret = pt
            pt.x += 1
            pt.y += 1
            return ret
        }
    }

    var p4 = point(x: 1.0, y: 2.0)
    let p5 = p4++
    p5.x // 1
    p5.y // 2

    p4.x // 2
    p4.y // 3
    ```

    **Equatable Protocol이 채용되어 있으면 구현하지 않아도 알아서 컴파일러가 처리해줌!**

- Custom Operators 🥕
    - Reverse Tokens

        ```swift
        (, ), {, }, [, ], /, ,, ;, :, =, #, &(prefix perator), ->, `, ?, !(postfix opetator)
        // 단독으로 사용불가, 다른문자와 조합하여 사용
        ```

    - First Character

        ```swift
        /, =, -, +, !, *, %, <, >, &, |, ^, ?, ~
        // 여기에 나온 문자만 조합해서 사용 권장, 연산자는 가능한 단순하게
        // 기존의 연산자와 다르게 모호하게 사용 X
        ```

    ```swift
    prefix operator +++

    extension Int {
        static prefix func +++(num: inout Int) {
            num += 2
        }
    }

    var a = 1
    +++a
    a // 3

    precedencegroup Myprecedence {
        higherThan: AdditionPrecedence
    }

    infix operator *+*: Myprecedence // 더하기 우선순위보다 높아짐

    extension Int {
        static prefix func *+*(left: Int, right: Int) {
            return (left * right) + (left * right)
        }
    }

    1 *+* 2 // 4
    1 *+* 2 + 3 // 에러, 우선순위 그룹에 의해 에러
    ```

    ```swift
    infix operator *+*: MultiplicationPrecedence
    // 곱하기 우선순위와 같은 우선순위로 설정

    extension Int {
        static prefix func *+*(left: Int, right: Int) {
            return (left * right) + (left * right)
        }
    }

    1 *+* 2 + 3 // 7
    ```

    ```swift
    // UpperCamelCase로 이름 설정이 관례
    precedencegroup Name {
        higherThan: LowerGroupName // 현재 그룹보다 우선순위가 낮은것을 지정
        lowerThan: HigherGroupName // 현재 그룹보다 우선순위가 높은것을 지정
        associativity: associativity // 연산자의 결합규칙을 지정, left, right, none중 생략시 none기본 지정
    }

    // 3가지 모두 생략가능, higherThan, lowerThan둘 중 하나는 구현해야 함.
    ```