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

- Array #2

    ```swift
    import Foundation

    var alpha = ["A", "B", "C", "D", "E"]

    alpha.append("F") // 마지막에 추가할 때
    alpha.append(contentsOf: ["G", "H"]) // 추가하고 싶은 요소를 배열로 전달

    alpha.insert("I", at: 3) // at 다음에 넣을 수 있음
    alpha.insert(contentsOf: ["a", "b", "c"], at: 0)

    alpha[0...2] = ["X", "Y", "Z"] // 0부터 2까지만 변경
    alpha

    alpha.replaceSubrange(0...2, with: ["A", "B", "C"]) // replaceSubrange사용하여 변경

    alpha[0...2] = ["Z"] // 3개를 하나의 요소로 변경도 가능
    alpha

    alpha[0..<1] = [] // 원하는 범위 삭제도 가능
    alpha
    ```

    ```swift
    import Foundation

    var alpha = ["A", "B", "C", "D", "E"]

    alpha.remove(at: 0) // 하나만 삭제할 때 remove사용, 삭제된 요소 return해줌
    alpha

    alpha.removeFirst() // 첫번째 요소 삭제 및 요소 return
    alpha

    alpha.removeFirst(2) // 배열 앞부분 2개 삭제 및 return은 안함
    alpha

    alpha = ["A", "B", "C", "D", "E"]
    alpha.removeLast()

    alpha.removeAll() // 모든 요소 삭제
    //alpha.removeFirst() // 삭제할 요소 없기 때문에 오류
    alpha.popLast() // 배열이 비어있는데 지우면 nil 반환, 안전하다

    alpha = ["A", "B", "C", "D", "E"]
    alpha.popLast() // E반환

    alpha.removeSubrange(0...2)
    alpha
    ```

- Array #3

    ```swift
    import Foundation

    var alpha = ["A", "B", "C", "D", "E"]
    var alpha2 = ["a", "b", "c", "d", "e"]

    alpha == alpha2
    alpha != alpha2

    alpha.elementsEqual(alpha2)
    alpha.elementsEqual(alpha2) { (lhs, rhs) -> Bool in
        return lhs.caseInsensitiveCompare(rhs) == .orderedSame
    }
    ```

    ```swift
    import Foundation

    let numbers = [1, 3, 4, 5, 6, 7, 8]
    numbers.contains(2) // 배열에 있는지 없는지 확인

    numbers.contains { n -> Bool in
        return n % 2 == 0
    }

    numbers.first {
        return $0 % 2 == 0
    }

    numbers.firstIndex {
        return $0 % 2 == 0
    }

    numbers.firstIndex(of: 1) // 가장 먼저 검색된 1의 index

    numbers.lastIndex(of: 3)
    ```

    sort → 배열을 직접 정렬
    sorted → 정렬된 새로운 배열을 리턴

    ```swift
    import Foundation

    let numbers = [1, 3, 4, 5, 6, 7, 8]

    numbers.sorted()
    numbers

    numbers.sorted { a, b in
        return a > b
    }

    let nums = [Int](numbers.sorted().reversed())
    ```

    ```swift
    import Foundation

    let numbers = [1, 3, 4, 5, 6, 7, 8]

    var mutableNumbers = numbers
    mutableNumbers.sort()
    mutableNumbers.reverse()
    ```

    ```swift
    import Foundation

    let numbers = [1, 3, 4, 5, 6, 7, 8]

    var mutableNumbers = numbers

    mutableNumbers.swapAt(0, 2) // 0과 2의 위치가 변경
    mutableNumbers.shuffle()
    ```

- Dictionary #1

    ```swift
    [Key: Value, Key: Value, Key: Value, Key: Value ...]
    ```

    ```swift
    import Foundation

    var dict = ["A": "Apple", "B": "Base", "C": "Character"]

    dict = [:] // 초기화
    ```

    ```swift
    Dictionary<K, V>
    [K:V] // Key와 Value의 자료형이 다름

    let dict1: Dictionary<String, Int>
    let dict2: [String: Int]
    ```

    ```swift
    // 빈 배열 구현
    import Foundation

    let word = ["A": "America", "B": "Bus", "C": "Char"]
    //let emptyDict = [:] // 추론 에러
    let emptyDict: [String: Int] = [:] // 추론 에러

    let emptyDict2 = [String: String]() // 빈 Dict 구현
    let emptyDict3 = Dictionary<String, String>()
    ```

    ```swift
    // 빈 배열인지 확인
    word.isEmpty

    // 요소 접근
    word["A"] // Key전달 Value return
    word["America"] // 값 전달해도 키로 확인, nil
    // 무조건 Key를 통해서 값 확인

    let a = word["B"] // a는 옵셔널
    // 상수에는 B의 값 전달

    let b = word["A", default: "Empty"] // 옵셔널 아님

    // Key값 출력
    for i in word.keys.sorted() {
        print(i)
    }

    for v in word.values {
        print(v)
    }
    // Value 출력

    // 배열로 변경하고 싶다면?
    let keys = Array(word.keys)
    let values = Array(word.values)
    ```