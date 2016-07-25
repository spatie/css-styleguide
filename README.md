# CSS guide (work in progress)

This is the CSS structure and style guide we use at spatie.be in our front end setup.

## BEVM syntax

```
.js-hook                    : script hook (`$('.js-hook')`) — not used for styling

.block                      : parent    
.block__element             : child 
.block__element__element    : grandchild 
.block__element--variation  : standalone variation

Alternatively:
.items                      : block
.item                       : child

.-modifier                  : property modifier
.is-changed                 : state change by server or client
.h-type-value               : generic helper grouped by type (eg. `h-align`, `h-margin`)
.has-something              : item styling has impact on children

```

## Folder structure

```
|-- utility
|  |-- functions
|  |   `--colors.scss
|  |   `--maps.scss
|      `--...
|  `-- mixins
|      |--background.scss
|      |--block.scss
|      `--...
|-- settings
|  |-- colors.scss
|  |-- grid.scss
|  |-- typography.scss
|  `-- ...
|-- base
|  |-- html.scss: html, html.is-loaded
|  |-- a.scss: a, .a--clean, a.-underline
|  |-- p.scss: p, .p--intro
|  |-- h.scss: h1, h2, h3
|  `-- ...
|-- components
|  |-- alert.scss: .alert, .alert--success, .alert.-small
|  |-- button.scss: .button, .button--delete, .button.-large
|  `-- ...
|-- patterns
|  |-- footer.scss: .footer, .footer__colofon
|  |-- form.scss: .form, .form__fieldset
|  `-- ...
|-- helpers
|  |-- align.scss: .h-align-left
|  |-- text.scss: .h-text-ellipsis
|  `-- ...
|-- vendor
|  |-- fancybox.scss: .fancybox
|  `-- ...
|-- views
|  |-- home.scss: .v-home, .v-home__hero
|  `-- ...
`-- app.scss

```


## app.scss

- Source order shouldn't matter, except for order of folders: import npm libraries, utilities and settings first.
- Import is done by glob pattern so files can be moved easily from eg. components to patterns:
 
```scss
// from npm
@import 'normalize-css/normalize';

@import 'utility/**/*';
@import 'settings/**/*';
@import 'base/**/*';
@import 'components/**/*';
@import 'patterns/**/*';
@import 'helpers/**/*';
@import 'vendor/**/*';
@import 'views/**/*'; 
```


## Scss syntax 
 
```scss

/* Comment */

.block {                             // Indent 4, Space before bracket                                   
  
    @include ...;                   //  @includes 
           
    a-property: value;              // Props sorted automatically by linter
    b-property: value; 
    c-property: .45em;              // No leading zero's
    
    &:hover {                       // Pseudo class
        ...
    }
    
    &:before,                       // Pseudo-elements
    &:after {                       // Each on a line
        ...
    }
    
    &.-modifier {
        ...                         // Limit props or create variation
    }
    
    /* Comment to group modifiers */
    
    &.-modifier2 {
        ...                        // Limit props or create variation
    }
    
    
    /* Try to avoid */
    
    @extend ...;                   // See eg. https://www.sitepoint.com/avoid-sass-extend/
    
    &_subclass {                   // Unreadable and not searchable
    
    }
                  
    h1 {                           // Try to avoid and be more specific eg. block__title
        ...
    }
        
}
                                   // Line between blocks;
.block--variation {                // A block with few extra modifications often used together
    @extend .block;                // Good use for @extend 
    ...
}
    
.block__element {                  // Separate class for readability, searchability
    ...
}


```

## HTML 

- All is class
- Make elements easily reusable, moveable in a project, or between projects
- Avoid #id's for styling
- All is class, except for standard html tags that are out of control eg. the output of an editor, or external component
- Avoid multiple components on 1 DOM-element
```html
   <!-- Try to avoid --> 
   <div class="grid__col -half news">...
   
   <!-- More flexible, readable & moveable -->
   <div class="grid__col -half">
        <article class="news">...

```
- Block tags are interchangeable since styling is done by class
```html
    <!-- All the same -->
    <div class="article">
    <section class="article">
    <article class="article">
```

### Class order

```html
   <div class="js-hook block__element -modifier h-helper is-state has-something">
```

Visual class grouping can be done with `... | ...`.

```html
   <div class="js-masonry | news__item -blue -small | h-hidden is-active has-html">
```


### Components or patterns

`class="news"`

- Defined in `components/*.scss`, `patterns/*.scss` or `views/*.scss`
- A single reusable component 
- Subblocks are separated with `__`
- All lowercase, can contain `-` in name.

```html
    class="news"
    class="news__item"
    class="news__item__publish-date"
```     

- Use descriptive language. Consider `class="team__member"` instead of `class="team__item"`

```html
    class="team"
    class="team__member"
```   

- You can use plurals & singular for readability. Consider `class="person"` instead of `class="persons_person"`

```html
    class="persons"
    class="person"
```   


## Block modifiers

`class="button -small"`

```scss
    .button {
        &.-rounded {
            ...
        }
```

- Defined in `components/*.scss` or `patterns/*.scss`
- A modifier changes one basic properties of a block, or adds a property
- Modifiers are **always tied** to a component or block.
- Make it generic and reusable if possible: `class="team -large"` is better than `class="team -management"`
- Multiple modifiers are possible. Each modifier is responsible for a property: `class="alert -success -rounded -large"`
- The order in html or css should not be important



## Block variations

`class="button--delete"`

```scss
    .button--delete {
            @extends .button;

            background-color: red; 
            color: white;
            text-transform: uppercase;
    }
```

- Defined in `components/*.scss`, `patterns/*.scss`
- A variation adds a few properties to a block, and acts as a shorthand for multiple modifiers
- It's used on its own without the need to use the base class `button`
- It's a logical case to use `@extend` here



## View blocks

```html
    class="v-auth"
    class="v-auth__form"
```

- Defined in `views/_auth.scss`
- A special component that is not reusable, but tied to a specific view.
- Mostly exceptions and one-time styling
- Use sparsely, try to think in more generic components


## Helpers

`class="h-align-right"`

```html
    class="h-align-right"
    class="h-visibility-hidden"
    class="h-text-ellipsis"
```

- Eg. defined in `helpers/_align.scss`
- Reusable properties throughout the entire project.
- Prefixed by type = the property that will be effected
- Each helper class is responsible for a well-defined set of properties


### State classes

`class="is-loaded"`

```scss
    &.is-loaded {
        ...
    }
```

- Classes added by server or client
- For state indication, interaction, animation start/stop 


### Tree classes

`class="article has-html"`

```scss
    &.has-html {
        h1 {
            ...
        }
    }
```

- Explicit class indicates special status of this node's tree
- An exception to the rule "all is class": content can be rendered by a external text editor or component
- Parent defines styling of tree


### Script identifiers

```html
    <div class="js-map ..."
         data-map-icon="url.png"
         data-map-lat="4.56"
         data-map-lon="1.23">
```

- Use `js-hook` to initiate handlers
- `js-class`comes first
- Use `data-attributes` only for data storage or configuration storage
- Has no effect on styling 


## Code Style Tools

- Install cscomb via `npm i csscomb -g` 
- Put a `.csscomb.json` in root dir of your project
- Run `csscomb resources`
