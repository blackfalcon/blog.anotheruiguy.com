# How to better make use of variables in your Sass

There are three ways to use common variables in Sass. Here are some examples.

#### Single value

```
$var: value
```

#### List value

All the following options are legal. The only law in Sass lists is that you are consistent with your delimiters.

```
$var: value value value value;
$var: value, value, (value value), value;
$var: "value", "value", "value", "value";
$var: (key value) (key value) (key value);
$var: key value, key value, key value;
```

#### List maps

```
$var: (
  key: value,
  key: value,
  key: value
);
```

## How we can better use these

Of course the standard `$var: value` method is pretty straight forward, but as more and more variables get mixed into code, there is added complexity to keep concepts separate. The most common method to keep things identifiable is to use a naming convention:

```
$form-field__border-radius:       $standard_round_corner;
$form-field__text:                $primary_text;
$form-field__height:              35;
$form-field__padding:             6;
$form-label__weight:              bold;
$form-label__line-height:         20;

$inline-alert__line-height:       30;
$inline-alert__left-padding:      12;
$inline-alert__weight:            bold;
$inline-alert__top-margin:        12;
$inline-alert__border-width:      1;
```

As you can see, this is quickly getting out of hand. But what you can see is that there are 'families' of concepts that we can use. So, why not make use of variables and lists to help up do this better? (that was rhetorical BTW)


### Common variable lists

So taking the previous, long names, variables from above we can then create variable lists that reduce the naming conventions and make the scenarios more readable.

```
$form-field:
  border-radius $standard_round_corner,
  text $primary_text,
  height 35px,
  padding 6px,
  font-weight bold,
  line-height 20 !default;

$inline-alert:
  line-height 0,
  left-padding 12px,
  weight bold,
  top-margin 12px,
  border-width 1 !default;
```

To gain access to the values within the variable list, it's most common to create a function that will then in turn go into the list and parse out the value. But this introduces a level of abstraction that can be difficult to follow and creates a single purpose tool. We like tools that have multiple purposes.

To get around this, we can make a different function that allows you to pick the list you want to parse through and then get the value you want. This then removes that abstraction and increases opportunity for discoverability and makes the code easier to read IMHO.

So ... our solution

```
@function match-get($list, $key) {
  @each $item in $list {
    $index: index($item, $key);
    @if $index {
      $return: if($index == 1, 2, $index);
      @return nth($item, $return);
    }
  }
  @warn "'#{$key}' match was not found in provided list, please double check the argument provided.";
  @return null;
}
```

Using this we can do the following:

```
input[type=text] {
  border-radius: match-get($form-field, border-radius);
  line-height: match-get($form-field, line-height);
  font-family: match-get($form-field, text);
  font-weight: match-get($form-field, font-weight);
  height: match-get($form-field, height);
  padding: match-get($form-field, padding);
}
```

And we get ...


```
input[type=text] {
  border-radius: 3px;
  line-height: 20;
  font-family: Verdana;
  font-weight: bold;
  height: 35px;
  padding: 6px;
}
```


As you can see here, there is a common naming convention used in respect with the attributes needed for the selector. This makes it WAY easier to maintain consistency within the app using variables without having to repeat really long naming conventions.

### Lists are cool, but list-maps are better

While variable lists are pretty cool when using the custom `match-get` function, it's limited in scope. The `match-get` function is a HUGE help when you have a lot of legacy code where the values are already created as lists. But this method is limited, so using the `map-get` functions that are native to Sass is WAY BETTER!

Let's take that previous example and make that a list-map versus a simple list.

```
$form-field: (
  border-radius: $standard_round_corner,
  text: $primary_text,
  height: 35px,
  padding: 6px,
  font-weight: bold,
  line-height: 20
) !default;

$inline-alert: (
  line-height: 0,
  left-padding: 12px,
  weight: bold,
  top-margin: 12px,
  border-width: 1
) !default;
```

Then using the native `map-get` function in Sass we can actually use the same syntax as the `match-get` function, but it's way more powerful. Read more about [list map functions in the Sass api](http://sass-lang.com/documentation/Sass/Script/Functions.html#map-functions).

```
input[type=text] {
  border-radius: map-get($form-field, border-radius);
  line-height: map-get($form-field, line-height);
  font-family: map-get($form-field, text);
  font-weight: map-get($form-field, font-weight);
  height: map-get($form-field, height);
  padding: map-get($form-field, padding);
}
```

and the output is the same:

```
input[type=text] {
  border-radius: 3px;
  line-height: 20;
  font-family: Verdana;
  font-weight: bold;
  height: 35px;
  padding: 6px;
}
```

## In conclusion

So what we are seeing here are some basic, but core, examples about how to best use variables in your Sass. Some simple rules to follow are;

1. When you have a simple variable concept that is intended to be global, using a single value variable is the best way to go
1. When you have a list of values per a variable, going with a standard list will be sufficient
1. When you have a complex list of value pairs, going with a complex list and using tools like `match-get()` or `nth()` is a great way to access the values
1. When creating complex groupings of variables for a component or larger supporting architecture, list-maps are preferred
