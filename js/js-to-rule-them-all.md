# JS dominance over the presentation layer

tl:dr - *the following is a long rant about the complexities of presentation layer development as it collides with the growing complexities of JavaScript front-end frameworks. This is a summary of my experiences, issues and proposed solutions to today's world of client-side application development.*

## Back in the old days (4 years ago)

It was a trend with the traditional model of app development that the app dev team would take a particular dominance over the presentation layer, as it was rightly so, their domain. The deign teams of the time resorted to non-code ways of communicating their intentions as to influence the design. The 'comp' and 'red-line' documents were the primary resource of communication. Practices that are not all that dead, people still do this stuff you know.

We all know by now that these processes don't really work. They are fraught with error and caused countless hours of frustration between designers and developers. Witness the rise of the 'designer - developer' or 'The Designer who Codes' articles, and the growing rate of job descriptions looking for designers who know HTML/CSS/JS.

A new age has been ushered in when the presentation layer developer was given a space in which not only to influence the design, but to directly contribute code to the application on a domain that they controlled. They are responsible for the HTML/CSS/JS, up to a point. This was the first definition of the 'front-end developer'. I say 'up to a point' as many of these positions are restricted to 'prototyper' or 'web designer'. They are rearely integrated into the actual application development level, and that is a problem, but one rant at a time here.

## The pillars of web app development

**HTML** of course, most app devs couldn't be bothered to care about the DOM. It was perfectly acceptable to just wrap everything in a DIV and litter with classes to your heart's content. I really liked working with haml after a while, but one thing that always troubled me was the pattern of use.

```
.selector
  { code - content }
  .selector
    { code - content }
```

What haml does is, when you just name a selector, it processes the haml to the following HTML:

```
<div class="selector">
  { code - content}
    <div class="selector">
      { code - content }
    </div>
</div>
```

As you can see here, the haml is really easy to see/read/follow, but the HTML output is shit. Just a series of semantic-less containers used only for the purpose of layout and design.

Before a11y was a thing, many 'front-end devs` were vocal about using the DOM as an actual document structure, not just scaffolding to hold up a design as was commonly seen.

**CSS**, ahh our old friend CSS. Devs of yesterday and today wish we could live in a world without CSS. It's nothing but a bothersome technology that just simply doesn't work. Or at least, work the way they want to. The main thing, it's cascading global scope. WOW APP DEVS HATE THAT! Nothing else is worse then that or works like that. They simply can't deal! When developing an application and you put logic code into a global scope, you are liable to be ostracized from the WORLD!

**JavaScript**. This is the preverbal "*Great tool, what should I do with it?*" story. JavaScript has ALWAYS been a solution looking for a problem. In the days of the HTML/CSS/JS dev, there were two pretty clear camps, those who wrote early Javascript for the purpose of more back-end application support and those who used jQuery. The jQuery camp was clearly on the side of the presentation layer developer, remember jQuery-UI? And there were many other JS libraries that started this new revolution. Mootools, prototype, scriptaculous, all were mostly presentation based.

For the majority of these libraries, in the beginning, their main purpose was to address JavaScript incompatibilities and browsers and don't forget all those jazzy animations!

## The winds of change

Then all things started to change. A new generation of developers began to emerge. A post-Rails world where many of these developers have been born and raised in a JavaScript world and they see this not only as a great tool, but the **only** tool. This drastically changed the definition of the 'front-end developer'.

See, the early 'front-end developer' emerged from the design side of things, this new evolution does not. In fact, most could care less about design and/or UX at all. They are simply the next generation of application developers, it just so happens that their technology of choice is, in the 'front-end', the client space, the browser. This is not a design concern, this is an application concern. But since they have dominance in this area, it 'feels' appropriate that they have total dominance over HTML and CSS too.

This transition period was interesting and confusing. I knew something was going on when one week I had two interviews for a 'front-end developer' position. The first interview was shocked that I didn't have a 'design portfolio' and the second interview was shocked that I didn't know Angular.js inside and out. As you can see there, these two interviews show a huge gap in expectations as to this role.

I, like many others, have fell into this donut hole of skills. We still hold true to caring about design and UX, but we are not suited well for full brand design or large aesthetic decisions, personally, I just don't think like that any more. We also hold true to a craft that is not 100% consumed by the latest JavaScript framework. We still see the value in common technology separations. Web standards as championed by “godfather of web standards” Jeffrey Zeldman.

Then the very father of the internet, Tim Berners-Lee, who wrote in [Principles of Design](http://www.w3.org/DesignIssues/Principles.html) about the very *Principle of Least Power*;

> Computer Science in the 1960s to 80s spent a lot of effort making languages which were as powerful as possible. Nowadays we have to appreciate the reasons for picking not the most powerful solution but the least powerful. The reason for this is that the less powerful the language, the more you can do with the data stored in that language. If you write it in a simple declarative from, anyone can write a program to analyze it in many ways. The Semantic Web is an attempt, largely, to map large quantities of existing data onto a common language so that the data can be analyzed in ways never dreamed of by its creators. If, for example, a web page with weather data has RDF describing that data, a user can retrieve it as a table, perhaps average it, plot it, deduce things from it in combination with other information. At the other end of the scale is the weather information portrayed by the cunning Java applet. While this might allow a very cool user interface, it cannot be analyzed at all. The search engine finding the page will have no idea of what the data is or what it is about. This the only way to find out what a Java applet means is to set it running in front of a person.

This next part is the interesting one. The new JavaScript community has fallen into what is referred to as [Atwood's Law:](http://blog.codinghorror.com/the-principle-of-least-power/) **any application that can be written in JavaScript, will eventually be written in JavaScript.**

Adding to the dominance of the JavaScript community are those who have fallen into the dogmatic following of frameworks. It's like splinter groups of splinter groups. Factions of utmost loyalty and when a thought leader at the latest [enter-name].js conf speaks up and says '*enter colloquial phrase*' the masses rise up and embrace with cheer and celebration. "Death to CSS!!" is the latest of these rally calls.

## Rise up JavaScript devs

I remember being at Cascadia Ruby some years ago, this was **the** development event of the year in the Seattle area. I happened to sit next to a man who had this idea, something like Cascadia Ruby, except for JavaScript. I said to myself, "*That's interesting, but a whole conf on JavaScript?*" See, at the time, to me JavaScript was a separate concern of interactivity, not the entire platform. I didn't have the vision or insight he did apparently. CascadiaJS is one of the largest conferences in the Pacific Northwest!

Conferences aside, and there are a LOT of JavaScript conferences, I have also interacted with a few teams over the past few years during the rise up of the JavaScript frameworks and there is a pattern I have witnessed. A senior developer exerting dominance over the presentation layer is interested only long enough to satisfy their need to control it. There is no interest to build something that will scale with the project, there is no interest to have a design layer what will grow with the product and embrace all the needs and expectations of the application as it grows at scale over the years to come. Scale and growth is only a concern of the application layer.

The main interest is to build something, typically supported by JavaScript, so that they can get started. It's shinny and fun. There is a LOT of inclusion in the architecture and opinion of what and how this will work. Where things typically fall apart is when the work gets more complex then architectural work. It is at the point where the UI framework needs to be used, issues need to be address and the question of scale comes into play. You have to write CSS. This is not their preferred playground and they tap out requesting to be put back on JavaScript application projects.

There is mainly the desire to install the design API and once this is no longer fun, they retreat from the complexities of the issues and go back to the safety of the JavaScript framework of choice, yearning for the consolation of peer developers who chant, "Why can't we just live in a world without design and CSS?!" and "Why can't we just use Bootstrap!?"


## The bane of front-end frameworks

For a long time I railed against front-end frameworks, especially Bootstrap. Why? Because it doesn't scale. FACT. Software is complex, we all know that. There is NOT a framework on the planet, even the mighty Rails or .NET that answers all the questions and solves all the answers. That is why we are called '*developers*' not '*assembly line workers*.' We can all agree on that.

Even the most dogmatic of JavaScript developer will identify cracks in their beloved solution of choice. But again, when it comes to HTML and CSS there is more effort put into dismissing these concerns then embracing.

So devs LOVE Bootstrap. Why? Easy, it's a design API. Follow this pattern, put the codes here and BAM! You have a UI! Remember the days when people used to say, "*It looks like a developer designed it.*" People don't say that any more, what they say now is, "*Yup, looks like Bootstrap.*"

I get it, but again, no support for scale. Look at the evolution of Bootstrap. The very foundation that it was built on does not work. Did you know that there are over 100 references to the `!important` flag in Bootstrap? And that there are over 7000 lines of CSS? This is a clear indicator that the process that in which it is designed to be used does not work. This is 7000 lines of code BEFORE YOU DO ANYTHING!

At the point in which you want to customize this for your site, very few developers ever customize the library itself. I am not saying that people don't, I am saying that it's not common practice. What I see more often then not is the adding of more code to address the customization of the deign.

This happens in many ways. If the developer really does hate CSS and doesn't want to be bothered, the solution is simple, make the update in the JavaScript. Just manipulate the DOM, that works great. Yes, it does work and it does solve a problem, but the new problems for consistent scaleability of the UI and the application greatly outweigh the value of this solution.

Another common practice I see is the process of 'shimming'. What is 'shimming'? Basically, this is when there is a clear deficit in the library that will not address the needs of the work to be done. So a developer will create a new file or simply add some more code in another file that is already 4583 lines long, back in the HTML view template they will append a new ID or some other class that breaks the model that the UI framework is supposed to support and they address the missing part. Remember Jurassic Park? Remember how there was missing DNA with the dinos? Remember how they used frog DNA to 'fill the gap'? Remember how that worked out?

Now shimming is a VERY common practice in app development. Shimming or 'monkey patching' as it was called in Rails, adds technical debt to the project. These are one-off solutions that are specific to a small part of the application and will require documentation and maintenance. Although this process requires more support, it is rarely given and it is left up to the tribal knowledge of the group to maintain this 'shim'. What happens when the group grows? Shimming does not scale. It now comes in the form of, "*OMG! Who did this and WHY?!*" Work stories triple in size because of the endless refactoring needed to not only undo the shim, but address the problem correctly and **THEN** do the work that is now being requested. This is a huge problem for companies, the growing scale of technical debt is expensive and hard to address over time.

## One code to rule them all

It is at this point when the true dominance of the modern JavaScript developer emerges. They see all these issues and they don't want to deal. There is a strong movement in the direction of consuming all things into JavaScript, react.js I am looking at you, and truth be told, I see their point.

The growing complexity of applications, especially with the increasing interest in component level functionality development, has really put a HUGE strain on all of this. CSS is even HARDER! The knee-jerk reaction appears to be coming back to '**WE NEED MORE BOOTSTRAP!**' as this is the familiar tool. The 'safe haven' if you will. That comfortable pair of shoes. The problem is that Bootstrap, and BEM style OOCSS for that matter, are NOT going to solve this problem. These solutions DO NOT SCALE, coupled with their fragile application that creates opportunity for gaps which in turn necessitate the shimming practice will still happen. Don't care if you are building all your software on a brand new platform with the best intentions in the world, if you are repeating the same old practices you did with the last version, you are doomed to repeat. Then typically what will happen is that a dev will say something like,

> "We set up better this time, other's didn't follow the new plan, it's all screwed up and there is noting I can do about it."

I have stated publicly and I will state it again, I can't get on board with the idea that we need to package all our presentation into the same layer as application of the sake that we can't deal with the cascade.

## A brighter future

There is a brighter future for all of this. The component architecture is in it's infant stages of life. No one really knows what the hell we are doing, but I stand firm that the pillars of development have gotten us this far and I stand strong that they will get us to the next level. Men like Tim Berners Lee were amazing individuals who saw the simplicity of technology and how when leveraging the simpler concerns together we can accomplish great things.

I feel strongly that there is a way to 're-think' the presentation layer where it can, with the help (not dominance) of JavaScript do the things that we need it to do.

Allow for the cascade to do what it's supposed to do. Think of the UI as smaller parts that when assembled correctly will do the things that you need them to do.

Yes, there will be times when there are parts of the UI that are specific to a component, and yes there are ways that we can package that UI with the JS as to deliver only that thing. We can all do that without having to burn it all down and rebuild in a new shinny way.

Look to history. Understand how all the parts work. Let the 'subject matter experts' have a say in the architecture of things. Stop sweeping issues under the rug and always looking for new and cleaver ways to address concerns. Address issues at the core. Yes it's hard work. Yes, it will not always be fun. Yes there will be challenges.

I guess I am just an old fart and when the hell did 6 figure incomes mean that we need to have fun all the damn time and work was going to be easy and we went to the office in our pajamas and were fed gourmet lunches all day anyway?
