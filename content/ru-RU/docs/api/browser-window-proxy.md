## Класс: BrowserWindowProxy

> Манипулирование дочерним окном браузера

Процесс: [Renderer](../glossary.md#renderer-process)

`window.open` возвращает `BrowserWindowProxy` и предоставляет ограниченное управление дочерним окном.

### Методы экземпляра

Экземпляр объекта `BrowserWindowProxy` содержит следующие методы:

#### `win.blur()`

Дефокусирует дочернее окно.

#### `win.close()`

Принудительно закрывает дочернее окно без вызова события отгрузки.

#### `win.eval(code)`

* `code` String

Позволяет запускать код в дочернем окне.

#### `win.focus()`

Производит фокусировку дочернего окна (переносит окно на передний план).

#### `win.print()`

Вызывает диалог print в рамках дочернего окна.

#### `win.postMessage(message, targetOrigin)`

* `message` String
* `targetOrigin` String

Отправляет сообщение дочернему окну с указанием ротидельского окна или `*` без указания.

In addition to these methods, the child window implements `window.opener` object with no properties and a single method.

### Instance Properties

Экземпляр объекта `BrowserWindowProxy` содержит следующие свойства:

#### `win.closed`

`Булево значение`, которое возвращает true после того как дочернее окно закрывается.