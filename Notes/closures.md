### Замыкания

Замыкание -- самодостаточный блок кода, который может быть присвоен переменной соответствующего типа либо передан в функцию в качестве аргумента.

Синтаксис:

```swift
{ (параметры) -> тип_результата in
  выражения
}
```

В замыканиях можно использовать `inout` и `...`.

Пример:

```swift
let sortedNames = names.sorted(by: { 
    (s1: String, s2: String)  -> Bool in
    return s1 > s2 
})
```

Здесь компилятор может вывести типы из контекста, поэтому их можно опустить.

```swift
let sortedNames = names.sorted(by: { s1, s2 in return s1 > s2 })
```

В таком коротком выражении `return` тоже можно опустить.

```swift
let sortedNames = names.sorted(by: { s1, s2 in s1 > s2 })
```

Продолжаем укорачивать: имена аргументов можно заменять на `$0`, `$1`, `$2` и т. д. Получается очень коротко:

```swift
let sortedNames = names.sorted(by: { $0 > $1 })
```

Наконец, финальный аккорд: можно обойтись и без замыкания, просто указав функцию (получается не замыкание, но зато супер-коротко):

```swift
let sortedNames = names.sorted(by: >)
```

#### Последующее замыкание

Последующее замыкание -- это ситуация, когда замыкание является последним аргументом функции. Его можно записать в специальном удобном виде:

```swift
let sortedNames = names.sorted { $0 > $1 }
``` 

т. е. круглые скобки можно опустить.

#### Захват значений

Замыкания могут захватывать константы и переменные из окружающего контекста. После захвата замыкание может ссылаться или модифицировать значения, даже если область, в которой они были объявлены, уже не существует.

```swift
var counter: Int = 0

var incrementCounter: () -> Int = {
    counter += 1
    return counter
}

func doSomething(message: String, fn: () -> Int) {
    let value = fn()
    print("\(message) \(value)")
}

doSomething(message: "Counter now is", fn: incrementCounter)
doSomething(message: "And now is", fn: incrementCounter)
```

#### Сбегающее замыкание

Когда замыкание было передано в функцию в качестве аргумента и вызывается уже после того, как функция вернула значение, говорят, что замыкание сбежало (escaped). В этом случае нужно писать `@escaping` перед типом параметра.

```swift
var completionHandlers: [() -> Void] = []

func someFunction(handler: @escaping () -> Void) {
    completionHandlers.append(handler)
}
```

Если не указать `@escaping` в примере выше, будет ошибка компиляции.

Определение замыкания через `@escaping` означает, что вы должны сослаться на `self` явно внутри замыкания.

#### Автозамыкания

Автозамыкания (autoclosures) -- замыкания, которые автоматически создаются для заключения выражения, которое было передано в качестве аргумента функции.

```swift
var customers = ["Ewa", "Barry", "Daniella"]

func serve(customer: @autoclosure () -> String) {
    print("Now serving \(customer())")
}

serve(customer: customers.remove(at: 0))
```
