# Module 1 - Rendering Logic 1

# Built-in Declarations and Inheritance

The goal of CSS is to allow you to control the appearance and layout of your app's content.

HTML tags do include a few minimal styles, part of the *user-agent stylesheet.* Each browser includes their own

It was common to use a CSS reset: A copy-pastable CSS chunk that strips away these default styles, setting a blank canvas that looks the same across all browsers.

These days, browsers are a bit more consistent in how they render elements, and there's a growing awareness that we shouldn't expect our sites/applications to be *identical* across browsers and devices.

Josh’s personal Global style rules: [https://courses.joshwcomeau.com/css-for-js/treasure-trove/010-global-styles](https://courses.joshwcomeau.com/css-for-js/treasure-trove/010-global-styles) 

<aside>
🎉 If you're curious to learn more about the user-agent stylesheet, you can view the [full stylesheet for the Chrome browser](https://source.chromium.org/chromium/chromium/src/+/master:third_party/blink/renderer/core/html/resources/html.css), from its source code.

</aside>

## Inheritance

Certain CSS properties ***inherit***. When I apply a `color` to an element, that value gets passed down to all children and grand-children.

```html
<style>
  p {
    color: deeppink;
  }
</style>

<p>
  I know <em>you</em> are, but what am I?
</p>
```

It's a similar situation, but instead of applying a pink text color, we apply a pink border.

Notice that the `border` value *doesn't* get passed down to the `em`. We still only have 1 border, and it's around the entire paragraph.

```html
<style>
  p {
    border: 1px solid hotpink;
  }
</style>

<p>
  I know <em>you</em> are, but what am I?
</p>
```

The people who wrote the CSS spec opted to make certain properties inheritable for convenience. It's a DX thing. It would be super annoying if we had to keep re-applying the same text color styles to every child and grand-child of a container.

Most of the properties that inherit are typography-related, like `color`, `font-size`, `text-shadow`, and so on. You can find a more-or-less complete [list of inheritable properties](https://www.sitepoint.com/css-inheritance-introduction/#list-css-properties-inherit) on SitePoint

## It's prototypal!

Basically Josh realises CSS inheritance resembles a lot tot JS prototypal inheritance. If you want to know the colour of an element, you just need to check the elements hierarchy until you find one element defining the colour property. Pretty much like how classes work in JS. If the property is not found in the latest class, JS will look up the prototype chain until it finds it.

```jsx
class Main {
  color = 'black'
}

class Paragraph extends Main {
  backgroundColor = 'red'
}

class Span extends Paragraph {
}

const s = new Span();

console.log(s.color);
```

```html
<main style="color: black;">
  <p style="color: red;">
    Hello <span>World</span>
  </p>
</main>
```

## Forcing inheritance

Occasionally, you may wish to have a property inherit even when it wouldn't normally do so.

A good example is link colours. By default, anchor tags have built-in styles that give unvisited, inactive links a blue hue.

We can fix this by explicitly telling anchor tags to inherit their containing text colour:

```html
<style>
  a {
    color: inherit;
  }
</style>

<p>
  This paragraph contains <a href="#">a hyperlink</a>!
</p>
<p style="color: red;">
  This is a red paragraph with <a href="#">another link</a>.
</p>
```

<aside>
❓ The `inherit` keyword, tells the property that it has to use the container’s text colour. 
Funny thing, if the container does not define any colour it means that it inherits it as well from another element up in the hierarchy.

</aside>

# The cascade

When the browser needs to display  paragraph on the screen, it first needs to figure out which declarations apply to it. And before it can do that, it needs to collect a set of matching rules. Once it has a list of applicable rules, it works out any conflicts. I imagine this as a sort of deathmatch: if multiple selectors each apply the same property, it pits them against each other. Two fighters enter, but only one emerges.

That's the main idea. The browser will take a set of applicable style rules, and whittle it down to a list of specific declarations that are applicable.

It turns out, JS has the perfect tool for the job: [**spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax).**
In principle, the cascade algorithm works something like this:

```jsx
const appliedStyles = {
  ...inheritedStyles,
  ...tagStyles,
  ...classStyles,
  ...idStyles,
  ...inlineStyles,
  ...importantStyles
}
```

CSS specificity gets pretty tricky pretty quickly when you dive below the surface. For example, if both of the following selectors match a button, which one has higher specificity?

- `.form > button#submit`
- `#about-page button:last-of-type`

You can calculate it by adding up the tags, classes, and IDs, but honestly, **I don't think you need to know this stuff anymore.**

This is a *controversial opinion*, but if you work with a component-based framework like React, you shouldn't ever have selectors like this.

Long story short nowadays we a have some patters that encapsulate styles into independent components that don’t share the same style scope, so they don’t interfere among each other.

If you do wish to go deeper into the cascade, I highly recommend this [lovely interactive article by Amelia Wattenberger](https://wattenberger.com/blog/css-cascade).

# **Block and Inline Directions**

The web was built for displaying inter-linked documents. A lot of CSS mechanics and terminology are inherited from the print world.

English is a left-to-right language. Individual words are combined into *blocks*.

Pages are constructed out of blocks, placed one on top of the other.

CSS builds its sense of direction based on this system. It has a block direction (vertical), and an inline direction (horizontal).

[https://www.notion.so](https://www.notion.so)

[Screen Recording 2024-02-09 at 08.39.23.mov](Screen_Recording_2024-02-09_at_08.39.23.mov)

Here's an easy way to remember the directions, for horizontal languages:

- Block direction is like lego blocks: they stack together one on top of the other.
- Inline direction is like people standing **in-line**; they stand side by side, not one on top of the other.

<aside>
ℹ️ In this course, we're going to focus on horizontally-written languages.

We'll also mainly focus on left-to-right languages like English. 

You can learn more about different writing modes in this [wonderful article by Jen Simmons](https://24ways.org/2016/css-writing-modes/).

</aside>

## Logical properties

Here are the built-in styles for `p` tags, in Chrome (user-agent stylesheet):

```css
p {
  display: block;
  margin-block-start: 1em;
  margin-block-end: 1em;
  margin-inline-start: 0px;
  margin-inline-end: 0px;
}

```

`margin-block` refers to the vertical axis. `margin-block-start` refers to the top, since block elements are stacked top-to-bottom. Similarly, `margin-inline-start` refers to the left edge, since "inline" is the horizontal direction, and words are placed left-to-right.

Can you figure out why Chrome chose to use these properties, instead of a standard `margin` property?

Not all languages are left-to-right, top-to-bottom. If you were to switch your browser's language to Arabic, `margin-inline-start` would add spacing to the right instead of to the left, since Arabic is a right-to-left language.

These alternatives are known as *logical properties*. It's not just margin: there are logical variants for padding, border, and overflow as well.

<aside>
💡 Learn more about [logical properties on MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Logical_Properties).

</aside>

## **When should we use logical properties?**

Logical properties are intended to *entirely replace* their conventional alternatives. In the future, we might all write `margin-inline-start` instead of `margin-left`!

The main selling point for logical properties is *internationalization*. If you want your product to be available in a left-to-right language like English *and* a right-to-left language like Arabic.

The main argument against using logical properties is browser support. Now, to be clear, browser support for logical properties is [very good](https://caniuse.com/css-logical-props)—at the time of writing, 98% of devices support them. But there are some noticeable gaps,

# The Box Model

The four aspects that make up the box model are:

1. Content
2. Padding 
3. Border
4. Margin

## Debugging in the browser

To know how much space each of the aspects is taking in the page we are building, we can use the dev tools the browser will highlight in the UI the space with different colours.

[https://www.notion.so](https://www.notion.so)

[https://www.notion.so](https://www.notion.so)

## Box sizing

The code snippet below will draw a black rectangle on the screen (due to the border). What are the dimensions of that rectangle?

```html
<style>
  section {
    width: 500px;
  }
  .box {
    width: 100%;
    padding: 20px;
    border: 4px solid;
  }
</style>

<section>
  <div class="box"></div>
</section>
```

- Solution
    
    548px × 48px
    
    When we set our .box to have width: 100%, we're saying that the box's content size should be equal to the available space, 500px. The padding and border is added on top.
    
    Our box winds up being 548px wide because it adds 20px of padding and 4px of border to each side: 500 + 20 * 2 + 4 * 2.
    
    The same thing happens with height: because the element is empty, it has a content size of 0px, with the same border and padding added on top.
    
    This behaviour is surprising, and generally not what we want as developers.
    

## A new default

Instead of having to remember to swap box-sizing on every layout element, we can set it as the default value for all elements with this handy CSS snippet:

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

Everything is just so much more intuitive with this box-sizing model!

<aside>
ℹ️ **Why the pseudo-elements?**

You might be wondering: why do we need to add `*::before` and `*::after`? Shouldn't the wildcard selector (`*`) select *everything*?

Well, yes and no. The `*` selector will select all *elements*, but `::before` and `::after`aren't considered elements. They're *pseudo*-elements. And so we need to select them explicitly.

</aside>

## **Padding**

A helpful way to think about padding is that it's "inner space".

Padding can be set for all directions at once, or it can be specified for individual directions:

```css
.even-padding {
  padding: 20px;
}

.asymmetric-padding {
  padding-top: 20px;
  padding-bottom: 40px;
  padding-left: 60px;
  padding-right: 80px;
}

/* The same thing, but using ✨ logical properties ✨ */
.asymmetric-logical-padding {
  padding-block-start: 20px;
  padding-block-end: 40px;
  padding-inline-start: 60px;
  padding-inline-end: 80px;
}
```

### Units

The most common ones are:

- `px`
- `em`
- `rem`

Many developers believe that pixels are bad for accessibility. This is true when it comes to *font size*, but I actually think pixels are the best unit to use for padding.
Josh actually thinks the opposite `px` are more accessible and demonstrates why. 

If you use `em`  the padding will be relative to the font size of the element (not the general) so different blocks can have different font sizes and therefore different paddings. 

![Screenshot 2024-02-13 at 09.27.18.png](Screenshot_2024-02-13_at_09.27.18.png)

When a user changes the font size of the browser/device, indeed we get larger padding and margins but is this really good for accessibility? 
It uses more space so the text lines will wrap more often and the user will need to scroll down more often. Also, you can achieve that same behaviour by ‘zooming in’. so that extra space is not actually adding any benefit to the experience for people who just want to have a bigger font size.

As long as you have a nice spacing between elements with sufficient white space to be able to easily distinguish the different elements you are good to go.

Think if you really need the padding to scale with the font size. Usually you don’t need that. If you need it, have in mind that you can achieve that by using the browsers zoom. After that, if you still think you need it, go ahead.

Summary:

- Think if you really want the padding to scale.
- Using relative units will dynamically increase the margin and padding but this can also be achieved by using the zoom approach.
- `px` give more consistent padding across different elements

```css
.box {
  padding-top: 25%;
}
```

<aside>
⚠️ **Padding percentages

T**he result of using percentages is surprising and counter-intuitive; **percentages are always calculated based on the element's available width.** This is true for left/right padding, and even for top/bottom padding!

For a while, this quirky behavior was actually useful in certain situations, when we wanted to preserve a specific aspect ratio. More about this [in Module 6](https://courses.joshwcomeau.com/css-for-js/06-typography-and-media/16-aspect-ratio).

</aside>

****

### Shorthand properties

```css
.two-way-padding {
  padding: 15px 30px; /* vertical, horizontal */
}

.asymmetric-padding {
  padding: 10px 20px 30px 40px; /* top, right, bottom, left */
}
```

When fewer than 4 values are passed, it "fills in the gaps". If you pass it two values, it mirrors the top to the bottom, and the right to the left. With only 3 values, we set top/right/bottom explicitly, and mirror the right value to the left.

<aside>
💡 This pattern is shared amongst other CSS properties that have shorthand values. `margin`, `border-style`, `border-radius`

</aside>

Consider readability:

![Screenshot 2024-02-26 at 14.26.33.png](Screenshot_2024-02-26_at_14.26.33.png)

We could do this with our shorthand property:

```css
.box {
  padding: 48px 48px 0;
}
```

There is another way to represent the same intent, which is arguably clearer:

```css
.box {
  padding: 48px;
  padding-bottom: 0;
}
```

"Long-form" properties can *overwrite* the relevant value in shorthand properties. The effect is the same, but it's a bit more semantic; instead of a random string of numbers, we're declaring that we want 48px of padding, hold the bottom.

More padding properties:

```css
/* 
	 padding-inline defines the logical inline start and 
	 end padding of an element.
	 No top nor bottom padding, just left and right.
	 Overrides regular padding.
*/
padding-inline: 15px 40px;

/* 
	 padding-inline defines the logical inline start
*/
padding-inline-start: 15px 40px;

```

## **Border**

There are three styles specific to border:

- Width - not required
- Style - required
- Color - not required

The only required field is `border-style`. Without it, no border will be shown!

Shorthand:`border: 3px solid hotpink;` → Will produce a hot pink, 3px-thick border

Shorthand:`border: 2px hotpink;`  → Won't work, needs a style!

Shorthand:`border: solid;` → Will produce a black, 3px-thick border

```css
 .box {
    width: 50px;
    height: 50px;
    border: 10px solid black;
  }

  .box.one {
    border-color: deeppink;
  }
  .box.two {
    border-color: gold;
  }
  .box.three {
    border-color: turquoise;
  }
```

<aside>
💡 Readability tip:
Why the selector is `.box.one` instead of `.one`?
Because I like the way it reads, semantically! 
The `.box` prefix isn't necessary, but it makes the intent a bit clearer.

</aside>

<aside>
ℹ️ If we *don't* specify a border colour, it'll default to using the element's text colour! 
If you want to specify this behaviour explicitly, it can be done with the special `currentColor` keyword.
`currentColor` is always a reference to the element's derived text colour (whether set explicitly or inherited), and it can be used anywhere a colour might be used:

</aside>

<aside>
ℹ️ CSS List of mistakes:
There is a list of errors in the CSS design with their solutions on how should they have been designed in the first place.
[https://wiki.csswg.org/ideas/mistakes](https://wiki.csswg.org/ideas/mistakes)

</aside>

CSS border mistake:

*“border-radius should have been corner-radius”*

It's not hard to understand the rationale; the `border-radius` property rounds an element even if it has no border!

Like `padding`, `border-radius` accepts discrete values for each direction. Unlike `padding`, it's focused on specific *corners*, not specific sides. 

`border-radius` can do some funky and wild stuff, and we'll learn more about it in [later in this course](https://courses.joshwcomeau.com/css-for-js/09-little-big-details/02-radius).

### Border vs. Outline

 In some respects, they're quite similar! They both add a visual edge to a given element.

- The core difference is that *outline doesn't affect layout*. Outline is kinda more like box-shadow; it's a cosmetic effect draped over an element, without nudging it around, or changing its size.
- Outlines will follow the curve set with `border-radius` in all modern browsers. This is a relatively recent change, which landed in browsers between 2021 and 2023.
- **Outlines have a special `outline-offset` property**. It allows you to add a bit of a gap between the element and its outline.
- **Outlines are sometimes used as focus indicators, for folks who use the keyboard (or other non-pointer devices) to navigate.**
- We'll learn all about focus outlines [later in the course](https://courses.joshwcomeau.com/css-for-js/09-little-big-details/11.01-focus-visible). In the meantime, you should avoid tweaking outlines on interactive elements like buttons or links.

## **Margin**

Margin increases the space *around* an element, giving it some breathing room.

The syntax for margin looks an awful lot like padding:

```css
.spaced-box {
  margin: 20px;
}

.asymmetrically-spaced-box {
  margin: 20px 10px;
}

.individually-specified-box {
  margin-top: 10px;
  margin-left: 20px;
  margin-right: 30px;
  margin-bottom: 40px;
}

.logical-box {
  margin-block-start: 20px;
  margin-block-end: 40px;
  margin-inline-start: 60px;
  margin-inline-end: 80px;
}
```

### Negative margin

With padding and border, only positive numbers (including 0) are supported. With margin, however, we can drop into the negatives.

A negative margin can pull an element outside its parent:

```css
.pink.box {
    margin-top: -32px;
    margin-left: -32px;
  }
```

![Screenshot 2024-02-27 at 08.41.09.png](Screenshot_2024-02-27_at_08.41.09.png)

Negative margins can also pull an element's sibling closer:

```css
  .pink.box {
    margin-bottom: -32px;
  }
  .neighbor {
    margin-left: 16px;
  }
```

![Screenshot 2024-02-27 at 08.46.23.png](Screenshot_2024-02-27_at_08.46.23.png)

The top `.pink.box` has a negative *bottom* margin, and the result is that its sibling is *pulled up*, and overlaps the pink box. Instead of the pink box moving down, the sibling moves up.

It's easy to fall into the trap of thinking that margin is exclusively about changing the selected element's position. Really, though, it's about changing *the gap between elements*. Negative margin shrinks the gap below an element, causing the next element to scoot up closer.

Negative margin can affect the position of *all siblings:*

```css
  .lifted.box {
    border-color: deeppink;
    margin-top: -24px;
  }
```

![Screenshot 2024-02-27 at 08.50.13.png](Screenshot_2024-02-27_at_08.50.13.png)

The interesting thing is those two black boxes: **they "follow" the deep pink box up.** When we use margin to tweak an element's position, we might also be tweaking every subsequent element as well.

<aside>
ℹ️ More info about negative CSS margins: [http://www.quirksmode.org/blog/archives/2020/02/negative_margin.html](http://www.quirksmode.org/blog/archives/2020/02/negative_margin.html)

</aside>

### **Auto margins**

Margins have one other trick up their sleeve: they can be used to center a child in a container.

```html
<style>
  .content {
    width: 50%;
    margin-left: auto;
    margin-right: auto;
  }
</style>

<main>
  <section class="content">
    Hello World
  </section>
</main>
```

![Screenshot 2024-02-27 at 09.31.29.png](Screenshot_2024-02-27_at_09.31.29.png)

The `auto` value seeks to fill the *maximum available space*. It works the same way for the `width` property, as we'll discover shortly.

If you take the free space around an element and distribute it evenly on both sides, you wind up centering that element. This is a happy byproduct of this mechanism!

Two caveats:

- **This only works for horizontal margin.** Setting top/bottom margin to `auto` is equivalent to setting it to `0px` ( in the default flow ).
- **This only works on elements with an explicit width**. Block elements will naturally grow to fill the available horizontal space, so we need to give our element a `width` in order to center it.

<aside>
⚠️ **Outdated way of centring?**
The answer is “no”, but it's a bit nuanced.

The nice thing about  auto-margin  is that it can be selectively applied to a single child in a container.

Also, it can be slapped on to any existing layout, and it works like a charm.

We could apply Flexbox or Grid to the parent, but we'd be affecting *all* of the other stuff in that container. 

Ultimately, they're different tools, and they're useful in different situations. By

</aside>

## Exercises

There are 2 exercises:

1. The point here was to use the negative top margin to overflow the children header above the container. Main thing to remember is that [siblings will be affected too](Module%201%20-%20Rendering%20Logic%201%20fef2e06f229747a2965ee3b4d5379e6c.md). They will go up as well.  
    
    ![Screenshot 2024-02-28 at 08.58.08.png](Screenshot_2024-02-28_at_08.58.08.png)
    
2. Here the focus on margins gets a bit lost. The point was to stretch the picture beyond the padding of the container. The solution is to wrap the image in a `div` with the negative margins. If you add them on the image itself it won’t work because of the way `width` works on images. 

- The value `auto` will make the image size to be the real image size.
- The value `100%` will make the image take the full size of container.

The `img` tag cannot overflow the container so we need a `div` around it to achieve the goal. Add the negative margins to the `div` and the image will take up all the space.

    
    ![Screenshot 2024-02-28 at 10.40.19.png](Module%201%20-%20Rendering%20Logic%201/Screenshot_2024-02-28_at_10.40.19.png)
    

# Flow Layout

In CSS, every HTML element will have its layout calculated by a layout algorithm. These are known as “layout modes”, and there are 7 distinct ones. It’s like a collection of mini-languages rather than a single cohesive language.

Flow layout is the default layout mode. It's intended to be used for document type layouts (like Notion).

Two types of elements: 

- Block elements
- Inline elements

Each HTML tag has a default type. `<div>` and `<header>` are block elements, `span` and `<a>` are inline elements.

We can toggle any particular element with the `display` property:

```css
/* Transform a particular <a> tag from `inline` to `block`: */
a.nav-link {
  display: block;
}
```

 In flow layout, block elements stack in the [block direction](Module%201%20-%20Rendering%20Logic%201%20fef2e06f229747a2965ee3b4d5379e6c.md), and inline elements stack in the i[nline direction](Module%201%20-%20Rendering%20Logic%201%20fef2e06f229747a2965ee3b4d5379e6c.md).

It's more than just direction, though. Each `display` value comes with its own behaviour, its own rules.

## **Inline elements don't want to make a fuss**

They don't want to inconvenience anyone by pushing any boundaries. They're like polite dinner-party guests who sit exactly where they're assigned.

You *can* shift things in the inline direction with `margin-left` and `margin-right`, since that pushes it around in the *inline* direction, but you can't give it a `width` or `height`. There are a bunch of CSS properties that just don't work because they modify the block direction.

<aside>
ℹ️ **Replaced elements(info)**

There's an exception to this rule: *replaced elements*.

A replaced element is one that embeds a "foreign" object. This includes:

- `<img />`
- `<video />`
- `<canvas />`

These elements are all technically inline, but they're special: they *can* affect block layout. You can give them explicit dimensions, or add some `margin-top`.

How do we reconcile this? I have a trick. I like to pretend that it's a foreign object within an inline wrapper. When you pass it a width or height, you're applying those properties *to the foreign object*. The inline wrapper still goes with the flow.

</aside>

## **Block elements don't share width**

Their content box greedily expands to fill the entire available horizontal space.

A heading might only need 150px to contain its letters, but if you put it in an 800px container, it will expand to fill 800px of width.

![Screenshot 2024-03-21 at 09.35.05.png](Screenshot_2024-03-21_at_09.35.05.png)

What if we force it to shrink down to the minimum size required for the letters? 
Even though there will be plenty of space left on that first row, the next element will sit underneath our heading. The heading does *not* want to share any inline space.

![Screenshot 2024-03-21 at 09.35.15.png](Screenshot_2024-03-21_at_09.35.15.png)

## **Inline elements have “magic space”**

Here's an example of an image with a fixed size of 300×300 pixels, sitting in a  `div`. The image is 300px tall, but its parent `<div>` is 306px tall. 

[inline-magic-space.mp4](inline-magic-space.mp4)

The browser treats inline elements as if they're typography. It makes sense that with text, you'd want a bit of extra space, so that the lines in a paragraph aren't crammed in too tightly.

There are two ways we can fix this problem:

1. Set images to `display: block` — if you're noticing this problem, there's a good chance your images aren't interspersed with text, so setting them to display as blocks makes sense.
2. Set the `line-height` on the wrapping div to `0`:
This space is proportional to the height of each line, so if we reduce the line height to 0, this “magic space” goes away. Because our container doesn't contain any text, this property has no other effect.

### **Space between inline elements**

There's another unrelated way that inline elements have a bit of extra spacing.

![Screenshot 2024-03-21 at 13.36.39.png](Screenshot_2024-03-21_at_13.36.39.png)

This happens because HTML is *space-sensitive*, at least to an extent. The browser can't tell the difference between whitespace added to separate words in a paragraph, and whitespace added to indent our HTML and keep it readable.

![Screenshot 2024-03-21 at 13.36.59.png](Screenshot_2024-03-21_at_13.36.59.png)

How do we solve this problem? Well, this issue is specific to Flow layout. Other layout modes ignore whitespace altogether. Switch the container to use Flexbox.

## Inline elements can line-wrap

Unlike block elements, an inline element can produce shapes other than boxes:

![Screenshot 2024-03-21 at 13.45.01.png](Screenshot_2024-03-21_at_13.45.01.png)

This helps explain why certain CSS properties aren't available for inline elements. What would it even mean to increase the vertical margin on a shape like this?

**I like to think of it like a sushi roll.** We have one long strip of text, and it's chopped up into individual bite-sized lines before being presented.

## The deal with inline-block

In terms of layout, it's treated as an inline element. We can drop it in the middle of a paragraph, without totally borking the layout. But *internally*, it acts much more like a block element.

So it is an inline element that inside behaves like a block element.

```html
<style>
  strong {
    display: inline-block;
    color: white;
    background-color: red;
    width: 100px;
    margin-top: 32px;
    text-align: center;
  }

  strong:hover {
    transform: scale(1.2);
  }
</style>

<p>
  <strong>Warning:</strong> Alpaca may bite.
</p>
```

In this example, `<strong>` is set to be inline-block. It is now secretly a block-level element and has access to the *full universe* of CSS. Usually, properties like `width` and `margin-top` have no effect on an inline element, but they *do* work on inline-block elements.

We've effectively turned our `strong` element into a block element, as far as *its own* CSS declarations are concerned. But from the *paragraph's* perspective, it's an inline element. It lays it out as an inline element, in the inline direction beside the text.

### **Inline-block doesn't line-wrap**

**Here's the big downside with inline-block:** It disables line-wrapping.

This might not seem like a big tradeoff, but consider what happens when we try to use this on longer-length links:

![Screenshot 2024-06-28 at 15.31.51.png](Screenshot_2024-06-28_at_15.31.51.png)

Because the link can't line-wrap, it forces the *entire link* onto the next line, adding an awkward gap.

You may be tempted to pick really-short link text, to avoid this problem, but that would be a mistake; descriptive link text is [important for accessibility](https://www.a11yproject.com/posts/2019-02-15-creating-valid-and-accessible-links/). Therefore, we should only use effects like this when the links aren't part of a paragraph (eg. navigation links).

# Width Algorithms

Block-level elements will expand to fill the available space. It's easy to assume that this means that they have a default `width` of `100%`, but that wouldn't quite be right.

Block elements have a default `width` value of `auto`, not `100%`. `width: auto` works very similar to `margin: auto`; it's a hungry value that will grow as much as it's able to, but no more.

When we use percentage-based widths, those percentages are **based on the parent element's content space**.

If the `body` tag makes 400px of space available, any child with 100% width will become 400px wide, regardless of any other circumstances. This calculation happens *first*, before the margin is applied.

## Keyword values

Broadly speaking, there are two kinds of values:

1. Measurements (100%, 200px, 5rem)
2. Keywords (auto, fit-content)

Measurement-based values are either completely explicit (eg. 200px), or relative to the parent's available space (eg. 50%)

Keywords, on the other hand, let us specify different sorts of behaviours

### auto

`auto` will let our element greedily consume the available space while respecting any constraints.

### **min-content**

When we set `width: min-content`, we're specifying that we want our element to become as narrow as it can, *based on the child contents*. This is a totally different perspective: we aren't sizing based on the space made available by the parent, we're sizing based on the element's children!

This value is known as an *intrinsic value*, while measurements and the `auto` keyword are *extrinsic*. The distinction is based on whether we're focusing on the element itself, or the space made available by the element's parent.

![Screenshot 2024-08-05 at 16.17.02.png](Screenshot_2024-08-05_at_16.17.02.png)

### **max-content**

This value is similar in principle, but it takes an opposite strategy: it *never* adds any line-breaks. The element's width will be the smallest value that contains the content, without breaking it up:

[Screen Recording 2024-08-05 at 16.18.20.mov](Screen_Recording_2024-08-05_at_16.18.20.mov)

### **fit-content**

Like `min-content` and `max-content`, the width is based on the size of the children. If that width can fit within the parent container, it behaves just like `max-content`, not adding any line-breaks.

If the content is too wide to fit in the parent, however, it adds line-breaks as-needed to ensure it never exceeds the available space. It behaves just like `width: auto`.

![Screenshot 2024-08-05 at 16.43.12.png](Screenshot_2024-08-05_at_16.43.12.png)

## Min and max widths

We can add constraints to an element's size using `min-width` and `max-width`.

![Screenshot 2024-08-05 at 16.47.40.png](Screenshot_2024-08-05_at_16.47.40.png)

The particularly exciting thing about `min-width` and `max-width` is that they let us *mix units*. We can specify constraints in pixels, but set a percentage `width`.