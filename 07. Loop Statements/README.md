# 07. Loop Statements

- for-in Loop

    ```swift
    print("HELLO")
    print("HELLO")
    print("HELLO")
    print("HELLO")
    print("HELLO")
    ~~
    // 계속 복사할 수 없음 ㅋㅋㅋ
    ```

    ```swift
    for LoopConstant in Range {
        Statements
    } // 범위연산자를 사용하여 지정, Range가 Int면 LoopConstant도 Int
    ```

    ```swift
    for i in 1...10 { // 10번 반복
        print("HELLO")
    }

    for _ in 1...10 { // 반복상수를 사용하지 않으면 _를 사용하여 생략 가능
        print("HELLO")
    }

    let num = 10
    var res = 1

    for _ in 1...num {
        res *= 2
    }
    res // 1024

    for value in stride(from: 0, to: 10, by: 3) {
        print(num) // 3의 배수만 출력
    }
    ```

    ```swift
    let list = ["HI", "MY", "NAME", "IS"]

    for i in range list {
        print(i) // 반복으로 나옴
    }

    for i in 2...9 {
        for j in 1...9 {
            print("\(i) * \(j) = \(i * j)") // 구구단 출력
        }
    }
    ```

- while Loop

    for in Loop → Range와 Collection, 범위에 따라 결정

    while Loop → Condition, 조건에 따라 반복횟수 결정

    ```swift
    while Condition {
        Statements
    }

    // Condition은 Boolean 형식
    ```

    ```swift
    var num = 1
    var sum = 0

    while num <= 100 {
        sum += num
    }
    print(sum)

    // 위 코드는 무한루프, num은 항상 1이기 때문에 그럼
    ```

    ```swift
    repeat {
        Statements
    } while Condition

    // 먼저 Statements를 실행하고 그 다음 Condition확인, C의 do while문과 동일
    ```

    ```swift
    num = 0
    repeat {
        num += 1
    } while num < 100
    print(sum)
    ```