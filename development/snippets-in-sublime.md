# Code Snippets In Sublime Text

Here is a quick rundown without all the typical blah blah blah ...

### Create new snippet
Create a new snippet, go to **Tools > New Snippet**

This step creates an empty file for you to enter your new code snippet into. It should look like the following:

  <snippet>
    <content><![CDATA[
  Hello, ${1:this} is a ${2:snippet}.
  ]]></content>
    <!-- Optional: Set a tabTrigger to define how to trigger the snippet -->
    <!-- <tabTrigger>hello</tabTrigger> -->
    <!-- Optional: Set a scope to limit where the snippet will trigger -->
    <!-- <scope>source.python</scope> -->
  </snippet>

### Customize your new snippet
The code for your new snippet will go between

  <content><![CDATA[
    Hello, ${1:this} is a ${2:snippet}.
  ]]></content>

Let's make a new snippet that adds some CSS to a stylesheet, the `@font-face` rules for example. To make this a snippet we could do this:

  <snippet>
    <content><![CDATA[
  @font-face {
      font-family:'SourceCodePro';
      src:url("/fonts/sourcecodepro-regular-webfont.eot");
      src:url("/fonts/sourcecodepro-regular-webfont.eot?#iefix") format("embedded-opentype"),
        url("/fonts/sourcecodepro-regular-webfont.woff") format("woff"),
        url("/fonts/sourcecodepro-regular-webfont.ttf") format("truetype"),
        url("/fonts/sourcecodepro-regular-webfont.svg#SourceCodePro") format("svg");
      font-weight:normal;
      font-style:normal
  }
    ]]></content>
    <tabTrigger>font-face</tabTrigger>
    <scope>source.css</scope>
  </snippet>

Notice the indention of the CSS rules, I did that for a reason. White space will be represented in the output of the snippet. Test it out for yourself.

Outside of the code that we need in the snippet there are two additional lines of code.

`<tabTrigger> ... </tabTrigger>` is pretty self explanatory. What you put in here is the string you will enter at any line in your project code and then hit the `tab` key.

`<scope> ... </scope>` this is optional. By putting in a source type, this will restrict the use of this snippet to that file type. In this example we are setting the scope to `.css`. If you wanted to increate the scope, comma delineation is your answer.

  <scope>source.css, source.html, source.htm</scope>

### Save the file
You would think that this would be straight forward, but it's not. First you want to make sure that you are saving it in the proper directory

  ~/Library/Application Support/Sublime Text 2/Packages/User

Then, I would suggest making sub-directories here, something like `snippets` and then organizing a sub-directory in there as well. Something like `snippets/css` for example.

#### Add the file extension
When you hit the save button, notice that the file window simply says `untitled`. Be sure to add the `.sublime-snippet` file extension.

### Use the new snippet
You are now ready to use your new snippet. So that this example works, be sure to open/create a `.css` file. Remember, we scoped this snippet to that file type.

Line one type in `font-face` + `tab` and your snippet should appear.

### Make it dynamic
I am sure you are looking at this and saying "ahhhh, I don't always want to use SourceCodePro?" I am sure you don't, so ... using the ${ ... } syntax we can create a tabbing order. But knowing this example, there are places where we don't want to have to repeatedly enter code. Notice how how I am repeating `${1:font-family}` and `${2:file-name}`. As the step is repeated, Sublime will select multiple instances of the code for you to enter new code.

Another thing to notice in these steps is the placement of the default `${step:default}`. When you call your new snippet into your code, the default will be selected and as you type you will over-write.

  <snippet>
    <content><![CDATA[
  @font-face {
      font-family:'${1:font-family}';
      src:url("/fonts/${2:file-name}.eot");
      src:url("/fonts/${2:file-name}.eot?#iefix") format("embedded-opentype"),
        url("/fonts/${2:file-name}.woff") format("woff"),
        url("/fonts/${2:file-name}.ttf") format("truetype"),
        url("/fonts/${2:file-name}.svg#${1:font-family}") format("svg");
      font-weight:normal;
      font-style:normal
  }
    ]]></content>
    <tabTrigger>font-face</tabTrigger>
    <scope>source.css</scope>
  </snippet>

### Conclusion
That's it. It's pretty simple. Creating snippets is a great way for you to kill it on productivity!

### Backup your snippets
Put this in your RSYNC script

  rsync -avz --progress --delete ~/Library/Application\ Support/Sublime\ Text\ 2/Packages/User/snippets/* ~/Dropbox/st-snippets

Article also published at [coderwall.com](https://coderwall.com/p/jvavpq?i=1&p=1&q=author%3Ablackfalcon&t%5B%5D=blackfalcon)
