### Приведение типов

Приведение типов - это способ проверить тип экземпляра и/или способ обращения к экземпляру так, как если бы он был экземпляром суперкласса или подкласса откуда-либо из своей собственной классовой иерархии. (*Из SwiftBook*)

Приведение типов в Swift реализуется с помощью операторов is и as (Похоже на C#, не правда ли?).

Проверка принадлежности тому или иному классу:

```swift
var movieCount = 0
var songCount = 0
 
for item in mediaLibrary {
    if item is Movie {
        movieCount += 1
    } else if item is Song {
        songCount += 1
    }
}
 
print("Фильмов: \(movieCount), песен: \(songCount)")
```
Собственно приведение. Безопасная форма с помощью `as?`

```swift
for item in mediaLibrary {
    if let movie = item as? Movie {
        print("Movie: \(movie.name), dir. \(movie.director)")
    } else if let song = item as? Song {
        print("Song: \(song.name), by \(song.artist)")
    }
}
```

Принудительная форма приведения с помощью `as!`

```swift
let movie = item as! Movie
print("Movie name: \(movie.name)")
```

#### Типы Any и AnyObject

Swift предлагает две версии псевдонимов типа для работы с неопределенными типами:

* `Any` может отобразить экземпляр любого типа, включая функциональные типы.

* `AnyObject` может отобразить экземпляр любого типа класса.

Можно делать "списки всего":

```swift
var things = [Any]()
 
things.append(0)
things.append(0.0)
things.append(42)
things.append(3.14159)
things.append("hello")
things.append((3.0, 5.0))
things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman"))
things.append({ (name: String) -> String in "Hello, \(name)" })

// а теперь пройдёмся по этому "списку всего"

for thing in things {
    switch thing {
    case 0 as Int:
        print("zero as an Int")
    case 0 as Double:
        print("zero as a Double")
    case let someInt as Int:
        print("an integer value of \(someInt)")
    case let someDouble as Double where someDouble > 0:
        print("a positive double value of \(someDouble)")
    case is Double:
        print("some other double value that I don't want to print")
    case let someString as String:
        print("a string value of \"\(someString)\"")
    case let (x, y) as (Double, Double):
        print("an (x, y) point at \(x), \(y)")
    case let movie as Movie:
        print("a movie called \(movie.name), dir. \(movie.director)")
    case let stringConverter as (String) -> String:
        print(stringConverter("Michael"))
    default:
        print("something else")
    }
}
```

