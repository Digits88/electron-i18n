# Electron Belgeleme Stil Rehberi

Electron belgeleri yazmak için rehberler.

## Başlıklar

* Her sayfa en yukarıda `#` seviyesinde bir başlığa sahip olmalıdır.
* Aynı sayfadaki bölümler `##` seviyesinde başlığa sahip olmalıdır.
* Sub-chapters need to increase the number of `#` in the title according to their nesting depth.
* "ve", "ile" gibi bağlaçlar dışında sayfanın başlığındaki tüm kelimeler büyük harfle başlamalıdır.
* Bölüm başlıklarının sadece ilk harfi büyük harfli olmalıdır.

Örnek olarak `Hızlı Başlangıç`:

```markdown
# Hızlı Başlangıç

...

## Ana Süreç

...

## Renderer Süreç

...

## Uygulamanızı başlatın

...

### Dağıtım olarak başlatın

...

### El ile indirilmiş Electron binary'si

...
```

API referansları için bazı istisnalar mevcut.

## Markdown kuralları

* Kod bloklarında `sh` yerine `cmd` kullanın. (Sözdizimi işaretleyicisinden dolayı).
* Satırlar 80. karakterde bitmelidir.
* No nesting lists more than 2 levels (due to the markdown renderer).
* Tüm `js` ve `javascript` kod blokları [standard-markdown](http://npm.im/standard-markdown) ile kontrol edilir.

## Kelime seçimi

* Use "will" over "would" when describing outcomes.
* Prefer "in the ___ process" over "on".

## API references

The following rules only apply to the documentation of APIs.

### Page title

Each page must use the actual object name returned by `require('electron')` as the title, such as `BrowserWindow`, `autoUpdater`, and `session`.

Under the page title must be a one-line description starting with `>`.

Using `session` as example:

```markdown
# session

> Manage browser sessions, cookies, cache, proxy settings, etc.
```

### Module methods and events

For modules that are not classes, their methods and events must be listed under the `## Methods` and `## Events` chapters.

Using `autoUpdater` as an example:

```markdown
# autoUpdater

## Events

### Event: 'error'

## Methods

### `autoUpdater.setFeedURL(url[, requestHeaders])`
```

### Classes

* API classes or classes that are part of modules must be listed under a `## Class: TheClassName` chapter.
* One page can have multiple classes.
* Constructors must be listed with `###`-level titles.
* [Static Methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/static) must be listed under a `### Static Methods` chapter.
* [Instance Methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes#Prototype_methods) must be listed under an `### Instance Methods` chapter.
* All methods that have a return value must start their description with "Returns `[TYPE]` - Return description" 
  * If the method returns an `Object`, its structure can be specified using a colon followed by a newline then an unordered list of properties in the same style as function parameters.
* Instance Events must be listed under an `### Instance Events` chapter.
* Instance Properties must be listed under an `### Instance Properties` chapter. 
  * Instance properties must start with "A [Property Type] ..."

Using the `Session` and `Cookies` classes as an example:

```markdown
# session

## Methods

### session.fromPartition(partition)

## Properties

### session.defaultSession

## Class: Session

### Instance Events

#### Event: 'will-download'

### Instance Methods

#### `ses.getCacheSize(callback)`

### Instance Properties

#### `ses.cookies`

## Class: Cookies

### Instance Methods

#### `cookies.get(filter, callback)`
```

### Metodlar

Metodların bölümü aşağıdaki formda olmak zorundadır:

```markdown
### `objectName.methodName(required[, optional]))`

* `required` String - A parameter description.
* `optional` Integer (optional) - Another parameter description.

...
```

The title can be `###` or `####`-levels depending on whether it is a method of a module or a class.

For modules, the `objectName` is the module's name. For classes, it must be the name of the instance of the class, and must not be the same as the module's name.

For example, the methods of the `Session` class under the `session` module must use `ses` as the `objectName`.

The optional arguments are notated by square brackets `[]` surrounding the optional argument as well as the comma required if this optional argument follows another argument:

```sh
required[, optional]
```

Below the method is more detailed information on each of the arguments. The type of argument is notated by either the common types:

* [`Dize`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)
* [`Sayı`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)
* [`Nesne`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
* [`Dizi`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
* [`Boole değeri`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)
* Or a custom type like Electron's [`WebContent`](api/web-contents.md)

If an argument or a method is unique to certain platforms, those platforms are denoted using a space-delimited italicized list following the datatype. Values can be `macOS`, `Windows`, or `Linux`.

```markdown
* `animate` Boolean (optional) _macOS_ _Windows_ - Animate the thing.
```

`Array` type arguments must specify what elements the array may include in the description below.

The description for `Function` type arguments should make it clear how it may be called and list the types of the parameters that will be passed to it.

### Olaylar

Olaylar bölümü aşşağıdaki form da olmak zorundadır:

```markdown
### Event: 'wake-up'

Returns:

* `time` String

...
```

The title can be `###` or `####`-levels depending on whether it is an event of a module or a class.

Bir olayda ki argümanlar metodların takip ettiği kurallar ile aynıdır.

### Özellikler

Metodların bölümü aşağıdaki formda olmak zorundadır:

```markdown
### session.defaultSession

...
```

The title can be `###` or `####`-levels depending on whether it is a property of a module or a class.

## Belge Çevirileri

See [electron/electron-i18n](https://github.com/electron/electron-i18n#readme)