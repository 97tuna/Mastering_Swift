# 20. Initializer and Deinitializer

- Initializers

    생성자

    ```swift
    새로운 instance생성 -> 초기화 = initializer, 모든 속성을 초기화
    ```

    ```swift
    class Position {
        var x: Int = 0
        var y: Int // 초기화 된 값이 없으면 에러
        var z: Int? // 옵셔널은 기본값이 없으면 nil로 초기화 되기 때문에
        
        init() {
            self.y = 0
        }
    }
    ```

    ```swift
    class Position {
        var x: Int = 0
        var y: Int = 0
        var z: Int? = nil
        
        init() {
            // 컴파일러가 알아서 default initializer가 생성함
        }
    }

    let p = Position() // 컴파일러가 알아서 default initializer가 생성함
    ```

    ```swift
    init(parameter) {
        initializer
    }

    TypeName(parameter)
    ```

    ```swift
    class Sizeobj {
        var width = 0.0
        var height = 0.0
        
        init(width: Double, height: Double) {
            self.width = width
            self.height = height
        }
        convenience init(value: Double) {
    //        self.width = value
    //        self.height = value, 중복 제거
            self.init(width: value, height: value) // initializer Delegation
        }
    }
    ```

    - Memberwise Initializer

        ```swift
        struct SizeValue {
            var width = 0.0
            var height = 0.0
        }

        let s = SizeValue()
        SizeValue(width: <#T##Double#>, height: <#T##Double#>) // 파라미터 이름도 속성과 동일, 개수도 파라미터 수랑 동일
        // 컴파일러가 알아서 default initializer가 생성하는데 생성하면 위의 Memberwise Initializer사용 불가
        ```

- Memberwise Initializer

    ```swift
    struct First {
        let a: Int
        let b: Int
        let c: Int
    }

    let f = First(a: 1, b: 3, c: 5)

    struct Second {
        let a: Int = 0
        let b: Int = 1
        let c: Int
    }

    let s = Second(c: 3) // 초기화되지 않은 c만 초기화, let이라 초기화 후 값 변경 x

    struct Third {
        var a: Int = 0
        var b: Int = 1
        var c: Int
    }

    let t = Third(a: 3, b: 4, c: 4)
    let t1 = Third(c: 3) // 기본값에 따라 제공되는 initializer가 다름
    // 구조체에서 직접 initializer를 지정하면 사용 불가

    extension Third {
        init(value: Int) {
            a = value
            b = value
            c = value
        }
    }
    // extension 구현시 Memberwise Initializer 계속 사용 가능
    ```

- Class Initializers
    - Designated Initializer → 모든 속성을 초기화

        ```swift
        init(parameter) {
            initialization
        }
        ```

    - Convenience Initializer

        ```swift
        convenience init(parameter) {
            initialization
        }
        ```

    ```swift
    class Position {
        var x: Double
        var y: Double
        
        init(x: Double, y: Double) {
            self.x = x
            self.y = y
        }
        
        convenience init(x: Double) {
            self.init(x: x, y: 0.0) // 초기화 코드 중복 x
        }
    }
    ```

    ```swift
    class Figure {
        var name: String

        init(name: String) {
            self.name = name
        }

        func draw() {
            print("draw \(name)")
        }
            convenience init() {
            self.init(name: "unknown")
        }
    }

    class Rectangle: Figure {
        var width: Double
        var height: Double
        
        init(name: String, width: Double, height: Double) {
            // self.name = name
                    super.init(name: name) // 이렇게 해결
            self.width = width
            self.height = height
        }
        // SuperClass의 Designated Initializer를 호출하지 않음
        // 상속받은 name속성에 접근

            override init(name: String) { // Designated Initializer
            width = 0
            height = 0
            super.init(name: name)
        }
            convenience init() {
            self.init(name: "unknown")
        }
            // Convenience Initializer는 항상 동일한 클래스의 다른 initializer를 호출
            // SuperClass의 Initializer를 직접 호출은 불가
    }
    ```

    Swift가 initializer를 상속하는 조건

    1.  SubClass의 모든 속성이 초기화 되어있고, Designated Initializer를 직접 구현하지 않았다면, SuperClass의 모든 Designated Initializer가 상속됨
    2. SubClass가 모든 Designated Initializer를 상속받았거나 Overriding했다면 모든 Convenience Initializer가 상속됨.
- Required Initializer

    ```swift
    required init(parameter) {
            initialization
    }
    ```

    모든 클래스는 이니셜라이저를 구현해야 함.

    ```swift
    class Figure {
        var name: String

        required init(name: String) {
            self.name = name
        }

        func draw() {
            print("draw \(name)")
        }
    }

    class Rectangle: Figure {
        var width = 0.0
        var height = 0.0
        // 만약 name parameter의 initializer사용을 강제하고 싶다면?
        init() {
            width = 0
            height = 0
            super.init(name: "unknown")
        }
        
        required init(name: String) {
            width = 0
            height = 0
            super.init(name: name)
        } // SuperClass와 완전히 동일한 상태로 구현
    }
    ```

- Initializer Delegation

    초기화 코드에 대한 중복 제거 및 효율적으로 초기화 하기 위해 사용

    ```swift
    struct Size {
       var width: Double
       var height: Double

       init(w: Double, h: Double) {
          width = w
          height = h
       }

       init(value: Double) {
          width = value
          height = value
       }
    } // 문법적 오류는 없으나 초기화 규칙이 바뀌면 모든 Initializer수정해야 함.
    ```

    모든 속성 초기화 Initializer를 구현하고 다른 Initializer를 호출하는 방식이 좋음

    ```swift
    struct Size {
       var width: Double
       var height: Double

       init(w: Double, h: Double) {
          width = w
          height = h
       }

       init(value: Double) {
          self.init(w: value, h: value)
       }
    }
    ```

    - Class

        상속을 지원하고 모든 Initializer가 순서대로 호출되도록 구현해야 함.

        ```swift
        Rule

        1. designated initializer는 반드시 SuperClass의 designated initializer를 호출해야 합니다.
        	Delegate Up
        2. convenience initializer는 동일한 클래스의 initializer를 호출해야 합니다.
        	Delegate Across
        3. convenience initializer를 호출했을때, 최종적으로 동일한 Class의 designated initializer가 호출되어야 합니다.
        ```

        ```swift
        class Figure {
            let name: String
            
            init(name: String) {
                self.name = name
            }
            
            convenience init() { // Delegate Across
                self.init(name: "unKnown")
            }
        }

        class Rectangle: Figure {
            var width = 0.0
            var height = 0.0
            
            init(n: String, w: Double, h: Double) {
                width = w
                height = h
                super.init(name: n) // Delegate Up
            }
            
            convenience init(value: Double) {
                self.init(n: "rect", w: value, h: value)
            }
        }

        class Square: Rectangle {
            convenience init(value: Double) {
                self.init(n: "Square", w: value, h: value)
            }
            
            convenience init() {
                self.init(value: 0.0)
            }
        }
        ```