# Clean out your Sass junk-drawer

__*CSS has had a long and sordid past. A developer never sets out with the goal of making a complete and total mess of things. Their intention is not to build something that is practically illegible, impractical to maintain and is limited in scale. But somehow, this is where many inevitably end up. Luckily, all is not lost. With some simple strategies, organizational methods and out-of-the box tools, we can really help get that junk-drawer inline.*__

For many of us getting started with Sass, at one time or another have created a *junk-drawer* of files. For most, this was a rookie mistake, but for others, this is a continuing issue with our architecture and file management techniques. Sass doesn't come with any real rules for file management so developers are pretty much left to their own devices.

Large CSS files and increased complexity
---
CSS started out with very simple intentions, but as [tableless web design][1.1] began to really take a foothold, our stylesheets quickly began to grow in size. We tried to break our stylesheets into smaller documents, but these strategies proved to have serious performance issues. Linking to multiple stylesheets meant [multiple server round-trips][1.2] and [CSS' @import feature][1.3] had such a negative impact on [web page performance][1.4] it's practice was quickly abandoned.

Looking for new solutions, developers adopted the use of [CSS pre-processors][1.5], but sadly didn't change old habits. Still clinging to large documents they placed [mixins][1.6] and [variables][1.7] at the head of the doc and simply hashed out a bunch of CSS rules in the body.

Looking for better management techniques, many began to break these large stylesheets into smaller documents based on common principles like *variables* and *mixins*. *Typography*, *forms* and *design* soon followed. Sure this reduced file size and increased readability, but without a real strategy this process was easily doomed. As files grew in number, sub-directories quickly gave way to *junk-drawers* of haphazardly daisy-chained files.

Controller/action based styles
---
Inspired by [Model-View-Controller(MVC) frameworks][2.1], developers began to look at these file structure solutions to help solve their issues. Organizing [styles based on controllers][2.2] was a popular approach. While there is some merit to this in regards to template/layout styles, this practice inevitably lends itself to creating styles that are too specific to the view and not easily reused throughout the rest of the application.

Abuse of Sass' [nesting rule][2.3] was the most egregious transgression. A feature that made it seem so right, so awesome and so natural to [build CSS that mimicked our markup][2.4] was so wrong. Succumbing to the pressures of specificity to dominate the cascade, developers found themselves in a [CSS selector nightmare][2.5]. Style rules were difficult to reuse and extremely fragile to edit.

Between the views, duplicated code began to reveal itself. Attempts to abstract the code, following another MVC pattern, typically resulted in the creation of a [/partials][2.6] directory. Basically a simple repository for custom built mixins, variables and other reusable code. In essence, a *junk-drawer*.

In another attempt to abstract away from the view developers tried to organize their files based on actions. If the visual elements are part of an action, making directories based on these actions makes sense, right? Sadly, this quickly falls apart as not all UI elements can be easily categorized this way. Random files again populate the directory, universal widgets, plug-ins, and custom mixins begin to collect. Once again we find ourselves suffering from the *junk-drawer* effect.

Learning from our mistakes
---
Life is about journeys. It was during my journey of *doing it wrong* that I began to see clearly. Ironically, I had the right solution all along, but didn't realize it. While part of a team developing an enterprise CMS, our process was to decompose a site's UI to it's lowest common elements. From those elements we could then build modules and then finally assemble the view templates. Each step building on the previous. Although my stylesheet management techniques weren't perfect, my concept of UI abstraction was solid.

Working with a new team, sans a CMS, I went into the project with the same conceptual understanding, but the outcome was drastically different. The code became increasingly harder to reuse and making simple edits resulted in the reengineering of HTML as well as CSS. Post launch, I sat down and analyzed the code I wrote. I came to the realization that we were engineering our UI (CSS and HTML) from __entirely the wrong perspective__. We were approaching our development from the __full page perspective__. Engineering all our visual elements from the __outside-in__ and scoped to a specific view.

I started thinking back to the processes I pioneered with the CMS. Patterns established in the framework dictated we start from the elemental perspective; type, colors, forms, basic UI chrome (borders, shadows, icons, etc) all coded first. Once those element styles were completed, it was a matter of applying the *skin* to the CMS modules. The modules then in-turn were used to assemble the view. It worked quickly and seamlessly. Building the UIs from the __inside-out__ was clearly the solution.

Elements, modules and layouts
---
Applying these principals to new projects was the next challenge. First we need to be better at decomposing the designs. The *inside-out* approach is key to this process, the goal of the file structure, and necessary for a scaleable architecture. Simply put, *code the element, create the module and assemble the layout*.

Here I propose the following file structure that embodies this point of view. In the root there are individual Sass partials to address the elemental parts, directories for more complex concepts and last is a manifest file to aggregate all the awesome.

```
  |- sass/
  |--- buttons/
  |--- color/
  |--- forms/
  |--- layouts/
  |--- modules/
  |--- typography/
  |--- ui_patterns/
  |--- _buttons.scss
  |--- _config.scss
  |--- _forms.scss
  |--- _global_design.scss
  |--- _reset.scss
  |--- _typography.scss
  |--- style.scss

```
[see illustration][illus.1]

Sass partials and the manifest
---
[Partials][2.6] are a powerful weapon in the Sass arsenal. Simply put, any file that has an *underscore* before the name, `_partialName.scss`, will __not__ be processed into a `.css` file by itself. It is required to be imported into a file that __will__ be processed into CSS.

__Manifest files__

At this level of the file structure example, the only file that is processed into CSS is the [style.scss][5.1] manifest.  It's here where all your custom add-ons, configs, elements, modules, views, mixins, extends, etc., are all [imported][5.2] and processed into a production stylesheet. It is important that this file be kept devoid of any CSS rules.

This is not to say that `style.scss` is the only Sass manifest file. Manifests can import other manifests, [as seen in this example][5.3] from the Toadstool style guide framework. A pattern that helps to keep the `style.css` manifest easy to read while keeping sub-directory files nicely organized.

Follow *[The Inception Rule][5.4]: don’t go more than four levels deep.* That is if you want to keep your sanity. Going beyond this rule will increase unnecessary complexity in your app's UI.

__Manual manifest files or glob-imports__

*Manual* manifests are just that, manual management of imported Sass files from sub-directories. This is a good practice to follow when you need specific control over the inheritance of files. Example, rule A needs to come before rule B in the output cascade.

*Glob-imports* on the other hand is a way for you to simply point to a directory in your manifest, `@import "directory/*";` and Sass will import all the files in alphabetical order. This is great for a directory of mixins or functions that simply need to be loaded in memory for Sass to process the CSS. If you want to use the *glob* function but require a specific order, a naming convention like `_01_mixin.scss` could work as well.

While *glob-import* is a feature available to Rails projects, this is not a native feature of Sass. If you are not developing a Rails project, [Chris Eppstein][Chris Eppstein] wrote a [RubyGem][RubyGem] that can be used with either Sass or Compass.

Configurable options
---
An advanced concept of using a Sass structure like this is using a `_config.scss` file to manage the [smart defaults][variable defaults] for your UI. Using this technique will help to keep all your UI configuration options easily accessible and manageable, especially when you are using extended Sass libraries like [Zurb's Foundation][Zurb Foundation] or [Toadstool w/Stipe][Toadstool config].

It is important that there are no CSS rules in the `_config.scss` file. Typically you will include it at the head of your primary *Sass manifest* file, such as `style.scss`. Depending on how your architecture progresses, there may be times when you need to import your `_config.scss` file again in another module. As long as you keep any CSS rules out of this document, there is nothing wrong with this practice.

Element partials
---
*Elemental partials* is where we get to work. Here we write Sass rules that will create your UI foundational layer. `_buttons.scss`, `_forms.scss`, `_global_design.scss`, `_reset.scss` and `_typography.scss` all contain Sass rules that will process into CSS. While they will __import__ other *partials*, *mixins* and *silent placeholder* rules, it is important to remember that these files are engineered __only to output CSS__.

Taking *buttons* as an example; between gradients, `:hover` and `:active` states, one could go a little mad over the complexities in styling. It is important to keep your Sass logic out of these files and focus purely on the rules that will produce CSS for your selector.

Using a [Compass Extension][Compass Extensions] to quickly engineer a button is a great example. In our `_buttons.scss` partial we would only have code like so:

```
  button, a.button {
    @include button($button-color);
  }
```

Keeping functional Sass separate from presentational Sass is important in order to maintain readability, search-ability and scaleability of your code. Patterns like placing mixins in the same file as presentational Sass leads to overly complex files to scan and opportunities for accidental pollution of your processed CSS.

Custom mixins, placeholder selectors and custom function organization
---
Custom code for a project is the one area where I see the most issues with file management. Polarizing concepts, like keeping things global or local, paralyze many developers. They want to keep the code as accessible as possible, but inevitably end up creating functional Sass that is specific to a type of UI or module. So instead of ignoring this, I recommend embracing this.

Using our button example again, let's say that you need to roll your own from scratch. In the file structure there is a corresponding `buttons/` directory where you will keep your `_mixin.scss`, `_extend.scss` and custom function files. This solution will keep your presentational Sass clean and readable, while placing your functional Sass in a directory that is modular and is easy to find.

Modules and UI patterns
---
Now that we have established the architecture for our UI foundation, it is time to start assembling some modules. In essence, modular Sass is an assembly of foundational elements with only enough additional presentational Sass to hold it together. The use of elemental styles to build a module is strongly encouraged; while defining new elements in the scope of building a module is strongly discouraged.

Module Sass is exclusive to a particular interaction of the application. Modules will come in all shapes and sizes, while larger modules may also consist of smaller modules or UI patterns.

UI patterns are subject to personal interpretation. In practice when engineering modules, from one to the next, UI patterns will emerge. It is practical to try and encapsulate these smaller patterns for reuse, but I don't lose sleep over them.

__Module__

A module is a singular functional deliverable and will be unchanged in form, functionality and content. Examples are site header, main navigation and footer.

One could argue that a module is engineered as a *'plug-n-play'* element. Taking the main navigation for example, there would be no good reason why you would want to repurpose this UI element and functionality in another module? That would be very confusing to your users.

Your functional code, your Sass and your application should represent this as a singular modular object.

__UI Patterns__

UI Patterns, on the other hand, are representations of assembled UI elements. Dialog boxes are a great example. These patterns consist of design elements such as, typography, arrangement, color, border and spacing. These patterns can be repurposed again and again throughout the application/site. But the content and functionality of this pattern are subject to redefinition based on use.

__The file structure__

Module and UI patterns are directories unto themselves. An exploded module directory may look like the following:

```
  |- sass/
  |--- modules/
  |----- registration/
  |------- _extends.scss
  |------- _functions.scss
  |------- _mixin.scss
  |------- _module_registration.scss
  |------- _module_personalInfo.scss
  |----- purchase/
  |------- _extends.scss
  |------- _functions.scss
  |------- _mixin.scss
  |------- _module_summary.scss
  |------- _module_purchase.scss
```


The idea here is that while engineering modules you may need to create complex functional Sass that is exclusive to a module. While I strongly encourage making UI logic as abstract as possible and available to the whole app, this process discourages the practice of creating *junk-drawers*.

Keeping these logic files close to the actual use-case helps maintain clean organization of your code. As shown in the example above, a primary module may consist of smaller modules. As a naming convention, I will name the primary module Sass file after the name of the directory prefixed with `module_`. For example: `_module_registration.scss`. Any sub-modules in this directory will simply be named by the purpose in which it serves, `_module_personalInfo.scss` for example.

Sub-modules of a UI in many cases will contain similar characteristics. This close relationship between a module's functional Sass and it's presentational Sass also serves code reuse and management purposes. Take for example the use of a *silent placeholder*. In the `extends.scss` file you may engineer a reusable UI module that utilizes several variables. In the corresponding presentational Sass module file you can `@extend` this UI while resetting some of the default variables.

All modules should be name-spaced by the semantic name of the module itself. `.registration {}` or `.purchase {}` for example. If the sub-module is exclusive to the primary module then it would extend the name like so, `.purchase_summary {}`.

Keep in mind that at the level we are working at it is scoped to the module itself. Keeping the selectors shallow will encourage reuse throughout the application without causing additional engineering. If you find yourself engineering complex UIs within the module, this may be an opportunity to abstract into a mixin or silent placeholder selector.

Assemble the layout
---
Prior to this process, we started developing at the layout level. Abstracting concepts like modules and elements were extremely difficult to do and typically overlooked. By completing our UI development journey with the layout, we get to take advantage of all of our hard work done so far. Our layout Sass files should contain no more information then is needed to assemble a series of modules and elements. Imagine a sketch with gray boxes in the view, this is the document that creates that structure. Elements are __never defined__ and modules are __never engineered__ here.

I never advocate for sub-directories per layout as things should never get that complex. At this level, assembling the layout should be taking 100% advantage of the elements and modules already engineered. If you find yourself involved in more complex development at this phase, I would argue that you need to review your work before engaging in more complex levels of code.

Typically I will name a layout Sass file after the semantic meaning of the view. In an MVC app, a great convention is either use the name of the `controller` or the `layout` template file and append the word `_container` or `_layout`. An example would be `sessions_container.scss` or `sessions_layout.scss`. To keep things simple, I would then scope all the presentational Sass in a document by the same name, `.sessions_container {}` for example. Using this class name can be achieved by dynamically adding the class to the `<body>` tag when the view renders, creating multiple layout templates with a static class applied or whatever works for you.

Our #1 goal with layout Sass files is to place control of the template UI into the hands of the CSS itself rather then depending on presentational classes in our markup. This becomes even more important when considering __mobile/content first__ and __responsive web design__ strategies.

It's a folder structure, it's a way of thinking
---
Without a doubt, a folder structure should follow some sort of methodology. In my experience, I can say that it is this lack of methodology that leaves the developer with feelings of dread and unhappiness. Is this process and file management technique good for you? I have no idea. Does it work for me? Yes. Regardless of project, technology, framework, language, etc. it has helped me build scalable UIs that stand the test of time.



[1.1]: http://en.wikipedia.org/wiki/Tableless_web_design "wikipedia entry on tableless design"
[1.2]: https://developers.google.com/speed/docs/best-practices/rtt "Google's best practices in web development"
[1.3]: http://www.cssnewbie.com/css-import-rule/ "why the @import rule is bad"
[1.4]: http://www.stevesouders.com/blog/2009/04/09/dont-use-import/ "Steve Sounders addresses page performance"
[1.5]: http://www.awwwards.com/css-preprocessors-focused-decisions.html "css preprocessors focused decisions"
[1.6]: http://thesassway.com/intermediate/leveraging-sass-mixins-for-cleaner-code "The Sass Way: Leveraging Sass Mixins"
[1.7]: http://thecodingdesigner.com/tutorials/variables-css-sass "Variables, CSS and Sass"

[2.1]: http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller "Wikipedia MVC"
[2.2]: http://codefastdieyoung.com/2011/03/css-js-organization-best-practice/ "Opinion: Organize CSS based on Controllers"
[2.3]: http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html#nested_rules "Sass docs: nested rules feature"
[2.4]: http://37signals.com/svn/posts/3003-css-taking-control-of-the-cascade "37signals: dominate the cascade"
[2.5]: http://www.thesassway.com/beginner/the-inception-rule#css_selector_nightmare "Inception rule: The CSS Selector Nightmare"
[2.6]: http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html#partials "Sass docs: partials "

[illus.1]: https://speakerdeck.com/anotheruiguy/clean-out-your-sass-junk-drawer?slide=23 "Slide Deck Illustration"

[5.1]: https://github.com/Toadstool-stipe/toadstool/blob/master/sass/style.scss "Toadstool style manifest file"
[5.2]: http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html#import "Sass docs: import feature"
[5.3]: https://github.com/Toadstool-stipe/toadstool/blob/master/sass/style.scss#L43-L45 "Toadstool import example"
[5.4]: http://thesassway.com/beginner/the-inception-rule "The Sass Way: The Inception Rule"

[Chris Eppstein]: https://github.com/chriseppstein "Chris Eppstein: Compass creator, Sass contributor"
[RubyGem]: https://github.com/chriseppstein/sass-globbing "Gem: sass-globbing"
[variable defaults]: http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html#variable_defaults_ "Sass docs: variable defaults feature"
[Zurb Foundation]: https://github.com/zurb/foundation/blob/master/scss/foundation/_settings.scss "Zurb Foundation default settings"
[Toadstool config]: https://github.com/Toadstool-stipe/toadstool/blob/master/sass/_config.scss "Toadstool config file"
[Compass Extensions]: http://compass-style.org/help/tutorials/extensions/ "Compass: Extensions feature"
