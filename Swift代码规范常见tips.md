
# 常见问题总结
#### 1. guard 和 if 的选择
##### 1. 优先选择使用 guard 提前返回来解决多嵌套问题。

```
// 推荐
func eatDoughnut(at index: Int) {
    guard index >= 0 && index < doughnuts.count else {
        // return early because the index is out of bounds
        return
}

    let doughnut = doughnuts[index]
    eat(doughnut)
}

// 不推荐
func eatDoughnut(at index: Int) {
    if index >= 0 && index < doughnuts.count {
        let doughnut = doughnuts[index]
        eat(doughnut)
    }
}
```
##### 2. 如果多个条件分支处理方式相同时，可以写在同一个 guard 底下。

```
// combined because we just return
guard let thingOne = thingOne,
    let thingTwo = thingTwo,
    let thingThree = thingThree else {
    return
}

// separate statements because we handle a specific error in each case
guard let thingOne = thingOne else {
    throw Error(message: "Unwrapping thingOne failed.")
}

guard let thingTwo = thingTwo else {
    throw Error(message: "Unwrapping thingTwo failed.")
}

guard let thingThree = thingThree else {
    throw Error(message: "Unwrapping thingThree failed.")
}
```

#### 2. 声明一个可选类型变量时，默认赋值为 nil，不需要显式赋值 = nil

```
// 推荐
var a: String?
// 不推荐
var a: String? = nil
```

#### 3. 定义类常量时，可以放在 Enum 下面。好处是无 case 的 Enum 不会被实例化

```
// 推荐，好用！
enum Math {
    // no case 
    static let e = 2.718281828459045235360287
    static let root2 = 1.41421356237309504880168872
}

let hypotenuse = side * Math.root2
```

#### 4. 当一行代码过长时，考虑将其拆分为多行（比如多参数方法的调用、多参数闭包的调用、构造一个很大的数组或字典时）

```
someFunctionWithManyArguments(
    firstArgument: "Hello, I am a string",
    secondArgument: resultFromSomeFunction(),
    thirdArgument: someOtherLocalProperty)
```

#### 5. 删除无用代码，包括 Xcode 模板代码、占位符注释、实现为空的方法应该被删除

#### 6. 命名时，不要增加 OC 风格的类前缀。比如 KK

#### 7.使用 `UpperCamelCase` 拼写法，来命名类型名称。（如 struct、enum、class、typedef、associatedtype等）

#### 8. 使用 `lowerCamelCase`（初始小写字母）拼写法，命名`函数，方法，属性，常量，变量，参数名称，枚举类型`等。

#### 9. 在创建自定义委托方法时，第一个参数应该是委托源，不能省略
```
//推荐
func namePickerView(_ namePickerView: NamePickerView, didSelectName name: String)

func namePickerViewShouldReload(_ namePickerView: NamePickerView) -> Bool
```

```
//不推荐
func didSelectName(namePicker: NamePickerViewController, name: String)

func namePickerShouldReload() -> Bool
```
#### 10. `var` 和 `let`，优先使用 `let`

#### 11. 对集合类进行转换操作时，优先考虑 `map`、`filter`、`reduce` 等的组合，不是进行遍历操作。

```
// 推荐
let stringOfInts = [1, 2, 3].flatMap { String($0) }
// ["1", "2", "3"]

// 不推荐
var stringOfInts: [String] = []
for integer in [1, 2, 3] {
    stringOfInts.append(String(integer))
}

// 推荐
let evenNumbers = [4, 8, 15, 16, 23, 42].filter { $0 % 2 == 0 }
// [4, 8, 16, 42]

// 不推荐
var evenNumbers: [Int] = []
for integer in [4, 8, 15, 16, 23, 42] {
    if integer % 2 == 0 {
        evenNumbers.append(integer)
    }
}
```

#### 12. 在函数返回参数时，使用元组来返回参数，当参数个数超过3个时，考虑使用 `class` `struct`

#### 13. 在 enum 枚举中：避免写出 enum 完整格式，尽可能使用简写方式。类方法的小问题在于没有代码自动补全提示。出于简洁性也可以省去类名
```
// 推荐
imageView.setImageWithURL(url, type: .person)

// 不推荐
imageView.setImageWithURL(url, type: AsyncImageView.Type.person)
```

```
// 有代码补全
imageView.backgroundColor = UIColor.white
// 代码比较整洁
imageView.backgroundColor = .white
```

#### 14. 声明方法时，如果该方法不会被重写，标记为 final，有助于优化编译时间

#### 15. 定义类函数或类属性时，推荐使用 `static` 而非 `class`。
* 仅当在子类中重写类方法或属性时，才使用 `class`

#### 16. 如无必要，尽量使用 Swift 的原生类型，如 Int、String，而不是 NSInteger、NSString

#### 17. 优先使用 `for-in`,而非 `for`、`while` 样式的循环

```
// 推荐
for _ in 0..<3 {
    print("Hello three times")
}

for (index, person) in attendeeList.enumerated() {
    print("\(person) is at position #\(index)")
}

for index in stride(from: 0, to: items.count, by: 2) {
    print(index)
}

for index in (0...3).reversed() {
    print(index)
}
```

```
// 不推荐
var i = 0
while i < 3 {
    print("Hello three times")
    i += 1
}

var i = 0
while i < attendeeList.count {
    let person = attendeeList[i]
    print("\(person) is at position #\(i)")
    i += 1
}
```

#### 18. 如果在 Switch 中，多个 case 处理方法是一样时，尽量使用值列表并列的方法（如 `case 1,2,3`），而非 `fallthrough `。
```

enum Problem {
    case attitude
    case hair
    case hunger(hungerLevel: Int)
    case mind
    case xmind
}

func handleProblem(problem: Problem) {
    switch problem {
    case .attitude:
        print("At least I don't have a hair problem.")
    case .hair:
        print("Your barber didn't know when to stop.")
    case .hunger(let hungerLevel):
        print("The hunger level is \(hungerLevel).")
    case mind, xmind:
        // do something
    }
}

```

#### 19. 使用 `// MARK: - ` 和 `extension` 来实现隔离协议实现与其它代码段

#### 20. 尾随闭包的使用，例外情况是闭包的含义在没有参数名称的情况下会不明显时（例如，如果方法有成功和失败闭包的参数）。
```
// trailing closure
doSomething(1.0) { (parameter1) in
    print("Parameter 1 is \(parameter1)")
}

// no trailing closure
doSomething(1.0, success: { (parameter1) in
    print("Success with \(parameter1)")
}, failure: { (parameter1) in
    print("Failure with \(parameter1)")
})
```

#### 21. 数组访问方式，尽可能使用 `.first` 或 `.last` 来访问，避免越界崩溃。尽可能优先使用 `for item in items` 语法，而不是 `for i in 0 ..< items.count` 语法。





