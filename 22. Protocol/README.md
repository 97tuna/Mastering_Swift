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