---
layout: post
title:  "Basic Swift"
date:   2021-12-06 14:22:25 +0900
categories: Application
tags: Swift Hanssem
---

<br>
- Kotlin을 공부하던 와중, Swift에 대한 흥미가 생겨버렸다.

<br>
* * *

# Basic Swift

Lower Camel Case - function, method, instance

Upper Camel Case - Class, Structure, Extension, Protocol ...
<br><br>

# 변수

var [변수명]: [데이터타입] = [값] // 코틀린과 비스무리 하네요

```swift
var name: String = "JiYoung"
var age: Int = 24
var job = "Junior Software Engineer" // 타입 추론

age = 30 // 값 변경 가능 
job = "Student" // 동일한 타입의 값으로 변경
```

→ 변수나 상수를 생성할 시 데이터 타입 생략하면 컴파일러가 변수 값의 타입을 추론하여 지정한다. 

→ ;을 붙이는 것은 선택 사항
<br><br>

# 상수

let [상수명]: [데이터타입] = [값] 

```swift
let name: String = "Jiyoung"
var age: Int = 24
var job = "Junior Software Engineer" // 타입 추론

age = 99
job = "Student" // 변수들은 변경 가능 
name = "Change" // 상수는 변경할 수 없으므로 오류가 난다.
```

→ 상수는 변하지 않는 값이므로, 정해져 있어 바뀌면 안되는 원주율 같은 값을 지정하는 것이 좋다. 

→ 또는 최대, 최소 범위를 지정할 때도 쓰인다. 
<br><br>

# 콘솔 로그

print() 함수를 이용하면 콘솔 로그를 찍을 수 있다. 

print("Hello Console")을 입력할 시, 콘솔 로그에 해당 문장이 찍히며 자동으로 줄바꿈 문자가 삽입된다. 
<br><br>

# 데이터 타입

- 정수 타입

Int → +,-부호를 포함한 정수

UInt → -부호를 포함하지 않고 0을 포함한 양의 정수 

```swift
var integer: Int = -100
let unsignedInteger: UInt = 50 // 음수 안됨

integer = unsignedInteger // 오류 발생 -> Int UInt는 다른 데이터 타입
integer = Int(unsignedInteger) // 형변환 필요 
```

- Bool

true or false 

```swift
let boolean: Bool = true
let iLoveYou: Bool = true
let isTimeUnlimited: Bool = false

print("시간은 무한한가요? : \(isTimeUnlimited)") // false가 대입되어 나온다. 
```

- 실수 타입

Float → 32비트의 부동 소수 표현 / 64비트 환경에서 최대 6자리 십진수 표현 가능 

Double → 64비트의 부동 소수 표현 / 64비트 환경에서 최대 15자리 십진수 표현 가능 

```swift
var floatValue: Float = 1234567890.1 // 수용할 수 있는 범위를 넘김 

var doubleValue: Double = 1234567890.1 // 표현 가능 
```

→ 뭘 쓸지 모르겠다면 그냥 Double을 사용하자.

- Character

→ 문장이 아닌 단 하나의 문자 표현

→ 영어, 유니코드 지원하는 모든 언어 및 특수기호 등을 사용할 수 있음.

```swift
let alphabetA: Character = "A"
let commandCharacter: Character = "♡" // 유니코드 문자 사용가능 
let 한글변수: Character = "ㄱ" // 변수명도 유니코드 문자(한글) 사용 가능
```

- String

→ 문자의 나열 즉 문자열 표현 가능

→ Character과 같이 유니코드 사용 가능 

```swift
let name: String = "JiYoung"

var introduce: String = String() // 변수로 선언, 빈 문자열 선언 가능 
introduce.append("제 이름은") // 문자열 이어 붙이기 가능 
introduce = introduce + " " + name + 입니다. // + 연산자 사용 가능 

print(introduce)
```
<br><br>

# 함수

→ 재정의(오버라이드)와 중복 정의(오버로드) 모두 허용

func 함수 이름(매개변수 ...) → 반환 타입 {

//실행 구문

return 반환 값

}

- 매개 변수가 있는 함수

```swift
func hello(name: String) -> String{
	return "Hello \(name)!"
}

let greeting: String = hello(name: "Jiyoung")
print(greeting) // Hello Jiyoung!
```

- 매개 변수가 없는 함수

```swift
func hello() -> String{
	return "Hello world!"
}

print(hello()) // Hello world!
```

- 매개 변수가 여러 개인 함수

```swift
func hello(myName: String, yourName: String) -> String{
	return "Hello \(yourName)! I'm \(myName)."
}

print(hello(myName: "Jiyoung", yourName: "Haha"))
// Hello Haha! I'm Jiyoung.
```
<br><br>

# 컬렉션 타입

- 배열

→ swift의 배열은 Linked LIst의 구조를 따른다. 

```swift
var names: Array<String> = ["Jiyoung", "Jiyoung2" ... ]
var names: [String} = ["Jiyoung", "Jiyoung2" ... ]
// 두 선언은 배열을 선언하는 같은 의미 

var emptyArray: [String] = [String]()
var emptyArray: [String] = Array<String>()
var emptyArray: [String] = []
// 빈 배열을 선언하는 같은 의미 

print(emptyArray.isEmpty) // true
print(names.count) // names배열 내 요소의 수 
```

→ 각 요소에 0부터 시작하는 인덱스를 통해 접근할 수 있음 

→ 맨 처음과 맨 마지막 요소는 'first'와 'last 프로퍼티를 통해 가져올 수 있다.

→ 'index(of:)' 메서드를 사용하면 해당 요소의 인덱스를 알아낼 수도 있다. 

→ 만약, 중복된 요소가 있다면 제일 먼저 발견된 요소의 인덱스를 반환한다. 

→ 맨 뒤에 요소를 추가하고 싶다면 'append(_:)' 메서드를 사용한다. 

→ 중간에 요소를 삽입하고 싶다면 'insert(_:at:)' 메서드를 사용하면 된다. 

→ 요소를 삭제하고 싶다면 'remove(_:)' 메서드를 사용하게 되는데, 메서드를 사용하면 해당 요소가 삭제된 후 반환된다.

- 딕셔너리

→ 순서 없이 키의 값과 쌍으로 구성되는 컬렉션 타입

→ 키를 통해 각 값에 접근

만약 내부에 없는 키로 접근해도 오류가 생기지 않는다 → nil 반환

```swift
*// 키는 String, 값은 Int 타입인 빈 딕셔너리를 생성합니다.*
**var** numberForName:**Dictionary**<**String**,int> =**Dictionary**<**String**,int>()

*// 위의 선언과 정확히 동일한 표현입니다.*
**var** numberForName: [**String**:**Int**] = [**String**:**Int**]()
*// [String: Int]는 Dictionary<String, int>의 축약 표현입니다.

// 딕셔너리의 키와 값 타입을 정확히 명시해줬다면 [:]만으로도 빈 딕셔너리를 생성할 수 있습니다.*
**var** numberForName: [**String**:**Int**] = [:]

**var** numberForName: [**String**:**Int**] = ["yagom":100, "chulsoo":200, "jenny":300]
*// 초깃값을 주어 생성해줄 수도 있습니다.*

print(numberForName.isEmpty)*// false*
print(numberForName.count)*// 3*
```
<br><br>

# 구조체

struct 키워드로 정의한다.

struct [구조체 이름] {

[프로퍼티와 메서드들]

}

```swift
struct BasicInformation {
	var name: String
	var age: Int
}
```

→ 사람을 정의하는 간단한 구조체 

- 구조체의 생성 및 초기화

→ 멤버와이즈 이니셜라이즈 사용

```swift
var JiyoungInfo: BasicInformation = BasicInformation(name: "Jiyoung", age: 24)
// var로 선언하면 초기에 이니셜라이즈한 값을 바꿀 수 있다. 
JiyoungInfo.age = 50
JiyoungInfo.name = "Jiyoung2"

let NotJiyoungInfo: BasicInformation = BasicInformation(name: "Jiyoung2", age: 25)
NotJiyoungInfo.age = 100 // let으로 선언 시 변경 불가 
```
<br><br>

# 클래스

→ 스위프트의 클래스는 부모 클래스가 없어도 상속 없이 단독으로 정의 가능

class [클래스 이름} {

[프로퍼티와 메서드들]

}

상속받는 경우에는 

class [클래스 이름]: [부모 클래스 이름] {

[프로퍼티와 메서드들]

}

```swift
class Person {
	var height: Float = 0.0
	var weight: Float = 0.0
}
// 사람의 기본 정보를 프로퍼티로 갖는 클래스 정의 
```

- 클래스의 생성 및 초기화

```swift
var Jiyoung: Person = Person()
Jiyoung.height = 123.4
Jiyounh.weight = 123.4

let NotJiyoung: Person = Person()
NotJiyoung.height = 123.4
NotJiyoung.weight = 123.4
// let으로 선언해도 그 값을 바꿀 수 있다.
// class는 인스턴스 참조 타입이기 때문. 
```

값을 소멸시키고 싶을 경우, 그 값 안에 nil을 넣어주거나 deinit 메소드를 사용한다.

```swift
class Person {
	var height: Float = 0.0
	var weight: Float = 0.0
	deinit {
			print("Person의 클래스 인스턴스가 소멸됩니다.")
	}
}

var Jiyoung: Person? = Person()
Jiyoung = nil // Person class의 인스턴스가 소멸됩니다. 
```