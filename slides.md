---
# try also 'default' to start simple
theme: seriph

# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: /padenghe_beach3.jpg

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
  sans: 'Comfortaa'
  serif: 'Indie Flower'
  mono: 'Fira Code'
  weights: '200,400,600,700'

defaults:
  layout: default

title: Berry does Frontend
layout: cover
---
# Berry theme explained
*And some of my pics about Italy
<style>
    h1 {
        text-shadow:  1px 1px 10px black;
        transform: translateY(-50px);
        color: white !important;
    }
</style>



---
layout: section
---
# What's inside
*Theme building blocks*

- **HTML**: UI components and Custom Templates
- **CSS**: BEM, Namespaces, ITCSS
- **JS**: JavaScript es6+ modules
- **Tools**: gulp/sass/rollup
<style>
strong {
    color: teal;
}

ul {
    width: 480px;
    margin: 0px auto;
    text-align: left;
}
</style>



---
layout: section
---
# HTML
## UI components and Custom Templates



---
layout: default
---
# UI components and Custom Templates
Philosophy

Don't **try to get the layout you want** from Drupal default,

but

**Code your layout** and connect Drupal data and data-structures




---
layout: default
---
# UI components and Custom Templates
Philosophy

### Before
You got the default Drupal markup and **force** the layout with css:
- lots of float (old school layouts)
- lots of tricks
- no way to architechture your code

#
### Now
Frontenders need the freedom to code the markup/css/js the want,

Especially since the wide adoption of new CSS layout techniques (read Flexbox and Grid) that often requires wrappers around certain elements



---
layout: default
---
# UI components and Custom Templates
My Workflow when starting a new project

- **Identify all UI blocks** inside the design (repeatable or nor, w/o variations)
  - eg: hero, herobig, heroslider, simpletext, imagetext
- **Code each component**:
  - HTML: twig code inside '/components' folder
  - SCSS: separate scss file (typically inside 'src/scss/components' folder)
  - JS: if needed, separted file inside 'src/js/parts' folder
- Use well-known **Drupal templates** to **include**/use/connect your **custom components**



---
layout: image-right
image: /padenghe_beach5.jpg
---
# UI components and Custom Templates
First of all, Component-Based is an approach to UI Design and Development.

Shortly, your website is no more designed as a set of pages,

but as a list of small and reusable blocks (components).

In other words, your pages become "simply" compositions of these blocks, sorting them in different order.



---
layout: image-right
image: /padenghe_castle.jpg
---
# UI components and Custom Templates
Write the frontend code you want detached from the underlying CMS


The CMS becomes more your source of data rather then a magic box where your markup comes from.

You're free to use whatever markup/classes/architecture you want.

Frontend libraries integration is pushed away from the CMS (eg. Drupal lightbox modules vs UIkit or lightgallery integration)



---
layout: default
image: /padenghe_castle.jpg
---
# UI components and Custom Templates
Hero example, 3 params: image, title, text

/templates/components/c-hero.html.twig

```twig
<section class="c-hero js-hero">
    <div class="c-hero__bgimage" style="background-image: url('{{ image }}')" ></div>
    <div class="c-hero__overlay"></div>
    <div class="c-hero__content">
        <div class="o-container o-grid">
            {% if title %}
                <h1 class="c-hero__title">{{ title }}</h1>
            {% endif %}

            {% if text %}
                <div class="c-hero__text">
                    {{ text }}
                </div>
            {% endif %}
        </div>
    </div>
</section>
```



---
layout: default
image: /padenghe_castle.jpg
---
# UI components and Custom Templates
Hero example, style (uncompleted example)

/src/scss/6-components/_c-hero.scss

```scss
.c-hero {
    @include bg-image();
    @include py($space_4xl);

    &__overlay {
        @include abs-fill();
        background: rgba($color_black0, 0.5);
    }

    &__content__inner {
        @include grid-cell(2, 6);
    }

    &__title {
        @include fs2();
        color: $color_white;
    }
}
```



---
layout: default
image: /padenghe_castle.jpg
---
# UI components and Custom Templates
Lightbox gallery example (uncompleted)

/src/js/parts/c-gallery.js

```js
/*
    Lightbox gallery with lightgallery.js
*/
import 'lightgallery.js';
import 'lg-fullscreen.js';

const galleryItems = document.querySelectorAll('.js-lightbox');

for (let item of Array.from(galleryItems)) {
    lightGallery(item, {
            mode: 'lg-slide',
            cssEasing: 'cubic-bezier(0.645, 0.045, 0.355, 1.000)',
            speed: 600,
            loop: true,
            selector: 'a'
        });
}

```



---
layout: default
---
# UI components and Custom Templates
Inside well-known Drupal Twig file, use include statement passing an object containing relevant data coming from fields:


```twig
{% include '@berrytheme/components/c-hero.html.twig' with {
    'title': content.field_title,
    'text': content.field_text,
    'image': content.field_image.0
}%}
```

The "well-known Drupal Twig file" depends on how you structured the content.

Usually:
  - paragraphs templates (eg. paragraph--card.html.twig)
  - views templates (eg. views-view-fields--hero.html.twig)
  - nodes templates (eg. node--news--teaser.html.twig)



---
layout: default
---
# UI components and Custom Templates
Fighting with Drupal

**Drupal** makes the **assumption you don't want plain value from a field** to be used directly in Twig templates, but you usually want a formatted value or **worse a prebuit markup depending on the field formatter and the templates** coming from modules and base theme you're using.

**Component-based dev requires you to get plain data/values**

It's not so straightforward

This is my short list of code snippets to access various fields type data:

https://www.andberry.me/articles/drupal-component-based-frontend



---
layout: default
---
# UI components and Custom Templates

Avoid wrappers from Drupal

.theme file
```php
/*
    Field template suggestion
    for some fields we just need the field content, without wrapperd and labels:
    add fields names to $content_only_fields_single and $content_only_fields_multiple accordingly
*/
function divertns_theme_suggestions_field_alter(&$suggestions, &$variables) {
    $content_only_fields_single = array(
        // drupal node title aka label
        'title',

        // fields used across content-types / paragraphs
        'field_content',
        'field_summary'
    );

    if (in_array($variables['element']['#field_name'], $content_only_fields_single)) {
        $suggestions[] = 'field__content_only__single';
    }

```

---
layout: default
---
# UI components and Custom Templates
Avoid wrappers from Drupal

.theme file
```php
    $content_only_fields_multiple = array(
        'field-flexible-contents',
        'field_cards',
        'field_subjects',
        'field_languages',
        'field_author',
    );

    if (in_array($variables['element']['#field_name'], $content_only_fields_multiple)) {
        $suggestions[] = 'field__content_only__multiple';
    }
}
```



---
layout: default
---
# UI components and Custom Templates
Avoid wrappers from Drupal

field__content_only__single.html.twig
```twig
{{ items.0.content }}
```

field__content_only__multiple.html.twig
```twig
{% for item in items %}
    {{ item.content }}
{% endfor %}
```



---
layout: image-right
image: /val_fassa.jpg
---
# UI components and Custom Templates
From Drupal POV
  - Use **Paragraphs** a lot
  - Use **View Modes** a lot (News Content type and then teaser and list view mode with corresponding node--news templates)
    - eg. Divert insights (demo)



---
layout: section
---
# CSS
## BEM, Namespaces, ITCSS



---
layout: image-right
image: /padenghe_beach3.jpg
---
# The f*ck** truth
> Because writing CSS is easy; looking after it is not.

-- <cite>[csswizardry](https://twitter.com/csswizardry)</cite>



---
layout: default
---
# Our mission as FE developers
- Be more **confortable** as possible with our code
- **Reduce** CSS code and making it more **maintainable**
- **DRY** as much as possible
- **Avoid unwanted side-effects**: be sure that when we touch a line of css we're not messing up a piece of UI somewhere else
  - eg. we fixed the padding of the **CTA in homepage**, and we also changed the style of the **submit button of the form** in contact page



---
layout: image-right
image: /padenghe_beach4.jpg
---
# Some methodolgies that can help us a lot!

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
image: /padenghe_beach1.jpg
---
# BEM
Block Element Modifier

It's a methodolgy, or a **naming convention suggestion, to use when deciding class names**.

It helps us giving classes:
- roles
- relationships
- responsibilities
- and states

in a clear way.


---
layout: image-right
image: /padenghe_beach2.jpg
---
# BEM
Block Element Modifier

### Block
the standalone entity that has a meaning by itself

### Element __ (double underscore)
a part of the block, without meaning if standalone by itself

### Modifier -- (double dash)
a flag, a variation, or a state associated to a block or element



---
layout: center
---
# BEM
See it in action

This is the HTML code for a button

```html
<a class="button button--big">
    <span class="button__text">I'm a button!</span>
    <i class="button__icon fa fa-angle-right"></i>
</a>
```



---
layout: center
---
# BEM
See it in action

This is the SASS code for a button


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
layout: image-right
image: /val_fassa.jpg
---
# CSS Namespaces
How many times have you looked at a piece of HTML only to wonder:
- which classes do what
- which classes are related to each other
- which classes are optional
- which classes are recyclable
- which classes can you delete, and so on?

A lot of times, I bet.



---
layout: image-right
image: /molveno_lake.jpg
---
# CSS Namespaces
Namespace will tell us exactly how classes behave in a more global sense.

Again, this methodolgy is meant to be used **when deciding class names**.



---
layout: image-right
image: /padenghe_castle2.jpg
---
# CSS Namespaces

In no particular order, here are the individual namespaces and a brief description.
- **o-**: this is an **Object**, an **abstract layout implementation**
  - used in any number of unrelated contexts: tread carefully.
  - eg. **o-grid, o-sidebar-left**
- **c-**: this is a **Component**. This is a concrete, **implementation-specific piece of UI**.
    - eg. **c-hero, c-card**



---
layout: image-right
image: /molveno_lake.jpg
---
# CSS Namespaces
- **u-** : this class is a **Utility** class. It has a **very specific role** and it can be **reused** and is **not tied to any specific piece of UI**.
  - eg. **u-color-primary, u-text-center**
- **t-**: this class **adds a theme to a view**. Override default style be due to the presence of a theme.
  - eg. **t-light, t-learning-section, t-saint-valentine**


---
layout: image-right
image: /padenghe_castle2.jpg
---
# CSS Namespaces
- **is-, has-**: this piece of UI is currently styled a certain way because of a state or condition.
  - used everywhere, not tighten to specific UI block
  - eg. **is-active, has-2-items**
- **js-**: this piece of the DOM has some JavaScript binds onto it.
  - eg. **js-carousel, js-accordion**



---
layout: section
---
# ITCSS
Inverted Triangle CSS





---
layout: default
---
# ITCSS
Inverted Triangle CSS

It helps us to **organize our CSS files** to better deal with CSS specifics/issues/pitfalls like:
- global namespace
- cascade
- selectors specificity.

At the end, we're splitting CSS properties based on their level of specificity and importance.



---
layout: default
---
# ITCSS
Inverted Triangle CSS

<img src="/itcss.svg" class="w-sm mx-auto my-8" />

Going from top to bottom symbolizes an increase in specificity.

Each subsection of the triangle may be considered a separate file or group of files



---
layout: default
---
# ITCSS
Inverted Triangle CSS

My personal implementation:
- **1-settings**: sass variables: *font, colors, spacings*
- **2-tools**: mixins and functions: *bg-image(), abs-center()*
- **3-base**: generic + elements, normalize/reset + base html elements style without classes: *body, h1, table* (this is the first CSS output)
- **4-layout**: (remember objects?) abstract layouts, unstyles patterns: *grid, layout-sidebar, media*
- **5-components**: style for a specific piece of UI: *c-hero, c-footer*
- **6-modules**: aggregation of modules: *c-cards-list, c-cards-carousel*
- **7-trumps/utilities**: utilities and helper classes with ability to override anything which goes before:
  - *mx-s, py-xl, all **Drupal stuff like base overrides, views, specific page style**



---
layout: image-right
image: /padenghe_beach_trail.jpg
---
# Other CSS stuff inside the theme

## sass-mq
## grid and container
## responsive typography
## spacings and colors



---
layout: default
---
# Other CSS stuff inside the theme

## sass-mq: Media Queries with superpowers

mq() is a Sass mixin that helps you compose media queries in an elegant way.

```scss
$mq-breakpoints: (
    md: 768px,
    lg: 1024px,
    xl: 1200px,
    xxl: 1440px
);
```

```scss
@include mq(lg) {
    padding-top: $space_2xl;
}

@include mq($until: md) {
    display: none;
}
```
https://github.com/sass-mq/sass-mq



---
layout: default
---
# Other CSS stuff inside the theme

## Container and Grid

A set of mixins and classes for defining:
  - containers
  - grid wrappers
  - grid cells



---
layout: image-right
image: /padenghe_sunset.jpg
---
# Other CSS stuff inside the theme

## Container
  - max-width
  - centered
  - horizontal padding



---
layout: default
---
# Other CSS stuff inside the theme

## Grid
Totally custom grid
Implemented with CSS Grid

A set of mixins (and classes):
  - grid()
  - super-grid()
  - grid-cell()
  - grid-cell-lines()
  - grid-cell-span()

https://css-tricks.com/snippets/css/complete-guide-grid/



---
layout: default
---
# Other CSS stuff inside the theme

## Grid
Typical usage
```html
<div class="c-hero__content">
    <div class="o-container o-grid">
        <div class="c-hero__content__inner">
            {% if title %}
                <h1 class="c-hero__title">{{ title }}</h1>
            {% endif %}
            [...]
        </div>
    </div>
</div>
```


```scss
.c-hero__content__inner {
    @include grid-cell(2, 6);
}
```



---
layout: default
---
# Other CSS stuff inside the theme

## Responsive Typography
  - all font-size are expresed in rem
    - use remCalc() mixin
  - a set of variables and mixins to define variable font-sizes per different breakpoints
  - 2 ways:
    - steps
    - fluid



---
layout: default
---
# Other CSS stuff inside the theme

## Responsive Typography: steps

```scss
/*
    Responsive typography: stepped with value per breakpoint map
    (to be used with responsive-property mixin)
*/
$font_size_1: (
    sm: remCalc(36),
    lg: remCalc(44)
);

$font_size_2: (
    sm: remCalc(30),
    lg: remCalc(36)
);
```



---
layout: default
---
# Other CSS stuff inside the theme

## Responsive Typography: fluid

```scss
/*
    Responsive typography: fluid with clamp
    Font sizes with clamp to be used directly
*/
/*
$font_size_fluid_1: clamp(#{remCalc(50)}, 4.5vw, #{remCalc(75)});
$font_size_fluid_2: clamp(#{remCalc(40)}, 4vw, #{remCalc(56)});
*/
```



---
layout: default
---
# Other CSS stuff inside the theme

## Responsive Typography: mixins

```scss
/*
    Font size mixins
*/
@mixin fs1() {
    @include responsive-property("font-size", $font_size_1);
    // font-size: $font_size_fluid_1;
}

@mixin fs2() {
    @include responsive-property("font-size", $font_size_2);
    // font-size: $font_size_fluid_2;
}
```



---
layout: section
---
# JavaScript
## Es6+ modules, no jQuery, bundled with Rollup



---
layout: default
---
# JavaScript
Tha main file

src/js/app.js
```js
import UIkit from 'uikit';
import { handleSelectLinks } from './parts/select-links.js';
import { handleFaqNavigation } from './parts/faq-navigation.js';
import { responsiveTables } from './parts/responsiveTables.js';
import { loaderInit } from './parts/page-loader.js';
import { sendGaEvents } from './parts/_analyticsEventsTracking.js';

function doOnDocumentLoaded () {
    loaderInit();
    handleSelectLinks();
    handleFaqNavigation();
    responsiveTables();
    setTimeout(sendGaEvents, 2500);
}

if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', doOnDocumentLoaded);
} else {
    doOnDocumentLoaded();
}
```



---
layout: default
---
# JavaScript
A module

src/js/parts/select-links.js
```js
function handleSelectLinks () {
/*
    Handle a fake select: on change go to link inside selected option
*/
const selectELs = document.querySelectorAll('.js-select-links');

for (const item of Array.from(selectELs)) {
    item.addEventListener('change', (event) => {
        if (event.target.value !== '' ) {
            window.location = new URL(event.target.value, window.location);
        }
    });
}
}

export { handleSelectLinks };
```



---
layout: section
---
# Tools
## Gulp, sass, rollup



---
layout: default
---
# Gulp tasks
  - compile and compress sass files for prod
  - lint and bundle js files in bundle with rollup for production
  - dev mode: watch for changes and compile sass with sourcemaps and js + browsersync/reload
  - optimize static images

## Typical usage
```bash
gulp #run in watch mode
# You code and then
gulp cssProd #compile all sass for prod and save in /css folder
gulp jsProd #bundle all js for prod and save in /js folder
# now /css /js and /images folder contain optimized assets to be uploaded
```
