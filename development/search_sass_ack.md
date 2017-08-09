# Searching Haml and Sass with ack

## Because all good things need to be duplicated ...
originally posted @[plataformatec.com](http://effectif.com/articles/searching-haml-and-sass-with-ack)

If, like me, you've recently discovered the wonderful speed of the [ack](http://betterthangrep.com/) search tool, you may be frustrated to find that it ignores some of your source code. Files that you know should be appearing in your search results just don't show up, as ack doesn't consider .haml or .scss files to be worth searching. Well, we can fix that.

Ack has a list of "types" of text that it will search for your phrase, such as "HTML" or "C source code". Each type corresponds to one or more filename extensions. For example, .htm, .html, .shtml and .xhtml files are considered to contain HTML, and .c, .h and .xs are recognised as C source.

To see the full list of types, enter:

```
$ ack --help types
```

If you want to tell ack to search your `.haml`, `.sass` and `.scss` files as well, you can configure it with the `~/.ackrc` file:

```
$ cat >> ~/.ackrc
--type-add
html=.haml
--type-add
css=.sass,.scss
```

For more info see the ack man page (which you can read by typing `ack --man` if `man ack` can't find it).
