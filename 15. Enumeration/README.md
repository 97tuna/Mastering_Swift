# 15. Enumeration

- Enumeration Types

    Enumeration Type안에 Enumeration Case존재

    ```swift
    import Foundation

    let left = 0
    let center = 1
    let right = 2

    var alignment = center
    // 문자열이면 오타율도, 정수면 변경가능성도 있기 때문에 그런 위험율을 줄이고자 함.

    if alignment == "Center" {
        // 정수에 비해 가독성은 증가했지만 오타 위험성 증가
    }
    ```

    ```swift
    enum TypeName {
        case caseName
        case caseName, caseName
    }
    ```

    ```swift
    import Foundation

    enum Alignment {
        case left
        case right
        case center
    }

    Alignment.center
    Alignment.right

    var textAlignment = Alignment.center
    textAlignment = .center

    // textAlignment = .swap // 케이스로 지정한것이 아닌것을 적으면 에
    // 대소문자도 구분
    ```

    ```swift
    if textAlignment == .center {
        
    }

    switch textAlignment {
    case .left:
        print("left")
    case .center:
        print("center")
    case .right:
        print("right")
    }

    // 비교시에 switch문으로 구현하는것이 더 코드를 단축시킬 수 있음
    ```

- Raw Values

    ```swift
    enum TypeName: RawValueTypes {
        case caseName = Value
    }
    ```

    ```swift
    enum Alignment: Int {
        case left
        case right = 123
        case center
    }
    // 선언된 순서로 부터 0부터 할당

    Alignment.left.rawValue // 원시값이 출력됨, 0
    Alignment.right.rawValue
    Alignment.center.rawValue // 직접 지정하지 않으면 이전값에 +1 되어서 지정

    // Alignment.left.rawValue = 100 // 원시값 변경은 에러
    Alignment(rawValue: 0) // rawValue를 전달하면 그에 맞는 case값 전달함.
    Alignment(rawValue: 200) // 케이스가 없으면 nil 반환
    ```

    ```swift
    enum Weekday: String {
        case sunday
        case monday
        case tuesday
        case wednesday
        case thursday
        case friday
        case satureday
    }

    Weekday.sunday.rawValue
    // 원시값 지정 없이 하면 그 case값 출력, 지정하면 그 값 출력

    enum ControlChar: Character {
        case tab = "\t"
        case newLine = "\n"
    }
    // 문자 자료형은 원시값 지정 없이 진행할 경우 오류
    ```

- Associated Values 🥕

    ```swift
    enum TypeName {
        case caseName(Type)
        case caseName(Type, Type, Type...)
    }
    ```

    ```swift
    import Foundation
    import UIKit

    //enum VideoInterfaceSection: String {
    //    case dvi = "1028x768"
    //    case hdmi = "2048x1536"
    //    case displayPort = "3830x2160"
    //    case usbc = "7680x4320"
    //}
    // 해상도와 함께 버전도 같이 표기하려고 함.

    enum VideoInterfaceSection {
        case dvi(witdh: Int, height: Int)
        case hdmi(Int, Int, Double, Bool)
        case displayPort(CGSize)
    }

    var input = VideoInterfaceSection.dvi(witdh: 1028, height: 768)

    switch input {
    case .dvi(witdh: 2048, height: 1536):
        print("2048x1536")
    case .dvi(witdh: _, height: 1536):
        print("div Any * 1536")
    case .dvi:
        print("dvi")
    case .hdmi(let width, let height, let version, var audio):
        print("HDMI \(width) x \(height)")
    case let .displayPort(size):
        print("DP")
    default:
        break
    }

    input = .hdmi(3840, 2160, 2.1, true)
    // input변수의 자료형은 VideoInterfaceSection의 3개중 하나가 저장, 열거형의 자료형과는 다르다
    ```

- Enumeration Case Pattern 🥕

    연관값 매칭할 때

    ```swift
    case Enum.case(let name):
    case Enum.case(var name):

    case let Enum.case(name):
    case var Enum.case(name):
    ```

    ```swift
    import Foundation
    import UIKit

    enum Transportation {
        case bus(number: Int)
        case taxi(company: String, identifier: Int)
        case subway(lineNumber: Int, express: Bool)
    }

    var bus = Transportation.bus(number: 751)

    switch bus {
    case .bus(let n):
        print("노선번호 : \(n)")
    case .taxi(let c, var n):
        print(c, n)
    case let .subway(l, e):
        print(l, e)
    default:
        break
    }

    var subway = Transportation.subway(lineNumber: 2, express: false)

    if case let .subway(2, express) = subway {
        if express {
            
        } else {
            
        }
        // 2호선 지하철인지 확인하고, 급행인지에 따라 코드 분기
    }

    if case .subway(_, true) = subway {
        print("급행")
        // 바인딩 하지 않기 때문에 var, let사용 안함
    }
    ```

    ```swift
    let list = [Transportation.subway(lineNumber: 2, express: false), Transportation.bus(number: 751), Transportation.subway(lineNumber: 7, express: true), Transportation.taxi(company: "Kakao", identifier: 8888)]

    for case let .subway(n, _) in list {
        print("Subway \(n)")
    }

    for case let .subway(n, true) in list {
        print("급행 지하철 \(n)")
    }

    for case let .subway(n, true) in list where n == 2 {
        print("지하철 : \(n)")
    }
    ```

- CaseIterable 🥕

    열거형 기능 확장 프로토콜

    ```swift
    import Foundation
    import UIKit

    enum Weekday: Int {
        case sunday
        case monday
        case tuesday
        case wednesday
        case thursday
        case friday
        case satureday
    }

    // 하나의 요일 랜덤으로 뽑기
    let random = Int.random(in: 0...6)

    Weekday(rawValue: random)

    // sunday rawValue변경하면 올바르지 않은 알고리즘
    ```

    ```swift
    import Foundation
    import UIKit

    enum Weekday: Int, CaseIterable {
        case sunday
        case monday
        case tuesday
        case wednesday
        case thursday
        case friday
        case satureday
    }

    // CaseIterable 모든 case를 Collection으로 배열처럼 사용 가능

    let random = Int.random(in: 0...Weekday.allCases.count)
    Weekday(rawValue: random)

    // allCases에는 배열 return, 모든 case저장되어 있음
    Weekday.allCases.randomElement()

    for i in Weekday.allCases {
        print(i)
    }
    ```

- Nonfrozen Enumeration (Swift 5+) 🥕

    ```swift
    enum ServiceType {
        case online
        case offline
        case onoffMix
    }
    // 없던것을 추가하면 defalut없어서 오류 발생, switch문에 추가해야지 오류 해결
    let selecttype = ServiceType.online

    switch selecttype {
    case .online:
        print("온라인 코스 이메일 보내기")
    case .offline:
        print("오프라인 코스 이메일 보내기")
    //case .onoffMix:
    //    print("온, 오프라인 코스")
    @unknown default:
        break // 새로운 case에 다른 문법 적용하지 않으면 그대로 둠.
    }

    // 실제 프로젝트에서는 열거형을 다양한곳에 사용해 버리기 때문에 문제가 발생
    // @unknown 문법 적용, 경고를 통해서 새로운 case처리 안되는것을 확인
    ```