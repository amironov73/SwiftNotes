### Встроенные функции

#### print

Самая известная функция. 😀

Имеется аналог: debugPrint.

#### readLine

Считывает строку из стандартного ввода. Есть полезная перегрузка:

```swift
readLine(strippingNewline: <#T##Bool#>)
```

#### abs(value: Numeric)

Абсолютное значение числа.

#### dump(object: Any)

Выводит содержимое объекта в стандартный вывод:

```swift
var languages = ["Swift", "Objective-C"]
dump(languages)
// ▿ 2 elements
//   - [0]: Swift
//   - [1]: Objective-C
```

#### fatalError(message: String)

Аварийно завершает программу

#### max(x: Comparable, y: Comparable)

Максимальное значение. Аналогично min(x, y).

