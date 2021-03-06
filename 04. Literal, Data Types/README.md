# 04. Literal, Data Types

- Data Types with Memory

    모든 데이터 타입에 따라 공간을 할당할 수 는 없어서 미리 정한 규율을 통해 저장

    Swift에서는 5가지 데이터 타입을 제공

    - Integer Types → 정수 저장
    - Floating-point Types → 소수점 포함 실수 자료형
    - Boolean Types → 참, 거짓 자료형
    - Character Types → 문자 자료형, 하나의 문자
    - String Types → 문자열 자료형, 하나 이상의 문자

    내장 자료형, 사용자 정의 자료형으로 분류 가능

    Memory → 1과 0으로 저장할 수 있는 공간

    가장 작은 Bit부터 시작

    - Bit → 0과 1을 저장, 정보의 저장 단위
    - Byte → Bit가 8개 모임, 프로그램 언어는 Byte가 기본 단위

    1Byte에는 8Bit이므로 256개의 경우를 표현할 수 있음.

    양수만이면 0~255까지, 음수면 -128에서 127까지 저장 가능

    22 → 00010110(2)를 메모리에 저장하면

    ```swift
    <------Data Bit------>
    0  0  0  1  0  1  1  0
    MSB                LSB
    MSB: Most Significant Bit
    LSB: Least Significant Bit
    ```

    Signed: 양수, 음수 저장 가능

    UnSigned: 양수만 저장 가능

    MSB가 0이면 양수, 1이면 음수

    2의 보수 방식으로 음수를 표현

    22 → 00010110 → 11101001 → +1하면 → 11101010으로 표현

    실수의 경우 더 복잡. 고정 소수점을 컴퓨터에 저장할 수 없기 때문임.

    ```swift
    <-------------------------------Data Bit------------------------------>
    0         0         0         1         0         1         1         0
    Sign Bit  <------Exponet------>         <-----------Fraction---------->
    ```

    - 실수를 저장할 때는 지수와 가수를 나눠서 저장
    - 동일한 메모리 크기에 비해 정수보다 더 많은 범위를 표현할 수 있다
    - 부동 소수점 오차로 인해서 100% 정확한 수를 저장할 수 없다

    **위의 3가지 필수로 알고있을 것**

    CPU에서 Memory를 접근할 때 주소 레지스터를 통해서 접근 가능.

    Overflow → Swift는 에러로 처리

    Overflow란 메모리의 저장될 수 있는 범위보다 더 많은것을 저장하려고 할 때 발생

- Numbers

    코드에 숫자를 입력해 보자

    ```swift
    12 3 //  12와 3으로 인식
    12_3 // 은 오류

    // 숫자는 양수와 음수로 표현
    // 코드에서는 +123이 아니라 +는 생략하여 표시, 음수는 -를 붙임(생략 불가)

    1.23 // 실수
    .23 // 은 불가, 정수부분이 0이어도 생략 불가

    1.23e4 // 지수로 표현 밑은 10으로 고정
    0xAp2 // 밑이 16인 지수로 표현

    1,000 // 금액으로 표기시 오류
    1_000_000 // _문법으로 표기해야함
    ```

    진수 표현법

    ```swift
    10 // 10진수 표현
    0b1010 // 10을 2진수로 표현
    0o12 // 8진수로 표현
    0xA // 16진수로 표현
    ```

    숫자 저장 자료형

    Integer Type은 4개의 자료형으로 구분

    ```swift
    Int8: 8비트 즉 1byte저장 가능
    Int16: 16비트 즉 2byte저장 가능
    Int32: 32비트 즉 4byte저장 가능
    Int64: 64비트 즉 8byte저장 가능

    Int8.min // 최소값 return
    Int8.max // 최대값 return

    MemoryLayout<Int8>.size // Byte단위로 return

    Signed Unsigned
    Int8   UInt8
    Int16  UInt16
    Int32  UInt32
    Int64  UInt64
    // 두 자료형의 저장 범위가 다름
    ```

    기본적으로는 Int로 사용 -> 정수를 가장 빠르게 처리하기 때문

    실수 저장 자료형

    Floating-point Type은 2개의 자료형으로 구분

    ```swift
    Float: 32비트 즉 4byte저장 가능, 최대 6자리까지 정확성 보장
    Double: 64비트 즉 8byte저장 가능, 최대 15자리까지 정확성 보장

    // 정수형 타입보다 더 많이 저장할 수 있음. Int보다 더 많이 저장
    // 메모리의 크기에 따라서 소수점의 정확도가 달라짐
    ```

- Boolean

    ```swift
    let valid1 = true // Bool로 설정
    let valid2 = True // 컴파일 에러

    valid1 = false
    let valid3: Bool = 0 // 0은 에러, false는 가능, 0은 Int로 swift는 인식

    // 기본적으로 has 또는 is를 붙여서 변수의 이름을 만듦.
    let str = ""
    str.isEmpty // 문자열이 비어있으면 true반환
    ```

- Strings and Characters

    ```swift
    Hi There // 문자열이 아님
    "Hi There" // 문자열, 큰따옴표로 감싸야함
    "123" // 문자열임, 숫자이지만 큰 따옴표로 감싸져있기 때문에 문자열임
    123 // 숫자
    "3" // 문자열

    let str = "Hi"
    print(type of: str) // String임

    let ch: Character = 'c' // 문자임
    let ch1: Character = 'cc' // 컴파일 오류, 문자가 두개 있기 때문에 문자가 아님

    let emp: Character = "" // 빈문자열은 에러, " "공백으로 표기해야함.
    ```

- Type Inference

    ```swift
    let num = 987
    type(of : num) // Int.type -> 자료형을 설정하지 않으면 알아서 설정
    // Int가 정수를 가장 빠르게 처리하기 때문에 Int로 설정 

    let num1 = 11.5
    type(of : num1) // Double.type

    let str1 = "HI"
    type(of : str1) // String.type

    let a = true
    type(of : a) // Bool.type

    로 자동으로 설정

    let val // 오류
    // 1. 메모리 공간 파악을 해야해서 자료형 선언 확인
    // 2. 직접 판단해야해서 초기값 확인
    // 3. 오류
    ```

    - 100 → Int
    - 1.11 → Double
    - "HI" → String
    - true → Bool
    - false → Bool

- Type Annotation

    ```swift
    let varName: Type = val // 로 선언
    // 두개의 자료형 지정은 불가

    let num: Int = 10

    let val: Float // 초기화 하지 않아도 가능
    val = 10.5

    let ch: Charachter = "a" // 컴파일 시간을 줄여줌 -> Type Annotation을 할 때!
    ```
    
- Type Safety

    Swift는 형식 안정성을 위해 자료형을 엄격히 다룸!

    ```swift
    let ch: Character = 123 // 오류!
    let num: Int = 1.23 // 오류!

    let num1: Int8 = a // 오류!
    let num2: Int64 = a // 오류!, 자료형의 이름이 다르면 호환 불가

    let a = 15
    let b = 17.23
    let res = a + b // 계산이 불가, 타입이 맞지 않기 때문
    ```

    ```swift
    let rate: Int = 1.34 // 오류, 미리 값을 수정할 수 있도록 오류 제시
    let rate1 = 1.54 // Double로 형식 설정
    let amount = 10_000_000

    let result = rate * Double(amount) // result -> Double로 실행

    ```

- Type Conversion

    Type Conversion VS Type Casting

    > Type Conversion는 메모리에 저장된 값을 다른 형식으로 변경 후 저장.

    > Type Casting은 메모리에 저장된 값은 유지, 컴파일러가 다른 형식으로 사용하도록 함.

    ```swift
    let a = 123
    let b = 1.23
    a + b // 오류

    Double(a) + b // 123.0 + 1.23
    a + Int(b) // 123 + 1
    // 서로 다른 결과

    let c = Int8(a) // 이것이 Type Conversion

    let d = Int.max
    let e = Int(d) // 컴파일 에러, 값을 저장할 공간이 충분하지 않음

    let str = "123"
    let num = Int(str) // 성공, 

    let str = "HI"
    let num = Int(str) // nil 발생
    ```

- Type Alias

    ```swift
    typealias Coordinate = Double

    let latitude: Double = 12.3456
    let longitude: Double = 78.9123

    let latitude: Coordinate = 12.3456
    let longitude: Coordinate = 78.9123
    ```