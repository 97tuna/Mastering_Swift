# 10. Functions

- Functions

    ```swift
    print("HI") // print함수
    ```

    함수가 없다면 직접 사용자가 구현해야함.

    ```swift
    Reusablity
    ```

    [Apple Developer Documentation](https://developer.apple.com/documentation/swift/swift_standard_library)

    - Calling Functions

        함수를 가져와서 사용, 호출한다

        ```swift
        functionName(parameters)

        print("HELLO") // HELLO 출력, print함수 호출
        ```

    - Defining Functions

        직접 함수를 제작하여 사용.

        ```swift
        func name(parameter) -> ReturnType {
            statement
        }

        // 이름은 lowerCamelCase
        // 함수의 이름은 함수의 역할을 나타내는 직관적인 이름 추천
        // -> Return Arrow
        ```

        ```swift
        func printHI() {
            print("HI")
        }

        printHI() // 이렇게 호출해야지 사용가능
        ```

- Return Values

    ```swift
    func name(parameter) -> ReturnType {
        statement
        return expression
    }
    ```

    ```swift
    func add() -> Int {
        return 5 + 5
    }
    // return문 뒤의 표현식 검증

    add() // 함수 호출도 표현식임.

    if add() == 10 {
        print("10")
    }

    func somethingToDo() {
        let random = Int.random(in: 1...10)
        
        if random % 2 == 1 {
            return
        }
        print(random)
    }

    somethingToDo()
    ```

- Parameters

    ```swift
    func name(parameter) -> ReturnType {
        statement
        return expression
    }

    (name: Type, name: Type)
    ```

    ```swift
    func add() -> Int {
        return 1 + 2
    }

    func addSome(a: Int, b: Int) -> Int {
        // a, b는 모두 let Type임, 임시상수
        return a + b
    }
    ```

    - Calling Function

        ```swift
        functionName(paraName: expression)
        ```

        ```swift
        print(addSome(a: 5, b: 7)) // 12출력
        ```

    - Default Value

        ```swift
        (name: Type = Value)
        ```

        ```swift
        func printHI(to: String = "Anonymous") {
            print("HELLO, \(to)")
        }

        printHI(to: "SWIFT") // HELLO, SWIFT
        printHI() // HELLO, Anonymous
        ```

    - Scope, Life Cycle

        ```swift
        {} // 함수 body안에서만 사용
        // 바깥에서 to사용은 불가

        // 파라미터와 Life Cycle도 함수의 실행이 끝나면 사라짐.
        ```

- Argument Label

    ```swift
    func printHI(to: String = "Anonymous") {
        print("HELLO, \(to)")
    }

    printHI(to: "SWIFT") // Argument Label
    // to: Argument Label이다.

    (name: Type)
    // name = Parameter Name이자 Argument Label

    (label name: Type)
    // label = Argument Label, 함수 이름의 가독성을 높이기 위해 사용
    // name = Parameter Name
    ```

    ```swift
    func printHI(to name: String = "Anonymous") {
        print("HELLO, \(name)")
    }

    printHI(to: "HI")
    ```

    - Wildcard Pattern

        ```swift
        func printHI(_ name: String = "Anonymous") {
            print("HELLO, \(name)")
        }

        printHI("HI")
        ```

- Variadic Parameters 🥕

    ```swift
    print("HELLO")
    // HELLO -> argument

    print("HELLO", "SWIFT")
    // paramter는 2개이지만 실제로는 1개
    // 가변 파라미터
    ```

    (name: Type...) → 가변 파라미터, 배열 형태로 전달

    ```swift
    func printSum(of numbers: Int...) {
        var sum = 0
        for number in numbers {
            sum += number
        }
        print(sum)
    }
    // 가변 파라미터

    printSum(of: 1, 2, 3, 4, 5, 6, 7, 8, 9)
    ```

    ```swift
    func printSum(of numbers: Int..., b: Double...) {
    // 가변 파라미터는 1개만 사용 가능, 가변 파라미터 선언은 위치 상관은 없음
    // 가변 파라미터는 기본값 사용 X
        var sum = 0
        for number in numbers {
            sum += number
        }
        print(sum)
    }
    // 가변 파라미터

    printSum(of: 1, 2, 3, 4, 5, 6, 7, 8, 9)
    ```

- In-Out Parameters

    ```swift
    var num1 = 12
    var num2 = 34

    func swap(_ a: Int, with b: Int) {
    //      var temp = a
    //      a = b
    //      b = temp
    }

    swap(num1, with: num2) // 함수에 복사본을 전달

    (name: inout Type)

    functionName(argLabel: &expression)
    ```

    ```swift
    var num1 = 12
    var num2 = 34

    func swap(_ a: inout Int, with b: inout Int) {
        var temp = a
        a = b
        b = temp
    }

    swap(&num1, with: &num2) // &사용
    ```

    ```swift
    var num1 = 12
    var num2 = 34

    func swap(_ a: inout Int, with b: inout Int) {
        var temp = a
        a = b
        b = temp
    }

    swap(&num1, with: &num2)
    // copy in -> 함수내부로 값이 복사
    // copy out -> 함수가 종료될때 함수가 argument에 외부로 복사
    ```

    1. 동일변수 두가지를 같이 입력하는것은 불가
    2. 상수 let 불가
    3. literal을 직접 전달, 불가
    4. 기본값 지정 불가, 기본값은 메모리 할당이 안되있기 때문
    5. 설령 메모리 할당이 되있는 변수를 사용해도 불가
    6. 가변 파라미터 불가
- Function Notation

    ```swift
    functionName(label:)
    functionName(label:label:) // 파라미터가 여러개
    functionName // 파라미터가 없다면
    ```

    Reference Docs를 보기 편하고, Function Type을 이해하기 편함

- Function Types

    Swift는 First-Class Citizen

    1. 변수나 상수에 저장할 수 있다.
    2. 파라미터로 전달할 수 있다.
    3. 함수에서 리턴할 수 있다.

    ```swift
    () // 비어있는 튜플, 대부분 ()을 사용함
    Void // Return이 없다!
    ```

    ```swift
    (ParameterTypes) -> ReturnType
    ```

    ```swift
    func sayHI() {
        print("HI, Swift")
    }

    let func1 = sayHI
    print(type(of: func1)) // () -> ()

    func1() // HI, Swift
    ```

    ```swift
    func printHI(to name: String) {
        print("HELLO, \(name)")
    }

    let func2: (String) -> () = printHI(to:)
    func2("HI")

    let func3 = printHI(to:)
    func3("LEE") // Argument Label 사용 불가
    ```

    ```swift
    func addSum(a: Int, b: Int) -> Int {
        return a + b
    }

    var func4: (Int, Int) -> Int = addSum(a:b:)

    func4(5, 10) // 15
    ```

    ```swift
    func addSum(a: Int, b: Int) -> Int {
        return a + b
    }

    var func4: (Int, Int) -> Int = addSum(a:b:)

    func4(5, 10) // 15

    func addSum(_ a: Int, _ b: Int) -> Int {
        return a + b
    }

    func4 = addSum(_:_:)
    ```

    ```swift
    func swap(_ a: inout Int, _ b: inout Int) {
        
    }

    let func5 = swap(_:_:) // inout 사용
    ```

    ```swift
    // 가변 파라미터 사용
    func sumNumbers(a: Int...) {
        
    }

    let func6 = sumNumbers(a:)
    ```

    ```swift
    // 실생활
    func addFunc(_ a: Int, _ b : Int) -> Int {
        return a + b
    }

    func subFunc(_ a: Int, _ b : Int) -> Int {
        return a - b
    }

    func multiFunc(_ a: Int, _ b : Int) -> Int {
        return a * b
    }

    func devideFunc(_ a: Int, _ b : Int) -> Int {
        return a / b
    }

    typealias ArithmeticFunction = (Int, Int) -> Int

    func selectFunc(from op: String) -> ArithmeticFunction? {
        switch op {
        case "+":
            return addFunc(_:_:)
        case "-":
            return subFunc(_:_:)
        case "*":
            return multiFunc(_:_:)
        case "/":
            return devideFunc(_:_:)
        default:
            return nil
        }
    }

    let af = selectFunc(from: "+") // af에 addFunc함수 저장
    af?(5, 3)

    selectFunc(from: "*")?(5, 4)
    ```

- Nested Functions 🥕

    ```swift
    func out() {
        print("OUT")
    }

    func inn() {
        print("in")
    }

    // 그냥 함수다, Global Scope에서 선언

    out()
    inn()
    ```

    ```swift
    func outt() -> () -> () {
        func inn() {
            print("in")
        }
        print("OUT")
        return inn
    }

    // 내포된 함수, 다른 함수 안에 들어있다.

    outt()
    //  inn() // 오류

    let func1 = outt()
    func1()
    ```

    **return된 함수에서 inn을 호출할 수 있도록 scope만 조정 된 것.**

- Implicit Return

    ```swift
    func name(Parameters) -> ReturnType {
        statements
        return expression // Explicit Return, Return Keyword사용
    }

    func name(Parameters) -> ReturnType {
        single_expression // return keyword생략 가능
    }
    ```

    ```swift
    func add(a: Int, b: Int) -> Int {
        a + b // 단일 표현식 이어야 함.
    }

    add(a: 1, b: 5)
    ```

- Nonreturning Function 🥕

    ```swift
    func something() -> Int {
        return 0
    }

    let res = something()
    print(res)

    func nothing() {
        return // 제어를 호출문 이후로 전달
    }

    nothing()
    print("DONE")
    ```

    **Nonreturning: 제어를 전달하지 않는다!!**
    함수를 호출하는 코드 다음으로 제어를 전달하지 않는다.

    1. 프로그램이 종료
    2. 에러를 처리하는 코드로 제어를 전달

    ```swift
    func function() -> Never {
        // instance 생성 불가
        // Uninhabited Type
        // 빈 열거형
        fatalError("CRITICAL")
    }

    // Nonreturning함수

    function()
    print("HI") // print함수로 넘어가지 않음
    ```

    ```swift
    enum MyError: Error {
        case error
    }

    func myFunction() throws -> Never {
        throw MyError.error
    }

    do {
        try myFunction()
        print("TRY") // 출력이 절대 안됨
    } catch {
        print("ERROR")
    }
    ```

    ```swift
    func terminate() -> Never {
        fatalError("CRITICAL") // 여기서 걸림
    }

    func somthing(with value: Int) -> Int {
        guard value >= 0 else {
            terminate()
        }
        return 0
    }

    somthing(with: -1)
    ```

    ```swift
    // 또 다른 형식

    Void // Never는 프로그램을 종료하거나 또는 에러를 던지도록 함
    // 함수가 값을 return안하는것을 확인, 제어는 정상적임, 에러를 던지도록 강제도 아님

    ```

- @discardableResult

    ```swift
    import UIKit

    @discardableResult // 아무런 의미가 없음
    func something() {
        print("Something")
    }

    @discardableResult
    func anything() -> String {
        return "HI"
    }

    class Viewcontroller: UIViewController {
        override func viewDidLoad() {
            super.viewDidLoad()
            
            something()
            anything()
            // 경고표시, 함수가 return하는 결과를 사용하지 않으면 경고가 뜸
            // 결과를 사용하지 않는다고 해서 문제가 생기는것은 아님
            // 경고를 지우고 싶다면 discardableResult을 사용
        }
    }
    ```

    ```swift
    import UIKit

    func something() {
        print("Something")
    }

    func anything() -> String {
        return "HI"
    }

    class Viewcontroller: UIViewController {
        override func viewDidLoad() {
            super.viewDidLoad()
            
            something()
            _ = anything() // WildCard문법으로 해결가능
        }
    }
    ```