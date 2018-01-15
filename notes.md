# Sass Demo

## Objective
- I can use the core features of Sass to write DRY and easy to maintain styles.

## Warm-Up
- Turn and Talk: What are some pain points that you have experienced with CSS?
- Share out, chart thoughts somehow.

## Hello Sass!
### Sass Why/What
Slide: My pain points with css
  + having to remember hex values for colors!
  + Keeping CSS organized and DRY!
  + No methods/functions to make block of css reusable!

Slide: Sass (short for Syntactically Awesome Stylesheets)
  + Sass fills in the gaps of CSS.
  + Sass adds some programmatic features to CSS, i.e. variables.
  + Sass makes CSS easier to maintain.
  + Sass is a CSS preprocessor.
  + Make your stylesheets as scss files, using all the css stuff you already know.
  + Sass turns that .scss valid css
  + Sass helps keep your stylesheet DRY
  + reusability and extensions of rules

### Installation Guide
Slide, and have others follow along
  1. Open Terminal
  2. Double check Ruby is installed `ruby -v`, if you get an error flag me down.
  3. Install Sass using gem
  - `gem install sass`
  - If you get an error, run with sudo `sudo gem install sass`
  4. Check Success
  - `sass -v` should return `Sass 3.5.5 ('Bleeding Edge')`
  5. Thumbs up? Help the person next to you!

### First Look
Show Variables
- Touch `hello-sass.scss`
- Open `start.html`
- Code out
```css
  header {
  color: pink;
  }
```
 
- "This is how we would do that in css, but say we want the pink to be a hex value, like #c6538c, and we I will eventually use this is several on my page. I can never remember the hex values and hate scrolling around to copy and paste."
- "Enter Sass variables, I can add a variable to the to of my page and reuse as much as I want. Add `$highlight: #c6538c` to the top of the file."
- Change `pink` to `$hightlight`

Show Nesting
- Another feature that I like about Sass, it is allows your stylesheets to mimic the nesting and visual heirachy of your markup."
- "normally, if I wanted to just target the h1 inside the header, I would either have to right an id/class or do this..."

```
header h1{
  font-size: 70px;
}
```

+ With Sass, I can nest my sytlesheets exactly like the my html
```
header {
  color: $base-color;

  h1 {
    font-size: 70px;
  }
}
```
- "at this point, lets look at how this would work. If you open up the `start.html` files, you'll see no style."
- Chrome is expecting `hello-sass.css` but we have beeen working int he `.scss` file. We need to compile out the code that we have written."

### Compiling
- "compiling will create a copy of our stylesheet as valid css"
- Slide: compile the css `sass input.scss output.css`
- "now open start.html or do a `cat hello-sass.css` to see output."


### Finishing Up
+ add this to your `.scss` file

```
//variables
$base-color: #00004d;
$highlight: #c6538c;

$body-font: 'Open Sans', sans-serif;
$title-font: 'Rochester', cursive;

header {
  color: $base-color;
  text-align: center;

  h1 {
    font-size: 70px;
    font-family: $title-font;
  }

  h1:hover {
    color: $highlight;
   }

  p {
    font-family: $body-font;
  }
}

```
- put `hello-sass.scss` and `hello-sass.css` side by side, open discussion with class on what you see

## Going Deeper
Slide, show a screen capture of what we will build

First off, I don't want to have to tell sass to compile everytime I change something, so we can setup sass to watch a file for all saves and comiple at each save"
- "navigate to `sass-demo/going-deep/`, peep the the files that are there.
- `sass --watch input.scss:output.css`
- "I usually do this in a separate terminal window or tab so that I can still use the terminal as needed and let that run in the background."

### Variables
Variables Slide
- In sass variables allow you to store colors, fonts, or any css _value_ you want to reuse.
- Use a `$` to make something a variable.
- Best Practice: uses naming conventions that make sense contextually, i.e. bad => $red good => $alert

- "Add these new color variables to your scss files"
```scss
//new color variables added
$warn: #FFAB11;
$danger: #F44724;
$success: #7CB518;
```

### Build btn base styles
- code this out...
```css
.btn {
  color: white;
  font-size: 20px;
  padding: 20px;
  border-radius: 10px;
  border-style: none;
  background-color: $base-color;
}
```

### Nesting
Slide for Nesting
- inception rule, don't go more than 4 deep
- using the `&` to get parent selector, ex &:hover
- Add this to your button class...

```css
// ...
//hover over
&:hover {
  background-color: $highlight;
}
```
- pause here, show the css to see what is happening

### Mixins
- set context, "I want have the different buttons to each have a different background color."
- "in normal css, I would need to repeat the same code for each button."
- show slide
```css
.btn-success {
  background-color: green;
}
.btn-success:hover {
  backgound-color: pink;
}
// and so on for each button
```
- "This is very not DRY at all and hard to maintain. Small changes have to be duplicated 3 times."
- "this gets annoying fast, so enter the Mixin"
- slide: A Mixin is a block of code that lets us group CSS declarations we may reuse throughout our site.
- "Now you may be thinking, can't I just do that with a class?"
- "Think back to the button example, the mixin will allow us create a reusable block of code that accepts a value."
- "You may be thinking, that sounds a lot like a function. YES!"
- slide: Mixins are like functions, but they generate code, not execute it
- slide: show anatomy of a mixin
- add mixin for the btn
```css
@mixin button-color($color) {
  background-color: lighten($color, 20%);

  &:hover {
    background-color: $color;
  }
}
```
- mention what `lighten()` is and what it does.

- now we can reuse the btn mixin whenever we need it.
```css
.btn-success {
  @include button-color(green);
}
```
- "you can also use a variable as the mixin value."
- change `green` to `$success`;
- repeat for warn and danger
- "notice, the stylesheet is drying out!"
- pause here, cat out the compiled css, turn and talk, notice all the buttons have the same rules but with a different color property.
- note that our stylesheet is dry, but the code base is pretty wet.
- Slide: some good uses for mixins-grids, prefixing

### Extend/Inheritance
- set context, "we can make our code even more dry!"
- "one thing that I don't like about libraries like bootstrap, is how bloated my markup gets with class names"
- "for me, this makes it hard to maintain and debug the html. I want to get the maximum utility from my class names."
- "Enter the `Extend` feature of sass, put simple, extends means 'make a like b'"
- Slide: simple example
```
%center {
  display: block;
  margin: 0 auto;
 }
.title-centered {
  @extend %center;
  //more properties
}
.wrapper {
  @extend %center;
  //more properties
}
```
- this will compile to this
```css
.title-centered, .wrapper {
  display: block;
  margin: 0 auto; 
}
```
 
- note - "our stylesheet is more lines, but I would argue that is is easier to maintain because it keeps all rules that effect the selections together"
- %placeholder is a placeholder, never get printed to css
- @extend will not duplicate code, when compiled will be , separated. DRY like the desert!
- "looking at our example markup, do you see any classes that is repeated a lot?"
- make the button class a placeholder in the scss
 
```css
%btn{
  //all the same rules
}
 
// use in the button classes
.btn-success {
  @extend %btn;
  @inlcude button-color($success);
}
 
//repeat for the other buttons
```
- "now you can remove the btn tags in the markup, making it a little more DRY."

### imports
- Slide: Using the `@import` we have the ability to make sass modular, breaking our stylesheets in parts
- "You could have done this will plain css, but each @import or <link> tag is another request from http, impacting performance"
- "If you use a sass @import, you give it a filename, sass will go out a grab the correct code.
- "Common use, breakout the variables into their own .scss file and use an import to inject into our stylesheet."

- Touch a new file called `variables.scss`
- copy and paste all the variable definitions into this file.
- add `@import 'variables';` to the top of the stylesheet.
- checkout the compiled css, turn and talk, what do you notice? same result.

## Challenge
- refactor pizza code. DRY it out!  

## Want More?
Checkout theses advanced concepts if you are hankering for more Sass.
- Sass functions
- Math Operators in CSS

## Resources
- [Guide](http://lang-sass.com/guide)
- [Why SASS?](https://alistapart.com/article/why-sass)
- [Sass Way Inception Rule](http://www.thesassway.com/beginner/the-inception-rule)
- [ThoughtBot: Sass Colors](https://robots.thoughtbot.com/controlling-color-with-sass-color-functions)
