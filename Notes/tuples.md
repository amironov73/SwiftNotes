### Кортежи

Кортеж (tuple) -- пользовательский тип, позволяющий группировать значения произвольных типов в составной объект.

Кортеж является типом-значением, при присваивании он копируется.

Объявление кортежа:

```swift
let result = (200, "OK", true)

// или

var result: (Int, String, Bool)
result = (200, "OK", true)
```

Доступ к элементам кортежа:

```swift
var result = (200, "OK", true)
print(result.0) // 200
result.1 = "Not found"
```

Декомпозиция кортежа:

```swift
let result = (200, "OK", true)
var (code, message, flag) = result

// если какие-то элементы не нужны
var (code, message, _) = result
```

Именование элементов кортежа:

```swift
var result = (code: 200, message: "OK", flag: true)
print(result.code)
result.message = "Not found"
```

Опциональный кортеж:

```swift
var result: (code: Int, message: String, flag: Bool)?
result = (200, "OK", true)
```
