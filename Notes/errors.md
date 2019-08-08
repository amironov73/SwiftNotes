### Обработка ошибок

В Swift ошибки являются значениями типов, реализующих протокол `Error` (это пустой протокол, так что реализовать его легко).

Кастомные ошибки чаще всего создаются так:

```swift
enum IrbisError: Error {
    case generalFailure
    case networkFailure
    case badData
}

// где-то в коде
func handleData() throws {
    throw IrbisError.networkFailure
}
```

Можно создавать довольно изощрённые ошибки:

```swift
enum GameError: Error {
    case needMoreGold(coinsNeeded: Int)
}

// где-то в коде
func gameLoop() throws {
    if haveCoins < totalBudget {
        throw GameError.needMoreGold(coinsNeeded: totalBudget - haveCoins)
    }
}
```

В Swift предусмотрено четыре способа обработки ошибок. 

1) можно передать (propagate) ошибку в вызывающий код,
2) можно обработать ошибку в блоке `do-catch`,
3) можно обработать ошибку как опциональное значение,
4) можно поставить утверждение, что в данном случае ошибка исключена.

Несмотря на знакомые слова `try`, `catch` и `throw` при возникновении ошибки не происходит раскрутки стека, как, например, в C#. Обработка ошибки происходит здесь, сейчас и явно.

Чтобы указать, что функция, метод или инициализатор могут генерировать ошибку, к ним приписывают ключевое слово `throws`:

```swift
func canThrowErrors() throws -> String {
    throw IrbisError.badData
}

func cannotThrowErrors() -> String {
    return "Good data"
}
```

Общий вид конструкции `do-catch`:

```swift
do {
    try выражение
    ещё_выражение
} catch шаблон_1 {
    выражение
} catch шаблон_2 where условие {
    выражение
}
```

Вот как это выглядит в коде:

```swift
func nourish(with item: String) throws {
    do {
    try vendingMachine.vend(itemNamed: item)   
  } catch is VendingMachineError {
    print("Invalid selection, out of stock, or not enought money.")
  }
}

func vendingLoop() {
    do {
    try nourish(with: "Beet-Flavored Chips")  
  } catch {
    print("Unexpected non-vending-machine-related error: \(error)")
  }
}
```

Преобразование ошибки в опционал

```swift
func someThrowingFunction() throws -> Int {
    throw IrbisError.badData
}

let x = try? someThrowingFunction()

let y: Int?
do {
    y = try someThrowingFunction()
} catch {
    y = nil
}
```

Запрет на передачу ошибок

```swift
let photo = try! loadImage(atPath: "./Resources/John.jpg")
```

Действия по очистке (Cleanup)

```swift
func processFile(filename: String) throws {
    if exists(filename) {
    let file = open(filename)
    defer { close(file) }
    while let line = try file.readline() {
        print(line)
    }  
    // close(file) вызывается автоматически
  }
}
```
