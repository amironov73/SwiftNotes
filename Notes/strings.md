### Строки

Страница: [https://developer.apple.com/documentation/swift/string](https://developer.apple.com/documentation/swift/string)

Строки в Swift хранятся в переменных типа String, который представляет собой мост к типу NSString.

Константность строки регулируется тем, как она объявлена: `var` или `let`.

Строки являются типами-значениями. При присваивании они копируются.

#### Литералы

Чтобы всё запутать, литералы-символы и литералы-строки обозначаются одинаково: двойными кавычками. Одинарный символ, заключённый в такие кавычки, будет считаться строкой. Чтобы указать, что это символ, надо писать так:

```swift
let exclamationMark: Character = "!"
```

Имя типа `String` при инициализации литералом можно опускать, компилятор подразумевает тип `String` по умолчанию.

```swift
let something = "Some literal value" // простой литерал

// Многострочный литерал
let quotation = """
У попа была собака,                
Он её любил.
"""
// Первый и последний переводы строки 
// в этом примере игнорируются.
// Ведущие пробелы тоже игнорируются.
```

Ситуацию с пробелами в многострочных литералах иллюстрирует следующая картинка (заимствовано у Apple):

![multiline](img/multilineStringWhitespace_2x.png)

Расширенные многострочные литералы:

```swift
let threeMoreDoubleQuotationMarks = #"""
Here are three more double quotes: """
"""#
```

В литералах предусмотрены стандартные escape-последовательности: `\n`, `\r`, `\0`, `\t`, `\v`, `\\`, `\"` и `\'`. Unicode-символы кодируются неожиданно: `\u{2665}`.

#### Цикл по символам

```swift
for c in "Dog" {
    print(c)
}
```

### Операции над строками

Конкатенация:

```swift
let string1 = "hello"
let string2 = " there"
var welcome = string1 + string2

// Имеется оператор +=
var instruction = "look over"
instruction += " here"
```

Интерполяция:

```swift
let multiplier = 3
let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)"
```

Расширенные ограничители строки не помеха интерполяции:

```swift
print(#"Write an interpolated string in Swift using \(multiplier)."#)
```

Число символов в строке:

```swift
let text = "Koala 🐨, Snail 🐌, Penguin 🐧, Dromedary 🐪"
print(text.count)
```

Выдаются именно символы, а не байты или что-либо аналогичное. Иллюстрирующий пример:

```swift
var word = "cafe"
print("\(word): \(word.count)")
// cafe: 4

word += "\u{301}"    // COMBINING ACUTE ACCENT, U+0301

print("\(word): \(word.count)")
// café: 4"
```

Доступ к отдельным символам:

```swift
let greeting = "Guten Tag!"
greeting[greeting.startIndex]
// G
greeting[greeting.index(before: greeting.endIndex)]
// !
greeting[greeting.index(after: greeting.startIndex)]
// u
let index = greeting.index(greeting.startIndex, offsetBy: 7)
greeting[index]
// a
```

Вставка и удаление:

```swift
var welcome = "hello"
welcome.insert("!", at: welcome.endIndex)
// welcome now equals "hello!"

welcome.insert(contentsOf: " there", at: welcome.index(before: welcome.endIndex))
// welcome now equals "hello there!"

welcome.remove(at: welcome.index(before: welcome.endIndex))
// welcome now equals "hello there"

let range = welcome.index(welcome.endIndex, offsetBy: -6)..<welcome.endIndex
welcome.removeSubrange(range)
// welcome now equals "hello"
```

Суб-строки:

```swift
let greeting = "Hello, world!"
let index = greeting.firstIndex(of: ",") ?? greeting.endIndex
let beginning = greeting[..<index]
// beginning is "Hello"

// Convert the result to a String for long-term storage.
let newString = String(beginning)
```

В произвольном случае это выглядит довольно громоздко:

```swift
print(text[text.index(text.startIndex, offsetBy: 2)..<text.index(before: text.endIndex)])
```

Для сокращения можно сделать так:

```swift
func subStr(_ text: String, _ start: Int, _ length: Int) -> String {
    let offset1 = text.index(text.startIndex, offsetBy: start)
    let offset2 = text.index(offset1, offsetBy: length)
    let result = text[offset1..<offset2]
    return String(result)
}
```

Сравнение строк медленное, но аккуратное:

```swift
// "Voulez-vous un café?" using LATIN SMALL LETTER E WITH ACUTE
let eAcuteQuestion = "Voulez-vous un caf\u{E9}?"

// "Voulez-vous un café?" using LATIN SMALL LETTER E and COMBINING ACUTE ACCENT
let combinedEAcuteQuestion = "Voulez-vous un caf\u{65}\u{301}?"

if eAcuteQuestion == combinedEAcuteQuestion {
    print("These two strings are considered equal")
}
// Prints "These two strings are considered equal"
```

Сравнение с началом/концом строки:

```swift
if text.hasPrefix("pref") {
    // do something
}

if text.hasSuffix("suff") {
    // do something other
}
```

Просмотр строки в различных представлениях:

```swift
let dogString = "Dog‼🐶"

for codeUnit in dogString.utf8 {
    print("\(codeUnit) ", terminator: "")
}
print("")
// Prints "68 111 103 226 128 188 240 159 144 182 "

for codeUnit in dogString.utf16 {
    print("\(codeUnit) ", terminator: "")
}
print("")
// Prints "68 111 103 8252 55357 56374 "

for scalar in dogString.unicodeScalars {
    print("\(scalar.value) ", terminator: "")
}
print("")
// Prints "68 111 103 8252 128054 "
```

Преобразование в различные кодировки и обратно:

```swift
let bytes: Data? = "У попа была собака".data(using: .windowsCP1251)
let text: String? = String(data: bytes, encoding: .windowsCP1251)
```

