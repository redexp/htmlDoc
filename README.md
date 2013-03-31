htmlDoc
=======


Попытка описать докумениаиор для  html

## Способы описания doc блоков

По аналогии с javaDoc анотации начинаются с сиволна ```@```.
Блоки могут быть как однострочные так и многострочные

```html
<!-- @anotation value -->

<!-- 
 | @anotation value
 +-->
```

Анотации описываються непосредственно перед тегом.

В случае когда нужно описать анотацию выше чем прямо перед тегом (чтобы не ломать красоту вёрстки), можно указать xpath до необхолимого тега
```html
<!--
 | @any active
 |
 | ./li/a
 | @oneof disabled|active
 +-->
 <li>item name <a href="#" class="delete"></a></li>
```
В этом примере @any относиться к тегу li все анотации после xpath будут относиться к тегу a.
По сути xpath меняет контекст анотаций.
Каждый новый xpath отталкивается от предидущего контекста, т.е. если бы в теге a был тег span то xpath к нему выглядел как ```./span```.

# Нотации атрибута class

Классы описанные прямо в вёрстке являются обязательными и не могут быть удалены.
Далее идёт описание нотаций, которые описывают какие классы могут быть добавлены и в каких случаях их можно добавить.

Список нотаций

- @any
- @oneof

## @any

Перечисляет имена классов, которые могут быть добавлены тегу без ограничений

```html
<!-- @any active|selected|error -->
<div class="item">text</div>
```

## @oneof

Описывает какой только один из перечисленных классов может добавлен
```html
<!-- @oneof published|in-queue|draft -->
```
