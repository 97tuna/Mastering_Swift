# 12. Tuples

- Tuples

    두개이상의 값을 저장할 수 있음. Compound Type

    ```swift
    let t = (12, 34, 56) // Tuple 멤버
    // 괄호안에 저장하고 싶은 단어를 나열

    // 가상의 데이터 튜플로 저장하기

    let data = ("HTTP Request", 200, "OK", 11.22)

    print(Type of: data) // 자료형을 튜플로 감싸줌, (String, Int, String, Double)
    ```

    새 멤버 추가 및 삭제 불가, 단 수정은 가능!

    ```swift
    // 저장된 값 전문법으로 참조 가능
    Tuple.n // n은 0-based Index

    data.0 // 0은 String, 1은 Int, 2는 String, 3은 Double
    data.1
    data.2
    data.3

    data.1 = 404 // let이기 때문에 변경 불가, var로 수정하면 가능
    ```

    ```swift
    var mutableTuple = data // 값 형식이기 때문에 새로운 변수 선언시 복사 가능
    mutableTuple.1 = 404 // 404로 변경
    ```

- Named Tuples

    튜플 멤버에 이름을 붙이면 가독성이 높아짐

    ```swift
    let named = (body: "HTTP Request", status: 200, Message: "OK", size: 11.22)

    print(named.body)
    print(named.status)
    print(named.1)

    // TupleExpression.memberName
    ```

- Tuple Decomposition

    튜플 정보들을 개별 변수나 개별 상수로 저장하는 방법

    ```swift
    let (name1, name2, name3 ...) = tupleExpression
    var (name1, name2, name3 ...) = tupleExpression
    ```

    ```swift
    let data = ("HTTP Request", 200, "OK", 11.22)

    //  let body = data.0
    //  let code = data.1
    //  let msg = data.2
    //  let size = data.3
    // 가능

    let (type, num, msg, size) = data
    // 앞에서 부터 차례대로 저장
    // 이름의 숫자, 값의 숫자가 같아야함.
    ```

    ```swift
    let (type, num, msg, _) = data // 와일드 카드 사용하여 4번째 요소 분해 X
    ```

- Tuple Matching

    ```swift
    let res = (1920.0, 1080.0)

    if res.0 == 3840 && res.1 == 2160 {
        print("4K")
    }

    switch res {
    case let(w, h) where w / h == 16.0 / 9.0:
        print("16:9")
    case (_, 1080):
        print("FHD")
    case (3840...4096, 2160):
        print("4K")
    default:
        break
    }

    // 와일드 카드 조합하여 사용도 가능
    ```