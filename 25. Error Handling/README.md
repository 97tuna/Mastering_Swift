# 25. Error Handling

- Error Handling
    - Compile Time Error → 코드 등 문법 에러
    - Runtime Error → 프로그램 실행 시 에러

    에러 Protocol을 채용하면 Error Type이 됨.

    ```swift
    throw expression -> 에러 형식

    throw // 함수, 생성자, 클로저가 Error를 던질 수 있도록 선언

    func name(parameter) throws -> ReturnType { // Throwing Function / Method
            statement
    }

    init(parameter) throws { // Throwing initilzer
            statement
    }

    { (parameter) throws -> ReturnType in // Throwing Closure
            statement
    }
    ```

    ```swift
    enum DataParsingError: Error {
        // 필수 선언 문법 없음
        case invaildType
        case invaildField
        case missingRequiredField(String)
    }

    func parsing(data: [String: Any]) throws { // Throwing Function 선언
        guard let _ = data["name"] else {
            throw DataParsingError.missingRequiredField("name")
            // throw가 실행되면 같은 코드블록의 나머지 코드는 실행 X
        }
        
        guard let _ = data["age"] as? Int else {
            throw DataParsingError.invaildType
        }
    }
    ```

    throw문은 에러가 있을때만 호출되어야 함.

    - try Statements

        ```swift
        try expression
        try? expression
        try! expression
        ```

        ```swift
        // 가능하다면 사용하지 않는것이 좋음
        try? parsing(data: [:]) // nil Return후 종료
        // Optional try는 Crash발생 X
        ```

        에러 처리에는 크게 3가지로 구분

        1. do-catch문 사용 → 주로 코드에서 나는 오류를 개별적으로 처리할 때
        2. try Expression + Optional Binding
        3. hand over
- do-catch Statements

    ```swift
    do {
            try expression
            statements
    } catch pattern {
            statements
    } catch pattern where condition {
            statements
    }
    // catch블록을 통해 모두 처리되어야 함.
    // catch가 생략되면, 에러가 다른 코드로 전이될 수 있게 해야 함.
    ```

    ```swift
    enum DataParsingError: Error {
        case invalidType
        case invalidField
        case missingRequiredField(String)
    }

    func parsing(data: [String: Any]) throws {
        guard let _ = data["name"] else {
            throw DataParsingError.missingRequiredField("name")
        }
        
        guard let _ = data["age"] as? Int else {
            throw DataParsingError.invalidType
        }
        
        // Parsing
    }

    do {
        try parsing(data: [:])
    } catch DataParsingError.invalidType {
        print("invaild type Error")
    } catch {
        print("handle Error") // 첫번째 블록에서 매칭안되고, 두번째 블록에서 매칭
    }
    // 가장 까다로운 조건부터 작성해야 함.
    ```

    ```swift
    do {
        try parsing(data: [:])
    } catch  {
        print("handle Error")
    } catch DataParsingError.invalidType { // 두번째 블록은 절대 실행 안됨.
        print("invaild type Error")
    }
    ```

    ```swift
    func handleError() { // throws 추가
        do { // do 안쓰고 try만 써도 가능, catch 지우고!
            try parsing(data: [:])
        } catch DataParsingError.invalidType {
            print("invaild type Error")
        } // 에러
    }
    // 1. 모든 에러를 수용할 수 있는 catch문 작성
    // 2. 처리하지 않는 에러를 다른 코드로 전달할 수 있도록 작성
    ```

    ```swift
    func handleError() throws {
        do { // do 안쓰고 try만 써도 가능, catch 지우고!
            try parsing(data: [:])
        } catch {
                    if let error = error as? DataparsingError {
                            
                    }
            }
    }
    ```

    - Pattern이 없는 Catch 블록

        ```swift
        func handleError() throws {
            do { // do 안쓰고 try만 써도 가능, catch 지우고!
                try parsing(data: [:])
            } catch {
                if let error = error as? DataParsingError {
                    switch error {
                    case .invalidType:
                        print("invalidType")
                    default:
                        print("handle Error")
                    }
                }
            }
        }
        ```

- Multi-pattern Catch Clauses (Swift 5.3+)

    ```swift
    do {
        try parsing(data: [:])
    } catch DataParsingError.invalidType { // catch하나에 한 에러만 처리할 수 있었음.
        
    } catch DataParsingError.invalidField {
        
    } catch DataParsingError.missingRequiredField(let fieldName) {
        
    } catch {
        
    }
    ```

    ```swift
    // 예전에는 이렇게 처리했음
    do {
        try parsing(data: [:])
    } catch DataParsingError.missingRequiredField(let fieldName) {
    } catch let err as DataParsingError { // switch문 사용해서 해결
        switch err {
        case .invalidField, .invalidType:
            break
        default:
            break
        }
    }
    ```

    ```swift
    // 5.3버전에서는
    do {
        try parsing(data: [:])
    } catch DataParsingError.invalidType, DataParsingError.invalidField {
        // 콤마로 해결  
    } catch DataParsingError.missingRequiredField(let fieldName) {
        
    } catch {
        
    }
    ```
