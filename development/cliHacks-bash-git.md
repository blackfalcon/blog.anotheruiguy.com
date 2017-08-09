# CLI hacks with BASH (and some GiT too)

As professionals we look to always optimize our workload. Through better processes and shortcuts we can eliminate unnecessarily repetitive tasks. One of the tools I use quite often is the Command Line Interface (CLI) and Git, and this is an area that we can automate quite extensively through some really simple to learn BASH hacks.

We all have tasks that we do endlessly, and there are tasks that we do independently and don't realize that there is a definitive pattern that we can embrace, put them to use in a simpler function and alter the results on a per use basis. It is through years of doing things the hard way that I have found some simple tricks that makes my life much easier to maintain.

### What is BASH anyway?

Let's get this out of the way quickly first. In short, it is a Unix shell and command language most common on Linux and macOS and recently has made it's way to Windows as well. It is the common interface for running simple commands or other applications such as GiT, Curl, Ack, etc ...

If you are completely unaware of BASH or the Terminal.app, please read this insightful documentation on [Life in the Terminal](http://www.anotheruiguy.com/ux-design-dev/_book/learning-computers/life-in-the-terminal.html).

### What are we doing?

All of the code examples I will be using here will be written in a BASH resource on your computer located (on the mac) at `~/.bash_profile`.

Depending on your setup, or you could also do the same with your `.bashrc` file as well. If you are new to this, I'd suggest using the `~/.bash_profile` resource. If this file does not exist on your computer, you can simply crate it and this will work. In your command line, run the following command:

```
$ touch ~/.bash_profile
```

To learn more about the difference between the `~/.bash_profile` and `.bashrc` resources and why they exits, see this great article by _Josh Staiger_ [.bash_profile vs .bashrc](http://www.joshstaiger.org/archives/2005/07/bash_profile_vs.html).

### The Alias

Once we have our `~/.bash_profile` up and running, using a BASH alias is the simplest way of managing repetitive commands. I also find this a really great way of managing new commands that I have a hard time remembering. Instead of keeping a list of things in other places as a reference, make an alias.

For example, let's say that you have a project were you need to run Gulp and then a Gulp watcher within a project directory. If you are like me, then you will have your projects within a directory of a directory, etc ... This can be tedious to have to `$ cd` into that directory then run Gulp and then the watcher. So you could put this into a single BASH command like the following:

```
cd ~/codes/your-org/your-project && gulp && gulp watch"
```

Ok, it's not that bad, but it's a mouthful. Let's make this a simple alias. The syntax is simple to remember, use the `alias` keyword, then give it a name and last, in quotes, enter the string of the command you want to run with that alias.

```
alias myprojectgulp="cd ~/codes/your-org/your-project && gulp && gulp watch"
```

Now, from the command line you can simply run `$ myprojectgulp` and all those things happen. Cool. Many unnecessary characters were saved and you have way more time for reddit now.

I should also note that if you are in a shell and you make an update to the `~/.bash_profile` file, in order to get the CLI to see this update, you need to resource the prompt, meaning that you need to refresh the shell to understand the new commands you added.

Either you can close the exiting shell and open a new one, or get the current shell to be resourced with new commands. I prefer to resource the current shell I am in personally and this is pretty simple. Simply run the command of `$ source ~/.bash_profile`, or we can make this an alias too!

```
alias resource="source ~/.bash_profile"
```

Great! Now we can simply run `$ resource` and our shell will be updated with new commands.

### An Alias with an Argument (using functions)

Using a basic Alias is pretty awesome and you can get really far with them. Simply Google it and you will see a lot of people who have posted really great ideas. But what if you need something a little more complex. Say for example you have a command and you want to pass in an argument?

The CLI command for creating a new document is `touch`. But lets make an alias for that, say `tu`? For example with `alias tu="touch"` already set you can run the following command.

```
$ tu foo.html
```

Simple and gets the job done. But now we want to not only create a new file, but open it in an editor right away too. Let's also assume that you are using Sublime Text and have the `subl` [alias](http://www.anotheruiguy.com/ux-design-dev/_book/learning-computers/link-terminal-to-sublime.html) set up to. All that being said, what we need here is a function that will take a single argument and use it in multiple places.

We can create a function using the `function NAME {}` syntax, and then start writing our instructions. BASH has a pretty simple argument architecture that uses a convention of `$1`, `$2`, etc. and it will maintain that order of input. Other types of arguments that you should be aware of are;

* `$@` which will store all command line arguments
* `$#` which will store count of command line arguments
* `$0` which will store name of script itself

For our function we will name it `tuop`, short for _touch and open_, and use the a single argument of `$1` that will represent the name of the file we want to create and open.

```
function tuop {
  tu "$1" && subl "$1"
}
```

And that's it. Resource your CLI and run something like `$ tuop footer.html`. This will create the file locally and then open it in Sublime Text.

#### Turn this up to 11

If you use the CLI, and like me, didn't know there was a `find` command, this will blow your mind. Using the `find` function in BASH you can easily look up any file name in a directory. Pretty cool, but let's say that you want to not only find the file, but also open it in Sublime Text? We can do that.

In the following example, I created a function called `fopen`, which is short for _find and open_. In the function I am using the syntax of the `find` function to find the file I am looking for and piping that to a function which will open the found file(s) in Sublime Text. Also note the `command` substitution method that allows the output of a command to replace the command itself.

```
function fopen {
  command find . -name "$1" -print0 | xargs -0 subl;
}
```

With this function, you can run a command like `$ fopen index.html`, this will find `index.html` and open it in your Sublime Text editor. The function also takes wildcard entries as well, so things like `$ fopen index.*` will open all file types using that name.

At this point you have really upped your BASH skills and have entered a new world of awesome.

### Dynamically pass argument to function

This one is pretty fun and uses a few different things to string together a function that makes it really easy to swap between branches in Git. Keep in mind it also helps that you have some kind of naming convention with your branches for best possible results.

I have always made it a proper habit to name my feature branches after the Story number that I am working on. For example, `FN-2187__escape-with-rebel-scum`. So imagine that I have a bunch of branches to switch between and each time I either have to copy/paste or type in the whole name. Sure there is tab-complete, but this is even easier.

Lets create a function called `swap` that will track one argument and execute three commands.

First, I will run a `git branch` command, but pipe in a `grep` search using the value of the variable from when I ran the command. This will return a result, I can pipe that result into `bcopy` which will copy that result to the computer's clipboard.

Using the `&&` syntax I will append another command to this function once that first part is completed.

Next I want to assign the contents of the clipboard to the variable of `tmp`.

Using the `&&` syntax again, I am writing out the command of `git checkout` and using the value of `tmp` to write our the branch name.
```
function swap() {
  git branch | grep "$1" | pbcopy && tmp=$(pbpaste) && git checkout $tmp
}
```

With all that said and done, we have a really easy command now we can use, `$ swap 2187` and the computer does the rest of the work for you.

### An Alias Function with Multiple Arguments and a mix of GiT

This one gets pretty fun. Imagine, if you will, that you use that little SF startup, Github. Imagine if you will that you are working with a repo, you branch the code, make an update, have pushed that branch and want to make a pull request. For many of you, I would assume that this is a daily process. Well, we can use a BASH function to help us create a function that will assist in this process.

Let's call this new function `pr`. In this function we need access to two values; 1) your Git username and, 2) the current branch name.

In your Git `.config` file at the root of your computer's user profile there should be something like this:

```
[user]
  name = Robert McFarkson
  username = robmcfars
  email = bob.farkson@gmail.com
```

We can reach into that `config` file and look for the `user.username` value and assign that to the `USERNAME` variable.

The easiest way to get this function to work is for you to be on the branch you want to make a PR for. So by using the Git command of `rev-parse --abbrev-ref HEAD` we can easily append that value to the `CURRNETBRANCH` variable.

Great, now that we have that, we can use the `open` command in the CLI and this will open that file type in the default application. In the case of `.html`, this typically means your default browser. Then we can construct a URL using a mixture of passed in arguments and variable values.

```
function pr(){
  USERNAME=$(git config user.username)
  CURRENTBRANCH=$(git rev-parse --abbrev-ref HEAD)

  open https://github.com/"$1"/"$2"/compare/"$3"..."$USERNAME":"$CURRENTBRANCH"?expand=1;
}
```

* For `$1` you need to enter the org that contains the project repo you are performing the Pull Request against
* For `$2` you need to enter the repo that you created the branch from
* For `$3` name the branch you want to compare against, most of the time this will be `master`

So for example, say you forked Swipe from the bird. From `master` you made a branch and made some edits. After you have pushed that branch to Github, you can do the following:

```
$ pr the bird Swipe master
```

And like magic, this will open your default browser to this URL and you will be at the Pull Request screen ready to get things done!

#### Turn this up to 11

More fun with `.gitconfig`. In each repo there is a local `.git/config` file that you can edit. So to personalize this document, open it up and add the following for example:

```
[upstream]
  org = thebird
  repo = Swipe
```

Then we can update the `open` command in the following way:

```
open https://github.com/"$UPSTREAMORG"/"$UPSTREAMREPO"/compare/"$1"..."$USERNAME":"$CURRENTBRANCH"?expand=1;
```

No, when we are in that repo, we can simplify the PR command to the following and get the same results.

```
pr master
```

Now ... how cool is that?

#### Let's paint flames on that baby

There is a really cool tool in the CLI called [the cat command](http://www.linfo.org/cat.html). It has three related functions with regard to text files: displaying them, combining copies of them and creating new ones.

As described above you can easily edit your config file with additional information that you can access via BASH for developing really awesome functions. But using cat, you can easily read and write to files from the CLI as well.

To invoke cat, simply use the `cat` command. Using the `>>` operator you are telling cat to not destroy the [file] you are referencing. `<< EOF` is the opening [heredoc](https://en.wikipedia.org/wiki/Here_document) statement.

In between, add in the content that you want to add to the document.

Last, close out the string with a closing `EOF` statement.

Take the following example:

```
$ cat >> .git/config << EOF
> [things]
>   foo = bar
>   jazz = notFromUtah
> EOF
```

That was kind of a drive-by tutorial, but there is some good learning there. Good luck.


### Bash Reference Manual

So here you are ... learned some new things. But wait, there is more. A whole lot more. If you are interested to have your mind blown, feel free to check out the fill [Bash Reference Manual](https://tiswww.case.edu/php/chet/bash/bashref.html#Shell-Functions) ... cause, damn!
