# 09. Optionals

- Optionals

    값이 없다! 라고 생각

    ```swift
    let num = 100 // 100을 저장하지 않으면 오류!
    // 형식 추론으로 인해서 값이 없으면 오류

    let num: Int
    print(num) // 컴파일 에러, 변수와 상수는 값을 읽기전에 초기화 해야한다!
    ```

    **저장할 값이 없으면 어떻게 해야할까?**

    ```swift
    let num: Int = 0 // 계속 0으로 값이 없음을 표시하자!
    // 전혀 해결 방법이 아님

    let optionalNumber: Int? = nil // nil = 값이 없다!
    let optionalNumbe = nil // 오류, 타입 추론이 불가하기 때문

    le str: String = "SWIFT" // non-optional -> 항상 값을 가져야 함
    ```

    ```swift
    TypeName? // 옵셔널 사용

    let optionalString: String? = nil // 옵셔널 스티링이라고 읽음.
    // 값을 저장하지 않아도 되는 형식

    let num: Int? = nil // 옵셔널 인트
    let num2 = num // num2의 자료형은 Int?임.
    ```

    - Unwrapping

        ```swift
        var num: Int? = nil
        print(num) // nil

        num = 123
        print(num) // Optional(123)

        let number = 123
        print(number) // 123

        // 둘이 다름!!
        ```

        값이 wrap되어있다! → 값을 추출해야함.

        - Forced Unwrapping

            ```swift
            OptionalExpression!
            ```

            ```swift
            print(num) // Optional(123)
            print(num!) // 123

            // 포장된걸 꺼내서 사용!
            ```

            ```swift
            num = nil
            print(num!) // 에러, nil은 다시 꺼낼 수 없다
            // 값이 없는데 꺼내려니까 문제!
            ```

            값이 저장되있는지 확인하고 꺼내야함! → nil을 꺼내게 되면 앱이 Crash됨

            ```swift
            num = 100
            let Before = num
            let After = num! // Optional이 제거된 그냥 Int이다.
            ```

- Optionals Binding

    ```swift
    var num = Int? = nil // Optional -> 값을 저장할 수 도 안할수도
    print(num!) // 강제추출, 값이 없는데 강제추출은 오류!
    ```

    OptionalExpression을 평가하고 값이 있으면 Unwrapping되면서 저장

    - **if let**

        ```swift
        if let name: Type = OptionalExpression {
        // 바인딩이라고 부름, nil 이면 바인딩 실패
            Statements
        }
        ```

        ```swift
        if num != nil { // Condition이옴
            print(n!)
        } else {
            print("EMPTY")
        }

        if let num = num { // Binding이 옴 Unwrappinge되어 저장.
            print(num) // 강제 추출이 없음, 같은 이름써도 문제 없음. Scope에 대한 것.
        } else {
            print("EMPTY")
        }
        ```

    - **while let**

        ```swift
        while let name: Type = OptionalExpression {
            Statements
        }
        ```

    - **guard let**

        ```swift
        guard let name: Type = OptionalExpression else {
            Statements
        }

        var str: String? = "str"
        guard let str = str else { // str바꾸고 싶다면 let이 아닌 var 사용
            str // 오류, 인식 불가
            fatalerorr()
        }
        print(str) // str
        ```

- Implicitly Unwrapped Optionals

    암시적, 자동으로 추출된 옵셔널

    ```swift
    Type! // 자료형 + !

    let num: Int! = 1000
    let num1 = num // num1의 자료형은 Int? 이다, 자동으로 추출되지 않음

    let num2: Int = num // num2는 Int이다.
    // 다른 강의에서는 애매한 놈이라고 했다 ㅋㅋㅋㅋ

    let number: Int! = nil
    let number1: Int = number // 오류!
    ```

    ```swift
    var value: Int = 3 
    var valueToBeSet: Int! = 4
    var valueCanBeNil: Int? = 5

    value = nil // 에러!
    valueToBeSet = nil // 가능
    valueCanBeNil = nil // 가능

    value = valueToBeSet // 가능
    value = valueCanBeNil // 불가능
    value = valueCanBeNil! // 가능
    ```

    outlet을 처리할 때, API에서 IUO를 받을 때 그냥 optional로 처리하면 좋다!

- Nil-Coalescing Operator

    ```swift
    var str = ""
    var input: String? = "SWIFT"

    if let inputName = input {
        str = "HELLO, " + inputName
    } else {
        str = "HELLO, ANONYMOUS"
    }

    print(str) // HELLO, SWIFT
    ```

    ```swift
    var str = "HELLO, " + (input != nil ? input! " "ANONYMOUS")
    print(str) // HELLO, SWIFT

    // 코드가 단순해짐
    ```

    Nil-Coalescing을 사용하면 값 확인 및 추출하는 작업 안해도 된다!!!

    ```swift
    a ?? b
    OptionalExpression ?? Expression

    // a -> 옵셔널 스트링
    // b -> 스트링이어야 함.
    ```

    ```swift
    var str = "HELLO, " + (input ?? "ANONYMOUS")
    print(str) // HELLO, SWIFT
    // 코드가 더 단순해짐

    // input값을 return하는지 확인 -> Unwrapping함.
    // input이 없으면 오른쪽 피연산자를 평가하고 return
    // input이 nil 이면 HELLO, ANONYMOUS 출력
    ```

    단락평가 실행! → Side Effect주의

    위의 예에서 input가 nil이 아니면 뒤의 표현식은 평가 안함

- Optionals Chaining 🥕

    옵셔널을 연달아서 출력하기

    1. Optional Chaining의 결과는 항상 Optional이다.
    2. Optionals Chaining에 포함된 표현식 중 하나라도 nil이면 이어지는 표현식 평가 ❌ → nil Return

    ```swift
    struct Information {
        var schoolName: [String: String]
        var location: String
    }

    struct Person {
        var name: String
        var school: Information
        
        init(name: String, schoolName: String) {
            self.name = name
            school = Information(schoolName: ["school": schoolName], location: "Seoul")
        }
    }

    var p = Person(name: "Lee", schoolName: "Seoul")
    let a = p.school.location // p.Information.location 옵셔널 표현식 없음

    var opionalP: Person? = Person(name: "Lee", schoolName: "Seoul")
    let b = opionalP.school.location // 에러
    // 반드시 optionalP instance를 unwrapping해야한다. !를 사용해서도 가능 (nil이면 Crash)

    let b = opionalP?.school.location
    // opionalP가 nil이면 종료 아니면 Information에 접근

    opionalP = nil
    let c = opionalP?.school.location // nil, String? Type임
    // 옵셔널과 아닌것이 같이 공존 가능
    // adress는 String임, opionalP는 Optional이지만 c는 non-optional type임
    ```

    **b와 c는 non-optional Type이다. (중요)**

    ```swift
    struct Information {
        var schoolName: [String: String]
        var location: String?
    }

    struct Person {
        var name: String
        var school: Information?
        
        init(name: String, schoolName: String) {
            self.name = name
            school = Information(schoolName: ["school": schoolName], location: "Seoul")
        }
    }

    var p = Person(name: "Lee", schoolName: "Seoul")
    let a = p.school?.location

    var opionalP: Person? = Person(name: "Lee", schoolName: "Seoul")
    let b = opionalP.school.location // 에러

    let b = opionalP?.school?.location

    opionalP = nil
    let c = opionalP?.school?.location // nil, String? Type임
    ```

    ```swift
    struct Information {
        var schoolName: [String: String]
        var location: String
    }

    struct Person {
        var name: String
        var school: Information
        
        init(name: String, schoolName: String) {
            self.name = name
            school = Information(schoolName: ["school": schoolName], location: "Seoul")
        }
        func getSchool() -> Information? {
            return school
        }
    }

    var p = Person(name: "Lee", schoolName: "Seoul")
    p.getSchool()?.location // 메소드나 함수가 optional일 수 있음

    let f: (() -> Information?)? = p.getSchool
    f?()?.location // 나중의 function에서 다시 배울 것.
    ```

    ```swift
    struct Information {
        var schoolName: [String: String]?
        var location: String?
        
        func printLocation() {
            return print(location ?? "No address")
        }
    }

    struct Person {
        var name: String
        var school: Information?
        
        init(name: String, schoolName: String) {
            self.name = name
            school = Information(schoolName: ["school": schoolName], location: "Seoul")
        }
        func getSchool() -> Information? {
            return school
        }
    }

    var p = Person(name: "Lee", schoolName: "Seoul")
    p.getSchool()?.location // 메소드나 함수가 optional일 수 있음

    let d = p.getSchool()?.printLocation()
    // ()? -> optional Void

    if p.getSchool()?.printLocation() != nil {
        
    }

    if let _ = p.getSchool()?.printLocation() {
        
    } // Void return이니까 값을 반환하지 않아서 Wildcard문법으로 대체, 바인딩 여부만 확인

    let e = p.school?.schoolName?["school"]

    p.school?.schoolName?["school"]?.count
    // 딕셔너리 키를 가지고 와야할 때는 ?는 앞에
    // 값을 통해서 접근할때랑 메소드를 사용할 때 뒤에다가 ?

    p.school?.location = "Busan"
    p.school?.location
    ```

- Optional Pattern 🥕

    ```swift
    let a: Int? = 312312
    let b: Optional<Int> = 0

    if a == nil {
        
    }

    if a == .none {
        
    }

    if a == 0 {
        
    }

    if a == .some(0) {
        
    }

    if let x = a {
        print(x)
    }

    if case .some(let x) = a {
        print(x)
    }

    if case let x? = a {
        print(x)
        print(type(of: x))
    }

    let list: [Int?] = [0, nil, nil, 3, nil, 5]

    for item in list {
        guard let x = item else { continue }
        print(x)
    }

    // optioanlPattern으로 구현

    for case let x? in list {
        print(x)
    }

    // 실행결과는 동일하지만 코드가 깔끔해짐
    ```