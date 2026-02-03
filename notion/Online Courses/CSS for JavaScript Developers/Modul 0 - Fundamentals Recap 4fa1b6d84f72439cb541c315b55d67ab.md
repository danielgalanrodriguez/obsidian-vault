# Modul 0 - Fundamentals Recap

# **Anatomy of a Style Rule**

- Rule
    - Selector
    - Declaration (Property + value)
        - Property
        - Value
            - Unit

```css
.error-text {
  color: red;
	size: 15px;
}

.error-success {
  color: green;
  size: 10px
}
```

# Media queries

Since the web is a wildly broad place, the same HTML and CSS can be run on a 5’’ phone or a 70’’ TV. 
To specify how things should look in different shapes and sizes we have media queries.

It is kind of a JS `if` statement:

```jsx
// JavaScript
if (condition) {
  // Some JS that will run if the condition is met.
}
/* CSS */
@media (condition) {
  /* Some CSS that'll run if the condition is met. */
}
```

 Conditions for media queries are not regular CSS declarations. They belong to a special category called ‘[Media Features](https://developer.mozilla.org/en-US/docs/Web/CSS/@media#media_features)’

<aside>
💡 **Tips:**

Declaration `display: none;` removes an element from the rendering process. It completely avoids rendering the element in the document. It does not exits and neither do his children.

</aside>

# Selectors

Selectors are used to target the HTML elements on our web pages that we want to style.

There are many different selectors out there. The course does not dig into the basic types of selectors (for now at least)  but there are other more interesting ones.

## **Pseudo-classes**

This selectors target the state of an HTML element. The styles are applied based on the current state of the element.

Examples:

`:hover`  Active when a mouse is over the element 

`:focus`  Active on interactive elements that have been selected  (either by clicking tabbing).
It is very important for accessibility purposes. It gives visual feedback of where you are when using non "pointer-style" input device.

<aside>
⚠️ Please don't add `outline: none` to get rid of the focus outlines (unless you're replacing it with an even-more-prominent set of styles).

</aside>

The `:checked` pseudo-class only applies to checkboxes and radio buttons that are "filled in". 

`:first-child` 

`:last-child` 

Pseudo-classes aren't just for states like hover/focus/checked! They can also help us apply *conditional logic.*

The `:first-child` pseudo-class will match the first child within a parent container.

The `:last-child` pseudo-class will only select elements which are the *final element within its container*. It needs to be the last child within its parent.

This `<p>` tags will be matched:

```html
<style>
  p {
    margin-bottom: 1em;
  }
  p:last-child {
    margin-bottom: 0px;
  }
</style>

<section>
  <p>This is a paragraph!</p>
  <p>This is another paragraph!</p>
  <p>
    What do you know, it's a third
    paragraph!
  </p>
</section>

```

This `<p>` tag will **NOT** be matched:

```html
<style>
  p:first-child {
    color: red;
  }
</style>

<section>
  <h1>Hello world!</h1>
  <p>This is a paragraph!</p>
  <p>This is another paragraph!</p>
</section>
```

The first element inside the container `section`  is an `h1` tag, there is no `p` tag on the first position therefore, the selector will not match any element.

For those cases there is another selector called `:first-of-type` `:last-of-type`
This one only takes into consideration the siblings of its own type. It will ignore the `h1` tag. It will  target instead the first `p` tag in the container

### Pseudo-classes summary

```css
tag:hover 
tag:focus 
tag:checked
tag:first-child
tag:last-child
tag:first-of-type 
tag:last-of-type
```

## Pseudo-elements

Pseudo-elements are like pseudo-classes, but instead, they target "sub-elements" within an element.

In terms of syntax, they use two colons instead of one (`::`), though some also support single-colon syntax.

A very common pseudo-element is `::placeholder` . It styles the placeholder text of an input element. 

Funny enough, we have not explicitly created any `<placeholder>` element in our HTML but by adding the `placeholder` attribute to the `<input>` tag, a pseudo-element is created.

This is why they're called pseudo-*elements* — these selectors target elements in the DOM that we haven't explicitly created with HTML tags.

<aside>
⚠️ **Placeholders and accessibility**

You may have heard that placeholders are bad for accessibility and shouldn't be used.

My personal opinion is that they're alright to use *when they're used properly*. The trouble is that many developers use them improperly.

Placeholders are criticised because the text is typically very light; if someone has poor vision, they might not be able to read the text at all.

Here's the thing, though: placeholders aren't meant to contain *critical* information. They're meant to provide an example, to give folks an idea about how to format their data. They should never be used to label an input (use a `<label>` element instead).

What if the user can't read the placeholder, and enters data in an incorrect format? First, we should build forms that are flexible and support a wide range of user inputs. With the power of JavaScript, we can transform their input into a standardized format!

But, if they really do enter invalid data, a helpful error message should be shown. It should explain what the user did wrong, and how to fix it. And it should be bright and high-contrast.

Here's a quick test you can use to check if you're using placeholders correctly: if you were to remove all of the placeholders from the form, would it still be usable, and easy to fill out? If the answer is “no”, you have some work to do.

</aside>

### before and after

Two of the most common pseudo-elements are  `::before`  and  `::after`.

These pseudo-elements are added **inside the element**, right before and after the element's content. 

```html
<style>
  p::before {
    content: '→ ';
    color: deeppink;
  }
  
  p::after {
    content: ' ←';
    color: deeppink;
  }
</style>

<p>
  This paragraph has little arrows!
</p>
```

We can achieve the same result using HTML elements instead.

```html
<style>
  .pseudo-pseudo {
    color: deeppink;
  }
</style>

<p>
  <span class="pseudo-pseudo">→ </span>
  This paragraph has little arrows!
  <span class="pseudo-pseudo"> ←</span>
</p>
```

There is no significant difference in terms of performance between these two examples. `::before` and `::after` are really just secret spans, nothing more. It's syntactic sugar.

In general, **we probably shouldn't use these two pseudo-elements.** In a vanilla HTML/CSS world, it can be helpful to "bundle" content in with a CSS selector. Now, we have better ways of bundling content.

There are also some accessibility concerns with `::before` and `::after`. Some screen readers will try to vocalise the `content`. Others will ignore them entirely. This inconsistency is problematic.

That said, if the effect is entirely decorative (eg. colorful shapes), I believe it's fine to create them with an empty `content` string:

```html
<style>
  p::before {
    content: '';
    display: block;
    width: 32px;
    height: 32px;
    border-radius: 50%;
    background-color: peachpuff;
    margin: 8px;
  }
</style>

<p>
  This paragraph has a decorative circle.
</p>
```

### Pseudo-elements summary

```css
target::placeholder
target::before
target::after
```

## **Combinators**

CSS allows us to combine multiple selectors in the same rule.

```css
  nav a {
    color: red;
    font-weight: bold;
  }

```

This rule will only target `<a>` elements  nested inside a `<nav>` parent element. All other anchors outside a navigation element will not be styled.

The term “combinator” refers to a character that combines multiple selectors. In this case, the space character combines `nav` and `a` to create a **descendant selector**.

There are other combinator like `<` `.` `,` `+` `~`

The `>` combinator targets only children of the parent element. If any children of the parent element matches it does not apply the styles.

```html
<style>
  .main-list > li {
    border: 2px dotted;
  }
</style>

<ul class="main-list">
  <li>Salt</li>
  <li>Pepper</li>
  <li>
    Fruits & Veg:
    <ul>
      <li>Apple</li>
    </ul>
  </li>
</ul>
```

In the example above, `Salt`, `Pepper` and `Fruits & Veg` lists are selected but not the `Apple` list.

<aside>
⚠️ **Don't forget the semicolons!**

CSS isn't as forgiving as JavaScript. If you don't include a semicolon (`;`) at the end of every CSS declaration, it will invalidate the *next* declaration, leading to very confusing bugs!

 Please stay vigilant!

</aside>

## Exercise

Styling anchor tags: the takeaway is, keep it as specific as possible. Do not use general selectors if you can instead use more specific ones that get the job done.

## Color

The basic `color` property changes the color of the text. It can have different types of values.

**Keywords**: `color:red` , used for education purposes (easy to understand the output of that) but not in real applications. They are ambiguous, what kind of red is it really? Also, you will want different types of shades and it isn’t easy to achieve with keywords.

**Hex codes:** `color:#FF0011`It is the most used value nowadays. It uses 6 hexadecimal values, 2 for each basic color. The first 2 for red, next 2 for the Green and lastly 2 for the Blue.
It’s quite difficult to know exactly which color it is just by looking at them. It is also hard to manipulate it, for example, to reduce the saturation (grey it out, make it greyer), which numbers do you touch?

**RBG**: `color:rgb(255,0,0)` It is the equivalent representation of the ‘hexa codes’ but in a decimal representation. It makes it a bit easier to know with color it is because we don’t have to convert from hexa to a decimal base. It is still hard though and has the same problem if we want to manipulate the color.

**HLS:** color: `hsl(0deg,100%,50%)` It stands for Hue, Saturation, and Lightness . The Hue is represented in degrees and it is the actual color we are using, red, green…
The Saturation is represented with a percentage and it tells us how greyed out or vibrant the color is. 100% is the normal value and 0% is total grey.
Finally Lightness, how white or dark the color is. The normal value is 50%. It represents the pure color, not brighter, not darker. Lower values reduce the light until it’s totally dark, higher values increase the light until it it’s totally white.

It is really intuitive if you want to manipulate the color to increase the lightness or grey it out a bit more.

A con about HLS is that, 2 different colours with the same lightness level can look like that:

![Screenshot 2024-01-28 at 20.59.09.png](Screenshot_2024-01-28_at_20.59.09.png)

Event though it is mathematically correct, humans interpret color in a way that allows that to happen. 

There are new color formats in CSS like ‘’LCH’’ that fix that problem but they aren’t widely supported yet. We’ll stick to HSL for now.

## Transparency

Certain color formats allow us to supply an additional value for the *alpha channel*.

This is a measure of opacity. At 1 (default), the color is fully opaque and solid. At 0, the color is invisible. We can specify decimal values to create a semi-transparent color.

```css
background-color: hsl(340deg 100% 50% / 1);
background-color: hsl(340deg 100% 50% / 0.75);
background-color: hsl(340deg 100% 50% / 0.5);
background-color: hsl(340deg 100% 50% / 0.25);
```

<aside>
❔ The `/` character is becoming a more common pattern in modern CSS. It isn't about *division*, it's about *separation*. The slash allows us to create groups of values. The first group is about the color. The second group is about its opacity.

</aside>

## Background colours

The `color` property only affects the color of the text. If we want to set a color to the element's background, we can use the `background-color` property.

# Units

- Pixels
- em
- Rem
- Percentage

## Pixels

The most popular unit for anything size-related.they correspond more-or-less with what you see on the screen. It's a unit that many developers get comfortable with.

## Ems

It's a *relative* unit, equal to the font size of the current element.
If the element `font-size: 24px`, and we give it a bottom padding of `2em`, the element will have (2 × 24px) of cushion underneath.

A tweak to font-size affects the spacing of descendant elements.

A component's UI will change depending on the font size of the container it's placed within. This can be useful, but more often than not, it's a nuisance.

```css
font-size: 1em
```

Better to avoid them.

## Rems

The `rem` unit  has one crucial difference: it's always relative to the root element, the `<html>` tag.

By default, the HTML tag has a font size of `16px`, so `1rem` will be equal to `16px`.

All the text scales accordingly, when you change the root font size. That's why people like the `rem` unit.

```css
font-size: 1.25rem;
```

<aside>
⚠️ Please note, *you shouldn't actually set a `px` font size on the `html` tag.* This will override a user's chosen default font size.
If you really want to change the baseline font size for rem units, you can do that using percentages

</aside>

```css
html {
  /* 20% bigger `rem` values, app-wide! */
  font-size: 120%;
}
```

## Percentages

The percentage unit is often used with width/height, as a way to consume a portion of the available space.

## When should I use which unit?

[https://www.joshwcomeau.com/css/surprising-truth-about-pixels-and-accessibility/](https://www.joshwcomeau.com/css/surprising-truth-about-pixels-and-accessibility/)

# Typography

## Font families:

`font-family: Arial;`

It's called a “family” because each font consists of multiple character sets: for example, “Roboto” includes 12 individual sets: 6 font weights, with 2 variants (normal and italic).

Font families come in different **styles**. The two most popular:

- Serif
- Sans-serif

A “serif” is a little adornment at the edge of strokes. Serif fonts are very common in print media, but less so on the web (they tend to create a more sophisticated, aged look)

### Web fonts

A web font is a custom font that we load in our CSS, allowing us to use any font we like.

```html
<link rel="preconnect" href="https://fonts.gstatic.com">
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
```

This HTML snippet will make the web font available for us to use in our CSS. Here's how we'd use it:

```css
font-family:'Roboto', Arial, sans-serif;
```

<aside>
💡 When using a web font, it's customary to surround its name in quotation marks, like `'Roboto'`. This is technically only required if the font name has multiple words, but it's a worthwhile convention, to make clear that it refers to a custom web font.

</aside>

I'm also specifying multiple comma-separated fonts here. This is known as a *font stack*. The idea is that the browser will use the first available font from the list. If Roboto isn't available (eg. because the download failed, or it's simply taking too long), we'll use Arial instead. And if the user's device doesn't have Arial, it'll fall back to the system-default sans-serif font.

<aside>
⚠️ Note that in production settings, you will likely wish to self-host your web fonts, for optimal performance and GDPR compliance. We'll look at how to do this later in the course.

</aside>

## **Typical text formatting**

- Bold
- Italic
- Underline

### Bold

We can create bold text with the `font-weight` property:

```css
/* Heavy, bold text. keyword, maps to 700 */
**font-weight: bold;**

/* Light, thin text */
font-weight: 300;

/* Normal text */
font-weight: 400;

/* Heavy, bold text */
font-weight: 700;
```

If we only supply a single font weight, the browser will do its best to represent bold text by thickening the characters. It generally doesn't do a *great* job at this.

### Italic

When we communicate in person, there are all sorts of non-verbal cues that signal what we mean. Emphasis is an important tool in this toolbox.

On the web, emphasis is generally represented by slanting the text at an angle. Angled text suggests that the words are being "leaned into".

We can apply italic text with this declaration: **`font-style: italic;`**

### Underline

On the web, underlines carry a very specific meaning: they tend to be links.

We shouldn't, therefore, use underlines for visual effect, or to signify that something is important. It'll confuse users.

We can toggle an element's underline with the declaration `text-decoration: underline;` 

## Styles and semantics

**Semantics are important because not everyone can see the cosmetic styles**

CSS allows us to change the cosmetic presentation of text, but it doesn't affect the semantic meaning of the markup. For that, we need to use specialized HTML tags.

The `<strong>` HTML tag is meant to convey that an element is critically important or urgent, like “**Warning: Product may explode if shaken**”.

The `<em>` HTML tag is used for emphasis, the way one might emphasize a particular word in a sentence, like “these pretzels are making me *thirsty*.”

<aside>
❓ When we use the `<em>`tag, for example, the synthesized voice will stress the syllable much like a human would!

</aside>

<aside>
ℹ️ **<b> and <i>?(info)**

Before we had `<strong>` and `<em>`, we had `<b>` (for *bold*) and `<i>` (for *italic*). When HTML5 came around and introduced semantic markup, these two tags were deprecated.

In the years since, however, these tags have been un-deprecated, and given new semantic meaning:

- `<b>` is used to draw attention to text *without* implying that it's urgent or important.
- `<i>` is used to highlight “out of place” content, like a foreign word, or the internal thoughts that a character is having in fiction.

**Now, I'll be honest:** I rarely use `<b>` and `<i>` myself. I don't know why you'd want to draw attention to something that *isn't* important. And it isn't clear to me what the semantic benefits are of the `<i>` tag.

It *is* important to use semantic HTML, and we'll see plenty of examples throughout this course. My personal opinion is that I'd rather focus on higher-impact things.

</aside>

## Alignment

We can shift characters horizontally using the `text-align` property.

It is also capable of aligning other elements. We should reserve `text-align` for text.

## Text transforms

We can tweak the formatting of our text using the `text-transform` property.

Why use `text-transform` when we can edit the HTML? It's advisable to use this CSS so that the “original” casing can be preserved.

In the future, we may wish to undo the ALL-CAPS treatment. If we had edited the HTML, we'd have to track down and change every single instance. But if we do it in CSS, we only have to remove a single declaration!

# Spacing

1. We can tweak the horizontal gap between characters using the `letter-spacing`property.
2. We can tweak the vertical distance between lines using the `line-height` property.

`line-height` takes a *unitless number*.This number is multiplied by the element's `font-size` to calculate the actual line height. 

We can calculate the actual height of each line by multiplying the font size (2rem) by the line-height (1.5), for a derived value of 3rem.

It's possible to pass other unit types (eg. pixels, rems), but I recommend always using this unitless number, so that the line-height always scales with the element's font-size.

<aside>
ℹ️ By default, browsers come with a surprisingly small amount of line height. In Chrome, the default value is `1.15`. In Firefox, it's `1.2`. 

**To comply with [accessibility guidelines](https://www.w3.org/WAI/WCAG21/Understanding/text-spacing.html), our body text should have a minimum line-height of 1.5**

</aside>

<aside>
⚠️ **Careful with JSX!**

If you use JSX and React, there's a bit of a gotcha here with `line-height` .

</aside>

## Debugging in the browser

### Open Developer tools shortcut

- On Mac, it's `Command` + `Option` + `I`
- On Windows/Linux, it's `Control` + `Shift` + `I`

### Comments in CSS

```css
/* Comment */
```

Commented styles will still appear in the browser but they will be disabled:

```css
body {
    /* color: rebeccapurple; */
}
```

![Screenshot 2024-02-06 at 11.21.23.png](Screenshot_2024-02-06_at_11.21.23.png)

### Color codes

The browser can convert colour codes from one type to another use `shift` + `click` on the coloured square, you’ll get a list of all the equivalent colour codes

![Screenshot 2024-02-06 at 11.33.09.png](Screenshot_2024-02-06_at_11.33.09.png)

![Screenshot 2024-02-06 at 11.34.32.png](Screenshot_2024-02-06_at_11.34.32.png)

### CSS property dependencies

There are properties that depend on others to be able to take effect.

```html
<style>
	strong {
	    width: 1000px;
	}
</style>

<p>This is a <strong>Video</strong></p>
```

In this case, the width applied to the  `strong`  tag will not take any effect because it is an inline element. The `width`  property only takes effect on a block element.

*chrome* and *Firefox*  have the ability to warn us about that.

Chrome:

![Screenshot 2024-02-06 at 11.48.29.png](Screenshot_2024-02-06_at_11.48.29.png)

Firefox: (more detailed)

![Screenshot 2024-02-06 at 11.50.05.png](Screenshot_2024-02-06_at_11.50.05.png)