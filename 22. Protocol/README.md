# 22. Protocol

- Protocol Syntax

    형식에서 공통적으로 제공하는 Member목록
    프로토콜을 채용한다

    ```swift
    protocol ProtocolName {
        propertyRequirements
        methodRequirements
        initializerRequirements
        subscriptRequirements
    }

    protocol ProtocolName: Protocol, ... {

    }
    ```

    ```swift
    protocol Something {
        func dosomething()
    }
    ```

    ```swift
    enum TypeName: protocolName, ... {
        
    }

    struct TypeName: protocolName, ... {
        
    }

    class TypeName: protocolName, ... {
        
    }
    ```

    ```swift
    struct Size: Something { // Something의 요구사항을 채용한다는 의미
        func dosomething() { // 이것을 구현해야 함.
            
        }    
    }
    ```

    ```swift
    protocol ProtocolName: AnyObject { // Class전용 Protocol
        
    }

    protocol SomethingObject: AnyObject, Something {
        
    }

    struct Value: SomethingObject  { // Class전용이라 SomethingObject채용 불가
        
    }

    class Object: SomethingObject { // Something채용하고 있어서 dosomething구현해야 함.
        func dosomething() {

        }
    }
    ```

- Property Requirements

    ```swift
    protocol ProtocolName {
        var name: Type{ get set } // protocol에서는 항상 var로 제작, var의 가변 속성이 아닌 선언하는것이 속성이라는것을 알림
        static var name: Type { get set } // get과 set이 가변성 대표
    } // get만 포함이면 읽기, 형식 속성이면 static으로 선언
    ```

    ```swift
    protocol Figure {
        var name: String { get } // set추가시 Rectangle, Circle 오류
    }

    struct Rectangle: Figure {
        let name = "Rect" // var로 수정
    }

    struct Triangle: Figure { // 최소 요구사항, get만 포함이어도 읽기전용으로 선언 안해도 됨.
        var name = "Tri"
    }

    struct Circle: Figure {
        var name: String {
            return "Circle" // get, set구현해야 함.
        }
    }
    ```

    ```swift
    protocol Figure {
        static var name: String { get set }
    }

    struct Rectangle: Figure {
        static var name = "Rect" // var로 수정
    }

    struct Triangle: Figure { // 최소 요구사항, get만 포함이어도 읽기전용으로 선언 안해도 됨.
        static var name = "Tri"
    }

    class Circle: Figure {
        static var name: String { // static으로 선언하면 SubClass로 상속은 되지만 Overriding은 불가, static대신 class로 선언
            get {
                return "Circle"
            }
            set {
                
            }
        }
    } // static으로 선언돼있다고 해서 형식에서 static으로 꼭 선언해야 하는 것은 아님.
    ```

- Method Requirements

    ```swift
    protocol ProtocolName {
        func name(param) -> ReturnType
        static func name(param) -> ReturnType // TypeMethod
        mutating func name(param) -> ReturnType // 값 형식, Method내부에서 속성값을 변경해야 한다면 사용.
            // mutating은 값 형식 전용, protocol에서는 Method에서 속성 변경 가능 표현, 참조 형식도 가능
    }
    ```

    ```swift
    protocol Ressttable {
        func reset()
    }

    class Size: Ressttable { // struct로 하면 오류.
        var w = 0.0
        var h = 0.0
        
        func reset() { // Method형식 param이름, returnType일치, Body는 자유롭게 구현
            w = 0.0 // 값 형식에서 속성값 변경 시 mutating 키워드 작성 필, struct에서도 protocol에서도
            h = 0.0
        }
    }
    ```

    ```swift
    protocol Ressttable {
        mutating func reset()
    }

    struct Size: Ressttable { // struct로 하면 오류.
        var w = 0.0
        var h = 0.0
        
        mutating func reset() { // Method형식 param이름, returnType일치, Body는 자유롭게 구현
            w = 0.0 // 값 형식에서 속성값 변경 시 mutating 키워드 작성 필, struct에서도 protocol에서도
            h = 0.0
        }
    }
    ```

    ```swift
    protocol Ressttable {
        static func reset()
    }

    class Size: Ressttable {
        var w = 0.0
        var h = 0.0
        
        func reset() {
            w = 0.0
            h = 0.0
        }
        
        static func reset() { // overloading으로 타입메서드, 인스턴스메소드 둘다 가능
                // class로 선언 시 overriding도 가능, protocol 요구사항도 충족
        }
        
    }
    ```

- Initializer Requirements

    ```swift
    protocol ProtocolName {
        init(param) // Method와 동일하게 Body생략 후 사용
        init?(param)
        init!(param)
    }
    ```

    ```swift
    protocol Figure {
        var name: String { get }
        init(name: String) // n으로 바꾸면 오류.
    }

    struct Rectangle: Figure {
        var name: String // 생성자를 만들어야할 것 같지만 생성 안해도 Protocol 만족
    }
    ```

    ```swift
    class Circle: Figure { // class는 상속을 고려, 모든 subclass에서 protocol 만족해야함.
        var name: String
        required init(n: String) { // 그래서 required init으로 구현해야 함.
            name = n
        }
    }

    // 예외 사항

    final class Triangle: Figure { // final class는 더 이상 상속이 불가, 더 고려할 필요 없음.
        var name: String
        init(n: String) {
            name = n
        }
    }
    ```

    ```swift
    class Oval: Circle { // 상속이라 자동으로 요구사항 만족, Figure 채용이면 중복 -> 오류
        var prop: Int
        
        init() {
            prop = 0
            super.init(n: "Oval")
        }
        
        required convenience init(n: String) {
            self.init()
        }
    }
    ```

    ```swift
    protocol GrayScale {
        init(white: Double) // ?도 가능
    }

    struct Color: GrayScale {
        init?(white: Double) {
            // 충족불가, !는 가능, 초기화 실패시 오류
        }
    }
    ```

- Subscript Requirements

    ```swift
    protocol ProtocolName {
        subscript(param) -> ReturnType { get set }
    }
    ```

    ```swift
    protocol List {
        subscript(id: Int) -> Int { get set } // set지워도 가능
    } // get은 필수이고, set만 상황에 따라 추가 삭제 가능

    struct DataStore: List {
        subscript(idx: Int) -> Int {
            get {
                return 0
            }
            set {
                // protocol 요구사항 만족
            }
        }
    }
    ```

- Protocol Types 🥕

    First-class Citizen → 독립적인 형식

    ```swift
    protocol Resettable {
        func reset()
    }

    class Size: Resettable {
        var width = 0.0
        var height = 0.0
        
        func reset() {
            width = 0.0
            height = 0.0
        }
    }

    let s = Size()

    let r: Resettable = Size() // Superclass upCasting가능
    // protocol 속성에만 접근할 수 있음
    // r.witdh 불가
    ```

    - Protocol Conformance(프로토콜 적합성)

        ```swift
        instance is ProtocolName // 특정 형식이 Protocol을 채용하는지 확인

        instance as ProtocolName
        instance as? ProtocolName
        instance as! ProtocolName
        ```

        ```swift
        let s = Size()
        s is Resettable // true
        s is ExpressibleByNilLiteral // false
        ```

        ```swift
        let r = Size() as Resettable
        r as? Size // 원래 형식으로 ComplieCasting 불가
        ```

    - Collections of Protocol Types

        ```swift
        protocol Figure {
            func draw()
        }

        struct Triangle: Figure {
            func draw() {
                print("draw triangle")
            }
        }

        class Rectangle: Figure {
            func draw() {
                print("draw rect")
            }
        }

        struct Circle: Figure {
            var radius = 0.0

            func draw() {
                print("draw circle")
            }
        }

        let t = Triangle()
        let r = Rectangle()
        let c = Circle()
        // 모든 형식 Figure프로토콜 채용

        let list: [Figure] = [t, r, c] // 모두 다름, 값 형식 , 인스턴스 형식도 다름, Figure로 하면 가능

        for i in list {
            i.draw()
        //    i.radius // 이건 불가, Circle
            if let c = i as? Circle {
                c.radius
            }
        }
        ```

- Protocol composition 🥕

    Protocol & Protocol & ... 사용

    ```swift
    protocol Resettable {
        func reset()
    }

    protocol Printable {
        func printValue()
    }

    class Size: Resettable, Printable {
        var width = 0.0
        var height = 0.0
        
        func reset() {
            width = 0.0
            height = 0.0
        }
        
        func printValue() {
            print(width, height)
        }
    }

    class Circle: Resettable {
        var radius = 0.0
        
        func reset() {
            radius = 0.0
        }
    }

    class Oval: Circle {
        
    }

    let r: Resettable = Size()
    let p: Printable = Size()

    let rp: Resettable & Printable = Size()
    // rp = Circle()
    ```

    ```swift
    var cr: Circle & Resettable = Circle()
    cr = Oval() // 문제 없이 저장 가능
    ```

- Optional Requirements 🥕

    ```swift
    @objc protocol ProtocolName {
            @objc optional Requirements
    }
    ```

    ```swift
    @objc protocol Drawable {
        @objc optional var strokeWidth: Double { get set }
        @objc optional var strokeColor: UIColor { get set }
        func draw()
        @objc optional func reset() // class전용, 자동으로 any objc 상속 -> class Protocol
    }

    class Rectangle: Drawable {
        func draw() {
        }
    }

    let r: Drawable = Rectangle()
    r.draw()
    r.strokeWidth
    r.strokeColor // 속성에 nil이 저장 또는 형식에 선언이 안되있다는 것

    r.reset?() // optional형식이므로 옵셔널 체이닝 연산자 추가해야 함.
    // 호출한 메소드가 구현되어있지 않다는 뜻
    ```

- Protocol Extension 🥕

    형식이기 때문에 Extension으로 확장할 수 있음 → 채용한 형식에 기본 구현 제공
    실제로는 채용한 Type에 구현이 추가되는 것임. Protocol에 추가가 아니라!

    ```swift
    protocol Figure {
        var name: String { get }
        func draw()
    }

    extension Figure where Self: Equatable { // Equatable 채용된 형식에 제한적으로 구성
        func draw() {
            print("Draw Figure")
        }
    }

    struct Rectangle: Figure {
        var name = ""
    }

    let r = Rectangle()
    r.draw() // 자동으로 추가됨.
    ```

    ```swift
    // 직접 구현
    struct Rectangle: Figure {
        var name = ""
        func draw() {
            print("Draw Rectangle")
        }
    }

    let r = Rectangle()
    r.draw() // 자동으로 추가됨
    // 따로 구조체에 구현하면, Extension으로 구현된것은 무시됨, 높은 우선순위 가짐.

    // Rectangle이 Equatable채용되어야지 draw사용할 수 있음 -> Rectangle에 draw메소드 구현이 되어있지 않다는 가정하에
    ```

- Equatable 🥕

    Swift가 제공하는 가장 중요한 Protocol → 값을 비교할 수 있는 Type이라면 반드시 구현해야 함.

    - Equatable for Enumerations

        ```swift
        // 연관값이 선언되있지 않는 것은 Equatable이 자동으로 선언
        // 연관값을 가지고 있고, 모든 연관값의 형식이 Equatable구현한 형식이면, 열거형에 Equatable자동으로 구현

        enum Gender {
            case female
            case male
        }

        Gender.female == Gender.male // 이렇게 비교도 가능

        struct MySize {
            let width: Double
            let height: Double
        }

        enum VideoInterface: Equatable {
            case dvi(width: Int, height: Int)
            case hdmi(width: Int, height: Int, version: Double, audioEnabled: Bool)
            case displayPort(size: CGSize)
        }

        let a = VideoInterface.hdmi(width: 2560, height: 1440, version: 2.0, audioEnabled: true)
        let b = VideoInterface.displayPort(size: CGSize(width: 3840, height: 2160))

        a == b
        ```

    - Equatable for Structures

        ```swift
        struct Person: Equatable {
            let name: String
            let age: Int
        }

        let a = Person(name: "Steve", age: 50)
        let b = Person(name: "Paul", age: 27)

        a == b // 컴파일러가 자동으로 추가해 줌. Equatable만 채용하면!
        ```

    - Comparable for Classes

        ```swift
        // 직접 개인이 구현해야 함.
        class Person {
            // Class에서는 오류 -> 자동으로 추가 불가, Struct애서는 가능
            let name: String
            let age: Int
            
            init(name: String, age: Int) {
                self.name = name
                self.age = age
            }
        }

        // 구조체, 열거형도 아래와 같이 구현하면 가능
        extension Person: Equatable { // Protocol구현을 별도 Extension으로 분리 -> 코드 가독성 증가
            static func == (lhs: Person, rhs: Person) -> Bool { // Fix누르면 자동으로 추가
                return lhs.age == rhs.age && lhs.name == rhs.name
            }
        }

        let a = Person(name: "Steve", age: 50)
        let b = Person(name: "Paul", age: 27)

        a == b
        a != b
        ```

    - Equatable구현 시 주의해야할 점
        1. instance를 정확히 비교할 수 있도록 해야 함 → 특정 속성을 비교하도록 함.
        2. Protocol 구현 뒤에 3가지를 검증해야 함.
            1. 동일한 instance비교시 항상 true return 해야 함. (a == a)
            2. a == b가 true이면 b == a도 true이어야 함.
        3. a == b결과와 b == c결과가 동일하다면, a == c결과도 동일해야 함.

        모든 조건을 비교해버리면 위의 조건을 바로 만족 함.

- Hashable 🥕

    MD-5, SHA등등 알고리즘 사용, Set, Dict에서 Key로 사용, 반드시 구현해야 함.
    → 값의 유일성 보장 및 검색 속도 증가

    - Hashable for Enumerations

        ```swift
        enum ServiceType {
            case onlineCourse
            case offlineCamp
        }

        let types: [ServiceType: String] // dict key사용 가능
        let typeSet: Set = [ServiceType.onlineCourse]
        ```

        ```swift
        enum VideoInterface {
            case dvi(width: Int, height: Int)
            case hdmi(width: Int, height: Int, version: Double, audioEnabled: Bool)
            case displayPort(size: CGSize)
        }

        let interfaces: [VideoInterface: String] // 오류 -> Hashable 구현 필요함.
        let interfaceSet: Set = [VideoInterface.dvi(width: 100, height: 200)]
        // 둘다 에러, Hashable 구현 필요함.
        ```

        ```swift
        enum VideoInterface: Hashable {
            case dvi(width: Int, height: Int)
            case hdmi(width: Int, height: Int, version: Double, audioEnabled: Bool)
            case displayPort(size: CGSize) // CGSize는 Hashable 구현해줘야 함.
        } // 에러
        ```

    - Hashable for Structures

        ```swift
        struct Person: Hashable {
            let name: String
            let age: Int
        }

        let set: Set = [Person(name: "Lee", age: 10)]
        // Dict으로도 구현 가능
        ```

    - Hashable for Classes

        자동으로 구현되지 않음

        ```swift
        class Person { // Equatable도 함께 구현해 줘야 함 -> Hashable이 상속함.
            let name: String
            let age: Int
            
            init() {
                name = "Jane Doe"
                age = 0
            }
        }

        extension Person: Hashable {
            static func == (lhs: Person, rhs: Person) -> Bool {
                return lhs.age == rhs.age && lhs.name == rhs.name
            }
            
            func hash(into hasher: inout Hasher) { // 복잡한 계산은 Hasher가 대신 해줌.
                hasher.combine(name)
                hasher.combine(age)
            }
        }
        ```

        구현은 간단하나 2가지를 기억

        1. combine으로 전달할 때는 동일한 순서로 전달해야 함 → 특별한 이유가 없다면 모든 저장속성을 전달해야 함. → 그렇지 않으면 논리오류 발생, 디버깅 어려움
        2. combine으로 전달하는 속성은 반드시 Hashable로 구현해야 함.

        ```swift
        let pDict = [Person.init(): String.self]
        let pSet: Set = [Person.init()]
        ```

- Comparable 🥕

    >, ≥, <, ≤ 연산자 구현 → 값의 크기나 순서 비교, Equatable은 값의 동일성 비교

    - Comparable for Enumerations

        ```swift
        enum Weekday: Comparable {
            case sunday
            case monday
            case tuesday
            case wednesday
            case thursday
            case friday
            case saturday
        }

        Weekday.sunday < Weekday.monday // 에러, Swift 5.3미만이면 스스로 구현해야 함.
        ```

        ```swift
        extension Weekday: Comparable {
            static func < (lhs: Weekday, rhs: Weekday) -> Bool { // 이 연산자만 구현하면 자동으로 구현 됨.
                return lhs.rawValue < rhs.rawValue
            }
        }

        Weekday.sunday >= Weekday.monday // 가능
        ```

    - Comparable for Structures

        ```swift
        struct Person {
            let name: String
            let age: Int
        }

        extension Person: Comparable {
            static func < (lhs: Person, rhs: Person) -> Bool {
                return lhs.age < rhs.age
            }
        }

        let a = Person(name: "Paul", age: 12)
        let b = Person(name: "Smith", age: 33)

        a < b
        ```

    - Comparable for Classes

        ```swift
        enum MembershipGrade {
            case normal
            case premium
            case vip
            case vvip
        }

        class Membership {
            let name: String
            let grade: MembershipGrade
            let point: Int
            
            init(name: String, grade: MembershipGrade, point: Int) {
                self.name = name
                self.grade = grade
                self.point = point
            }
        }

        extension Membership: Comparable {
            static func < (lhs: Membership, rhs: Membership) -> Bool {
                return lhs.grade.hashValue < rhs.grade.hashValue
            }
            
            static func == (lhs: Membership, rhs: Membership) -> Bool {
                return lhs.grade.hashValue == rhs.grade.hashValue
            }
            
            
        }

        let a = Membership(name: "James", grade: .premium, point: 123)
        let b = Membership(name: "Yuna", grade: .vvip, point: 2020)
        let c = Membership(name: "Paul", grade: .normal, point: 37)

        a < b
        b > c
        ```