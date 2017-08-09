# Ooohhh Webkit and your default styles

There are these really awesome cases when Webkit is not only applying default styles to DOM elements that you probably don't want in your design, but then there are these really awesome cases when Webkit is applying these styles inline. DOH!

Webkit will default style input fields with rounded corners and a cute little drop shadow. The easy solution around this is to undo the default styling. And in attempts to be future proof and since others are following in the footsteps of Webkit, lets use some Compass magic to make this easier.

Lets use the power of Placeholder Selectors and a Compass Mixin to make this:

```sass
%remove-default-appearance
  +experimental(appearance, none !important)
```

Using a placeholder allows us to reuse without duplicating. Then in our `_forms.css.sass` we will have this:

```sass
input[type=email], input[type=password], input[type=text], input[type=number], input[type=date], textarea
  @extend %remove-default-appearance
```

Ok, this is pretty cool, but what about those nasty cases where Webkit will use inline styles? Oh the humanity of it all. Here we need to be a little more sledgehammer like and do the following:

```sass
input[style*=appearance]
  @extend %remove-default-appearance
```

Here we had to target all inputs that may have the style attribute of `appearance` and then we use the placeholder we created earlier.

__NOTE:__ I am not putting `input[style*=appearance]` in with the other input types because there will typically be other CSS rules that I may not want this selector to touch.

__FULL DISCLAIMER:__ In the first case of using the placeholder, we DO NOT need to use the `!important` CSS flag. With the initial use case, simply using the CSS rule of `appearance: none` will work.

But it is the case of over-riding an inline style that we do need the `!important` flag. Putting the flag on the placeholder reduces the need for using the mixin again, duplicating rules just for this single case.

Our combines Sass may look like the following:

```sass
%remove-default-appearance
  +experimental(appearance, none !important)

input[type=email], input[type=password], input[type=text], input[type=number], input[type=date], textarea
  @extend %remove-default-appearance
  padding: 0 0 0 em(10)
  margin: 0 0 em(10) 0
  line-height: em(20)

input[style*=appearance]
  %remove-default-appearance
```

Our expected CSS will look like the following:

```css
input[type=email], input[type=password], input[type=text], input[type=number], input[type=date], textarea, input[style*=appearance] {
  -webkit-appearance: none !important;
  -moz-appearance: none !important;
  -ms-appearance: none !important;
  -o-appearance: none !important;
  appearance: none !important; }

input[type=email], input[type=password], input[type=text], input[type=number], input[type=date], textarea {
  padding: 0 0 0 em(10);
  margin: 0 0 em(10) 0;
  line-height: em(20); }
```

Play with code (requires Compass and Ruby Sass)
---

```
// Compass v12

%remove-default-appearance
  +experimental(appearance, none !important)

input[type=email], input[type=password], input[type=text], input[type=number], input[type=date], textarea
  @extend %remove-default-appearance
  padding: 0 0 0 em(10)
  margin: 0 0 em(10) 0
  line-height: em(20)

input[style*=appearance]
  @extend %remove-default-appearance

```
