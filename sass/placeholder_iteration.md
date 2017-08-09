# Iterate list to produce placeholder classes

Ok, so here is the issue I need to solve. In the style guide I am writing I had a ton of repeated code. In order to display all these elemental parts of a design, each element needs to be an OOCSS class.

It is my desire to only use these classes in the style guide CSS, but not production. This lead me down the path of creating silent placeholders to be extended in the production CSS, but be made into OOCSS classes for the style guide.

Refactoring for life
---
To start out I long-hand wrote all the code. Each silent placeholder followed up by the OOCSS class that you would extend it. There has to be a better way.

We need variables
---
The fix for repeated code of course is to iterate through a list of variables. First we need to declare some variables with values. This would be addressed in an imported `_config.scss` file.

```scss
$alpha_color: red;
$bravo_color: blue;
$charlie_color: green;
```

Lists, we need lists
---
Next we need to create the list of objects to iterate through. __WARNING!__ I should point out, variable interpolation don't work inside control directives. I would LOVE to take a single list create selector names and variables, this is clearly not supported at this time.

To make this work we will have to duplicated 'some' code, but this is STILL way better then what I was doing before.

```scss
$color_names: alpha_color bravo_color charlie_color;
$color_vars: $alpha_color $bravo_color $charlie_color;
```

Iterate on @each
---
One of the awesome functions of Sass is its ability to iterate through a list. Using the variable list $color_names we can do the following.

```scss
@each $name in $color_names {
  do something awesome
}
```

If you are not familiar with `@each` loops, what is going to happen here is Sass will iterate through the list one at a time and apply the `something awesome` we ask it to do. In our example we will crate a new variable `$name` that will get updated on the fly with each value in the `$color_names` list. Given this example, it will iterate three times.

The awesome in the middle
---
For the acton in the middle, we need to pass the value of the `$name` variable into something that it can act on.  In the following example we are applying this to the class name via `.#{$name}`.

```scss
.#{$name} {
  background: $color_vars;
}
```

This is close, but not the answer. We are grabbing the `$name` variable from the iterated `@each` function, but using the `$color_vars` by itself we are going to get all the color values back in a single string. As you can see in this CSS output example, this is not what we are looking for.

```scss
.alpha_color { background: red blue green; }
.bravo_color { background: red blue green; }
.charlie_color { background: red blue green; }
```

The value of iteration
---
What we are looking for is a way for Sass to iterate through one list, keeping track of the iteration so it can extract the corresponding value from a second list. Basically, iteration 1 gets value 1 from both lists, iteration 2 gets value 2 from both lists, and so on. Sass' index function will return *"the position of the given value within the given list"* [[ref]](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html#index-instance_method). Now we can use the returned value of index to get the desired value in the second list.

To do this we need to create a new variable, `$i` for example. Sass will set the value of `$i` to be the index value of each `$name` in `$color_names` as shown in the following example.

```scss
$i: index($color_names, $name);
```

What to do with a value
---

Now that we have the index number in memory, we can use this to iterate through the two lists. Using Sass' `nth` function we can specify a list, then using the value of `$i` we can designate a specific value from the list to the `background` CSS property.

```scss
.#{$name} {
  background: nth($color_vars, $i);
}
```

Make it happen, make it awesome
---

Pulling this all together, your code should look like the following. This example code will iterate three times between `alpha_color` `bravo_color` and `charlie_color` creating the class names and applying the appropriate color variable which will get processed into the appropriate color value.

```scss
$alpha_color: red;
$bravo_color: blue;
$charlie_color: green;

$color_names: alpha_color bravo_color charlie_color;
$color_var: $alpha_color $bravo_color $charlie_color;

@each $name in $color_names {
  $i: index($color_names, $name);
  .#{$name} {
    background: nth($color_vars, $i);
  }
}
```

Silent placeholder selectors and extends
---

By making some simple updates we can now extend this same code to create a series of silent placeholders and extend those to OOCSS classes as needed. By simply replacing the `.` with a `%` this same code will generate a series of silent placeholder classes.

```scss
@each $name in $color_names {
  $i: index($color_names, $name);
  %#{$name} {
    background: nth($color_vars, $i);
  }
}
```

But to make use of these placeholders we need to extend them to actual CSS classes with a slight twist on the previous code. In the following example we will see how we can once again iterate through the list `$color_names` to get it's value and it's index and apply the value to the variable in `.#{$name}`. In the next line we apply the extend. This time we use the value in `$i` to iterate through `$color_names` for the extended placeholder.

```scss
@each $name in $color_names {
  $i: index($color_names, $name);
  .#{$name} {
    @extend %#{nth($color_names, $i)};
  }
}
```

Play with code
---
Reading code is cool, playing code is better. See the [SassMeister Gist](http://sassmeister.com/gist/4615645) and happy coding.

UPDATE
---
Doing some more reading on this issue, I came across an example that allowed for some refactoring.

In these examples, I am able to replace the `index` function by determining the `length` of the array. Then I can use a `@for` loop versus the `@each` loop.

This update removed one line of code and I have to now call the `nth` function on each CSS rule.

Is this better? Dunno. Is this different? Yes.

```scss
@for $i from 1 through length($color_names) {
  %#{nth($color_names, $i)} {
    color: nth($color_vars, $i);
  }
}

@for $i from 1 through length($color_names) {
  .#{nth($color_names, $i)} {
    @extend %#{nth($color_names, $i)};
  }
}
```
