### Отладка

Предусловия в функции

```swift
precondition(index > 0, "Индекс должен быть больше нуля")
```

Также можно вызвать функцию preconditionFailure(_:_:file:line:) для индикации, что отказ работы уже произошел, например, если сработал дефолтный кейс инструкции switch.

Утверждения

```swift
assert(age >= 0, "Возраст должен быть больше или равен нулю")
```

Если код уже проверил условие, то можно вызвать функцию assertionFailure(_:file:line:) для индикации того, что утверждение не выполнилось.

Разница между `assert` и `precondition` заключается в том, что `assert` удаляется из релизной сборки, а `precondition` остаётся и в релизе.



