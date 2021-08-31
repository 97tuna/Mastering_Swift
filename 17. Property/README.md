# 17. Property

- Stored Property

    ```swift
    var name: Type = DefaultValue -> 변수 저장 속성
    let name: Type = DefaultValue -> 상수 저장 속성
    // 선언과 동시에 기본값 저장, 형식 생략 가능
    ```

    ```swift
    class Person {
        let name: String = "Lee"
        var age: Int = 10
    }

    var p1 = Person()
    p1.age
    p1.name
    ```

    ```swift
    instanceName.propertyName
    instanceName.propertyName = NewValue

    Dot Syntax : 점문법
    Swift -> Explicit Member Expression : 명시적 멤버 표현식
    ```

    ```swift
    struct Person1 {
        let name: String = "Lee"
        var age: Int = 10
    }

    let p2 = Person1()
    p2.age
    p2.name

    p2.age = 11 // let 선언 시 모두 상수로 취급하여 변경 불가
    p2.age
    ```

    - Lazy Stored Properties

        ```swift
        lazy var name: Type = DefaultValue

        // 초기화 시점을 지연
        // instance가 초기화 된 이후 개별적으로 초기화 되기 때문에 var로만 사용 가능
        // 생성자에서 초기화 불가, 선언 시점에 기본값 저장 필수
        ```

        ```swift
        struct Image {
            init() {
                print("New Image")
            }
        }

        struct GitPost {
            let title: String = "Title"
            let content: String = "Content"
            let attachment: Image = Image()
        }

        let post = GitPost()
        // 오버헤드 발생 가능, 매번 인스턴스에 추가
        ```

        ```swift
        struct Image {
            init() {
                print("New Image")
            }
        }

        struct GitPost {
            let title: String = "Title"
            let content: String = "Content"
            lazy var attachment: Image = Image()
                // 로그 출력 되지 않음. attachment는 초기화 되지 않음.
        }

        let post = GitPost()
        post.attachment // 에러

        var post = GitPost()
        post.attachment // 가능
        ```

        ```swift
        import Foundation
        import UIKit

        struct Image {
            init() {
                print("New Image")
            }
        }

        struct GitPost {
            let title: String = "Title"
            let content: String = "Content"
            lazy var attachment: Image = Image()
            
            let date: Date = Date()
            
            lazy var formatterDate: String = {
                let f = DateFormatter()
                f.dateStyle = .long
                f.timeStyle = .medium
                return f.string(from: date)
            }() // return형 생략하면 클로저가 초기화 시점에 호출, 클로저가 return하는 값이 속성에 저장
        } // 저장 속성을 Closure로 초기화 할때 다른 속성에 접근 하려면 lazy속성으로 선언해야 함.
        ```

- Computed Property

    ```swift
    var name: Type {
        get {
            Statements
            return expression
        }
        set(name) {
            Statements
        }
    }

    // 선언 시점에 기본값 지정 불가, 형식 추론 불가
    // get 블록과 set블록 포함.
    // get은 속성값을 읽을때 실행, return -> 자료형과 동일한 값
    // set은 값을 저장할 때 실행, 속성에서 저장한 값이 파라미터에 저장
    ```

    ```swift
    import Foundation
    import UIKit

    class Person {
        var name: String
        var birthYear: Int
        
        init(name: String, year: Int) {
            self.name = name
            self.birthYear = year
        }
        
        var age: Int {
            get {
                let calender = Calendar.current
                let now = Date()
                let year = calender.component(.year, from: now)
                return year - birthYear
            }
    //        set {
    //            let calender = Calendar.current
    //            let now = Date()
    //            let year = calender.component(.year, from: now)
    //            birthYear = year - newValue
    //        } 읽기 전용 속성
        }
    }

    var person = Person(name: "Lee", year: 1997)
    person.age

    //person.age = 50
    person.birthYear
    ```

    - Read-only Computed Property

        ```swift
        var name: Type {
            get {
                statement
                return expression
            }
        }

        var name: Type {
            statement
            return expression
        }
        ```

        **Closure와 다른점은 = 표시가 없다는 점.**

- Property Observer

    속성 감시자

    ```swift
    var name: Type = DefalutValue {
        willset(name) {
            statement // 속성에 값이 저장되기 직전에, 파라미터 생략 시 NewValue사용
        }
        didset(name) {
            statement // 이전 값이 파라미터로 전달., oldValue
        }
    }
    ```

    ```swift
    var number: Int = 0 {
        willSet {
            print("바뀐 값 : \(newValue)")
        }
        didSet {
            print("이전 값 : \(oldValue)")
        }
    }

    number += 1

    // 바뀐 값 : 1
    // 이전 값 : 0
    ```

- Type Property

    ```swift
    static var name: Type = DefaultValue
    static let name: Type = DefaultValue
    // 형식 자체에 생성자 없어 원하는 값으로 초기화 해야함.
    // 속성에 처음으로 접근할 때 초기화 됨.
    // 인스턴스 이름으로 접근 불가
    // TypeName.propertyName
    ```

    ```swift
    class Math {
        static let pi = 3.14
    }

    let m = Math()
    m.pi // 에러, 형식 속성이므로 Math.pi로 접근
    Math.pi
    ```

    ```swift
    class Math {
        static let pi = 3.14
    }

    let m = Math() // 여기서는 초기화 되지 않음

    Math.pi
    // lazy속성이므로 처음 접근할 때 생성
    ```

    - Computed Type Properties

        ```swift
        static var name: Typr {
            get {
                statement
                return expression
            }
            set(name) {
                statement
            }
        }// 속성을 Static으로 선언하면 Subclass에서 Overriding 금지

        class var name: Typr { // static대신 class, 제한적 사용
            get {
                statement
                return expression
            }
            set(name) {
                statement
            }
        }
        // 속성을 class으로 선언하면 Subclass에서 Overriding 허용
        ```

        ```swift
        import Foundation
        import UIKit

        enum Weekday: Int {
            case sunday = 1, monday, tuesday, wednesday, thursday, friday, satureday // 나머지는 여기서 1씩 증가한 값.
            
            static var today: Weekday {
                let cal = Calendar.current
                let today = Date()
                let weekday = cal.component(.weekday, from: today)
                return Weekday(rawValue: weekday)!
            }
        }

        Weekday.today
        ```

- self & super
    - Self

        ```swift
        self // instance자체
        self.propertyName // instance 속성
        self.mothod() // instance 메소드 호출
        self[index] // Subscript호출
        self.init(parameter) // 동일한 형식의 다른 호출
        ```

        ```swift
        self는 직접 선언 X
        instance에 자동으로 추가
        ```

        ```swift
        self는 instance 멤버내부에서 접근하면 해당 instance내부로 접근
        TypeMember에서 접근한다면, 형식 자체에 접근
        ```

        ```swift
        class Size {
            var width = 0.0
            var height = 0.0
            
            func calArea() -> Double {
                return self.width * self.height // self 생략 가능
            }
            
            var area: Double {
                return self.calArea() // 주로 self생략하고 호출
            }
            
            func update(width: Double, height: Double) {
                self.width = width
                self.height = height
            }
            
            func doSomwthing() {
                let c = {self.width * self.height} // Closure내부에서 instance Member에 접근하려면 self에 캡처
            }
            
            static let unit = ""
            static func doSomeThing() {
        //        self.w // 형식 method에서 instance 속성에 직접 접근 불가
                self.unit // 둘다 Type멤버라 가능
                unit // 이름만으로 접근 가능
            }
        }
        ```

        1. self는 현재 instance에 접근하기 위해 사용하는 특별한 속성
        2. self를 Type member에서 사용하면 instance가 아닌 형식 자체를 나타낸다.

        ```swift
        struct Size {
            var width = 0.0
            var height = 0.0
            
            mutating func reset(value: Double) {
        //        width = value
        //        height = value // 개별 속성 일일이 초기화시 문제 X
                self = Size(width: value, height: value)
                // instance가 새로운 instance로 교체됨
                // 생성자를 통해 초기화 할 수 있음. Class는 불가
            }
        }
        ```

    - Super

        ```swift
        // class에서만 사용 가능
        super.propertyName
        super.mothod()
        super[index]
        super.init(parameter)
        ```

- Self type 🥕

    Self와 self는 다름!!

    ```swift
    extension Int { // Int타입 확장
        static let zero: Self = 0 // Self가 Int임
        // Int Type과 extension Type과 동일하면
        // static let zero: Int = 0 대신 Self사용 가능
        var zero: Self {
            return 0
        }
        
        func makeZero() -> Self {
                    Self.zero
            Int.zero // 둘다 동일 코드
            return Self() // 생성자 호출 코드도 가능
        }
    }

    extension Double {
        
    }
    ```

    **Type에 의존하지 않는 범용성 코드 작성이 가능**

- Property Wrapper #1 🥕

    ```swift
    // Protocol 더 듣고 다시 학습
    ```

- Property Wrapper #2 🥕

    ```swift

    ```

- Property Wrapper #3 🥕

    ```swift

    ```