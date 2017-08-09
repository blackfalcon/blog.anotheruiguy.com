# So, you want to play with List Maps

Sass 3.3 is out and you should start using Maps.

Using variables in Sass has been a core feature for years now. We have all used them to endless exhaustion and we have all seen things like this:

```
  // establish a core color
  $bravo_gray: #333;

  // assign core color to semantic variable
  $input-disabled-color:          $bravo_gray;

  // Use semantic variable as assigned to additional semantics
  $input-disabled-background:     lighten($input-disabled-color, 75%);
  $input-disabled-border:         lighten($input-disabled-color, 50%);
  $input-disabled-text:           lighten($input-disabled-color, 50%);
```

This works and it works really well. What we accomplished was a manual name spacing by convention. To use this, we would do the following:

```
  input[disabled] {
    background-color: $input-disabled-background;
    border-color: $input-disabled-border;
    color: $input-disabled-text;
  }
```

The expected CSS output would be:

```
  input[disabled] {
    background-color: #f2f2f2;
    border-color: #b3b3b3;
    color: #b3b3b3; }
```

We can do better. In Sass 3.3 we were given a new awesome thing to play with. Maps. Maps within lists to be exact. Sass has had lists for a long time now, but these lists were very flat. Maps give us more dimension to store and retrieve values from a list.

For those who use libsass, while libsass is lacking this as a core feature, there is an add-on library by Lu Nelson [sass-list-maps](https://github.com/lunelson/sass-list-maps) that you can use.

For the most part, it all works very much the same. There are some key differences in the syntax, there are no colons `:` between the key:value pairs and there are no trailing commas `,` after the last item in the array.

The following code examples make use of the Lu Nelson libsass add-on.

## Using List-Maps

The first really cool thing with Maps is that you can store key:value pairs (a hash) into a list for later retrieval. Using the primary variable `$input` as the namespace, we can then nest the extensions in the following way:

```
  $input: (
    disabled-background lighten($input-disabled-color, 75%),
    disabled-border lighten($input-disabled-color, 50%),
    disabled-text lighten($input-disabled-color, 50%)
  );
```

Now that we have all these values stored into an array with keys and values, we can start writing Sass rules that take advantage of this.

In the following example I used the `map-get` Sass function to dig into the variable of `$input` and find it's keys. The function `map-get` takes two arguments like so, `map-get($list, key)`

So, instead of using the old standard of:

```
  background-color: $input-disabled-background;
```

I can now use this:

```
  background-color: map-get($input, disabled-background);
```

All together, I can update the previous `input[disabled]` selector like so.

```
  input[disabled] {
    background-color: map-get($input, disabled-background);
    border-color: map-get($input, disabled-border);
    color: map-get($input, disabled-text);
  }
```

And this gives is the following CSS output:

```
  input[disabled] {
    background-color: #f2f2f2;
    border-color: #b3b3b3;
    color: #b3b3b3; }
```

## Nested objects in an array

Looking at the code, there is another pattern emerging. The concept of `disabled` is repeated. With Maps there is a way to do nest these concepts and retrieve the data as well.

The way that Maps work, the only restriction is that each key needs to have a value. Much like JSON, a value may be another set of key:value pairs. An example tree could be the following:

```
  $variable: (
    key (
      key value,
      key value,
      key (
        key value,
        key value
      )
    )
  );
```

Looking at that tree structure and then looking at my previous structure, instead of listing all the keys using a manual naming convention, lets use `disabled` as a name-space, like so:

```
  $input: (
    disabled (
      background lighten($input-disabled-color, 75%),
      border lighten($input-disabled-color, 50%),
      text lighten($input-disabled-color, 50%)
    )
  );
```

To make use of this, Maps allow us to retrieve values from a chain of keys using the `map-get-z()` function. In the following example you will notice how I am retrieving a series of chained keys delimitated by a comma `,`:

```
  background-color: map-get-z($input, disabled, background);
```

The whole selector would look like this:

```
  input[disabled] {
    background-color: map-get-z($input, disabled, background);
    border-color: map-get-z($input, disabled, border);
    color: map-get-z($input, disabled, text);
  }
```

Using this method we will get the following familiar output CSS:

```
  input[disabled] {
    background-color: #f2f2f2;
    border-color: #b3b3b3;
    color: #b3b3b3; }
```



## List-Maps are smart!

There are series of very useful functions for Maps, but one I will illustrate here is the `map-has-key()` function. In short, this will return a boolean value if a list has the key you are looking for. The `map-has-key()` function takes two arguments, the `$list` and the `key`. Using this with the `@if` directive, we can do the following:

```
  @if map-has-key($input, disabled) {
    input[disabled] {
      background-color: map-get-z($input, disabled, background);
      border-color: map-get-z($input, disabled, border);
      color: map-get-z($input, disabled, text);
    }
  }
```

Building out frameworks, this is very helpful. We can conditionally respond to available keys within the project versus throwing the all too familiar

```
  error: unbound variable $input-disabled-background
```

## Conclusion

In short, List-Maps are a pretty welcomed feature when creating larger and larger Sass projects. This feature will give us greater control over how we generate families of variables within a project and how we can apply them more sensibly.
