---

layout: sc5

style: |

    #Cover h2 {
        margin:30px 0 0;
        color:#FFF;
        text-align:center;
        font-size:70px;
        margin-left: -250px;
        }
    #Cover p {
        margin:10px 0 0;
        text-align:center;
        color:#FFF;
        font-style:italic;
        font-size:20px;
        }
    #Cover p a {
      color:#FFF;
    }
    #Cover img {
      max-width: 150%;
    }
    #Picture h2 {
        color:#FFF;
        }
    #SeeMore h2 {
        font-size:100px
        }
    #SeeMore img {
        width:0.72em;
        height:0.72em;
        }
    pre code::before {
      display: none;
    }
    h3 {
        font-size: 1.25em;
        font-weight: bold;
    }
    .no-title h2 {
      display: none;
    }
    .big {
        font-size: 100px;
    }
    .middle {
        font-size: 60px;
    }

---

# BEM Методология {#Cover}

![](pictures/story.jpg)

## [git.io/css-old-school](http://git.io/css-old-school)

### Небольшой пример страницы

* …Сделать меню в sidebar
* …В этом меню должна быть другая цветовая схема
* …Поместить кнопку на активный элемент меню
* …Кнопка в активном меню должна быть меньше
* …Так же маленькие кнопки в колонке main

## Что делает CSS сложным?

* Вертикальное центрирование
* Колонки равной высоты
* Неконсистентность браузеров
* Неочевидные трюки

<!--
Before we decide what is wrong with that peice, let's guess what is hard in CSS.

When ppl are asked, the repson is usually
- vertical centing
- making columns of equial height
- browers render CSS differently, so it takes special knowledge and work to make the interface consistent
- many solutions are unobvious tricks which needed to be memorized

But this is not true, this is easy or at least clear how to manage. You can google for all this questions.
-->

## Что <b>действительно</b> делает CSS сложным?

* Область видимости
* Специфичные конфликты
* Не детерменированные совпадения
* Управление зависимостями
* Удаление неиспользуемого кода

<!--
The real hard problems of CSS are here:
- No scoping. Everything is CSS is global.
- Specificity conflicts. I'll explain in detal later.
- Non-deterministic matches which naturally result from declarativeness of CSS language
- Dependency management
- Removing unused code
-->

## Где CSS сложный?
{: .no-title .hard-css }

<table><thead>

<th markdown="1">

Здесь CSS не сложный

</th>

<th markdown="1">

А здесь да!

</th>

</thead><tr>

<td markdown="1">

    #sidebar ul li a {
      <mark>color: red;</mark>
      <mark>display: block;</mark>
      <mark>padding: 1em;</mark>
    }

</td>

<td markdown="1">

    <mark>#sidebar ul li a</mark> {
      color: red;
      display: block;
      padding: 1em;
    }

</td>

</tr></table>

*Как мы проектируем инкапсулированные компоненты?*
{: .next }

<!--
Writing CSS is easy. It is very easy to read and it is very likely that you can
find the tricks you need at Stackoverflow. But architechting CSS is very difficult.

We need a way to architect independent encapsulated components.
Being put into any place on the page, the components should not break anything. Also, it should not be broken
itself.
-->

<style>
.hard-css em {
  font-size: 30px;
}
</style>

## Теория

    .БЛОК{__ЭЛЕМЕНТ[--МОДИФИКАТОР]}
{: style="font-size: 40px" }

## Просто кнопка

    <button class="<mark>button</mark>">Button button</button>
    <span class="<mark>button</mark>">Span button</span>
    <a class="<mark>button</mark>" href="#">Link button</a>

<button class="button">Button button</button>
<span class="button">Span button</span>
<a class="button" href="#">Link button</a>

<style>

.button {

    background: #EFEFEF;
    border: #999 1px solid;
    font-size: 1em;
    padding: 1em;
    color: #000;
    text-decoration: none;
    cursor: pointer;
}

</style>

## Selected button (модификатор)

    <button class="button">Button</button>
    <button class="button <mark>button--selected</mark>">
      Selected button</button>

<button class="button">Button</button>
<button class="button button--selected">
  Selected button</button>

    $element.addClass('button--selected')

<style>

.button--selected {
    border-width: 3px;
}

</style>

## Тема

    <button class="button">Button</button>
    <button class="button <mark>button--brand</mark>">Brand button</button>
    <button class="button <mark>button--brand</mark> <mark>button--selected</mark>">
      Selected brand button</button>

<button class="button">Button</button>
<button class="button button--brand">Brand button</button>
<button class="button button--brand button--selected">
  Selected brand button</button>

<style>

.button--brand {
    background: #e3b3b1;
    border-color: #c76864;
    color: #6e2a27;
    border-radius: 0.5em;
}

</style>

## Ещё одна тема

    <button class="button">Button</button>
    <button class="button button--night">Night button</button>
    <button class="button button--night button--selected">
      Selected night button</button>

<button class="button">Button</button>
<button class="button button--night">Night button</button>
<button class="button button--night button--selected">
  Selected night button</button>

<style>

.button--night {

    background-color: black;
    color: #FFF;
    border-color: #666;
}

</style>

## Состояние как модификатор

    <button class="button <mark>button--process</mark>">
      Doing smth button</button>

<button class="button">Button</button>
<button class="button button--process">Doing smth button</button>

    $element.toggleClass('button--process')
      // Sugar: $element.toggleMod('process')

<style>

@keyframes processMove {
  0% {
    background-position: 0 0;
  }
  100% {
    background-position: 50px 50px;
  }
}

@-webkit-keyframes processMove {
  0% {
    background-position: 0 0;
  }
  100% {
    background-position: 50px 50px;
  }
}

.button--process {

    background-image: linear-gradient(
      -45deg,
      rgba(255, 255, 255, .5) 25%,
      transparent 25%,
      transparent 50%,
      rgba(255, 255, 255, .5) 50%,
      rgba(255, 255, 255, .5) 75%,
      transparent 75%,
      transparent
    );
    background-size: 50px 50px;
    background-position: 0 0;
    animation: processMove 2s linear infinite;
    -webkit-animation: processMove 2s linear infinite;

}

</style>

## Комбинирование модификаторов

    <button class="button button--brand button--process">
        Doing smth brand button</button>

<button class="button button--brand button--process">
    Doing smth brand button</button>

<button class="button button--night button--process">
    Doing smth night button</button>

<style>

.button--night.button--process {
   /* Webkit */
   background-image:
      -webkit-gradient(radial, 50% 50%, 2, 50% 50%, 40, from(white), color-stop(0.1, rgba(248,255,128,.5)), to(transparent)),
      -webkit-gradient(radial, 50% 50%, 1, 50% 50%, 30, from(white), color-stop(0.1, rgba(255,186,170,.4)), to(transparent)),
      -webkit-gradient(radial, 50% 50%, 1, 50% 50%, 40, from(rgba(255,255,255,.9)), color-stop(0.05, rgba(251,255,186,.3)), to(transparent)),
      -webkit-gradient(radial, 50% 50%, 0, 50% 50%, 30, from(rgba(255,255,255,.4)), color-stop(0.03, rgba(253,255,219,.2)), to(transparent));

   /* Firefox */
   background-image:
      -moz-radial-gradient(circle, #FFFFFF 2px, rgba(248,255,128,.5) 4px, transparent 40px),
      -moz-radial-gradient(circle, #FFFFFF 1px, rgba(255,186,170,.4) 3px, transparent 30px),
      -moz-radial-gradient(circle, rgba(255,255,255,.9) 1px, rgba(251,255,186,.3) 2px, transparent 40px),
      -moz-radial-gradient(circle, rgba(255,255,255,.4), rgba(253,255,219,.2) 1px, transparent 30px);

   /* Background images size */
   background-size: 250px 100px, 150px 50px, 100px 170px, 120px 30px;

   /* Background images position*/
   background-position: 0 0, 30px 60px, 10px 70px, 70px 150px;
}
</style>

## Элемент

    <div class="input">
        <input class="<mark>input__field</mark>">
        <span class="<mark>input__keyboard</mark>"></span>
    </div>

<div class="input">
    <input class="input__field">
    <span class="input__keyboard"></span>
</div>

<style>
.input {
    display: inline-block;
    position: relative;
}
.input__field {
    background: white;
    border: #999 1px solid;
    font-size: 1em;
    padding: 1em;
    color: #000;
    text-decoration: none;
    cursor: pointer;
}
.input__keyboard {
    position: absolute;
    width: 54px;
    height: 100%;
    top: 0;
    right: 10px;
    background-image:url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNyIgaGVpZ2h0PSIxMiI+PHBhdGggb3BhY2l0eT0iLjMiIGZpbGwtcnVsZT0iZXZlbm9kZCIgY2xpcC1ydWxlPSJldmVub2RkIiBkPSJNMjUuNjkzIDBoLTI0LjE1M2MtMS4xMDQgMC0xLjUyNy40MjItMS41MjcgMS41Mjd2OC45NjFjMCAxLjEwNS40MjMgMS41MjcgMS41MjcgMS41MjdoMjQuMTUzYzEuMTA0IDAgMS4zMDctLjQyMiAxLjMwNy0xLjUyN3YtOC45NjFjMC0xLjEwNS0uMjAyLTEuNTI3LTEuMzA3LTEuNTI3em0tMi42NzIgMi4wNjFoMS45MzV2MS45MzRoLTEuOTM1di0xLjkzNHptLTYgMGgxLjkzNXYxLjkzNGgtMS45MzV2LTEuOTM0em0yLjkzNSAzdjEuOTM0aC0xLjkzNXYtMS45MzRoMS45MzV6bS01LjkzNS0zaDEuOTM1djEuOTM0aC0xLjkzNXYtMS45MzR6bTIuOTM1IDN2MS45MzRoLTEuOTM1di0xLjkzNGgxLjkzNXptLTUuOTM1LTNoMS45MzV2MS45MzRoLTEuOTM1di0xLjkzNHptMi45MzUgM3YxLjkzNGgtMS45MzV2LTEuOTM0aDEuOTM1em0tNS45MzUtM2gxLjkzNXYxLjkzNGgtMS45MzV2LTEuOTM0em0yLjkzNSAzdjEuOTM0aC0xLjkzNXYtMS45MzRoMS45MzV6bS01LjkzNS0zaDEuOTM1djEuOTM0aC0xLjkzNXYtMS45MzR6bS0zIDBoMS45MzV2MS45MzRoLTEuOTM1di0xLjkzNHptMCAzaDIuOTk0djEuOTM0aC0yLjk5NHYtMS45MzR6bTMuOTUgNC45MzNoLTMuOTc4di0xLjkzM2gzLjk3OHYxLjkzM3ptLjA1LTQuOTMzaDEuOTM1djEuOTM0aC0xLjkzNXYtMS45MzR6bTEzLjk1IDQuOTMzaC0xMi45NjN2LTEuOTMzaDEyLjk2M3YxLjkzM3ptLjA1LTcuOTMzaDEuOTM1djEuOTM0aC0xLjkzNXYtMS45MzR6bTQuOTUgNy45MzNoLTMuOTc4di0xLjkzM2gzLjk3OHYxLjkzM3ptLjAyOS0yLjk5OWgtMy45Nzl2LTEuOTM0aDMuOTc5djEuOTM0eiIvPjwvc3ZnPg==);
    background-size: 54px auto;
    background-repeat: no-repeat;
    background-position: 50% 50%;

    box-shadow: none;
    background-color: transparent;
    border: none;
}
</style>

## Элементы блоков

    <ul class="menu">
        <li class="<mark>menu__item</mark>">Item 1</li>
        <li class="<mark>menu__item</mark>">Item 2</li>
        <li class="<mark>menu__item</mark>">Item 3</li>
    </ul>

## Вложенные элементы

    <ul class="menu">
        <li class="<mark>menu__item</mark>">
            <a class="<mark>menu__link</mark>">Item 1</a>
        </li>
        ...
    </ul>

###Не нужно делать `menu__item__link`!

## Элементы с модификатором

    <ul class="menu">
        <li class="menu__item">Item 1</li>
        <li class="menu__item <mark>menu__item--current</mark>">Item 2</li>
        <li class="menu__item">Item 3</li>
    </ul>

## [git.io/css-bem-way](http://git.io/css-bem-way)

### Попробуем снова, но по БЭМу

* …Сделать меню в sidebar
* …В этом меню должна быть другая цветовая схема
* …Поместить кнопку на активный элемент меню
* …Кнопка в активном меню должна быть меньше
* …Так же маленькие кнопки в колонке main

## BEM в двух словах
{: .nutshell }

* Не `#id` а `.class`
* Никаких родительских селекторов
* "Пространство имен" для компонента
* Модификаторы под пространством имен

<style>
.nutshell h3 {
    font-size: 2em;
}
</style>

## SASS синтаксис (or LESS)
{: .sass }

<table>

<tr>

<td markdown="1">

    .block
      prop: val


      &--mod
        prop: val


      &__elem
        prop: val


        &--mod
          prop: val

</td>

<td markdown="1">

    .block {
      prop: val
    }

    .block--mod {
      prop: val
    }

    .block__elem {
      prop: val
    }

    .block__elem--mod {
      prop: val
    }

</td>

</tr></table>

<style>
.sass pre code {
    line-height: 1em;
}
</style>

## MindBEMding :-)

    $text__family-sans: "Proxima Nova", Helvetica, sans-serif;
    $text__size--beta: 30;
    $text__line--beta: 36;

    $text__colour--base: $color__neutral--dark;
    $text__colour--alpha: $color__primary;
    $text__colour--beta: $color__primary;

## Файловая структура

    components/
      header.sass
      footer.sass
      menu.sass
      ...

## Структура для нескольких технологий

    components/
      header/
        header.ctrl.js
        header.dir.js
        header.html
        header.sass
      footer/
      ...

## Структура для нескольких технологий

    logo/
      logo.sass
      logo.svg
    auth-form/
      auth-form.sass
      auth-form__background.gif
      auth-form.ctrl.js

## Одна библиотека для нескольких сайтов
{: .one-lib }

<div class="library">
    <div class="component-a"></div>
    <div class="component-b"></div>
    <div class="component-c"></div>
    <div class="component-d"></div>
    <div class="component-e"></div>
    <div class="component-f"></div>
    <div class="component-g"></div>
    <div class="component-h"></div>
</div>

<div class="site">
    <div class="component-a"></div>
    <div class="component-f"></div>
    <div class="component-e"></div>
</div>
<div class="site">
    <div class="component-h"></div>
    <div class="component-e"></div>
    <div class="component-b"></div>
</div>
<div class="site">
    <div class="component-e"></div>
    <div class="component-f"></div>
    <div class="component-d"></div>
</div>

<style>
.one-site,
.one-lib {
    text-align: center;
}
.one-site .site,
.one-lib .site {
    background-color: lightgrey;
    width: 29%;
    height: 150px;
    display: inline-block;
    margin: 1.5%;
    padding: 0.25em;
    box-sizing: border-box;
}

.one-site .library,
.one-lib .library {
    margin: 0 auto;
    display: block;
    width: 100%;
    height: 70px;
    text-align: center;
    background-color: silver;
    margin: 1em 0 3em;
    padding: 0.25em;
    border: darkgrey 2px solid;
}

.component-a {
    display: inline-block;
    width: 60px;
    height: 30px;
    background-color: pink;
}
.component-b {
    display: inline-block;
    width: 50px;
    height: 35px;
    background-color: lightyellow;
    border-radius: 1em;
}
.component-c {
    display: inline-block;
    width: 70px;
    height: 25px;
    background-color: lightgreen;
}
.component-d {
    display: inline-block;
    width: 40px;
    height: 40px;
    background-color: lightblue;
    border-radius: 50%;
}
.component-e {
    display: inline-block;
    width: 35px;
    height: 40px;
    background-color: #89e8e0;
    border: #1b877e 1px dashed;
}
.component-f {
    display: inline-block;
    width: 100px;
    height: 25px;
    background-color: #f2dbc4;
    border-radius: 50%;
}
.component-g {
    display: inline-block;
    width: 60px;
    height: 30px;
    background-color: #c0cacc;
    border: #fff 1px solid;
}
.component-h {
    display: inline-block;
    width: 60px;
    height: 30px;
    background-color: lightgreen;
    border: #189918 1px dotted;
    border-radius: 1em 2em;
}
</style>

## Один сайт, несколько библиотек
{: .one-site }

<div class="library">
    <div class="component-a"></div>
    <div class="component-c"></div>
    <div class="component-e"></div>
    <div class="component-g"></div>
</div>
<div class="library">
    <div class="component-b"></div>
    <div class="component-d"></div>
    <div class="component-f"></div>
    <div class="component-h"></div>
</div>
<div class="site">
    <div class="component-b"></div>
    <div class="component-e"></div>
    <div class="component-d"></div>
</div>

<style>

.one-site .library {
    width: 40%;
    display: inline-block;
    margin-left: 3.5%;
    margin-right: 3.5%;
}

</style>

## BEM Методология

### [tocher/bem](https://github.com/Tocher/bem-css-workshop/)
