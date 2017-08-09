# Why libsass is important to the community

Life is ever changing, right? Everything around us is always in a constant state of change. You learn early either to be flexible with these winds or you resist them.

## Evangelizing beyond the Rails developer

Back in 2008 I heard about this thing called Rails and a fellow developer told me about a new CSS language called 'Sass'. At the time I was working for a company that heavily leveraged Microsoft technologies and Ruby was not something that we could plug into our environment. Sass was a ncoew toy I could read about, that's as far as it went.

Fast forward just a year, I moved on and began working with a Rails team. Their first question, "You know Sass?" I said, "Nope, but it's a great time to learn!"

Since then I have been a huge advocate for the use of Sass and still continue to promote awareness. Within the Rails community, this is pretty easy to evangelize, moving away from Rails, even today it is difficult to convince people. I went on various speaking engagements, and spoke about better ways to manage our growing CSS libraries of code.

Part of this raising awareness, I continue to speak with groups of designers and developers. The message is; we as UI devs need to understand UI patterns and build larger, more complex, modules from smaller base UI parts. Accepting concepts like state, assembly and layout. All great ideas.

When SMACSS came out, I at least had validation in the market on these ideas. But what really separated me from @snookca was the fact that I did not see a way to make this work without Sass, and that was a problem. Out of all the early speaking events I did, only 1 of 12 was to a Rails group, and that was a 15 minute lightening talk. Guy says 'Sass', Rails kids say 'DUH!'.

For the other 11 engagements, when speaking guy says, "Just install Ruby?", crowed says, "Ummmm, not hip to that."

## Developer's complexity

For those who really wanted to play with Sass, but had no idea how to get Ruby working or install 'Ruby Gems', tools like CodeKit, Scout, and Compass.app came to their rescue. Livereload includes a Sass processor and Web Workbench is a great attempt at trying to solve the issues of working with C#/.NET.

While these are great tools, they all have a serious flaw, they work outside the development environment. In the past few years development environments continue to grow in complexity. We are easily past the days of FTP sites and working from a remote server. Development environments, for even the most common of web sites, are built and run locally and use tools like Git, Github and production deployment strategies. Using tools that are outside these environments are awkward, to say the least.

Version control and deployment are the key issues here. As your project's resources grow in complexity, version control becomes even more paramount. When using tools like Git, processed files are a conflict nightmare. When using Sass, you only want to commit your actual resources, not their static output.

When using Git as a key part of the deployment strategy, in order to create these static output files, you typically use a build process on the production environment to create the static output files.

Abstracting your dependencies away from the application means that somewhere along the deployment process, you need to create an asset that is committed to the repository for the sake of deployment and then is destroyed. This is called a 'dirty commit' and is not a best practice.

When it comes to Sass, the Rails kids have a solid solution for this. This was the only environment where I found it to be a seamless operation to have your Sass files as resources in the app and then on the production server they would be processed into their static output versions. All other development environments, not so much.

Then there are those in the Sass community who say, "C'mon, is it so bad to pin on a Ruby dependency?" And the lead developer says, "Just to process CSS? No way!"

## It's just development dependency anyway?

Some will advocate that Ruby is only a development dependency. That only those who use Sass have personal workstations with the tools necessary to edit and process Sass. On some teams, this may be true, but I have never worked on a team that feels that way.

Why should only a select few have access and/or the ability to edit a project's resource files? This is an unnecessary silo. That's like saying that the UI dev on a team has no business ever touching the app's core PHP, Ruby, .NET or Python code. While there are those who are better at some tasks then others, there is never a good reason to make one code base exclusive over another. If it's in the project, it's fair game to every developer on that team.

Not to mention, making Sass and Ruby a 'developer' dependency, that puts us back at the 'dirty commit' state. Even worse, we are not only dependent on a language that is not completely integrated into the application, but we are now dependent on specific people to do a job?

### That's a horse of a different color

I admit, I was in this camp. I lived in a bubble where other stack developers were very open to the idea of a Ruby dependency and I was working pretty exclusively with Rails devs as well. But, this was a small bubble I quickly found out.

I interviewed for a consulting position where I was asked to discuss the core differences between Sass and LESS, an easy argument. Then the lead dev asked, "How do we get this to work in our Python environment?" with that I replied, "Install Ruby?"

I didn't get the contract.

## The world according to JavaScript

As pre-processors gained in appreciation, thanks in part to Sass, teams went for the next best thing like LESS and Stylus. These pre-processors are built with JavaScript, thus there is no awkward language/platform dependency. JavaScript is EVERYWHERE!

I sat in many conversions as the subject matter expert on Sass and lost argument after argument, as exemplified above, in regards to using Sass over LESS in non-Ruby friendly development environments. The theme of these conversation is, if there is not a passionate front-end dev on the team when it comes to CSS, then all the power features of Sass fall to the side and the issue of dependencies takes precedence.

Typically there is a front-end dev on the team that LOVES JavaScript. I MEAN LOVE!!! So, why NOT use LESS or Stylus? JavaScript is BAKED into the application, if the whole thing is not built using JavaScript.

Writing awesome mixins and making the most of your CSS using the power  given to us from Sass don't matter. Having a good handful of features available is more then enough to suffice, and the lack of adding another dependency is the winning argument.

### Is JavaScript the holy grail?

While I think that JavaScript is interesting and there are amazing things we can do with it, I don't think that it will do away with all other languages. Not by a long shot.

While the flexibility of JavaScript is amazing, taking time to talk with leading devs in other language disciplines like .NET and Python, would they prefer a native solution over another JavaScript one? I feel that the answer is yes.

### As long as we are on the subject ...

And without going too far on the evangelism trail, languages like LESS and Stylus have another core difference with Sass. Sass is 100% dedicated to the CSS spec. This is important. CSS has a certain way of working, there are real rules like inheritance and the cascade. The core Sass team look at each new feature with a very specific microscope and decide qualitatively if the new feature is inline with the CSS spec or not.

LESS and Stylus on the other hand don't have this level of regard in my opinion. There are things that each language does that over-ride the CSS spec and as a result I have heard from developers that this creates confusion and implementation issues.

Just putting that out there.

## Getting to the metal

@hcatlin took a huge gamble. He saw this issue and wanted to see if he could find a way to fix it. The Ruby community was not really playing well with others and JavaScript is NOT always the answer. Shedding off layers in computing, you quickly get to the bare metal of it all, C/C++. The core language that rules most of our worlds.

libsass was born. The C/C++ port of the original Sass language that we have all come to love and admire. It is more universal and itt is FAST! 4000 times faster to be exact.

Some of the key differences between Ruby Sass and libsass is that libsass is only the core library. There is no task runner, there are none of the add-ons that we have come accustomed to in the Ruby version. There is no direct integration of this library into any application environment. In fact, you can't even run libsass without some kind of wrapper. The first of which was the C wrapper called 'SassC', cute huh?

Wow, did this just get harder then Ruby Sass? No, actually this made it easier. Now in order to use Sass, someone just has to write a wrapper for their language and plug Sass directly into their development environment. No bolt-ons or issues with running Sass outside the app. You are a Python dev? Cool, run Sass as if it were built in Python. You are a .NET dev? Cool, run Sass as if it is a standard part of your solution.

In order for Sass to break out of it's Ruby and Rails encasement, we need to allow all the other kids to come into our sandbox and play with our toys, the way that they want to play with them. That's the key right there.

### Is libsass perfect?

No.

At the time of writing this article, there are issues with libsass, I am not going to deny that. It doesn't have all the amazing features of Ruby Sass 3.3 that @nex3 and @chriseppstein  just released. It does have bugs and a key feature like `@extend` [isn't working as we expect](http://anotheruiguy.roughdraft.io/8794442-where-extended-placeholder-selectors-go-wrong-in-libsass) it to either.

Will it get there? Yes. @hcatlin is committed to this project and will see that it gets there.

Do we just cast it aside and ignore it? No.

As a development community, libsass needs our help. It needs our support and it needs our appreciation.

Are there others interested in helping with these issues?

YES! For example, one of the key arguments is that libsass does not support things like ListMaps. @lunelson threw his hat into the ring [and provides a solution](https://github.com/lunelson/sass-list-maps). Others are getting involved too. The community is growing and it was announced at CampSass that the original whitespace Sass syntax is to be included as well. YAY!

## Why do we need libsass?

The way I see it, we have a two choices here. One, we say to hell with libsass and maintain use of the Ruby version. Will this gain respect in the community or increase user and mind share? No. Will others who are not interested in a Ruby dependency look at other solutions? Yes.

Another choice is; do a complete re-write of Sass to JavaScript. But who will do this? @nex3 has already stated that given infinite time and infinite resources this is something that he thinks would be a good direction. But, he has neither infinite time or infinite resources, so I don't see that happening soon.

Is there someone in the JavaScript community interested in picking up this task? Yes and this has happened. Are these projects anywhere close to the original Ruby spec? No way in hell. Regardless, what I fear from these projects is that someone will try to port it, but it will be their thing. It will do things the way they want to do things. It won't be Sass.

@hcatlin gave us the idea of Sass, @nex3 played a huge role in breathing life into that idea. @hcatlin, while diverse in his implementation (C/C++ over Ruby), he is not divergent in the core thinking that is Sass. How it works, how it is part of the CSS community, libsass is not something that simply tries to dominate a space like others may.

libsass is a new bridge for getting all development teams excited about using the language. When I tell devs how easy it is to get Sass running in their project, I relish in their excitement that they too can now play with these tools.

## Previous work is not wasted

I also want to make this point very clear, I in no way am advocating for libsass in a way that encourages developers to simply walk away from Ruby Sass and only embrace the new hotness. The work that @nex3 and @chriseppstein have done is irreplaceable. They have created something amazing and I don't want to take anything away from that.

What I am saying is that we live in a world where great ideas need to evolve. If this were not true, then we would all still be writing apps using [COBOL](http://en.wikipedia.org/wiki/COBOL). In my heart of hearts, I really hope that everyone who came together to create Sass in the first place can find a way to continue working together and help Sass evolve into it's next evolution.


## libsass isn't perfect, but it's better then the alternative

In the past 6 months I have had opportunities to work on some really cool projects and each project has a theme. Want Sass, no Ruby. Two of these projects are Node.js and one is .NET. These teams were very excited about bringing me on as they are interested in the way that I use the language to solve larger scale problems. And they were even more excited when I explained to them that we can use Sass, even though it's not perfect.

Truth of the matter is, I'd prefer to use libsass with a few less features and some bugs then a language that doesn't have these features AT ALL or the community to support it.

Getting these issues solved isn't all @hcatlin's issue. @hcatlin is giving this to us for free, we need to pull together as a community and help. @jed_foster and I recognized that there was a lack of understating with libsass and that's why it is now included in [SassMeister](http://sassmeister.com). I encourage you to try things out for yourself. If you find a bug, make an issue in the repo. If you know how to fix the bug, pitch in. After all, this is what open source is supposed to be about, right?

Myself and others also recognize that there isn't a whole lot of documentation out there for getting libsass into your project. On the JavaScript side I put together a workshop that includes the instillation of libsass in a [Node.js project](http://www.anotheruiguy.com/node-workshop/_book/sass.html). The response has been great as this helps clarify how to get libsass set up.

Adding to this, I am also working with other developers to create similar project for Python, .NET and PHP. In short, with a world lacking in resources for this, I am pitching in to help fill that gap. If you are interested, I ask that you do the same.

It takes a village.
