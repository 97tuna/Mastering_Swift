# 16. Structure And Class

- Structures and Classes

    Programming Paradigm

    Swift역시 Multi Paradigm

    1. Object-Oriented Programming
        1. Object: 객체와 상태를 조작하여 원하는 기능을 구현
            1. Abstraction: 추상화 → 프로그램의 종류에 따라 달라짐
            2. 추상화를 프로그래밍한것이 Class
                1. Class → 하나의 Class를 통해 객체를 생성: Instance → 속성과 값은 다 다름
            3. Interaction: 상호작용 → 메시지를 보낸다
            4. **Struct: 비교적 작은 데이터 저장 및 값 형식이 필요한 경우 구현**
                1. Swift에서는 Class기능 Struct에 대부분 구현되어 있음.

    ```swift
    struct StructName { // UpperCamelCase
        property
        method
        initializer
        subscript
    }
    ```

    ```swift
    struct Person {
        var name: String
        var age: Int
        
        func speak() {
            print("Hello, World")
        }
    }

    var p1 = Person(name: "Lee", age: 10)
    // 새로운 메모리에 이름과 나이가 저장됨

    p1.age
    p1.name
    p1.speak()

    // 함수는 이름만으로 호출, 메소드는 인스턴스 이름으로 호출
    ```

    ```swift
    class ClassName {
        property
        method
        initializer
        deinitializer
        subscript
    }
    ```

    ```swift
    class Person {
        var name = "Lee"
        var age = 10
        
        func speak() {
            print("Hello")
        }
    }

    var p2 = Person()
    p2.age
    p2.name
    p2.speak()
    ```

- Initializer Syntax

    ```swift
    init(parameters) {
        Statements
    }
    ```

    ```swift
    let str = "123"
    let num = Int(str)

    // 생성자는 모든 속성의 초기값을 저장 -> instance초기화

    class Position {
        var x: Double
        var y: Double
        
        init() {
            self.x = 0
            self.y = 0
        } // 속성 초기화가 가장 중요, 모든 초기값이 저장되있어야 함.
        
        init(_ value: Double) {
            self.x = value
            self.y = value
        }
    }

    var pos = Position(10)
    pos.x // 10
    pos.y // 10

    pos = Position()
    pos.x // 0
    pos.y // 0
    ```

- Value Types vs Reference Types
    - 값 형식: Value Type
    - 참조 형식: Reference type

    ```swift
    struct PositionValueType {
        var x = 0.0
        var y = 0.0
    }

    class PositionReferType {
        var x = 0.0
        var y = 0.0
    }

    var v = PositionValueType()
    var r = PositionReferType()

    var v2 = v
    var r2 = r

    v2.x = 12
    v2.y = 34

    v.x // 0
    v.y // 0

    v2.x // 12
    v2.y // 34

    // v2는 v와 같은 형식을 복사한 것

    r2.x = 12
    r2.y = 34

    r.x // 12
    r.y // 34

    r2.x // 12
    r2.y // 34

    // class는 원본을 전달, 즉 참조를 전달한다
    // 어떤 속성을 전달해도 같은 것을 변경함.
    ```

- Identity Operator 🥕
    - ValueType은 값을 Stack에 저장
    - ReferenceType은 값을 Heap에 저장, 주소를 Stack에 저장
        - 메모리 주소 비교는 항등 연산자 사용

    ```swift
    classInstance === classInstance
    classInstance !== classInstance
    ```

    ```swift
    class First {
        
    }

    let A = First()
    let B = A
    let C = First()

    A === B // true
    A !== B // false

    A !== C // true
    ```

    - 메모리 주소가 같으면 → Identical 하다
    - 값이 같으면 → Equal
- Nested Types

    ```swift
    String.CompareOptions
    // Struct    Struct
    ```

    ```swift
    class One {
        struct Two {
            enum Three {
                case a
                
                class Four {
                    
                }
            }
        }
        var a = Two() // One생략 가능
    }

    //let two: Two = Two() // 에러
    let two: One.Two = One.Two()

    let four: One.Two.Three.Four = One.Two.Three.Four()
    ```

    ```swift

    ```