### Пространства имён

Обычных (как в C++ или в C#) пространств имён в Swift нет. Если в вашей программе есть класс `MyClass` и в используемом программой фреймворке `SomeFramework` есть класс `MyClass`, то никакого конфликта не произойдёт, линкёр увидит разные "искажённые" (mangled) имена функций и переменных.

Встретив в коде программы упоминание `MyClass`, компилятор молча выберет локальный класс и проигнорирует фрейворковский. Чтобы указать, что нужен именно фреймворковский класс, надо написать `SomeFramework.MyClass`.

Нет никаких проблем создать свой собственный класс `Array` -- компилятор не спутает его с системным. Тот по-прежнему останется доступным, но теперь уже обращаться к нему придётся так: `Swift.Array`.

Пространства имён внутри своего приложения/фреймворка можно имитировать так:

```swift
// В одном файле

struct FirstNamespace {

    static let BaseURL = "https://example.com/v1/"
    static let Token = "sdfiug8186qf68qsdf18389qsh4niuy1"

    static func doSomething() {
        print("Doing something in first manner")
    }

} // end of FirstNamespace

// В другом файле

struct SecondNamespace {

    static let BaseURL = "https://example.com/v2/"
    static let Token = "sdfiug8186qf68qsdf18389qsh4niuy2"

    static func doSomething() {
        print("Doing something in second manner")
    }

} // end of SecondNamespace

// В третьем файле

print(FirstNamespace.BaseURL)
print(SecondNamespace.BaseURL)

FirstNamespace.doSomething()
SecondNamespace.doSomething()
```

Если вы создаёте собственный фреймворк, то уделите больше внимания тому, что будет объявлено `public`, а что -- нет. По умолчанию всё понимается как `internal`. Вашему фреймворку будет назначено в качестве пространства имён имя продукта, сконфигурированное в Xcode.

Такие вот пироги с ласточками. :)
