### Функции

Функции в Swift объявляются с помощью ключевого слова `func`:

```swift
func greet(person: String) -> String {
    let greeting = "Hello, " + person + "!"
    return greeting
}
```

В Swift вышеприведённая функция имеет имя `greet(person:)`, и язык позволяет рядом с ней завести функцию `greet(collective:)`:

```swift
func greet(collective: String) -> String {
    let greeting = "Hello, collective " + collective + "!"
    return greeting
}
```

С точки зрения Swift между этими функциями нет противоречия, они имеют разные имена и могут сосуществовать в одной программе. В этом важное отличие Swift от C#, Java, C++ и прочих языков -- в них имена параметров не входят в сигнатуру функции.

Чтобы Swift мог различить, какую из функций мы вызваем, необходимо указывать имя аргумента:

```swift
greet(person: "Алексей")
greet(collective: "Библиотека ИРНИТУ")
``` 

Если функция ничего не возвращает, можно написать `-> Void`, можно просто ничего не писать:

```swift
func greetAgain(person: String) {
    print("Hello again, " + person)
}
```

Можно возвращать кортеж (tuple):

```swift
func minMax(array: [Int]) -> (min: Int, max: Int) {
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}

let bounds = minMax(array: [8, -6, 2, 109, 3, 71])
print("min is \(bounds.min) and max is \(bounds.max)")
// Выведет "min is -6 and max is 109"
```

Для параметров различаются внешние и внутренние имена:

```swift
func greet(person name: String) -> String {
    return "Hello, " + name
}
```

Здесь `person` -- внешнее имя, его необходимо указывать при вызове функции, а `name` -- внутренее имя, его используют в теле функции.

Чтобы указать, что внешнего имени для данного аргумента нет, пишут "_":

```swift
func greet(_ name: String) -> String {
    return "Hello, " + name
}
print(greet("Алексей"))
```

С точки зрения Swift имя такой функции `greet(_:)`.

Можно задавать значения по умолчанию для параметров:

```swift
func greet(person: String, greeting: String = "Hello") -> String {
    return greeting + ", " + name
}

print(greet(person: "Алексей")) 
// Hello, Алексей!

print(greet(person: "Алексей", greeting: "Привет"))
// Привет, Алексей!
```

Функции с вариативными параметрами:

```swift
func mean(_ numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}
print(mean(1.0, 2.0, 3.0))
```

Параметры функции по умолчанию являются константами -- в теле функции изменить их значение невозможно. Но с помощью модификатора `inout` можно указать, что тот или иной параметр является *сквозным*.

```swift
func swap(_ a: inout Int, _ b: inout Int) {
    let temp = a
    a = b
    b = temp
}

var a = 1
var b = 2
swap(&a, &b)
print(a, b)
// 2, 1
```

Использование функциональных типов (сохранение адреса функции в переменную):

```swift
func adder(first: Int, second: Int) -> Int {
    return first + second
}

func multiplier(first: Int, second: Int) -> Int {
    return first * second
}

var action: (Int, Int) -> Int = adder;

print(action(2, 3))
```

Вложенные функции:

```swift
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1 }
    func stepBackward(input: Int) -> Int { return input - 1 }
    return backward ? stepBackward : stepForward
}
var currentValue = -4
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
// moveNearerToZero теперь ссылается на вложенную функцию stepForward() 
while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
print("zero!")
// -4...
// -3...
// -2...
// -1...
// zero!
```
