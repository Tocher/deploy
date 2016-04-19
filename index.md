---

layout: sc5

style: |

    #Cover h2 {
        margin:30px 0 0;
        color:#FFF;
        text-align:center;
        font-size:70px;
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

# BEM {#Cover}

![](pictures/cover.jpg)
<!-- photo by John Carey, fiftyfootshadows.net -->

##[git.io/css-old-school](http://git.io/css-old-school)

### Little frontend exersise

* …Make another menu in the sidebar
* …This second menu should have different color scheme
* …Place a button in the active menu item
* …Button in the active menu item should be smaller
* …Use smaller buttons in the main column too

## What makes CSS hard?

* Vertical centering
* Equal height columns
* Browser inconsistencies
* Unobvious tricks

<!--
Before we decide what is wrong with that peice, let's guess what is hard in CSS.

When ppl are asked, the repson is usually
- vertical centing
- making columns of equial height
- browers render CSS differently, so it takes special knowledge and work to make the interface consistent
- many solutions are unobvious tricks which needed to be memorized

But this is not true, this is easy or at least clear how to manage. You can google for all this questions.
-->

## What <b>really</b> makes CSS hard?

* Scoping
* Specificity conflicts
* Non-deterministic matches
* Dependency management
* Removing unused code

<!--
The real hard problems of CSS are here:
- No scoping. Everything is CSS is global.
- Specificity conflicts. I'll explain in detal later.
- Non-deterministic matches which naturally result from declarativeness of CSS language
- Dependency management
- Removing unused code
-->

## Where CSS is hard?
{: .no-title .hard-css }

<table><thead>

<th markdown="1">

This is not hard in CSS

</th>

<th markdown="1">

This is!

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

*How do we architect encapsulated components?*
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

## Theory

    .BLOCK{__ELEMENT[--MODIFIER]}
{: style="font-size: 40px" }

## Just a button

    <button class="<mark>button</mark>">Button button</button>
    <span class="<mark>button</mark>">Span button</span>
    <a class="<mark>button</mark>" href="#">Link button</a>

<button class="button">Button button</button>
<span class="button">Span button</span>
<a class="button" href="#">Link button</a>

### Code: [bit.ly/b-e-m](http://bit.ly/b-e-m)

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

## Selected button (modifier)

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

## Theme

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

## Another theme

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

## State as a modifier

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

## Combine modifiers

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

## Element

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

## Blocks of elements

    <ul class="menu">
        <li class="<mark>menu__item</mark>">Item 1</li>
        <li class="<mark>menu__item</mark>">Item 2</li>
        <li class="<mark>menu__item</mark>">Item 3</li>
    </ul>

## Nested elements

    <ul class="menu">
        <li class="<mark>menu__item</mark>">
            <a class="<mark>menu__link</mark>">Item 1</a>
        </li>
        ...
    </ul>

###No need in `menu__item__link`!

## Modified elements

    <ul class="menu">
        <li class="menu__item">Item 1</li>
        <li class="menu__item <mark>menu__item--current</mark>">Item 2</li>
        <li class="menu__item">Item 3</li>
    </ul>

## [git.io/css-bem-way](http://git.io/css-bem-way)

### Let's try again

* …Make another menu in the sidebar
* …This second menu should have different color scheme
* …Place a button in the active menu item
* …Button in the active menu item should be smaller
* …Use smaller buttons in the main column too

## BEM in a nutshell
{: .nutshell }

* Not `#id` but `.class`
* No parent selectors
* "Namespace" for a component
* Modifiers under the namespace

### Miksi? Siksi! [getbem.com/faq/](http://getbem.com/faq/)

<style>
.nutshell h3 {
    font-size: 2em;
}
</style>

## SASS syntax (or LESS)
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

## File structure

    components/
      header.sass
      footer.sass
      menu.sass
      ...

## Multilingual structure

    components/
      header/
        header.ctrl.js
        header.dir.js
        header.html
        header.sass
      footer/
      ...

## Multilingual structure

    logo/
      logo.sass
      logo.svg
    auth-form/
      auth-form.sass
      auth-form__background.gif
      auth-form.ctrl.js

## Multilingual structure

### Real life example

###[git.io/jq5p](http://git.io/jq5p)
{: .big }

## One library for many sites
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

## One site, many libraries
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

## BEM CSS Workshop

### [varya.me/bem-css-workshop](http://varya.me/bem-css-workshop/)
{: .middle }
