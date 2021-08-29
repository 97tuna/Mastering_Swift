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
    [T]

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

- Dictionary #2

    ```swift
    import Foundation

    var dict = [String: String]()

    dict["A"] = "America"
    dict["B"] = "Bus"

    dict.count
    dict

    dict["B"] = "Bracket" // 있으면 값 변경, 새 요소 추가되지 않음
    dict

    dict.updateValue("Chess", forKey: "C") // 새로운 요소로 추가되었으면 nil 반환
    dict.updateValue("Ace", forKey: "A") // 새로운 요소로 추가 안되면 이전 값 return
    dict
    ```

    ```swift
    import Foundation

    var dict = [String: String]()

    dict["A"] = "America"
    dict["B"] = "Bus"

    dict.count
    dict

    dict["B"] = "Bracket" // 있으면 값 변경, 새 요소 추가되지 않음
    dict

    dict.updateValue("Chess", forKey: "C") // 새로운 요소로 추가되었으면 nil 반환
    dict.updateValue("Ace", forKey: "A") // 새로운 요소로 추가 안되면 이전 값 return
    dict

    // 삭제
    dict
    dict["B"] = nil // nil전달시 삭제
    dict
    dict["K"] = nil // 존재하지 않으면 에러없이 그냥 종료

    dict.removeValue(forKey: "A") // 삭제된 값을 return
    dict.removeValue(forKey: "A") // 이미 삭제 되있으면 nil return

    dict.removeAll() // 전체 삭제
    ```

- Dictionary #3

    ```swift
    import Foundation

    var dict1 = ["A": "America", "B": "Bus", "C": "Char"]
    var dict2 = ["A": "America", "C": "Char", "B": "bus"]

    dict1 == dict2
    dict1 != dict2

    // 대소문자 구분
    dict1.elementsEqual(dict2) {
        print($0.key, $1.key)
        return ($0.key.caseInsensitiveCompare($1.key) == .orderedSame) && ($0.value.caseInsensitiveCompare($1.value) == .orderedSame)
    } // 코드 변경 없음에도 결과가 달라짐

    let a = dict1.keys.sorted()
    let b = dict2.keys.sorted()

    a.elementsEqual(b) {
        guard $0.caseInsensitiveCompare($1) == .orderedSame else {
            return false
        }
        
        guard let lv = dict1[$0], let rv = dict2[$1], lv.caseInsensitiveCompare(rv) == .orderedSame else {
            return false
        }
        return true
    }
    ```

    ```swift
    var dict = ["A": "America", "B": "Bus", "C": "Char"]

    let search: ((String, String)) -> Bool = {
        $0.0 == "B" || $0.1.contains("a")
    }

    dict.contains(where: search) // 하나라도 true라면 return true
    dict.first(where: search)

    let res = dict.first(where: search)
    res?.key
    res?.value

    dict.filter(search) // 만족시키는 모든 요소가 새로운 형태로 return
    ```

- Set #1

    ```swift
    let arr = [1, 2, 3, 4, 4, 5, 6, 7, 7, 8]
    arr.count
    // 배열은 중복요소 가능, Set은 불가

    let set: Set<Int> = [1, 2, 3, 4, 4, 5, 6, 7, 7, 8]
    set.count
    set.isEmpty

    // 요소가 포함되어 있는지 확인
    set.contains(5) // 있으면 true return, Hashing이라 빠름
    ```

    ```swift
    import Foundation

    var words = Set<String>()
    var insertRes = words.insert("Swift")
    insertRes.memberAfterInsert
    insertRes.inserted

    insertRes = words.insert("Swift")
    insertRes.memberAfterInsert
    insertRes.inserted // 중복 요소 허용 X, false return

    var updateRes = words.update(with: "Swift") // 없으면 추가, 있으면 그냥 반환
    updateRes

    updateRes = words.update(with: "Banana") // 없으면 추가 nil 반환
    updateRes

    var value = "Swift"
    value.hashValue

    updateRes = words.update(with: value) // 교체한 문자로 전달
    updateRes

    value = "Hello"
    value.hashValue

    updateRes = words.update(with: value) // 교체한 문자로 전달
    updateRes
    ```

    ```swift
    import Foundation

    struct SampleData: Hashable {
        var hashValue: Int = 123
        var data: String
        
        init(_ data: String) {
            self.data = data
        }
        
        static func == (lhs: SampleData, rhs: SampleData) -> Bool {
            return lhs.hashValue == rhs.hashValue
        }
    }

    var sampleSet = Set<SampleData>()
    var data = SampleData("Swift")
    data.hashValue

    var r = sampleSet.insert(data)
    r.inserted
    r.memberAfterInsert
    sampleSet

    data.data = "Hello"
    data.hashValue

    r = sampleSet.insert(data)
    r.inserted // Hash값이 일치해서 삽입되지 않음
    r.memberAfterInsert
    sampleSet

    sampleSet.update(with: data) // Set에 저장된 동일한 요소를 반영하기 위해서 사용
    sampleSet
    ```

    ```swift
    words
    words.remove("Swift")
    words

    words.remove("Swift11") // 존재하지 않는 요소 삭제 시 nil return
    words.removeAll() // 전체 삭제
    ```

- Set #2

    ```swift
    import Foundation

    var a: Set = [1, 2, 3, 4, 5, 6, 7, 8, 9]
    var b: Set = [1, 3, 5, 7, 9]
    var c: Set = [2, 4, 6, 8, 10]
    var d: Set = [1, 7, 9, 5, 3]

    a == b
    a != b

    b == d

    b.elementsEqual(d) // 순서대로 비교하기 때문에 False, set은 정렬되지 않음

    // set을 배열로 바꾸고 비교해야함
    a.isSubset(of: a) // 부분 집합
    a.isStrictSubset(of: a) // 진 부분집합

    // 하위 집합인지 확인
    b.isSubset(of: a) // 부분 집합
    b.isStrictSubset(of: a) // 진 부분집합, 나 자신을 제외한 부분집합인지 확인

    // 상위 집합인지 확인
    a.isSuperset(of: a)
    a.isStrictSuperset(of: a)

    a.isSuperset(of: b)
    a.isStrictSuperset(of: b)

    a.isSuperset(of: c)
    a.isStrictSuperset(of: c)

    a.isSuperset(of: d)
    a.isStrictSuperset(of: d)

    // 교집합
    a.isDisjoint(with: b) // 교집합이 있으면 return false
    a.isDisjoint(with: c)
    a.isDisjoint(with: d)
    ```

    ```swift
    import Foundation

    var a: Set = [1, 2, 3, 4, 5, 6, 7, 8, 9]
    var b: Set = [1, 3, 5, 7, 9]
    var c: Set = [2, 4, 6, 8, 10]

    var res = b.union(c) // b와 c의 합집합이 새롭게 return

    res = b.union(a) // 새로운 set return, 원본 유지

    // 원본 변경
    b.formUnion(c) // let선언은 불가, 원본을 변경하기 때문

    a = [1, 2, 3, 4, 5, 6, 7, 8, 9]
    b = [1, 3, 5, 7, 9]
    c = [2, 4, 6, 8, 10]

    // 교집합
    res = a.intersection(b)
    res = c.intersection(b)

    // 원본 변경은 form 메소드 사용
    a.formIntersection(b)
    a

    b.formIntersection(c) // 교집합 없으면 빈 Set전달

    a = [1, 2, 3, 4, 5, 6, 7, 8, 9]
    b = [1, 3, 5, 7, 9]
    c = [2, 4, 6, 8, 10]

    // 여집합

    res = a.symmetricDifference(b) // 교집합을 제외한 새로운 값이 Set전달
    res = c.symmetricDifference(b)

    a.formSymmetricDifference(b) // 원본 변경

    a = [1, 2, 3, 4, 5, 6, 7, 8, 9]
    b = [1, 3, 5, 7, 9]
    c = [2, 4, 6, 8, 10]

    // 차집합
    res = a.subtracting(b)

    a.subtract(b) // 원본 변경
    ```

- Iterating Collections

    ```swift
    for element in collection {
        Statements
    }
    ```

    ```swift
    import Foundation

    print("Array", "==============")

    let arr = [1, 2, 3]

    for num in arr {
        print(num)
    }
    // 배열에 저장된 요소 수 만큼 반복, 루프상수로 전달됨

    print("Set", "==============")
    let set: Set = [1, 2, 4]

    for num in set {
        print(num)
    }
    // 배열과 동일하게 전달되나, 출력되는 순서는 실행할 때 마다 달라짐

    print("Dictionary", "==============")
    let dict = ["A": 1, "B": 2, "C": 3]
    for (key, value) in dict {
        print(key, value)
    }
    // 출력되는 순서는 실행할 때 마다 달라짐
    ```

    ```swift
    forEach

    import Foundation

    print("Array", "==============")

    let arr = [1, 2, 3]

    arr.forEach {
        print($0)
    }
    // 배열에 저장된 요소 수 만큼 반복, 루프상수로 전달됨

    print("Set", "==============")
    let set: Set = [1, 2, 4]

    set.forEach {
        print($0)
    }
    // 배열과 동일하게 전달되나, 출력되는 순서는 실행할 때 마다 달라짐

    print("Dictionary", "==============")
    let dict = ["A": 1, "B": 2, "C": 3]
    dict.forEach {
        print($0.key, $0.value)
    }
    // 출력되는 순서는 실행할 때 마다 달라짐
    ```

    ```swift
    func withForIn() {
        print(#function)
        let arr = [1, 2, 4]
        for num in arr {
            print(num)
            return // 바로 종료
        }
    }

    func withForEach() {
        print(#function)
        let arr = [1, 2, 4]
        arr.forEach {
            print($0) // 반복문이 아니기 때문에, Break, Continue사용 불가
            return // 외부에는 영향 미치지 않음, 반복 횟수도 마찬가지
        }
    }

    withForIn()
    withForEach()
    ```

- KeyValuePairs 🥕

    Swift가 제공하는 경량 컬렉션!
    동일한 Key를 중복 사용할 때, 순서를 꼭 지켜야 할 때 Dict대신 사용

    ```swift
    import Foundation

    let words: KeyValuePairs<String, String> = ["A": "Apple", "B": "Bus", "C": "Car"]
    // 형식은 생략해도 가능

    // Key형식에 제한 없음, 동일 Key 사용 가능, 저장 순서 유지 가능
    // Dict에 비해 느리지만, 대량의 용량 아니라면 큰 차이 없음

    words.count
    words.isEmpty

    // words["A"] // 불가
    // 배열처럼 Index로 접근
    words[0]
    words[0].key
    words[0].value

    for i in words {
        print(i.key, i.value)
        print(i)
    }

    // 동일한 순서대로 출력, Dict의 형태로 한 배열, 단 기능은 적음
    // Append, Insert, Update, Remove 불가능
    ```