# 08. Control Transfer Statements, Labeled Statements

- Control Transfer Statements

    Swift는 5가지 제어 전달문을 제공

    - Transfer Control

        ```swift
        for index in 1...10 {
            print("start")
            if index < 5 {
                continue
            }
            if index > 10 {
                break
            }
            print("end")
        }
        ```

- break Statement

    현재 실행중인 문장 중지, 다음 문장으로 제어 전달

    ```swift
    let num = 1

    switch num {
    case 1...10:
        print("begin block")

        if num % 2 != 0 {
            break
        }

        print("end block")
    default:
        break
    }

    print("Done")
    ```

    ```swift
    for i  in 1...10 {
        print(i)

        if i > 1 {
            break
        }
    }
    ```

    ```swift
    for i in 1...10 {
        print("OUTER LOOP", i)
            for i in 1...10 {
                    print("INNER LOOP", i)
                if j > 1 {
                    break // break는 인접한 루프를 정지시킴
                }			
            }
    }
    ```

- continue Statement

    무엇을 계속하고, 무엇을 종료할지 잘 알고 있어야 한다.

    ```swift
    for i in 1...15 {
        print(i)
    }

    for i in 1...15 {
        if i % 2 == 0 {
            continue // 짝수면 중지하고 다음 반복으로 이동
        }
        print(i)
    }

    // 출력은 1, 3, 5, 7, 9, 11, 13, 15
    ```

    ```swift
    for i in 1...10 {
        print("OUT")
        for j in 1...10 {
            if j % 2 == 0 {
                continue
            }
            print("IN")
        }
    }

    // 현재 반복을 중지하고 다음 반복으로 넘어감
    // 인접한 문장에 영향을 줌
    ```

- Labeled Statements

    Lable이 붙은 문장 → 문장에 이름을 붙이는 행위
    **동일한 Label을 가진 문장을 break 하거나 continue함**

    ```swift
    Label: Statements

    break: Label
    continue: Label
    ```

    ```swift
    for i in 1..5 {
        print("\(i): OUT")
        for j in 1..5 {
            print("\(j): IN")
            break
        }
    }

    // OUT은 5까지 돌고 IN은 1만 돎
    ```

    ```swift
    out: for i in 1..5 {
        print("\(i): OUT")
        for j in 1..5 {
            print("\(j): IN")
            break: out
        }
    }

    // break는 OUT문을 가진 문장에 효과를 줌.
    // OUT도 1, IN도 1만 출력하고 종료
    ```

    **바깥쪽의 for문이 종료되었기 때문에 안쪽도 종료해서 1만 출력된 것이다.**
    → 원하는 문장을 직접 종료할 때 사용