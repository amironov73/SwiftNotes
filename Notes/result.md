### Тип Result

В Swift 5 появился механизм возврата из функций/методов как нормальных результатов, так и ошибок -- тип [Result](https://developer.apple.com/documentation/swift/result).

Он объявлен как

```swift
@frozen enum Result<Success, Failure> where Failure : Error
```

и в нём предусмотрено всего два элемента: `success` типа `Success` и `failure` типа `Failure`. Последний должен быть производным от `Error`, например:

```swift
enum IrbisError: Error {
    case networkFailure
    case protocolError
    case badPassword
}

typealias IrbisResult = Result<Int, IrbisError>

func doSomething(command: String) -> IrbisResult {
    if command == "?" {
        return .failure(.protocolError)
    }
    
    return .success(10)
}

let result = doSomething(command: "A")
switch(result) {
case .success(let number):
    print("I got \(number)")
    
case .failure(let error):
    print("Error: \(error)")
}
```

Кроме `switch` можно задействовать `try` так

```swift
let result = doSomething(command: "A")
if let count = try? result.get() {
    print("I got \(count)")
}
```

или так:

```swift
let result = doSomething(command: "?")
do {
    let count = try result.get()
    print("I got \(count)")
} catch {
    print("Error: \(error)")
}
```

Можно обойтись без `try`:

```swift
let result = doSomething(command: "?")
if case .success(let count) = result {
    print("I got \(count)")
}
```

#### Трансформация результата

Тип `Result` предлагает четыре метода: `map`, `flatMap`, `mapError` и `flatMapError`, позволяющих обрабатывать результат в функциональном стиле.

```swift
let result = doSomething(command: "A").map{"I got \($0)"}
print(result) // Выведет: success("I got 10")
```

С помощью `flatMap` удобно организовывать цепочки вызовов, в которых каждый последующий выполняется только при условии, что все предыдущие выдали `success`:

```swift
func kneadTheDough(flavor: String) -> Result<String, Error> {
    return .success("\(flavor) dough")
}

func putIntoTheOven(dough: String) -> Result<String, Error> {
    return .success("\(dough) in the oven")
}

func bakeTheCake(cake: String) -> Result<String, Error> {
    return .success("baked \(cake)")
}

func tada(message: String) -> Result<String, Error> {
    print(message)
    return .success(message)
}

kneadTheDough(flavor: "vanilla")
    .flatMap{putIntoTheOven(dough: $0)}
    .flatMap{bakeTheCake(cake: $0)}
    .flatMap{tada(message: $0)}
```
