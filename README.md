htmlDoc
=======


Попытка описать стандарт для документирования HTML тегов.

## Способы описания doc блоков

По аналогии с javaDoc дескрипторы начинаются с сиволна ```@```.
Блоки могут быть как однострочные так и многострочные

```html
<!-- @descriptor value -->

<!-- 
 | @descriptor value
 +-->
```

Дескрипторы описываються непосредственно перед тегом.

В случае, когда нужно описать дескриптор выше чем прямо перед тегом (чтобы не ломать красоту вёрстки), можно указать xpath до необхолимого тега
```html
<!--
 | @any-class active
 |
 | ./li/a
 | @one-of-classes disabled|active
 +-->
 <li>item name <a href="#" class="delete">X</a></li>
```
В этом примере @any-class относиться к тегу `li` все дескрипторы после `./li/a` будут относиться к тегу `a`.
По сути xpath меняет контекст дескрипторов.
В примере выше xpath можно было бы описать проще `li/a` (так как `./` обозначает текущий контекст), но это сделано для того что бы отличить xpath от обычного текста (описания).
Так же его можно начинать с символа косой черты `/`.

Для того что бы дескриптор ссылался на атрибут тега нужно так же использовать xpath
```html
<!--
 | ./input/@value
 | @some-text 2 words
 +-->
<input type="text" value=""/>
```

## Дескрипторы атрибута class

Эти дескрипторы описываю какие имена класов могут быть добавленны в атрибут class.
Классы, описанные прямо в вёрстке, являются обязательными и не могут быть удалены.

Список дескрипторов

- @any-class
- @one-of-classes

### @any-class

Перечисляет имена классов, которые могут быть добавлены тегу без ограничений

```html
<!-- @any-class active, selected, error -->
<div class="item">text</div>
```
Это означает, что атрибут `class` тега `div` обязательно должен иметь класс `item` и любые из перечисленных классов в любом сочетании.

### @one-of-classes

Описывает какой (только один) из перечисленных классов может быть добавлен
```html
<!-- @one-of-classes published, in-queue, draft -->
<div class="item">text</div>
```

## Дескрипторы текста

Описывают какой текст должен быть на месте данного дескриптора.
Описываются непосредственно в том месте, где должен быть сгенерирован текст, в случае если место не переопределено xpath-ом.

Список дескрипторов

- @some-text
- @date
- @date-format
- @email

### @some-text

Заменяет стандартный `lorem ipsum...` одним дескриптором, который может быть использован js скриптом, для генерации текста.

Опционально принимает параметр в форматах: 

- `N words` N "Lorem" слов
- `N chars` N случайных букв, чисел или символов
- `N numbers` N случайных чисел
- `N letters` N случайных букв
- `N symbols` N случайных символов

```html
<p><!-- @some-text 12 words --></p>
<ul>
 <li>MD5: <!-- @some-text 32 chars --></li>
</ul>
```

### @date

Выводит форматированную текущую дату.
```html
<p>Today: <span class="date"><!-- @date m/d/yy --></span></p>
```

### @date-format

Так как определённого формата вывода даты в html нету, то должен использоваться тот формат, который указан в данном дескрипторе.

Список возможных форматов:

- `js-date-format` Является форматом по умолчанию. Описан в js библиотеке [dateFormat](http://blog.stevenlevithan.com/archives/date-time-format)
- `php-date` формат функции [date()](http://php.net/manual/en/function.date.php) в PHP
- `ruby-time-strftime` формат метода [strftime()](http://www.ruby-doc.org/core-2.0/Time.html#method-i-strftime) в классе `Time` в Ruby
- `python-time-strftime` формат функции [strftime()](http://docs.python.org/2/library/time.html#time.strftime) в библиотеке `time` в Python

Дескриптор достаточно описать один раз, например в заглавии index.html
```html
<!-- @date-format python-time-strftime -->
<html>
<body>
 <p>Date is <!-- @date %d %b %Y --></p>
</body>
</html>
```

### @email

Выводит случайно сгенерированный имейл, например `your.name@mail.com`
```html
<div class="email"><!-- @email --></div>
```

Дополнительным параметром может быть указан домен имейла
```html
<div class="email"><!-- @email at gmail.com --></div>
```
В таком случае будет генерироваться случайное имя, на домен будет `gmail.com`

Ещё одним параметром может быть имя за которым закрепляется сгенерированный имейл.
Это необходимо в случае если нужно вывести одинаковый имейл в нескольких местах.
```html
<div class="email"><!-- @email Mike --></div>
```
Будет сгенерирован имейл `mike@example.com` со случайным доменом.
Если необходимо указать и домен то можно написать `Mike at gmail.com`.
Псоле этого можно использовать только имя `Mike`.
```html
<span class="login">You logged in as <!-- @email Mike --></span>
```
В этом случае будет выведен такой же имейл как и в примере выше.
Если нужно вывести имейл с одинаковым именем, но разным доменом, то каждый раз нужно будет писать полную запись с доменом.

## Дескрипторы условий

Данный тип дескрипторов необходим, что бы указать yf cdzpb bkb eckjdbz

Список дескрипторов

- @joined-with
- @when

### @joined-with

Указывает на два элемента которые всегда должны быть вместе.
Этот дескриптор нужен в том случае когда нельзя поместить элементы в один контейнер,
но нужно помнить, что этого нужно избегать.
Дескриптор должен находиться всегда между двумя элементами.
```html
<div class="header"></div>
<!-- @joined-with -->
<div class="footer"></div>
```
Дескриптор `joined-with` не имеет параметров, по этому после него можно добавлять описание, почему элементы должны быть связаны.
```html
<div class="header"></div>
<!-- @joined-with footer, just because -->
<div class="footer"></div>
```

### @when

Дескриптор определяет условие при котором могут быть выполнены последующие дескрипторы. 
Описывается только в многострочном блоке, который объединяет дескрипторы в группу, 
таким образом указывая на какие дескрипторы будет влиять `@when`.
```html
<span class="date">
    <!-- 
     | @when Less than 3 days of cur date
     | @date m/dd/yy 
     | 
     | @when Cur date
     | @date dddd, mmmm dS, yyyy 
     +-->
</span>
```
Как видим, нету никаких переменных или операторов сравнения, условием есть обычное описание.
Ещё один пример с элементом.
```html
<!-- @when Legged in as admin -->
<button>Edit</button>
```
Если под условие должно попадать несколько елементов, то их можно объединить с помощью `@joined-with`.
```html
<!-- @when Legged in as admin -->
<button>Edit</button><!-- @joined-with --><button>Delete</button>
```
Но в таком случае лучше связанные элементы поместить в общий контейнер.
```html
<!-- @when Legged in as admin -->
<div class="toolbox">
    <button>Edit</button><button>Delete</button>
</div>
```
