### Тип Data -- буфер в памяти

Страница: [https://developer.apple.com/documentation/foundation/data](https://developer.apple.com/documentation/foundation/data)

Представляет собой обёртку над NSMutableData -- буфер байтов в памяти.

Инициализаторов много, вот самые интересные:

```swift
init() // создаёт пустой буфер
init(capacity: Int) // буфер указанной ёмкости
init(count: Int) // буфер, заполненный нулями
init(bytes: UnsafeRawPointer, count: Int) // копия данных из неуправляемой памяти
```

Методы, индексаторы и свойства:

```swift
var isEmpty: Bool // пусто?
var count: Int // количество байтов в буфере
subscript(Range<Data.Index>) -> Data // выделение диапазона
subscript(Data.Index) -> UInt8 // доступ к отдельному байту
func append(Data) // дописывает данные в конец буфера
func append(contentsOf: [UInt8]) // дописывает данные из массива байт
func append(UInt8) // добавление одного байта
func reserveCapacity(Int) // резервирует ёмкость
func remove(at: Int) -> UInt8 // удаляет байт по указанному смещению и возвращает его
func removeAll(keepingCapacity: Bool) // удаление всех данных
func removeSubrange(Range<Int>) // удаление диапазона байт
func replaceSubrange(Range<Data.Index>, with: Data) // замещение диапазона
func range(of: Data, options: Data.SearchOptions, in: Range<Data.Index>?) -> Range<Data.Index>? // поиск совпадающего диапазона
func prefix(Int) -> Data // диапазон с начала с начала буфера
func prefix(through: Int) -> Data // диапазон с начала буфера
func prefix(upTo: Int) -> Data // диапазон с начала буфера
func suffix(Int) -> Data // диапазон с конца буфера
func suffix(from: Int) -> Data // диапазон с конца буфера
func dropLast(Int) -> Data // пропуск конечных байтов
func dropFirst(Int) -> Data // пропуск начальных байтов
func insert(UInt8, at: Int) // вставка байта в указанную позицию
static func + <Other>(Data, Other) -> Data // сложение с буфером 
```

