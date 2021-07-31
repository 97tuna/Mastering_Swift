# Working with Variables

- Variable and Constants

    메모리에 값을 저장하는 방법 → 변수와 상수

    - 변수

    ```swift
    var variableName = Value // var로 선언하며, 그 뒤의 이름을 통해 변수를 구분
    ```

    `=` 는 메모리에 값을 저장 → 할당 연산자

    ```swift
    // 학교이름을 저장하는 변수 선언
    var schoolName = "Soongsil Univ" // 큰 따옴표로 감싸주기
    var check = true // 참과 거짓을 구분하는 변수, false도 가능

    // 연속으로 변수 선언하기, 한번에 하나씩이 더 깔끔함
    var a = 1, b = 2, c = 3.0
    ```

    메모리에 총 5개의 변수를 저장, 메모리에 있는 변수를 읽어오려면

    ```swift
    print(schoolName) // 을 통해서 schoolName값을 도출
    ```

    변수에 새로운 값 저장하기

    ```swift
    schoolName = "숭실대학교" // 값을 변경할때에는 var를 쓰지 않는다
    var schoolName = "숭실대학교" // 동일한 이름의 변수를 두개이상 선언하면 오류!

    schoolName = "서울대학교" // 로 다시 변경하고
    print(schoolName) // print로 확인하게 되면 마지막으로 저장된 값인 서울대학교가 출력

    var nextSchool = schoolName // nextSchool에 schoolName의 값을 저장

    // 만약 nextSchool에 다른 값을 저장한다면 schoolName값이 변경될까요?
    nextSchool = "숭실대학교"

    print(nextSchool) // 숭실대학교
    print(schoolName) // 서울대학교
    // 각 변수가 메모리의 같은 주소에 저장된것이 아닌 다른 공간에 저장되어있기 때문에 가능한 일

    var month = 12 // 변수 선언과 동시에 숫자를 저장하면 month의 변수는 정수 타입으로 고정
    month = "10" // 그래서 문자값을 저장할 수 없음
    ```

    - 상수

    var과 동일하게 선언할 수 있지만, 값을 변경할 수 없음

    ```swift
    let constantName = value
    ```

    상수에 값 저장하기

    ```swift
    let day = 10
    day = 31 // 오류, 변수에서는 문제 없었지만 상수에서는 오류발생
    // day는 let으로 선언하였기 때문에 값을 저장할 수 없음 (최초는 가능하나 저장 후 다음에는 불가)
    ```

    **변수와 상수는 값이 저장된 후 특징을 변경할 수 없다 (제일 중요!)**

    ```swift
    var Name = "백종원님"
    Name = 10 // 불가
    ```

- Naming Convention

    이름 정의 규칙 → 대부분의 코드가 이런느낌으로 짜여져 있기 때문에 권장!

    CamelCase: 낙타 등처럼 올록볼록한 변수이름

    - UpperCamelCase: 첫번째 단어도 대문자

    ```swift
    Name
    Year
    SchoolName
    ~~~
    ```

    사용하는 이름 → Class, Structure, Enumeration, Extension, Protocol

    - lowerCamelCase: 첫번째 단어는 소문자로 시작

    ```swift
    name
    year
    schoolName
    ~~
    ```

    사용하는 이름 → Variable, Constant, Function, Property, Parameter

- Scope: 코드의 범위

    선언 위치에 따라서 결정되는 것, 동일한 위치에 변수가 두개존재한다면 접근이 모호해짐!

    - 글로벌 변수
    - 지역 변수

    이 둘을 나누는 조건은 Brace로 구분!

    ```swift
    ~~
    #1 부분
    {          }
    ~~
    // 글로벌 변수

    ~~
    {#2 부분    }
    ~~
    {#3 부분    }
    // 지역 변수

    func example() {
    #1
    	if check {
    		#2
    	}
    #3
    }
    // 1과 3은 같은 부분, 1에서 변수 a를 선언하면 3에서는 불가
    ```