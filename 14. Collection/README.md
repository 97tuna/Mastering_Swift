# 14. Collection

- Collection Overview
    - Array
    - Dictionary : Key, Value로 구성
    - Set

    Class로 구현되있는것은 Foundation Collection: NS 접두어
    구조체로 구현되어있는것은 Swift Collection

    Foundation

    - 객체 형식 저장
    - 가변 Collection
    - 불변 Collection 별도 제공
    - mutable

    Swift

    - 객체
    - 값 저장 가능
    - Keyword를 통해서 결정
    
- Array #1

    배열은 저장된 순서대로 저장됨 → ordered, Single Type

    0-based Index

    ```swift
    [element, element, element, element]
    ```

    ```swift
    import Foundation

    var a = [1, 3, 5, 21, 4, 6, 7]
    // 앞에서 부터 순서대로 저장
    print(type(of: a)) // "Array<Int>
    ```

    ```swift
    Array<T>
    [T] -> 마찬가지로 사용 가능

    // 둘다 사용 가능, 아래가 단축 문법
    ```

    ```swift
    import Foundation

    var a = [1, 3, 5, 21, 4, 6, 7]
    // 앞에서 부터 순서대로 저장
    print(type(of: a))

    var b: [Int] = [12]
    var c: Array<String>

    let emptyArray = Array<Int>()
    print(emptyArray)
    let emptyArray2 = [Int]()
    let emptyArray3 = [Int](repeating: 0, count: 10)

    a.count // 배열 크기
    a.count == 0
    a.isEmpty
    ```

    ```swift
    import Foundation

    let fruit = ["Apple", "banana", "Mango"]
    fruit[0]

    fruit[0..<1]
    fruit[fruit.startIndex]
    fruit[fruit.endIndex] // endIndex를 사용할 수 없는 이유는 endIndex의 이전이 마지막이기 때문

    fruit[fruit.index(before: fruit.endIndex)]
    fruit.first

    print(type(of: fruit.first))

    if let first = fruit.first {
        print(first)
        print(type(of: first))
    }
    ```