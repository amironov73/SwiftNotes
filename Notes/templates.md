### Шаблоны

Шаблоны в Swift здорово напоминают таковые в C++/C#/Java, однако, имеются и (довольно приятные) отличия.

Синтаксис шаблонов довольно интуитивен, судите сами по примерам:

```swift
// шаблонная функция
func swapTwo<T>(a: inout T, b: inout T) {
    let temp = a
    a = b
    b = temp
}
 
var x = 1, y = 2
swapTwo(a: &x, b: &y)
print(x, y) // Выведет: 2 1
 
// шаблонный класс
struct Stack<Element> {
    var items = [Element]()
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
}
 
var stack = Stack<Int>()
stack.push(1)
stack.push(2)
stack.push(3)
print(stack.pop()) // Выведет: 3
```

Шаблоны, как и прочие типы в Swift, можно расширять:

```swift
extension Stack {
    var topItem: Element? {
        return items.last
    }
}
 
print(stack.topItem!) // Выведет: 2
```

На типы, используемые в шаблонах, можно накладывать ограничения:

```swift
func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
  // тело функции
}
 
// Пример наложения ограничений на тип
// Требуем, чтобы тип поддерживал сравнение на равенство
func findIndex<T: Equatable>(of valueToFind: T, in array:[T]) -> Int? {
  for (index, value) in array.enumerated() {
    if value == valueToFind {
      return index
    }
  }
  return nil
}
```
В Swift предусмотрены довольно разнообразные ограничения, в том числе такие, которых нет в C#. Например, мы можем потребовать, чтобы тип был совместим с арифметическими операциями (сложение, вычитание и т. д.), указав `T: Numeric`:

```swift
func calculateSum<T: Numeric>(_ items: T...) -> T {
    var result: T = 0
    for item in items {
        result += item
    }
    return result
}
 
print(calculateSum(1, 2, 3, 4)) // Выведет: 10
```
Расширения в Swift вообще очень мощный инструмент:

```swift
extension Array where Element: Numeric {
    func mySum() -> Element {
        return self.reduce(0, +)
    }
}
 
extension Array where Element == String {
    func mySum() -> Element {
        return self.reduce("", +)
    }
}
 
print([1, 2, 3].mySum()) // Выведет: 6
print(["1", "2", "3"].mySum()) // Выведет: 123
```

Согласитесь, довольно круто! C# так не умеет, к сожалению.
