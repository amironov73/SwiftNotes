### Коллекции

Основных коллекций в Swift три: массивы, словари и множества.

Изменяемость коллекции зависит от того, как она объявлена: `var` или `let`.

#### Массивы

Страница документации: [https://developer.apple.com/documentation/swift/array](https://developer.apple.com/documentation/swift/array)

Массив хранит элементы одного типа упорядоченным образом, так, чтобы к ним можно было обратиться по индексу. Массив в Swift связан с классом Foundation  [NSArray](https://developer.apple.com/documentation/foundation/nsarray).

Полная форма написания: `Array<Element>`, сокращённая `[Element]`.

Создание пустого массива:

```swift
var array1 = Array<Int>()
var array2 = [Int]()
var array3: [Int] = []
```

Инициализация массива при создании:

```swift
var array4 = Array(repeating: 0.0, count: 3)
```

Инициализация литералом:

```swift
var array5 = [1, 2, 3]
```

Свойства массива:

* `var count: Int` -- количество элементов
* `var capacity: Int` -- ёмкость массива
* `var description: String` -- описание в виде `[1, 2, 3]`
* `var debugDescription: String` -- то же самое
* `var isEmpty: Bool` -- признак пустого массива
* `var first: Element?` -- первый элемент
* `var last: Element?` -- последний элемент

Доступ к элементам:

```swift
var firstElement = array[0]
array[1] = newValue

// срез массива
let slice = array[2...3]

// заменяем три элемента массива двумя другими
array[4...6] = ["Bananas", "Apples"]
```

Итерация по массиву:

```swift
// простая итерация
for value in array {
    print(value)
}

// итерация с индексом
for (index, value) in array {
    print("\(index): \(value)")
}
```

Итерация в функциональном стиле:

```swift
var array1 = [1, 2, 3, 4, 5, 6]
array1.forEach{ print($0) }
```


Сложение массивов:

```swift
var array3 = array1 + array2
array3 += array4
```

Операции с массивами:

```swift
array.append(1)
array.append(contentsOf: otherArray)
array.insert(222, at: 0)
let removedElement = array.remove(at:0)
array.removeFirst()
array.removeLast()
array.removeAll()
array.removeSubrange(1...3)
array.reserveCapacity(10)
let last = array.popLast()
if array.contains(element) ...
let index1 = array.firstIndex(of: element)
let index2 = array.lastIndex(of: element)
let minimum = array.min()
let maximum = array.max()

let slice1 = array.prefix(10) // первые 10 элементов
let slice2 = array.suffix(10) // последние 10 элементов
let slice3 = array.dropFirst(10) // пропущены первые 10 элементов
let slice4 = array.dropLast(10) // пропущены последние 10 элементов

array.sort() // сортировка по умолчанию
array.reverse() // разворот массива
let second = array.reversed() // разворот в отдельном массиве
array.shuffle() // перемешивание
let third = array.shuffled() // перемешивание в отдельном массиве
```

Массивы при присваивании копируются:

```swift
var first = [1, 2, 3]
var second = first
first[0] = 1_000_000
print(first) // [1000000, 2, 3]
print(second) // [1, 2, 3]
```

Трансформации. Проецирование:

```swift
var array1 = [1, 2, 3, 4, 5, 6]
var array2 = array1.map{ $0 * $0 }
print(array2)
```

Свёртка:

```swift
var array1 = [1, 2, 3, 4, 5, 6]
var product = array1.reduce(1, { $0 * $1 })
print(product)
```

Сортировка:

```swift
var array1 = [5, 3, 6, 2, 1, 4]
array1.sort(by: { $0 < $1 })
print(array1)
```

или так

```swift
var array1 = [5, 3, 6, 2, 1, 4]
var array2 = array1.sorted(by: { $0 < $1 })
print(array1)
print(array2)
```

Сравнение массивов:

```swift
if array1 == array2 {
    print("Arrays are equal")
}

if array1.starts(array2) {
    print ("Array1 starts with array2")
}
```

Доступ к сырой памяти:

```swift
let numbers = [1, 2, 3, 4, 5]
let sum = numbers.withUnsafeBufferPointer { buffer -> Int in
    var result = 0
    for i in stride(from: buffer.startIndex, to: buffer.endIndex, by: 2) {
        result += buffer[i]
    }
    return result
}
print(sum) // 9
```

```swift
var numbers = [1, 2, 3]
var byteBuffer: [UInt8] = []
numbers.withUnsafeBytes {
    byteBuffer.append(contentsOf: $0)
}
print(byteBuffer)
// [1, 0, 0, 0, 0, 0, 0, 0, 2, 0, 0, 0, 0, 0, 0, 0, 3, 0, 0, 0, 0, 0, 0, 0]
```

