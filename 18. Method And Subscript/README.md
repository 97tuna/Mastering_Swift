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

- Subscript

    ```swift
    instance[index]
    instance[key]
    instance[range]
    ```

    ```swift
    let list  = ["A", "B", "C"]

    list[0] // Subscript
    ```

    ```swift
    subscript(parameter) -> ReturnType { // 대부분 2개 이하 parameter
        get {
            return expression // Return과 저장하는 Type이 같이 사용, 생략 불가
        }
        set(name) {
            statement
        }
    } // get과 set을 같이 구현하면 subscript에서 읽고 쓰기 가능.
    ```

    ```swift
    class List {
        var data = [1, 2, 3]
        
        subscript(index: Int) -> Int {
            get {
                return data[index]
            }
            set {
                data[index] = newValue
            }
        }
    }

    var l = List()
    l[0] // get블록 실행

    l[1] = 123 // set블록 실행
    l[1]

    l[0, 1] // 파라미터 일치 subscript가 없기 때문에 오류
    l["Strong"] // 에러, Type 불일치
    ```

    ```swift
    class List {
        var data = [1, 2, 3]
        
        subscript(i index: Int) -> Int {
            get {
                return data[index]
            }
            set {
                data[index] = newValue
            }
        }
    }

    var l = List()
    l[i: 0] // get블록 실행

    l[i: 1] = 123 // set블록 실행
    l[i: 1]
    // Argument Label 안쓰면 에러, subscript에서는 잘 쓰지 않는다.
    ```

    ```swift
    struct Matrix {
        var data = [[1, 2, 3],
                    [4, 5, 6],
                    [7, 8, 9]]
        subscript(row: Int, col: Int) -> Int {
            return data[row][col] // 바로 return, 읽기 전용 subscript
        }
    }

    let m = Matrix()
    m[0][0] // 에러, 두개의 파라미터지만 에러
    // 첫번째 subscript가 문제없이 평가된다면, 정수를 return하는데 이 정수에서 다시 subscript를 실행할 수 없기 때문에 에러가 발생한다.

    // 두 개 이상의 값을 전달할때는
    m[0, 0] // 이렇게 콤마로 구분하여 전달
    ```

    ```swift
    // 만약 범위를 넘어서는 index가 들어오게 된다면 어떻게 할 것인가?
    import Foundation

    struct Matrix {
        var data = [[1, 2, 3],
                    [4, 5, 6],
                    [7, 8, 9]]
        subscript(row: Int, col: Int) -> Int {
            print(data.count)
            if col > data.count && row > data.count {
                precondition(true) // 이렇게 처리
            }
            return data[row][col]
        }
    }
    ```