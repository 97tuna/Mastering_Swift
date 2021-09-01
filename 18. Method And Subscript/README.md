# 18. Method And Subscript

- Instance Method

    특정 형식에서 속하는 함수 → Method

    함수는 함수 이름으로 호출, 인스턴스는 인스턴스 이름으로 호출!

    ```swift
    func name(parameters) -> ReturnType {
        Code
    }

    instance.method(parameter)
    ```

    ```swift
    import Foundation

    class Sample {
        var data = 0
        static var sharedData = 111
        
        func doSomething() {
            print(data)
            // instance에서 TypeMember에 접근하려면
            Sample.sharedData // 로 접근해야함.
        }
        
        func call() {
            doSomething()
        }
    }

    let doA = Sample()
    doA.data // data 속성에 접근, data가 instance속성이기에 이름으로 접근
    doA.doSomething()
    doA.call()

    class Size {
        var width = 0.0
        var height = 0.0
        
        func larger() {
            width += 1.0
            height += 1.0
        }
    }

    let s = Size()
    s.larger()
    ```

    ```swift
    struct Sizes {
        var width = 0.0
        var height = 0.0
        
        mutating func larger() {
            width += 1.0
            height += 1.0
        }
    }
    var k = Sizes()
    k.larger()
    ```

    값 형식에서 속성을 바꾸는 메소드를 구현하면 mutating으로 구현

- Type Method

    메소드도 구분

    - Instance Method
    - type Method

    ```swift
    import Foundation

    static func name(parameters) -> ReturnType {
        statesment
    } // Overring금지

    class func name(parameters) -> ReturnType {
        statesment
    } // Subclass에서 Overring을 허용할 때 사용 -> class

    Type.method(parameter)
    ```

    ```swift
    class Circle {
        static let pi = 3.14
        var radius = 0.0
        
        // instance method
        func getArea() -> Double {
            return radius * radius * Circle.pi // Type속성은 Type이름으로
        }
        static func printPi() {
            print(pi) // Type Method는 Type 속성에 바로 접근 가능
        }
    }
    ```

    ```swift
    class StrokeCircle: Circle {
        override static func printPi() { // 에러
            print(pi) // static키워드로 선언한 method를 subclass에서 override할 수 없다.
        }
    }

    위 코드의 static func printPi()를 class func printPi() 하면 가능
    ```