# 23. Memory, Value Type and Reference Type

- Memory Basics

    ```swift
    00010110 -> 22
    ```

    ```swift
    2의 보수
    OriginalBit -> ~ -> +1

    Code -> 기계어 영역
    Data -> 정적, 전역 변수, 프로그램 실행 시 생성 종료 시 삭제
    Heap -> 지역변수, 파라미터, Return값
    Stack -> 함수 호출의 모든 값이 저장, 함수 종료 시 삭제

    Ref Type은 실제값은 Heap에 저장, 그 주소를 Stack에 저장
    값을 읽어올 때 Stack의 주소를 통해 접근
    삭제할때는 Heap영역의 값을 정확히 지우는것이 중요.
    ```

- Value Type vs Reference Type
    - Value Type
        - Structure
        - Enumeration
        - Tuple

        ```swift
        struct SizeValue {
            var width = 0.0
            var height = 0.0
        }

        var value = SizeValue() // 메모리 생성

        var value2 = value // 값이 복사
        value2.width = 1.0
        value2.height = 2.0 // 값을 변경해도 Value의 값에는 영향을 미치지 않음

        value
        value2
        ```

    - Reference Type
        - Class
        - Closure

        ```swift
        class SizeObject {
            var width = 0.0
            var height = 0.0
        }

        var object = SizeObject()

        var object2 = object

        object2.width = 1.0
        object2.height = 2.0

        object
        object2 // 값이 같이 변경됨.

        let v = SizeValue() // 상수로 선언하면 모든 속성도 상수로 변경.
        v.height = 3 // 불가

        let o = SizeObject() // 메모리 주소는 상수로 변경 불가, 인스턴스 속성은 제한 없음.
        o.width = 1.0
        o.height = 2.0
        ```

        ```swift
        ==, != -> Stack의 값을 비교
        참조 형식은 ==, !=, ===, !== Heap의 값을 비교, 실제 값을 비교
        ```

- ARC(Automatic Reference Counting)
    - Ownership Policy vs Reference Count
        - 소유자가 있으면 메모리에 유지, 없으면 제거
        - 소유자 수가 참조 카운트, 1이면 유지, 0이면 제거

        ```swift
        retain -> 소유 확인, Reference Count + 1
        release -> 소유권 포기, Reference Count - 1
        메시지 전달

        0이면 즉시 메모리에서 제거
        ```

    - Manual Reference Counting → 메모리 누수 등 관리문제 해결을 위해 Automatic Reference Counting 도입
        - Strong Reference

            ```swift
            retain -> 소유 확인, Reference Count + 1
            release -> 소유권 포기, Reference Count - 1
            소유자가 존재하는 동안 메모리에서 삭제 안됨.
            ```

            ```swift
            class Person {
                var name = "John Doe"
                
                deinit {
                    print("person deinit")
                }
            }

            var p1: Person?
            var p2: Person?
            var p3: Person?

            p1 = Person() // 강한참조, 1증가
            p2 = p1
            p3 = p1
            // 마찬가지로 강한참조, 2증가라 총 3

            p1 = nil
            p2 = nil
            // 소유권 포기, 1씩 감소 총 2감소

            p3 = nil
            // 소유권 포기, Ref Count = 0임, 이때 메모리 제거, 소멸자 실행
            ```

- Strong Reference Cycle

    ```swift
    class Person {
        var name = "John Doe"
        var car: Car?
        
        deinit {
            print("person deinit")
        }
    }

    class Car {
        var model: String
        var lessee: Person?
        
        init(model: String) {
            self.model = model
        }
        
        deinit {
            print("car deinit")
        }
    }

    var person: Person? = Person() // 강한 참조
    var rentedCar: Car? = Car(model: "Porsche") // 강한 참조

    person?.car = rentedCar // Car instance Ref Count는 2
    rentedCar?.lessee = person // person instance Ref Count는 2

    person = nil // person instance Ref Count는 1
    rentedCar = nil // Car instance Ref Count는 1
    // 아직 메모리에서 다 내려가있지 않음 -> 서로 소유중이기 때문
    // 단 person, rentedCar둘다 nil이기 때문에 접근도 불가해서 삭제할 수 없음

    person = nil
    rentedCar = nil
    // 이렇게 지워도 불가.
    ```

    강한 참조로 인해 생기는 문제를 Strong Reference Cycle라고 함

    - **instance를 참조하지만 소유하지는 않음 →** 옵셔널이냐 옵셔널이 아니냐의 차이
        - Weak Reference

            ```swift
            weak var name: Type?
            ```

            ```swift
            class Person {
                var name = "John Doe"
                var car: Car?
                
                deinit {
                    print("person deinit")
                }
            }

            class Car {
                var model: String
                weak var lessee: Person?
                
                init(model: String) {
                    self.model = model
                }
                
                deinit {
                    print("car deinit")
                }
            }

            var person: Person? = Person()
            var rentedCar: Car? = Car(model: "Porsche")

            person?.car = rentedCar
            rentedCar?.lessee = person

            person = nil
            rentedCar = nil
            // 메모리 누수없이 정상적으로 종료 완료
            ```

        - Unowned Reference

            ```swift
            unowned var name: Type -> 해제된 instance에 접근하면 nil이 아닌 runtime에러
            ```

            ```swift
            class Person {
                var name = "John Doe"
                var car: Car?

                deinit {
                    print("person deinit")
                }
            }

            class Car {
                var model: String
                unowned var lessee: Person

                init(model: String, lessee: Person) {
                    self.model = model
                    self.lessee = lessee
                }

                deinit {
                    print("car deinit")
                }
            }

            var person: Person? = Person()
            var rentedCar: Car? = Car(model: "Porsche", lessee: person!)

            person?.car = rentedCar

            person = nil
            rentedCar = nil
            ```

- Unowned Optional Reference (Swift 5+)

    5버전부터 optional로 구현 가능

    ```swift
    class Person {
        var name = "John Doe"
        var car: Car?

        deinit {
            print("person deinit")
        }
    }

    class Car {
        var model: String
        unowned var lessee: Person?

        init(model: String, lessee: Person) {
            self.model = model
            self.lessee = lessee
        }

        deinit {
            print("car deinit")
        }
    }

    var person: Person? = Person()
    var rentedCar: Car? = Car(model: "Porsche", lessee: person!)

    person?.car = rentedCar

    person = nil

    rentedCar?.lessee // 참조했던것이 사라지면 자동으로 nil로 초기화됨.
    // weak var lessee: Person? 일 경우
    // unowned var lessee: Person? 경우 바로 에러
    // rentedCar?.lessee = nil 로 직접 초기화 해야 함.

    rentedCar = nil
    ```

    unowned는 잘 쓰지는 않음 → Type이 non-optional인 경우에만 unowned옵션 사용 권장

- Closure Capture List

    ```swift
    class Car {
        var totalDrivingDistance = 0.0
        var totalUsedGas = 0.0
        
        lazy var gasMileage: () -> Double = {
            return self.totalDrivingDistance / self.totalUsedGas
        }
        
        func drive() {
            self.totalDrivingDistance = 1200.0
            self.totalUsedGas = 73.0
        }
        
        deinit {
            print("car deinit")
        }
    }

    var myCar: Car? = Car()
    myCar?.drive()

    // myCar = nil // car 인스턴스 정상적으로 해제

    myCar?.gasMileage()
    // self는 myCar 인스턴스, Closure가 self를 통해 instance소유, 강한 참조 발생

    myCar = nil
    // 강한 참조 때문에 제대로 해제 안됨.
    ```

    ```swift
    { [list] (parameter) -> ReturnType in
            code
    }

    // 캡쳐할 대상을 콤마로 나열
    // Closure Capture사용시 in 키워드 생략 불가
    ```

    - Value Type

        ```swift
        { [ValueList] in
                code
        }
        ```

        ```swift
        var a = 0
        var b = 0
        let c = { [a] in print(a, b) }

        a = 1
        b = 2
        c() // a의 값은 Capture시점인 0이 출력
        ```

    - Reference Type

        ```swift
        { [weak instanceName, unowned instanceName] in
                statements
        } // 대상 이름앞에 weak또는 unowned(비소유) 참조로 사용
        ```

        ```swift
        class Car {
            var totalDrivingDistance = 0.0
            var totalUsedGas = 0.0
            
            lazy var gasMileage: () -> Double = { [weak self] in
            guard let strongSelf = self else { return 0.0 }
                return strongSelf.totalDrivingDistance / strongSelf.totalUsedGas
            }
            
            func drive() {
                self.totalDrivingDistance = 1200.0
                self.totalUsedGas = 73.0
            }
            
            deinit {
                print("car deinit")
            }
        }
        // 이렇게 수정가능
        // unwrapping하는 방법과 Optional Chaining사용해서 해결

        var myCar: Car? = Car()
        myCar?.drive()

        myCar?.gasMileage()

        myCar = nil
        // 정상적으로 해제 완료
        ```

        ```swift
        lazy var gasMileage: () -> Double = { [unowned self] in
                return self.totalDrivingDistance / self.totalUsedGas
        } // unowned 참조는 Closure실행전에 해제될 수 있음
        // Closure의 생명주기보다 같거나 더 길때 사용
        ```

- Explicit Strong Capture (Swift 5.3+)

    self붙이는 것을 Implicit Strong Capture라고 칭함

    ```swift
    struct PersonValue {
        let name: String = "Jane Doe"
        let age: Int = 0
        
        func doSomething() {
            DispatchQueue.main.asyncAfter(deadline: .now() + 1) {
                print(name, age)
            }
            DispatchQueue.main.asyncAfter(deadline: .now() + 1) {
            print(self.name, self.age)
        }
            DispatchQueue.main.asyncAfter(deadline: .now() + 1) { [self] in
            print(name, age) // Explicit Strong Capture
        }
        }
    }

    class PersonObject {
        let name: String = "John Doe"
        let age: Int = 0
        
        func doSomething() {
            DispatchQueue.main.asyncAfter(deadline: .now() + 1) { [self] in // 강한 참조
                print(name, age)
            // 강하게 참조시 앞에 self붙이기
            }
        }
    }
    ```