# 21. Extension

- Extension - Syntax

    형식 확장에 사용 → Class, Struct, Enum, Protocol에 추가 가능

    ```swift
    extension Type {
        computedProperty
        computedTypeProperty
        instanceMethod
        typeMethod
        initializer
        subscript
        NestedType
    }
    // 지정 생성자, 소멸자는 원본에서 진행, 아니면 subClass에서 진행
    extension Type: Protocol, ... {
        requirements
    }
    ```

    ```swift
    struct Size {
        var w = 0.0
        var h = 0.0
    }

    extension Size {
        var area: Double {
            return w * h
        }
    }

    var s = Size()
    s.area

    extension Size: Equatable {
        public static func == (rhs: Size, lhs: Size) -> Bool {
            return rhs.w == lhs.w && rhs.h == lhs.h
        }
    }
    ```

- Adding Properties

    계산속성만 추가 가능

    ```swift
    import Foundation
    import UIKit

    extension Date {
        var year: Int {
            let cal = Calendar.current
            return cal.component(.year, from: self)
        }
        
        var month: Int {
            let cal = Calendar.current
            return cal.component(.month, from: self)
        }
    }

    let today = Date()
    today.year

    today.month
    ```

    ```swift
    extension Double {
        var radiusValue: Double {
            return (Double.pi * self) / 180.0
        }
        
        var degreeValue: Double {
            return self * 180.0 / Double.pi
        }
    }

    let dv = 45.0
    dv.radiusValue
    dv.radiusValue.degreeValue
    ```

- Adding Methods

    ```swift
    import Foundation
    import UIKit

    extension Double {
        func toFahrenheit() -> Double {
            return self * 9 / 5 + 32
        }
        
        func toCelsous() -> Double {
            return (self - 32) * 5 / 9
        }
        
        static func convertToFahrenheit(from celsius: Double) -> Double {
                return celsius.toFahrenheit()
        }
    }

    let c: Double = 30
    c.toFahrenheit()

    Double.convertToFahrenheit(from: c) // 원본형식의 TypeMethod와 동일
    ```

    ```swift
    import Foundation
    import UIKit

    extension Date {
        func toString(format: String = "yyyyMMdd") -> String {
            let privateFormatter = DateFormatter()
            privateFormatter.dateFormat = format
            return privateFormatter.string(from: self)
        }
    }

    let today = Date()
    today.toString()
    today.toString(format: "MM/dd/yy")
    ```

    ```swift
    import Foundation
    import UIKit

    extension String {
        static func random(length: Int, charIn chars: String = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789") -> String {
            var randomString = String()
            randomString.reserveCapacity(length)
            
            for _ in 0..<length {
                guard let char = chars.randomElement() else {
                    continue
                }
                randomString.append(char)
            }
            return randomString
        }
    }

    String.random(length: 5)
    ```

- Adding Initializers

    ```swift
    import Foundation
    import UIKit

    extension Date {
        init?(year: Int, month: Int, day: Int) {
            let cal = Calendar.current
            var comp = DateComponents()
            comp.year = year
            comp.month = month
            comp.day = day
            
            guard let date = cal.date(from: comp) else {
                return nil
            }
            
            self = date // 구조체라 할당해서 초기화 가능
        }
    }

    Date(year: 2021, month: 09, day: 15)
    ```

    ```swift
    import Foundation
    import UIKit

    extension UIColor {
        convenience init(red: Int, green: Int, blue: Int) {
            self.init(red: CGFloat(red) / 255, green: CGFloat(green) / 255, blue: CGFloat(blue) / 255, alpha: 1.0)
        }
    }

    UIColor(red: 0, green: 0, blue: 255)
    ```

    ```swift
    struct Size {
        var w = 0.0
        var h = 0.0
        
        init(value: Double) {
            w = value
            h = value
        }
    }

    Size() // 에러
    Size(w: 11, h: 22) // 에러
    // extension을 이용하면 해결됨
    ```

    ```swift
    struct Size {
        var w = 0.0
        var h = 0.0
    }

    extension Size {
        init(value: Double) {
            w = value
            h = value
        } // extension에 넣으면 기본으로 제공되는 initializer또한 사용 가능
    }

    Size()
    Size(w: 11, h: 22)
    ```

- Adding Subscripts

    ```swift
    extension String {
        subscript(idx: Int) -> String? {
            guard (0..<count).contains(idx) else {
                return nil
            }
            let target = index(startIndex, offsetBy: idx)
            return String(self[target])
        }
    }

    let str = "Swift"
    str[1]
    str[100]
    ```

    ```swift
    import Foundation
    import UIKit

    extension UIColor {
        convenience init?(red: Int, green: Int, blue: Int) {
            if red > 255 || green > 255 || blue > 255 { return nil }
            self.init(red: CGFloat(red) / 255, green: CGFloat(green) / 255, blue: CGFloat(blue) / 255, alpha: 1.0)
        }
    }

    UIColor(red: 0, green: 0, blue: 255)
    UIColor(red: 300, green: 400, blue: 0) // 이상한 숫자면 nil 반환

    extension String {
        subscript(idx: Int) -> String? {
            guard (0..<count).contains(idx) else {
                return nil
            }
            let target = index(startIndex, offsetBy: idx)
            return String(self[target])
        }
    }

    let str = "Swift"
    str[1]
    str[100]

    extension Date {
        subscript(component: Calendar.Component) -> Int? {
            let cal = Calendar.current
            return cal.component(component, from: self)
        }
    }

    let today = Date()
    today[.year]
    ```