# 11. Closures

- Closures

    짧고 독립적인 코드 '조각'

    1. Function → Named Closures
    2. Nested Function → Named Closures
    3. Anonymous Function → Unnamed Closures

    ```swift
    { (Parameters) -> ReturnType in 
        Statements
    }
    ```

    ```swift
    let str = { print("HELLO, SWIFT") }
    // 파라미터와 return형 생략타입

    print(type(of: str)) // () -> ()
    str()
    ```

    ```swift
    let str2 = { (str: String) -> String in
        return "HELLO, \(str)"
    } // Argument Label사용 X

    let res = str2("LEE")
    print(res) // Closure Argument Label X
    ```

    ```swift
    typealias SimpleStringClosure = (String) -> String

    func str3(closure: SimpleStringClosure) {
        print(closure("IOS"))
    }

    str3(closure: str2) // HELLO, IOS
    ```

    ```swift
    str3(closure: { (str: String) -> String in
        return "HI, \(str)"
    })
    ```

    ```swift
    import Foundation

    let products = ["Macbook Air", "Macbook Pro", "Mac Pro", "iPad Pro"]

    var proModels = products.filter({ (name: String) -> Bool in
        return name.contains("Pro")
    })

    print(proModels) // ["Macbook Pro", "Mac Pro", "iPad Pro"]
    ```

    ```swift
    print(proModels.sorted()) // 아스키 코드 테이블로 정렬해서 결과가 나옴
    // 오름차순으로 정렬해야함
    // ["Mac Pro", "Macbook Pro", "iPad Pro"]

    proModels.sort(by: { (lsh: String, rhs: String) -> Bool in
        return lsh.localizedCaseInsensitiveCompare(rhs) == .orderedAscending
    })

    print(proModels) // ["iPad Pro", "Mac Pro", "Macbook Pro"]
    ```

    ```swift
    print("START")
    print("END")

    DispatchQueue.main.asyncAfter(deadline: .now() + 5, execute: {
        print("END")
    })

    DispatchQueue.main.asyncAfter(deadline: .now() + 1) {
        print("END")
    }

    // 문법 최적화가 됨. 코드가 서로 같은 코드이지만 깔끔해짐
    ```

- Syntax Optimization

    ```swift
    proModels = products.filter({ (name) in
        return name.contains("Pro")
    })
    // parameter 형식 생략 가능
    // return형 생략 가능

    proModels = products.filter({
        return $0.contains("Pro")
    })
    // parameter, in Keyword 생략 가능, $와 숫자형태로 가능
    // 첫번째 파라미터는 0, 두번째 1~~

    proModels = products.filter({
        $0.contains("Pro")
    })
    // 단일 return이면 keyword생략 가능
    // implicit return

    proModels = products.filter() { // 괄호 생략도 가능
        $0.contains("Pro")
    }
    // inline closure에서 Trailing Closure
    ```

    ```swift
    proModels.sort(by: { (lsh: String, rhs: String) -> Bool in
        return lsh.localizedCaseInsensitiveCompare(rhs) == .orderedAscending
    })

    // 1
    proModels.sort(by: { (lsh, rhs) in
        return lsh.localizedCaseInsensitiveCompare(rhs) == .orderedAscending
    })

    // 2
    proModels.sort(by: {
        return $0.localizedCaseInsensitiveCompare($1) == .orderedAscending
    })

    // 3
    proModels.sort(by: {
        $0.localizedCaseInsensitiveCompare($1) == .orderedAscending
    })

    // 4
    proModels.sort() {
        $0.localizedCaseInsensitiveCompare($1) == .orderedAscending
    }

    // 5
    proModels.sort {
        $0.localizedCaseInsensitiveCompare($1) == .orderedAscending
    }
    ```

- Capturing Values

    ```swift
    var number = 0
    let a = { print("check 1 : \(number)") }
    a()
    // Closure가 외부의 값을 접근할 때 캡쳐한다.

    number += 1
    print("check 2 : \(number)")
    // 값을 참조해버린다는 것!
    ```

    **주소값 까지 같이 Capture해버림**

- Escaping Closure 🥕

    ```swift
    func performNonEspcape(closure: () -> ()) {
        print("START")
        closure()
        print("END")
    }

    performNonEspcape {
        print("Closure")
    }

    // 함수 종료전에 실행
    ```

    **시작 시점과 종료시점이 정확하지 않음!**

    ```swift
    import Foundation

    func Escaping(closure: @escaping () -> ()) {
        print("START")
        
        var num = 100
        
        DispatchQueue.main.asyncAfter(deadline: .now() + 3, execute: {
            closure()
            print(num)
                // 함수가 종료되더라도 Closure가 Capture함! 그래서 Closure종료까지 사용 가능
                // 메모리 에러 없이 실행
        })
        
        print("END")
    }

    Escaping {
        print("HI")
    }
    ```

- Autoclosure 🥕

    ```swift
    import Foundation

    func pickRandom() -> Int {
        return Int.random(in: 0...100)
    }

    func takeRes(param: Int) {
        print(#function)
        print(param)
    }

    takeRes(param: pickRandom())
    print("--------> ")

    func takeClosure(param: () -> Int) {
        print(#function)
        print(param())
    }

    takeClosure(param: { Int.random(in: 0...100) })
    print("--------> ")

    // 함수내부에서 Closure를 실행하지 않는다면 parameter인 random도 실행 안됨.
    ```

    ```swift
    func takeAutoClosure(param: @autoclosure () -> Int) {
        print(#function)
        print(param())
    }

    takeAutoClosure(param: Int.random(in: 1...100) )
    // @autoclosure는 Closure를 선언할 수 없음
    // 자동으로 Closure가 Wrapping됨
    ```

    사용하면 헷갈릴 수 있음 ㅎㅎ

    ```swift
    import Foundation

    func takeAutoClosure(param: @autoclosure @escaping () -> Int) {
        print(#function)
        
        DispatchQueue.main.asyncAfter(deadline: .now() + 1, execute: {
            print(param())
        })
        
        print(param())
    }
    // autoclosure는 non-escaping임
    takeAutoClosure(param: Int.random(in: 1...100) )
    ```

    ```swift
    import Foundation

    func takeAutoClosure(param: @autoclosure @escaping () -> Int) {
        print(#function)
        
        DispatchQueue.main.asyncAfter(deadline: .now() + 1, execute: {
            print(param())
        })
        
        print(param())
    }
    // autoclosure는 Closure에 parameter추가할 수 없음.
    // return Type은 자유롭게 선언 가능
    takeAutoClosure(param: Int.random(in: 1...100) )
    ```

- Multiple Trailing Closure

    ```swift
    func multi(first: () -> (), second: () -> (), third: () -> ()) {
        
    }

    // swift 5.2

    multi(first: <#T##() -> ()#>, second: <#T##() -> ()#>, third: <#T##() -> ()#>)
    // 마지막 파라미터만 Trailing으로 쓸 수 있었음.
    // 첫번째와 두번째는 inline으로만 작성가능

    multi(first: {  }, second: {  
        
        // 코드가 길면 가독성이 떨어짐.
        
    }) {
        // 코드가 길면 가독성이 떨어짐.
    }

    // swift 5.3

    multi { 
        <#code#>
    } second: { 
        <#code#>
    } third: { 
        <#code#>
    }

    // Xcode가 자동으로 Multiple Trailing Closure으로 바꿔줌
    // Argument Label은 필수로 써줘야함.
    ```