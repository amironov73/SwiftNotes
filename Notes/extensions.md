### Расширения

*Расширения* добавляют функциональность существующему классу, структуре, перечислению, причём это может быть класс из "чужого" фреймворка, даже системного.

Расширения в Swift могут:

* Добавлять вычисляемые свойства и вычисляемые свойства типа
* Определять методы экземпляра и методы типа
* Предоставлять новые инициализаторы
* Определять сабскрипты (индексы)
* Определять новые вложенные типы
* Обеспечить соответствие существующего типа протоколу

**Важно: расширения могут добавить новую функциональность, но не могут изменить уже существующую!**

Синтаксис простого расширения:

```swift
extension SomeType {
    // добавляем новую функциональность
}
```

Расширение может установить соответствие типа одному или нескольким протоколам:

```swift
extension SomeType: SomeProtocol, AnotherProtocol {
    // реализуем требования протоколов
}
```

Расширения можно использовать и для шаблонных типов.

Пример вычисляемых свойств в расширении:

```swift
extension Double {
    var km: Double { return self * 1_000.0 }
    var m: Double { return self }
    var cm: Double { return self / 100.0 }
    var mm: Double { return self / 1_000.0 }
    var ft: Double { return self / 3.28084 }
}

let oneInch = 25.4.mm

print("Один дюйм - это \(oneInch) метра")
// Выведет "Один дюйм- это 0.0254 метра"

let threeFeet = 3.ft
print("Три фута - это \(threeFeet) метра")
// Выведет "Три фута - это 0.914399970739201 метра"
```

Расширения могут добавлять новые вычисляемые свойства, но они не могут добавить свойства хранения или наблюдателей свойства к уже существующим свойствам.



