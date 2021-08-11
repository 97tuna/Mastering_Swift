# 06. Conditional Statements

- if Statement

    ```swift
    Token // 나눌 수 없는 가장 작은 단위

    if Condition { // Condition Boolean 표현식
        Statements
    }

    let id = "Root"
    let pw = "1q2w3e4r"

    if id == "Root" {
        print("valid id")
    }

    if pw == "1q2w3e4r" {
        print("valid password")
    }
    ```

    - 논리 연산자 결합

        ```swift
        if id == "Root" && pw == "1q2w3e4r" {
            print("valid Admin")
        } // 단락 평가 진행, Swift
        ```

        - 잘못 입력할 경우 처리

            ```swift
            print("틀린 값") // if에 관계없이 항상 처리

            if id != "root" || password != "1q2w3e4r" {
                print("틀린 값")
            }
            ```

    - else 구문 포함

        ```swift
        if Condition { // Condition Boolean 표현식
            Statements
        } else {
            Statements
        }
        ```

        ```swift
        if id == "Root" && pw == "1q2w3e4r" {
            print("valid Admin")
        } else {
            print("틀린 값")
        }
        ```

    - else if

        ```swift
        if Condition { // Condition Boolean 표현식
            Statements
        } else if Condition {
            Statements
        } else {
            Statements
        }
        // else는 하나만 작성 가능, else if 는 생략 가능
        ```

        ```swift
        let num = 100

        if num >= 0 {
            print("양수입니다")
        } else if num % 2 == 0 {
            print("2로 나누어 집니다")
        } else if num % 2 == 1 {
            print("홀수입니다")
        } else {
            print("음수입니다")
        }

        // print("양수입니다")만 출력, 항상 if문만 실행되고 나머지는 끝.
        ```

        ```swift
        let num = 100

        if num >= 0 {
            if num % 2 == 0 {
                print("2로 나누어 집니다")
            } else {
                print("홀수입니다")
            }
        } else {
            print("음수입니다")
        }

        // 깔끔해진 코드
        ```

        ```swift
        if num > 0 {
            print("양수")
        } else if num > 10 {
            print("10보다 큰 양수")
        } else if num > 100 {
            print("100보다 큰 양수")
        }

        // 가장 까다로운 조건이 먼저 와야함.
        ```

- switch Statement

    값의 일치 여부에 따라 실행할 결과가 달라짐

    ```swift
    switch ValueExpression {
    case pattern:
        statements
    case pattern, pattern: // 필요에 따라 매칭시킬 패턴을 콤마로 이음
        statements
    defalut:
        statements
    }
    ```

    ```swift
    let num = 1

    switch num {
    case 1:
        print("One")
    case 2, 3:
        print("Two or Three")
    } // default문이 없어서 오류, switch문은 모든 case를 처리할 수 있어야 함.
    ```

    ```swift
    switch num {
    case 1:
        print("One")
    case 2, 3:
        print("Two or Three")
    default:
        break // 아무것도 하지 않을 때
    }
    ```

    ```swift
    switch num {
    case let n where n <= 10:
        print(n)
    default:
        print("other")
    }

    // 다음 강의에서 소개
    ```

    ```swift
    // 범위 Interval, Interval Matching

    let temp = -8

    switch temp {
    case ..<10:
        print("추움")
    case 11...20:
        print("선선")
    case 21...27:
        print("낭낭")
    case 28...:
        print("뜨끈")
    default:
        break
    }
    ```

    - fall through

        ```swift
        switch num {
        case 1:
            print("One")
            fallthrough
        case 2, 3:
            print("Two or Three")
        default:
            break // 아무것도 하지 않을 때
        }

        // fallthrough 이어지는 블록까지 실행, case에 맞는 값이 아니어도 진행
        ```

    ```swift
    let tempp = 10

    switch tempp {
    case ..<10:
        print("warn")
    case 10:
        print("warn") // 여기서 걸림
        print("reset") // 여기도
    default:
        print("reset")
    }

    switch tempp {
    case ..<10:
        print("warn")
    case 10:
        print("warn")
        fallthrough // 위와 같이 동일 작동, 코드 중복을 줄일 수 있음
    default:
        print("reset")
    }
    ```

- guard Statement

    ```swift
    guard let Condition else {
        Statements
    }

    // guard는 else 생략 불가, Statement에서는 종료되는 구문을 적어줘야 함.

    guard optionalBinding else {
        Statements
    }

    // guard와 if문의 차이점
    // if가 늘어날수록 코드가 중첩될 수 있음. guard는 코드가 깔끔해질 수 있음.
    ```

    ```swift
    func validate(id: String?) -> Bool {
        guard let id = id else {
            return false
        }

        guard id.count >= 10 else {
            return false // id.count >= 10가 false면 else문 출력
        }

    //	guard let id = id, id.counnt >= 10 else {
    //		return false // 이거도 가능
    //	}

        return true
    }

    validate(id: nil) // nil은 문자열 없음을 의미

    validate(id: "aaaa") // false 호출, 첫번째 guard에서는 true이나 두번째 guard에서 false로 변경

    validate(id: "sswwiifftt") // true 반환
    ```

    ```swift
    func validateUsingIf() {
        var id: String? = nil

        if let str = id P
            if str.count >= 10 {
                print(str)
            }
        }
    }

    func validateUsingGuard() {
        var id: String? = nil
        guard let str = id else { return }
        guard str.count >= 10 else { return }
        print(str)
    }
    ```

- Value Binding Pattern  🥕

    ```swift
    case let name:
    case var name:

    let a = 1

    switch a {
    case let x:
        print(x) // 1출력
    }

    switch a {
    case let x where x > 100: // x가 100을 초과하는 경우에만
        print(x) // 1출력
    default"
        break;
    }

    // switch 매칭상수 a찾고, a를 x로 바인딩함. a의 값이 x로 복사되어서 출력
    // 주로 where절과 함께 사용됨
    ```

    ```swift
    switch a {
    case var x where x > 100: // x가 100을 초과하는 경우에만
        x = 200
        print(x) // 1출력
    default"
        break;
    }
    ```

    ```swift
    let pt = (1, 2)

    switch pt {
    case let(x, y):
        print(x, y)
    case (let x, let y):
        print(x, y)
    case (let x, var y):
        break;
    case (let x, _ y): // 필요없는값도 가능
    }
    ```

- Expression Pattern

    ```swift
    let a = 1

    switch a {
    case 0...10:
        print("0~10")
    default:
        break
    }

    // Pattern 매칭, Interval 매칭
    a ~= b

    struct Size {
        var width = 0.0
        var height = 0.0
    }

    let s = Size(width: 10, height: 20)

    switch s {
    case 0...9:
        print("0~10")
    default:
        break
    } // 오류남
    ```

    ```swift
    struct Size {
        var width = 0.0
        var height = 0.0

        // 자료형과 순서가 매우 중요, Pattern Matching시에
        static func ~= (left: Range<int>, right: Size) -> Bool {
            // 왼쪽에서는 Range로 오른쪽에는 비교할 그 자료형을
            return left.contains(Int(right.width))
        }
    }

    switch s {
    case 0...9:
        print("0~10")
    default:
        break
    } // 정상적으로 실행
    ```

    [Patterns - The Swift Programming Language (Swift 5.5)](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html)