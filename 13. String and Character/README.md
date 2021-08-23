# 13. String and Character

- Strings and Characters

    ```swift
    let str = "String" // 문자열
    let char = "C" // 문자열
    // 항상 문자열로 추론

    let ch: Character = "C"

    //  let empty: Character = "" // 빈 문자열은 큰따옴표 사이에 공백 추가
    let empty = "" // 빈 문자열
    print(type(of: empty))
    print(empty.count)

    let empty2 = String() // 생성자 사용
    ```

    String → Swift String, NSString → Foundation String

    ```swift
    import Foundation

    var nsstr: NSString = "Str"
    let swstr: String = nsstr as String // 두 자료형 호환 불가, as로 인한 Type Casting 진행

    nsstr = swstr as NSString // Type Casting 진행, Toll-Free Casting, 유니코드 처리방식 상이
    ```

    ```swift
    let immuStr = "HI"
    //  immuStr = "Swift" 불가

    var mutaStr = "HI"
    mutaStr = "Swift"
    // 변수는 수정 및 추가 삭제 가능
    ```

    ```swift
    let str = "Swift String" // 유니코드 독립

    str.utf8
    str.utf16
    // 문자열 그냥 사용해도 무방, 이렇게 변경할거면 공식 문서 참고

    var str1 = "😶‍🌫️"
    str1 = "\u{1F636} \u{200D} \u{1F32B} \u{FE0F}"
    print(str1)

    //  😶‍🌫️
    //  구름 속의 얼굴
    //  유니코드: U+1F636 U+200D U+1F32B U+FE0F, UTF-8: F0 9F 98 B6 E2 80 8D F0 9F 8C AB EF B8 8F
    ```

- Multiline String Literals

    ```swift
    let str = "Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum."

    let multiLine = """
    Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.
    """
    // 명시적인 줄바꿈 허용
    ```

    1. 큰 따옴표 조심, 같은 라인에서 시작하면 오류 발생
    2. 마지막 큰 따옴표는 한줄로 단독 사용
    3. 첫번째 줄과 동일선상 또는 왼쪽에 있어야 함. → 마지막 큰 따옴표가 문자열 들여쓰기의 기준이 됨
    4. \ 사용시 줄 바꿈 사라짐.
- Raw Strings (Swift 5+) 🥕

    ```swift
    var str = "\"Hello\", Swift"
    var rawStr = #"\"Hello\", Swift"#

    print(str)
    print(rawStr) // \문자도 같이 출력

    str = "Lorem\nIpsum"
    rawStr = #"Lorem\#nIpsum"# // #문자 추가시 Escapse Squence로 변경 
    rawStr = ###"Lorem\#nIpsum"### // #문자 수 일치해야함.

    print(str)
    print(rawStr)

    let value = 123
    rawStr = #"Hi, the value is  \#(value)"# // 모두 다 # 붙여서

    print(rawStr)
    ```

    ```swift
    // 정규식도 마찬가지로 # 붙여서 사용

    import Foundation

    var zipCodeReg = "^//d{3}-?//d{3}$"
    zipCodeReg = #"^/d{3}-?/d{3}$"# // 가독성 증가

    let zipCode = "123-456"
    if let _ = zipCode.range(of: zipCodeReg, options: [.regularExpression]) {
        print("Valid")
    }
    ```

- String Interpolation

    동적 문자열 구성

    ```swift
    \(expression)
    ```

    ```swift
    var str = "11.22GB"

    let size = 11.22
    str = String(size) + "GB" // Double과 String의 연결은 불가
    print(str)

    str = "\(size)GB"
    print(str)
    ```

    ```swift
    %char // 포맷 지정자, format String
    ```

    ```swift
    import Foundation

    var size = 11.22
    var str = String(format: "%.1fKB", size)

    print(str)

    String(format: "Hello, %@", "Swift")
    // 문자 대체 @
    String(format: "%d", 12)
    // 정수 대체 d, 실수 대체 f
    // 정수 대체 사용 후 소수 출력시 0 출력

    String(format: "%.3f", 12.122)
    // 3자리 소수점 출력

    String(format: "%010.3f", 12.122)
    // 총 10자리 소수점 출력

    String(format: "[%d]", 123)
    String(format: "[%10d]", 123) // 빈공간 오른쪽으로 정렬
    String(format: "[%-10d]", 123) // 빈공간시 왼쪽으로 정렬

    let firstName = "DongHeon"
    let lastName = "Lee"

    let korFormat = "나의 이름은 %2$@ %1$@입니다." // 순서 조정도 가능
    let engFormat = "My Name is  %@ %@."

    String(format: korFormat, firstName, lastName) // 빈공간 오른쪽으로 정렬
    String(format: engFormat, firstName, lastName) // 빈공간시 왼쪽으로 정렬
    ```

    [String Format Specifiers](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Strings/Articles/formatSpecifiers.html#//apple_ref/doc/uid/TP40004265-SW1)

    ```swift
    str = "\\" // Placeholder로 사용, 문자열에 직접 저장 불가
    print(str) // Escape Sequence

    print("A\tB")
    print("A\nB")
    ```

- New String Interpolation System (Swift 5+) 🥕

    ```swift
    import Foundation

    struct Size {
        var width = 12.34
        var height = 34.56
    }

    let s = Size(width: 1.2, height: 3.4)
    print(s)

    extension Size: CustomStringConvertible {
        var description: String {
            return "\(width) x \(height)"
        }
    }

    extension String.StringInterpolation {
        mutating func appendInterpolation(_ value: Size) {
            appendInterpolation("\(value.width) x \(value.height)")
        }
        
        mutating func appendInterpolation(_ value: Size, style: NumberFormatter.Style) {
            let formatter = NumberFormatter()
            formatter.numberStyle = style
            
            if let width = formatter.string(for: value.width), let height = formatter.string(for: value.height) {
                appendInterpolation("\(width) x \(height)")
            } else {
                appendInterpolation("\(value.width) x \(value.height)")
            }
        }
    }

    print("\(s)")
    print("\(s, style: .spellOut)")
    ```

- String Index

    ```swift
    import Foundation

    let str = "Swift"

    print(str[str.startIndex])

    let lastCharIdx = str.index(before: str.endIndex)
    // endIndex는 마지막 문자 다음 인덱스를 가리킴

    print(str[lastCharIdx])
    print(str[lastCharIdx])

    let secondCharIdx = str.index(after: str.startIndex)
    print(str[secondCharIdx])

    var thirdCharIdx = str.index(str.startIndex, offsetBy: 2)
    var thirdCh = str[thirdCharIdx]
    print(thirdCh)

    thirdCharIdx = str.index(str.endIndex, offsetBy: -3) // 뒤에서 볼 때는 -로 전달
    print(str[thirdCharIdx])

    if thirdCharIdx < str.endIndex && thirdCharIdx >= str.startIndex {
        
    }
    // 유니코드로 독립적인 형태로 문자열을 관리하기 때문에 숫자 인덱스로 불가능
    ```

- String Basics

    ```swift
    var str = "Hello, Swift!"

    str = "" // 빈문자열
    str = String() // 빈문자열
    ```

    Bool → String

    ```swift
    let a  = String(true)
    print(a) // 단순 문자열, true
    ```

    Int, Double → String

    ```swift
    let b = String(12)
    let c = String(11.22)
    ```

    ```swift
    let d = String(str)

    let hex = String(123, radix: 16)
    print(hex)

    let octa = String(123, radix: 8)

    let bi = String(123, radix: 2)
    ```

    ```swift
    var repeatStr = "😀😀😀😀😀😀"
    repeatStr = String(repeating: "😀", count: 100)

    // 반복도 가능
    let unicode = "\u{1f44f}"
    ```

    ```swift
    let e = "\(a) \(b)"
    let e = "\(a) \(b)"
    let f = a + b // 가능
    f = a + " " + b
    str += "!!"
    ```

    ```swift
    str.count // 문자열 길이 return 정수로
    str.count == 0 // 문자열 비어있음
    str.isEmpty
    ```

    ```swift
    "apple" != "Apple"

    "apple" < "Apple" // 사전순으로 비교, 같은 문자일경우 아스키 코드로 비교

    str.lowercased() // 모든 문자열 소문자로 변경
    str.uppercased() // 모든 문자열 대문자로 변경

    str.capitalized() // 첫글자만 대문자로 변경, 단어의 첫 문자 ㅇㅇ
    ```

    **ed로 끝나는 메소드는 원본은 건드리지 않음, ing도 마찬가지**

    ```swift
    for char in "Hello" {
        print(char)
    }

    // 문자열은 시퀀스라 가능
    let num = "1234567890"
    num.randomElement()
    num.shuffled() // 섞어서 문자 배열로 return
    ```

- Substring

    ```swift
    let str = "Hello, Swift, HI"

    let l = str.lowercased()
    유
    var firstCh = str.prefix(1) // String.SubSequence 임
    // 문자열 처리 시 메모리 절약 가능
    ```

    **Substring은 원본 문자열의 메모리를 공유**

    ```swift
    // Copy on Write Optimization

    firstCh.insert("!", at: firstCh.endIndex)
    print(str)
    print(firstCh)

    let newStr = String(str.prefix(1))
    ```

    ```swift
    // 특정 범위 추출

    let s = str[0..<2] // 범위에서도 정수 index아니라 String index사용해야함
    ```

    1. 원본 메모리를 공유한다
    2. s변수에 저장된 문자열을 변경하지 않으면 새로운 문자열은 생성되지 않는다
    3. s변수에 저장된 문자열을 바꾸면 그 시점에 새로운 문자열이 생성됨
    4. 직접 새로운 문자열을 생성하고 싶으면 String생성자를 사용해라

    ```swift
    var s = str[str.startIndex ..< str.index(str.startIndex, offsetBy: 2)]
    s = str[..<str.index(str.startIndex, offsetBy: 2)]
    str[str.index(str.startIndex, offsetBy: 2)...] // 3번째 인덱스 부터 마지막 까지!
    ```

    ```swift
    // 문자열 중간에 자르기....
    let lower = str.index(str.startIndex, offsetBy: 2)
    let upper = str.index(str.startIndex, offsetBy: 5)
    str[lower...upper]
    ```

- String Editing #1

    ```swift
    import Foundation

    var str = "Hello"
    str.append(", Swift")

    let s = str.appending("Swift") // 대상 문자열은 건들지 않음
    print(s)
    str
    s.append("D") // let이기 때문에 안됨.

    s.appending("!!!!")
    "File is Big : ".appendingFormat("%.1f", 12.12) // 원본 변경 하지 않음
    ```

    ```swift
    import Foundation

    var str = "Hello Swift"
    str.insert("!", at: str.index(str.startIndex, offsetBy: 5))

    print(str)

    if let sIndex = str.firstIndex(of: "S") {
        str.insert(contentsOf: "Awsome", at: sIndex)
    }

    print(str) // Hello! AwsomeSwift
    ```

    ```swift
    // 공백 추가하기

    if let blank = str.firstIndex(of: "S") {
        str.insert(contentsOf: " ", at: blank)
    }

    print(str)
    ```

- String Editing #2

    ```swift
    import Foundation

    var str = "Hello, objective-C" // "Hello, objective-C"

    if let range = str.range(of: "objective-C") {
        str.replaceSubrange(range, with: "Swift") // "Hello, Swift"
    }

    if let range = str.range(of: "Hello") {
        str.replacingCharacters(in: range, with: "HI") // "HI, Swift"
        str // "Hello, Swift"
    }
    ```

    ```swift
    var s = str.replacingOccurrences(of: "Swift", with: "Awsome Swift") // 있다면, 두번째 파라미터로 변경
    print(s) // 최종 결과를 s로 return

    s = str.replacingOccurrences(of: "swift", with: "Awsome Swift") // 대문자 구별
    print(s)

    s = str.replacingOccurrences(of: "swift", with: "Awsome Swift", options: [.caseInsensitive])
    print(s)
    ```

    ```swift
    // 범위 삭제 또는 특정 문자

    import Foundation

    var str = "Hello, Amazing Swift!"
    let lastChIndex = str.index(before: str.endIndex)

    var res = str.remove(at: lastChIndex)
    print(str)
    print(res) // 삭제된 문자가 저장됨.

    res = str.removeFirst()
    res = str.removeLast()
    str.removeLast(2)
    ```

    ```swift
    if let range = str.range(of: "Amazing") {
        str.removeSubrange(range)
        str
    }
    ```

    ```swift
    str.removeAll() // 메모리 공간도 삭제함

    str.removeAll(keepingCapacity: true) // 메모리는 남김
    ```

    ```swift
    str = "Hello, Amazing Swift!"

    var substr = str.dropLast() // 마지막 문자를 제외한것 까지 메모리 공유
    substr = str.dropLast(4)
    print(substr)

    substr = str.drop {
        return $0 != ","
    } // ,를 만날때 까지 삭제
    print(substr)
    ```

- String Comparison

    ```swift
    import Foundation

    let LargeA = "Apple"
    let SmallA = "apple"
    let b = "banana"

    LargeA == SmallA
    LargeA != SmallA

    var res = LargeA.compare(SmallA) == .orderedSame
    res = LargeA.caseInsensitiveCompare(SmallA) == .orderedSame // 대소문자 구분 없이

    LargeA.compare(SmallA, options: .caseInsensitive) == .orderedSame
    ```

    ```swift
    import Foundation

    let str = "HI, Swift, Happy Day"
    let prefix = "HI"
    let suffix = "Day"

    str.hasPrefix(prefix) // 문자열 옵션 사용 불가, 대소문자 일치해야함, 전부 소문자로 만들고 구분해도 좋음
    str.hasSuffix(suffix)
    ```

- String Searching

    ```swift
    import Foundation

    let str = "Hello, Swift"
    str.contains("Swift")

    str.range(of: "Swift")
    str.range(of: "swift", options: .caseInsensitive) // 대소문자 무시하고 범위 검색

    let str2 = "Hello, my name is tuna"
    let str3 = str2.lowercased()

    var common = str.commonPrefix(with: str2) // 공통 접두어 출력
    common = str.commonPrefix(with: str3, options: .caseInsensitive) // 대소문자 구분함
    // 메소드 호출 대상에 있는 접두어를 return하기 때문에 Hello가 return됨
    ```

- String Options #1

    ```swift
    import Foundation

    "A" == "a" // false
    "A".caseInsensitiveCompare("a") == .orderedSame

    "A".compare("a", options: .caseInsensitive) == .orderedSame
    NSString.CompareOptions.caseInsensitive // 전체이름은 코드가 길어지니 생략
    ```

    ```swift
    import Foundation

    let a = "\u{D55C}"
    let b = "\u{1112}\u{1161}\u{11AB}"

    a == b
    a.compare(b) == .orderedSame
    // 여러개의 코드 유닛을 합쳐서 비교함, 최종문자로 판단

    a.compare(b, options: .literal) == .orderedSame
    // literal 코드 유닛으로 확인 안함 -> 최종 결과 비교 아님, 훨씬 빠름
    ```

    ```swift
    import Foundation

    let korean = "행복"
    let english = "Happy"
    let arabic = "سعادة"

    // Swift는 Leading 에서 Trailing으로 봄

    if let range = english.range(of: "p") {
        english.distance(from: english.startIndex, to: range.lowerBound)
    }

    if let range = english.range(of: "p", options: .backwards) {
        english.distance(from: english.startIndex, to: range.lowerBound)
    }
    // backward입력시 Trailing에서 Leading으로
    ```

    ```swift
    import Foundation

    let str = "Swift is very very Good!"

    if let range = str.range(of: "Swift") {
        print(str.distance(from: str.startIndex, to: range.lowerBound)) // 첫번째 문자 Index출력
    } else {
        print("Not Found")
    }

    if let range = str.range(of: "Swift", options: .backwards) {
        print(str.distance(from: str.startIndex, to: range.lowerBound)) // 첫번째 문자 Index출력
    } else {
        print("Not Found")
    }

    if let range = str.range(of: "Swift", options: .anchored) {
        print(str.distance(from: str.startIndex, to: range.lowerBound)) // 첫번째 문자 Index출력
    } else {
        print("Not Found")
    }

    if let range = str.range(of: "Swift", options: [.anchored, .backwards]) {
        print(str.distance(from: str.startIndex, to: range.lowerBound)) // 첫번째 문자 Index출력
    } else {
        print("Not Found")
    }
    // 마지막 부분만 검색하기 때문에 Not Found 출력

    str.lowercased().hasSuffix("swift")
    str.lowercased().hasPrefix("swift")

    if let _ = str.range(of: "swift", options: [.anchored, .caseInsensitive]) {
        print("Same")
    }

    if let _ = str.range(of: "swift", options: [.anchored, .backwards, .caseInsensitive]) {
        print("Same")
    } else {
        print("Not Found")
    }
    ```

- String Options #2 🥕

    ```swift
    import Foundation

    "A" < "B"
    "a" < "B"
    // 문자에 할당된 크기로 비교, 아스키 코드

    let file9 = "file9.txt"
    let file10 = "file10.txt"

    file9 < file10

    file9.compare(file10) == .orderedAscending

    file9.compare(file10, options: .numeric) == .orderedAscending
    // numeric은 문자열안의 숫자를 숫자로 비교
    ```

    ```swift
    import Foundation

    let a = "Cafe"
    let b = "Cafè"

    a == b
    a.compare(b) == .orderedSame
    a.compare(b, options: .diacriticInsensitive) == .orderedSame
    // diacriticInsensitive 발음기호 무시
    ```

    ```swift
    import Foundation

    let a = "\u{30A1}"
    let b = "\u{ff67}"

    a == b
    a.compare(b) == .orderedSame
    a.compare(b, options: .widthInsensitive) == .orderedSame

    // 전각문자와 반각문자를 비교하기 위해서는 위와같은 옵션 추가
    ```

    ```swift
    import Foundation

    let upper = "STRING".lowercased()
    let lower = "string"

    upper == lower
    upper.compare(lower) == .orderedSame
    upper.compare(lower, options: .caseInsensitive) == .orderedSame
    upper.compare(lower, options: [.caseInsensitive, .forcedOrdering]) == .orderedSame
    // forcedOrdering는 caseInsensitive옵션을 무시함.
    // 엄격하게 같지않으면 비교가 강제로 반환

    let emailRegEx = "[A-Z0-9a-z._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,64}"
    let email = "ldh2033@tunahouse97.com🙁"

    if let _ = email.range(of: emailRegEx) {
        print("Found")
    } else {
        print("Not Found")
    }

    if let range = email.range(of: emailRegEx, options: .regularExpression), (range.lowerBound, range.upperBound) == (email.startIndex, email.endIndex){
        print("Found")
    } else {
        print("Not Found")
    }
    ```

- Character Set 🥕

    ```swift
    import Foundation

    let a = CharacterSet.uppercaseLetters
    let b = a.inverted

    let str = "loRem Ipsum"
    var charSet = CharacterSet.uppercaseLetters

    if let range = str.rangeOfCharacter(from: charSet) {
        print(str.distance(from: str.startIndex, to: range.lowerBound))
    }

    if let range = str.rangeOfCharacter(from: charSet, options: .backwards) {
        print(str.distance(from: str.startIndex, to: range.lowerBound))
    }
    ```

    ```swift
    import Foundation

    let str = " A P P L E "
    var charSet = CharacterSet.uppercaseLetters
    charSet = .whitespaces

    let trimmed = str.trimmingCharacters(in: charSet)
    print(trimmed)
    // 문자열 시작과 끝의 공백을 제거해줌, 중간은 삭제 X
    ```

    ```swift
    var editTarget = CharacterSet.uppercaseLetters
    editTarget.insert("#")
    editTarget.insert(charactersIn: "~!@") // 여러개 추가한것과 같은 효과
    editTarget.remove("A")
    editTarget.remove(charactersIn: "BDC") // 여러개 삭제한 효과
    ```

    ```swift
    let customCharSet = CharacterSet(charactersIn: "@.")
    let email = "ldh2033@tunahouse97.com"

    let component = email.components(separatedBy: customCharSet)
    // 문자열로 분리 후 새로운 문자열로 return
    ```