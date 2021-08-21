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