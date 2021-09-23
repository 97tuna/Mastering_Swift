# 24. Generics

- Generic Function

    형식에 의존하지 않는 범용코드 및 재사용성 유지보수성 높아짐

    - Type Parameters

        ```swift
        func swapInteger(lhs: inout Int, rhs: inout Int) {
            let tmp = lhs
            lhs = rhs
            rhs = tmp
        }

        var a = 10
        var b = 20

        swapInteger(lhs: &a, rhs: &b)
        a
        b

        func swapInteger16(lhs: inout Int16, rhs: inout Int16) {
            // ...
        }

        func swapInteger64(lhs: inout Int64, rhs: inout Int64) {
            // ...
        }

        func swapDouble(lhs: inout Double, rhs: inout Double) {
            // ...
        }
        ```

        ```swift
        func name<T>(parameter) -> Type { // <T> TypeParameter
            code
        }
        ```

        ```swift
        func swapValue<T>(lhs: inout T, rhs: inout T) {
            let tmp = lhs
            lhs = rhs
            rhs = tmp
        }

        a = 1
        b = 2

        swapValue(lhs: &a, rhs: &b)
        a
        b

        var c = 1.1
        var d = 2.2
        swapValue(lhs: &c, rhs: &d)
        c
        d
        ```

        T는 코드가 실행될때 변경되는 Placeholder일 뿐.

    - Type Constraints

        ```swift
        <TypeParameter: ClassName>
        <TypeParameter: ProtocolName>
        ```

        ```swift
        func swapValue<T: Equatable>(lhs: inout T, rhs: inout T) { // 프로토콜을 입력하면 프로토콜형식으로 제한 됨 -> 비교할 수 있는 값만 전달 가능
            if lhs == rhs { return } // Generic함수로 전달하는것은 제한이 없다, 두 값을 비교할 수 있는 연산자가 없다.
            // 비교기능 역시 직접 구현해야 함., Equatable로 해결
            
            
            let tmp = lhs
            lhs = rhs
            rhs = tmp
        }
        ```

    - Specialization

        ```swift
        func swapValue<T: Equatable>(lhs: inout T, rhs: inout T) { // 문자열도 가능
            print("generic version")
            
            if lhs == rhs {
                return
            }
            
            let tmp = lhs
            lhs = rhs
            rhs = tmp
        }

        func swapValue(lhs: inout String, rhs: inout String) {
            print("Specialized")
            
            if lhs.caseInsensitiveCompare(rhs) == .orderedSame {
                return
            }
            let tmp = lhs
            lhs = rhs
            rhs = tmp
        }

        var a = 1
        var b = 2

        swapValue(lhs: &a, rhs: &b)

        var c = "Swift"
        var d = "HI"

        swapValue(lhs: &c, rhs: &d) // Generic을 Override함, 우선순위도 높음, 모든 함수가 문자열 받을 수 있지만 두번째 함수 사용
        ```

- Generic Types

    기존 형식에 제네릭 타입만 선언하면 사용 가능

    ```swift
    class Name<T> {
        code
    }

    struct Name<T> {
        code
    }

    enum Name<T> {
        code
    }
    ```

    ```swift
    struct Color<T> {
        var red: T
        var green: T
        var blue: T
    }

    var c = Color(red: 128, green: 128, blue: 128)
    //  Color<Int> 형식임, Int는 타입 파라미터를 대체함, 생성자로 파라미터를 Int형을 주었기 때문

    // c = Color(red: 12.8, green: 12.3, blue: 11) // 에러 발생
    // Color<Int> 형식에 Double형식 불가, 두 형식은 별도의 형식

    var d = Color(red: 12.8, green: 12.3, blue: 11)
    // 타입 파라미터 자동으로 처리
    // let e = Color // 에러, 타입 파라미터 선언해야 함.
    let e: Color<Int>

    let arr: Array<Int> // Array형식도 제네릭 타입으로 구현되어 있음.
    let dict: Dictionary<String, Double>
    ```

    ```swift
    extension Color { // 제네릭 타입을 확장할때는 형식이름을 붙이지 않음, 붙이면 오히려 오류남.
        func getComponents() -> [T] {
            return [red, green, blue]
        }
    }

    let intColor = Color(red: 1, green: 2, blue: 3)
    intColor.getComponents() // Int 배열

    let dblColor = Color(red: 1.0, green: 2.0, blue: 3.0)
    dblColor.getComponents() // Double
    ```

    ```swift
    extension Color where T: FixedWidthInteger { // Int형식은 채용하고 있지만, Double형식은 채용하고 있지 않음.
        func getComponents() -> [T] {
            return [red, green, blue]
        }
    }

    extension Color where T == Int { // 아예 직접 지정
        func getComponents() -> [T] {
            return [red, green, blue]
        }
    }
    ```

- Associated Types

    ```swift
    associatedType Name
    ```

    ```swift
    // 이 프로토콜을 채용한 형식은 메소드를 구현할 때 파라미터와 리턴형의 자료형을 일치시켜야 함.
    protocol QueueCompatible {
        associatedtype Element
        // 연관형식을 제한하고 싶으면 아래와 같이 제한
        // associatedtype Element: Equatable
        func enqueue(value: Element)
        func dequeue() -> Element?
    }

    class IntegerQueue: QueueCompatible {
        typealias Element = Int
        
        func enqueue(value: Int) {
            <#code#>
        }
        
        func dequeue() -> Int? {
            return 0
        }
    }

    class DoubleQueue: QueueCompatible { // 연관 형식을 생략해도 큰 문제가 없음.
        // typealias생략
        func enqueue(value: Double) {
            
        }
        
        func dequeue() -> Double? {
            return 0
        }
    }
    ```