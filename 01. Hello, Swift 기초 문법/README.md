# Hello, Swift 기초 문법

- 주석 적용하기

    **주석**이란 다른 코드 부가설명 및 메모용도로 사용 (주석을 붙이고자 하는 문장 앞에 슬래시 두개를 붙이거나 Xcode에서 `⌘` +  `/` 입력)

    ```swift
    var str = "Hello, Swift"
    // var str = "Hello, Swift"
    ```

    **여러문장을 주석처리**하고 싶다면 아래와 같이 처리합니다.

    ```swift
    /*
    var str = "Hello, Swift"
    var str1 = "Hello, Playground"
    */
    ```

    주석안에 주석도 넣을 수 있음!

    다만 여는 주석과 닫는 주석의 수를 맞춰줘야 합니다.

    **인라인 주석**은 코드사이에 넣는 주석

    ```swift
    var str/* String */ = "Hello, Playground"
    ```

    작은 내용이어도 주석으로 남겨놓는 좋은 습관을 가지는게 좋다!

- 세미콜론

    Swift 컴파일러는 코드가 어디에서 끝나는지 알고있기 때문에 ;(세미콜론)으로 굳이 처리하지 않아도 가능!

- UIKit

    애플이 미리 만들어놓은 코드를 가져다 쓰겠다는 말!

    ```swift
    import UIKit
    ```

- print문

    ```swift
    print(str) // 확인하고 싶은 값을 알 수 있음.
    dump(str) // 값을 확인할 수 있는 두번째 방법
    ```

    구조체나 class를 확인하게 되면 dump가 자세하게 알려주기 때문에 유용!

- 대, 소문자 구분

    Swift는 대소문자를 완벽하게 구분하기 때문에 ⚠️주의⚠️ 필요

    ```swift
    var str: String = "이건 괜찮아요"
    var str: string = "타입이 안맞아요!"
    ```