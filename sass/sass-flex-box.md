# Living life with Flex-Box and Sass

So I was presented with this interesting problem by a co-worker to come up with a CSS solution for a very common problem. Basically we are looking at a common layout for a list of items that will be placed into a grid with two columns. Simple, right?

![Imgur](http://i.imgur.com/Gt6cKEe.png)

Wrong. How do we do this? There are so many ways we can solve this problem that the solutions are a problem in itself. In this article I will walk through the process that I went through and how I came to a solution that I feel is pretty flexible and will carry us into the future of better layout solutions.

## Rows and columns

A VERY common solution is to pretend this is a table and think of this as a series of rows and columns. I personally think that this is a very poor solution, why would we want to recreate tables for the sake of creating a table layout? For the sake of argument, I will walk through how this works.

First, the HTML. In this example I defined a parent container `.block`, then a container for each row `.row` and last, each container of content would go into the `.column` containers. This is a LOT of DOM!

```html
<div class="block">
  <div class="row">
    <div class="column">1</div>
    <div class="column">2</div>
  </div>
  <div class="row">
    <div class="column">3</div>
    <div class="column">4</div>
  </div>
  <div class="row">
    <div class="column">5</div>
    <div class="column">6</div>
  </div>
  <div class="row">
    <div class="column">7</div>
    <div class="column">8</div>
  </div>
</div>
```

Next, the supporting CSS. I don't need to write any CSS for `.row` as this is simply a block container, but for each column we need to specify some styles. The simplest way to address this IMHO is to set a less then 1/2 width for each column and then set the left and right margins using pseudo classes.

```css
.column {
  width: 49%;
  margin-bottom: 1em;
}

.column:first-child {
  margin-right: 1%;
}

.column:last-child {
  margin-left: 1%;
}
```

Like I said, this works but it is pretty lame. It's inflexible and to make it more flexible we will need to add more classes to the DOM to change margins and base widths. Not to mention, this row/column solution is really tough to deal with when rendering a list of content. You have to write more code to break this into blocks of `2` and skip through the rows. Fail.

## Block and elements

In this next evolution I am getting closer to a solution where the issue of displaying a list of content is much easer, so I would go with the following HTML. Here we see that there are no rows or columns, but simply there is the block and then each element.

```html
 <div class="block">
   <div class="element">1</div>
   <div class="element">2</div>
   <div class="element">3</div>
   <div class="element">4</div>
   <div class="element">5</div>
   <div class="element">6</div>
   <div class="element">7</div>
   <div class="element">8</div>
 </div>
```

For the CSS it looks pretty much the same, so there is very little win there. Although we addressed the complexities of programatic layout, we are still stuck with additional manual management of widths and margins.

```css
.element {
  width: 49%;
  margin-bottom: 1em;
}
.element:nth-of-type(odd) {
  margin-right: 1%;
}
.element:nth-of-type(even) {
  margin-left: 1%;
}
```

It was also at this time that saw that the `margin-bottom: 1em` is a real issue. I want a gutter between the stacked elements, but I don't want this gutter to be under the whole block of elements. To address this, I will use the `nth` pseudo class to find the last two children in the list of elements and removing the bottom margin. Again pretty cool, but very manual for maintenance.

```css
.element:nth-last-child(-n+2) {
  margin-bottom: 0;
}
```

In case you were still thinking that the row/column solution is still better, bear in mind that we can't use this `nth` solution as there are only two columns in a row, so all the margins would be removed.

Clearly moving on here, we are getting closer to a better solution.

If you are curious to see progress so far, I have included a JSBin of the solutions up to this point.

<a class="jsbin-embed" href="http://jsbin.com/xorisobeja/1/embed?css,output">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

## Flex box makes things better

At this point, we have a solution for a single layout. Looking forward, I knew this was NOT the end of the story. There would be more similar layouts and I for one do not want to get into the business of BEM'ing out a OOCSS solution where all the combinations are addressed.

> __FLAME ALERT:__ Before anyone gets ready to flame-on in the comments, I am well aware of the current state of browser support and don't care. Progressive enhancement leads the way and my code will be ready for the browser that can use it.

For the HTML, we are sticking with the clean block/element structure.

```html
<div class="block">
  <div class="element">1</div>
  <div class="element">2</div>
  <div class="element">3</div>
  <div class="element">4</div>
  <div class="element">5</div>
  <div class="element">6</div>
  <div class="element">7</div>
  <div class="element">8</div>
</div>
```

Onto flex-box. From the onset it looks pretty similar to the previous solution. With flex-box we get a TON of really nice configurable operations that we just don't get in any of the previous solutions. Don't believe me? Check out the [Visual Guide to CSS3 Flexbox Properties](https://scotch.io/tutorials/a-visual-guide-to-css3-flexbox-properties).

I should mention, we are NOT floating ANYTHING. Flex-box by itself addresses the 'floating' of things. In my example I need the elements to wrap into columns, so using `flex-wrap: wrap;` will address this layout attribute.

I am not setting widths on anything, I am setting a `flex-basis`. This is NOT a standard width or height attribute.

> This [flex-basis] property takes the same values as the width and height properties, and specifies the initial main size of the flex item, before free space is distributed according to the flex factors.

Next I will not mess around with margins for left/right either, this is being addressed with `justify-content: space-between;`. Nice.


```css
.block {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.element {
  flex-basis: 49%;
  margin-bottom: 1em;
}
.element:nth-last-child(-n+2) {
  margin-bottom: 0;
}
```

In the end, we are still stuck with this `.element:nth-last-child(-n+2)` solution and I am not really happy about that. And while this is pretty cool and allows for more flexibility in the layout, we are still stuck with poor maintenance for alternate solutions that would again have me running down the BEM/OOCSS path again. There is a better way.

## Flex-box && Sass === WAY better

We have had some fun playing with standard CSS and HTML, it's time to turn this up to 11 and have some real fun. The goal I now set for myself is that I want some functionality where I can create any series of selectors and by using some Sass magic I can easily apply a combination of flex-box scenarios and transform my layout.

#### flex-basis

The first problem to solve is that I don't want to have to set a value for the `flex-basis`. I should be able to come up with a number that works based on the number of columns I want in my layout.

To do this I am going to create a function that will accept two arguments, one `$arg` for the number of columns that I intend to use and then a boolean value `$gutter` whether or not if I want gutters between my columns.

To map this out, I am taking `1` and dividing this by `$arg` and using the `percentage` Sass function to come up with a value. If I want gutters, then I am simply reducing this value by `1%`. There may be a better way to do this, but this works for now.

```scss
@function gutter-value($arg, $gutter) {
  @if $gutter {
    $value: percentage(1 / $arg) - 1%;
  } @else {
    $value: percentage(1 / $arg);
  }
  @return $value
}
```

### Container and element

Now how flex-box works you need two parts and for this I am going to create two mixins, `flex-grid-container` and `flex-grid-item`.

For the container we need a few things. `display: flex` is a given, so I am simply going to hard-code that. For `flex-wrap` I am setting an argument for that and setting the default to `null`. By doing so, the attribute won't be printed out into the CSS if you don't want it. It saves is a `@if{}` step.

What's cool is that in Sass a boolean also equals if a variable has a value, it's not specific to `true` or `false`. So when `$wrap` has a value of `wrap` to satisfy the attribute, this also sends a `true` value to `@if $wrap` and then `justify-content: space-between;` will be process into the CSS. This time we are saving the need for an additional argument in the mixin.

```scss
@mixin flex-grid-container($wrap: null) {
  display: flex;
  flex-wrap: $wrap;
  @if $wrap {
    justify-content: space-between;
  }
}
```

Feeling good here. Moving onto the next mixin for the elements. It's here that we start to make use of the function we created earlier. This mixin takes three arguments, `$arg`, `$gutter` and `$margin`.

`$arg` will be where we enter the number of columns we are looking for. `$gutter` will be that boolean blue that we pass into the function. You can say `true` or `gutter` or `yes-gutters-please`. It will work as long as you don't use `false` or `null`.

Last is `$margin`, this is an actual value. With the default set to `null` none of the CSS inside that `@if` statement will process into CSS. When this is used, not only will set a bottom margin, but using the `$arg` value we can also set the   `:nth-last-child` pseudo selector.

```scss
@mixin flex-grid-item($arg, $gutter: null, $margin: null) {
  flex-basis: gutter-value($arg, $gutter);
  @if $margin != null {
    margin-bottom: $margin;
    &:nth-last-child(-n+#{$arg}) {
      margin-bottom: 0;
    }
  }
}
```

Pulling all this together we have a pretty nifty API to work with.

### How to use the API

To make this easy to follow along and play with, I created a [SassMeister Gist](http://sassmeister.com/gist/5e8eac11e4d0d6f6b78a) to play with.

<p class="sassmeister" data-gist-id="5e8eac11e4d0d6f6b78a" data-height="480" data-theme="tomorrow"><a href="http://sassmeister.com/gist/5e8eac11e4d0d6f6b78a">Play with this gist on SassMeister.</a></p><script src="http://cdn.sassmeister.com/js/embed.js" async></script>

And to make this even easier to play with for the number of columns, I re-wrote the HTML part using some haml magic. To change the number of elements in the `.block` container, simply update the `(1..6)` value. Say you want 12 elements? Ok, `(1..12)`. You want 24 elements, `(1..24)`. I think you get the point.

The `#{i}` part simply outputs the number of the column into the view.

```haml
.block
  -(1..6).each do |i|
    .element #{i}
```

To get this working, we only need the following code. For flex-box to work, you need to set some values for the outer container, `.block` in our case. Then you set values for the elements within the container, `.element`, in this example.

`flex-grid-container` takes one argument. If you want it to wrap, simply put in `wrap` or `true` and it will update `.block` to have the necessary rules. And yes, there will be times when you will invoke this mixin without setting a value and the `null` operator will work.

Next we have `flex-grid-item` and this takes three arguments. `$arg` for the number of columns, the `$gutter` argument which is looking for a boolean value and last there is `$margin` that is looking for a CSS value.


```scss
.block {
  @include flex-grid-container(wrap);
}

.element {
    @include flex-grid-item(2, true, 1em);
}
```

The previous example should look something like this:

![Imgur](http://i.imgur.com/2t64k92.png)

Now lets say that you only want 6 columns across with a gutter? Basically update the `$arg` to equal the number of elements and set the `$gutter` argument to true. Since there is no wrapping, there is no need to set the `$margin` value.

It looks weird, but we need to set the `$wrap` value so that we get the `justify-content: space-between;` properties. Without it, all you columns will be flush left. If it makes you feel better, just set `true` ;)

```scss
.block {
  @include flex-grid-container(true);
}

.element {
    @include flex-grid-item(6, true);
}
```

![Imgur](http://i.imgur.com/w1j8d8W.png)


### Hacking on an idea

This was an interesting accident I discovered, but if you set the `$arg` in `flex-grid-item` to be twice the value of the number of elements, 6 in this example, then your grid column is `50%` of the available space. Nothing special really, when you look at the output CSS it's simple math at this point. But, still cool.

So, for the haml keep the following:

```haml
.block
  -(1..6).each do |i|
    .element #{i}
```

And for the Sass, use this:

```scss
.block {
  @include flex-grid-container();
}

.element {
    @include flex-grid-item(12);
}
```

![Imgur](http://i.imgur.com/w23w2qq.png)


## In closing

This is not meant to be a framework, I won't be building a new site and getting a Twitter handle to promote the next amazing 'Sass Flexie Grid'. But I do hope that you can come away from this learning something new. I did.

Have fun!
