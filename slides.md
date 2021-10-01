---
# try also 'default' to start simple
theme: seriph

# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/Szk6Gp2OP9Q/1920x1080

# apply any windi css classes to the current slide
class: 'text-center'

# https://sli.dev/custom/highlighters.html
highlighter: shiki

# show line numbers in code blocks
lineNumbers: true

# some information about the slides, markdown enabled
info: |
  ## Berry theme explained
  Made with Slidev, Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)

# persist drawings in exports and build
drawings:
  persist: false

fonts:
  sans: 'Roboto'
  serif: 'Roboto Slab'
  mono: 'Fira Code'

defaults:
  layout: default

title: Berry does Frontend
layout: cover
---
# Berry theme explained

<style>
    h1 {
        text-shadow:  1px 1px 10px black;
        transform: translateY(-50px);
    }
</style>



---
layout: image-right
image: https://source.unsplash.com/oJnJghKVdgI
---
# The f*ck** truth
> Because writing CSS is easy; looking after it is not.

-- <cite>[csswizardry](https://twitter.com/csswizardry)</cite>



---
layout: image-right
image: https://source.unsplash.com/O2hBEYSuAJY
---
# Our mission as FE developers
- Take **full control** of our code
- **DRY** (Don't Repeat Yourself) as much as possible
- **be comfortable with the code** such that when we touch a line of css we're not messing up a piece of UI somewhere else
  - eg. fixing the padding of the CTA in homepage, we have to be sure we're not changing the submit button of the form in contact page



---
layout: center
class: text-center
---
# Program
Fortunately there are methodolgies that can help us a lot!

## BEM
## CSS Namespaces
## ITCSS



---
layout: section
---
# BEM
## Block Element Modifier



---
layout: image-right
image: https://source.unsplash.com/59GbsjBpjfA
---
# BEM
Block Element Modifier

It's a methodolgy, or a naming convention suggestion, to use when deciding class names on our markup.

It helps us giving classes:
- roles
- relationships
- responsibilities
- and states

in a clear way.


---
layout: image-right
image: https://source.unsplash.com/59GbsjBpjfA
---
# BEM
Block Element Modifier

## Block
the parent, the standalone entity that has a meaning by itself (top level abstraction of your component)

## Element
a part of the block, without meaning if standalone by itself (a child item)

## Modifier
a flag, a variation, a state to a block or element



---
layout: image-right
image: https://source.unsplash.com/59GbsjBpjfA
---
# BEM
See it in action (HTML)

Here is an example of BEM applied to a button

```html
<a class="button button--big">
    <span class="button__text">I'm a button!</span>
    <i class="button__icon fa fa-angle-right"></i>
</a>
```



---
layout: image-right
image: https://source.unsplash.com/59GbsjBpjfA
---
# BEM
See it in action (SASS)

Here is an example of BEM applied to a button


```sass
.button {
    display: inline-flex;
    align-items: center;
    padding: $space_l $space_xl;
    font-size: remCalc(16);

    &__text {
        color: $color_primary;
    }

    &__icon {
        margin-right: $space_m;
        font-size: remCalc(24);
    }

    &--big {
        font-size: remCalc(20);
    }
}
```



---
layout: section
---
# CSS Namespaces



---
layout: default
---
# CSS Namespaces

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.



---
layout: section
---
# ITCSS



---
layout: default
---
# ITCSS

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.







---
layout: image-right
image: https://source.unsplash.com/NzERTNpnaDw/1920x1080
---
# Layout: image-right
```sass
.mao {
    @include my($space_2xl);
}
```



---
layout: image-left
image: https://source.unsplash.com/SXihyA4oEJs/1920x1080
---
# Layout: image-left



---
layout: center
class: 'text-center'
---
# Layout: center
Displays the content in the middle of the sreen.



---
layout: default
---
# Layout: default
The most baisc layout, to display any kind of content.



---
layout: fact
---
# Layout: fact
To show some fact or data with a lot of prominence on the screen.



---
layout: full
class: 'text-center'
---
# Layout: full
Use all the space of the screen to display the content.





---
layout: statement
---
# Layout: statement
Make an affirmation/statement as the main page content.



---
layout: two-cols
---
# Layout: two-cols
Separates the page content in two columns.

::right::
I'm on the right
