# 19. Inheritance and Polymorphism

- Inheritance

    ```swift
    class ClassName: SuperClassName {

    }

    // 다중 상속은 지원되지 않음
    ```

    ```swift
    class Figure {
        var name = "Unknown"
        
        init(name: String) {
            self.name = name
        }
        
        func draw() {
            print("\(name)")
        }
    }

    class circle: Figure {
        var radius = 0.0
    }

    let c = circle(name: "Circle")
    c.draw()
    c.radius
    ```

    SubClass는 SuperClass로부터 member를 상속받는다.

    - Final Class

        ```swift
        final class ClassName: SuperClassName {

        }
        ```

        ```swift
        final Class는 상속 금지된 Class

        final class Rectangle: Figure {
            var width = 0.0
            var height = 0.0
        }

        class Square: Rectangle {
            // 에러   
        }
        ```

- Overriding

    Overriding이 가능한 속성 → method, properties, subscript, initializers

    ```swift
    class Figure {
        var name = "Unknown"
        
        init(name: String) {
            self.name = name
        }
        
        func draw() {
            print("\(name)")
        }
    }

    class circle: Figure {
        var radius = 0.0
        
        override func draw() {
            super.draw() // Figure의 draw먼저 실행
            print("👻")
        }
    }

    let c = circle(name: "Circle")
    c.draw() // overriding된 method 출력
    ```

    ```swift
    class Circle: Figure {
        var radius = 0.0
        
        var diameter: Double {
            return radius * 2 // 읽기전용 계산 속성
        }
        override func draw() {
            super.draw() // Figure의 draw먼저 실행
            print("👻")
        }
    }

    let c = Circle(name: "Circle")
    c.draw() // overriding된 method 출력

    class Oval: Circle {
        override var diameter: Double {
            get {
                return super.diameter
            }
            set {
                super.radius = newValue / 2 // self는 위험.
            }
        }
        
        override var radius: Double {
            // return super.radius // 읽기와 쓰기 속성이 읽기전용으로 overriding할 수 없다.
            get {
                return super.radius
            }
            set {
                super.radius = radius
            }
        }
    }
    // 재귀호출이 반복적으로 일어나지 않게 유의할 것.
    ```

    ```swift
    class Oval: Circle {
        override var diameter: Double { // 읽기전용 속성을 감시할 수 없다.
            willSet {
                print(newValue)
            }
            didSet {
                print(oldValue)
            }
        }
        
        override var radius: Double {
            willSet {
                print(newValue)
            }
            didSet {
                print(oldValue)
            }
        }
    }
    ```

    ```swift
    class Figure {
        var name = "Unknown"
        
        init(name: String) {
            self.name = name
        }
        
        final func draw() { // draw method overriding 금지
            print("\(name)")
        }
    }

    class Circle: Figure {
        final var radius = 0.0
        
        var diameter: Double {
            return radius * 2 // 읽기전용 계산 속성
        }
    }

    let c = Circle(name: "Circle")

    class Oval: Circle {
        
    }

    let o = Oval(name: "Oval")
    o.radius // 상속받은 radius속성에 접근
    o.draw() // 마찬가지
    ```

- Upcasting and Downcasting

    ```swift
    class Figure {
        var name = "Unknown"
        
        init(name: String) {
            self.name = name
        }
        
        func draw() {
            print("\(name)")
        }
    }

    class Rectangle: Figure {
        var width = 0.0
        var height = 0.0
        
        override func draw() {
            super.draw()
            print("\(width) \(height)")
        }
    }

    class Square: Rectangle {
        
    }

    let f = Figure(name: "Unknown")
    f.name

    let r = Rectangle(name: "Rect")
    r.width
    r.height
    r.name

    let s: Figure = Square(name: "Square") // Upcasting
    //s.width
    //s.height
    //s.name
    // Upcasting은 오류가 잘 생기지 않음

    let k = s as! Rectangle // Downcasting
    k.height
    k.width
    k.name

    class Rhomnus: Square {
        var angle = 45.0
    }

    let dr = s as! Rhomnus // angle에 접근이 안되기 때문에 에러.
    // 원본보다 아래에 있는 Subclass로부터 Downcasting이 안됨.
    // angle 속성이 없어도 안됨.
    ```

- Type Casting

    ```swift
    expression is Type // 이항 연산자, 형식 확인용
    ```

    ```swift
    let num = 123
    num is Int // true
    num is Double // false
    num is String // false

    let t = Triangle(name: "Triangle")
    let r = Rectangle(name: "Rectangle")
    let s = Square(name: "Square")
    let c = Circle(name: "Circle")

    r is Rectangle
    r is Figure
    r is Square // Square는 Rectangle상속
    // 컴파일러는 항상 성공하는지 아닌지만 알려줌.
    ```

    ```swift
    expression as Type -> Compile Time Casr
    // 서로 호환되는 형식을 Casting하는거면 Bridging 

    Runtime Cast
    expression as? Type -> Conditional Cast
    expression as! Type -> Forces Cast
    // 왼쪽 피연산자 형식이 오른쪽 피연산자 형식과 호환된다면, 오른쪽 형식으로 Casting된 형식 return
    ```

    ```swift
    t as? Triangle
    t as! Triangle

    var upcasted: Figure = s
    upcasted = s as Figure // Upcasting은 항상 성공 및 안전함

    upcasted as? Square
    upcasted as! Square

    upcasted as? Rectangle
    upcasted as! Rectangle

    upcasted as? Circle // nil return, 서로 상관관계 없음.
    upcasted as! Circle // 에러

    if let c = upcasted as? Circle {
        
    } // 안전

    let list = [t, r, s, c] // 서로 다른 형식으로 저장이 됨. 동일한 요소가 있다면 인접한 Superclass로 Type이 upcast됨.
    for i in list {
        print(i)
        
        if let c = i as? Circle {
            c.radius
        }
    }
    ```

- Any and AnyObject

    범용 자료형

    ```swift
    Any : 모든 자료형
    AnyObject : 모든 Class
    ```

    ```swift
    var data = 1 // Int 형식
    data = 2.3 // 에러

    var dataAny: Any = 1
    dataAny = 3.4 // 가능
    dataAny = "HI"
    dataAny = NSString() // 값 형식, 참조 형식 둘다 가능, Any기 때문에

    var obj: AnyObject = NSString()
    obj = 1 // 에러

    if let str = dataAny as? String {
        print(str.count)
    } else if let list = dataAny as? [Int] {
        
    }
    ```

    ```swift
    Type Casting Pattern

    switch dataAny {
    case let str as String:
        print(str.count)
    case let list as [Int]:
        print(list.count)
    case is Double:
        print("Double Value")
    default:
        break
    }
    ```

- Overloading
    - Overriding → 상속된 Member를 현재 Class에 적합하게 다시 구현할 때
    - Overloading → 하나의 형식에서 동일한 이름을 가진 다수의 Member를 구현할 때

    ```swift
    func process(value: Int) {
        print("Int Process")
    }

    func process(value: String) {
        print("String Process")
    }

    process(value: 3)
    process(value: "DF")

    // 적합한 파라미터를 선택하여 함수를 실행
    ```

    1. 함수 이름이 동일하다면, 선언된 파라미터 수로 식별
    2. 이름과 파라미터 수가 동일하면, 파라미터 자료형으로 식별
    3. 이름, 파라미터 수, 자료형도 동일하면, Argument Label로 판별
    4. 3번까지 동일하면, return형으로 식별

        ```swift
        func process(value: Double) -> Int {
            return Int(value)
        }

        func process(value: Double) -> String? {
            return String(value)
        }

        let res = process(value: 12.34) as Int // 이렇게 구분할바에 그냥 이름을 변경할 것.
        ```

    ```swift
    struct Reactangle {
        func area() -> Double {
            return 0.0
        }
        
        static func area() -> Double {
            return 0.0
        }
    }

    let r = Reactangle()
    r.area()
    Reactangle.area()
    // instance method와 Type method로 구현해도 괜찮음
    ```