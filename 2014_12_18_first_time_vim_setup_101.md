At [Turing](http://turing.io) we have quite a few folks who are taking the leap and getting into [Vim](https://twitter.com/j3/status/516796431286677504).  It can be intimidating to learn the basics of movement and modal editing especially for new programmers, but in my experience everyone has been impressively self-sufficient on that front.  There's a lot to learn, but documentation/tutorials aren't in short supply and in the end smart, deliberate practice will yield results (duh).  More often than not the questions weren't "how do I do x in vim?", they were "how do I make my vim not look terrible?".

This post is about the other side of Vim, your setup.  Beware rabbit holes.

This is the base setup I used when I got into Vim, and I've heard reports of success for others who've used it.  For brand new users it will seem like a lot, but it's not so much that it's impossible to wrap your head around and you'll have some seriously powerful functionality to play around with.

## Preface: Learn how to fish and such

As a general rule, always find where tools you plan on using are hosted (most of the plugins mentioned below can be found on GitHub), and read the READMEs.  Much will be revealed.  With Vim plugins, you'll find a `doc/` directory in the project's root.  This is where you'll find more detailed documentation about that particular project, and it should be your next stop if you have any questions.  Vim also supports reading these files in editor, [here](http://usevim.com/2012/12/21/vim-101-help-tags/) is a great (and short) article on helptags.

Reading is a huge part of being a developer - both code and documentation.  It's also a skill you can develop just like any other.  Experienced developers who you seek out for questions will massively appreciate if you've exhausted all of your publicly available resources.

## Part 1: Plugin Managers

Before you can install plugins you'll need a way of installing them.  That's a confusing way of saying we'll be using a plugin manager.  There are two main options: [Pathogen](https://github.com/tpope/vim-pathogen) and [Vundle](https://github.com/gmarik/Vundle.vim).  This article originally had instructions only for pathogen, but I've since switched to Vundle so we'll go over both.

### Pathogen

There are three points to cover in installing pathogen: how to install pathogen itself, adding `execute pathogen#infect()` to your `.vimrc`, and actually installing your own plugins.

The general idea is that you'll have two directories: `~/.vim/autoload/` and `~/.vim/bundle/`.  `autoload/` is where pathogen will live, and you'll install plugins in `bundle/`.  Here is the one-liner from the project's README which will create both directories, and drop `pathogen.vim` into `autoload/`:

```
mkdir -p ~/.vim/autoload ~/.vim/bundle && \
curl -LSso ~/.vim/autoload/pathogen.vim https://tpo.pe/pathogen.vim
```

Next you'll want to add `execute pathogen#infect()` to your `.vimrc`, and to be safe put it somewhere towards the top (code that appears later in your `.vimrc` may rely on pathogen).  If you're new to Vim, `.vimrc` is your configuration file and it will covered in depth later.  For now if you don't have one, simply create one:

```
touch ~/.vimrc
```

Now we need to actually install some plugins.  The process is awesomely simple: 1) `cd` into your `bundle/` directory, 2) clone the git repository (most often from GitHub).

```
cd ~/.vim/bundle
git clone git@github.com:pangloss/vim-javascript.git
```

That's it.  Seriously.

### Vundle

Vundle takes a different approach, where you'll declare each plugin directly in your `.vimrc`, and run `:PluginInstall` to install your plugins.

Find Vundle on github [here](https://github.com/gmarik/Vundle.vim).  Full installation instructions are available there and I recommend you at least browse them.  Here I'll add an abbreviated version and some notes of my own.

To install Vundle:

```
git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

The general idea is that you'll put the necessary Vundle code at the top of your `.vimrc` (there is a detailed example in the project's README), and declare your plugins with a partial path in between `call vundle#begin()` and `call vundle#end()` like so:

```
set nocompatible              " be iMproved, required
filetype off                  " required

set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()

Plugin 'gmarik/Vundle.vim'    " required

" Here are your plugins
Plugin 'tpope/vim-rails'

call vundle#end()            " required
filetype plugin indent on    " required
```

After you add plugins to your `.vimrc`, run `:PluginInstall` to install.

For adding new plugins, pay attention to the syntax in `Plugin 'tpope/vim-rails'`.  For all of the plugins we'll go over today (which are hosted on GitHub) you'll want to follow this pattern, which is:

```
Plugin '<github username>/<repo name>'
```

Then you'd save the file and run `:PluginInstall` within Vim.  This will use [curl](http://curl.haxx.se/) to download the listed plugins and place them in `~/.vim/bundle/`.

For example, you'd add a plugin called nerdtree (located at [https://github.com/scrooloose/nerdtree](https://github.com/scrooloose/nerdtree)) like so:

```
Plugin 'scrooloose/nerdtree'
```

To be safe, just copy and paste the end of the url as it appears in your browser, as you can run into issues with some repositories having the file extension `.vim`, such as jelly beans: [https://github.com/nanotech/jellybeans.vim](https://github.com/nanotech/jellybeans.vim).

Keep in mind Vundle doesn't only support plugins hosted on GitHub, check the [README](https://github.com/gmarik/Vundle.vim/blob/master/README.md) for more options, such as [http://vim-scripts.org/](http://vim-scripts.org/).

## Part 2: Plugins

There are a LOT of plugins for vim, sometimes overwhelmingly so.  In reality they are simply programs written in [VimScript](http://learnvimscriptthehardway.stevelosh.com/) that give Vim some kind of extra functionality that doesn't exist out of the box.  They can be small in scope, like [rainbow_parentheses](https://github.com/kien/rainbow_parentheses.vim), which is a syntax highlighting plugin for the lisp enthusiasts of the world.  They can also be larger in scope, like [fugitive](https://github.com/tpope/vim-fugitive), a powerful and useful tool to move your git workflow into Vim.

Here are some plugins to get started (in Ruby/Rails/JavaScript of course).

* [NERDtree](https://github.com/scrooloose/nerdtree) - Vim has a built-in file tree explorer, but NERDtree has some great features like modifying, deleting, and adding files directly from the tree.
* [Syntastic](https://github.com/scrooloose/syntastic) - On the fly syntax checking, this can catch syntax errors in-editor, saving you from wasting time with syntax issues at runtime.
* [Ctrl-p](https://github.com/kien/ctrlp.vim) - Fuzzy finder (files, buffers, etc).  If you like ctrl-p in sublime text, or cmd-t in textmate, this will be a must-have.
* [vim-ruby](https://github.com/vim-ruby/vim-ruby) - Extra features for editing Ruby in Vim.  Select entire methods from def to end, go to the next method definition, etc.
* [vim-rails](https://github.com/tpope/vim-rails) - Extra features for Rails, one of the best being `:A` to switch between test and implementation files.
* [vim-javascript](https://github.com/pangloss/vim-javascript) - Enhanced indentation and syntax-highlighting for JavaScript.
* [Fugitive](https://github.com/tpope/vim-fugitive) - Move your git workflow into Vim.
* [vim-airline](https://github.com/bling/vim-airline) - Status bar with things like your current git branch and obvious visual cues to know which mode you're in.
* [vim-multiple-cursors](https://github.com/terryma/vim-multiple-cursors) - Select multiple instances of a word or phrase and change them in one go.
* [vim-ragtag](https://github.com/tpope/vim-ragtag) - Make editing html/xml not terrible.

Install them using your preffered plugin manager, though you may want to wait until you pick your `.vimrc` in the next section.

## Part 3: .vimrc

As mentioned above your `.vimrc` is your configuration file.  This is where you'll go to add keybinds, write functions to add functionality, set options like color themes, etc.  If you're feeling particularly brave there is a great book, [Learn VimScript the Hard Way](http://learnvimscriptthehardway.stevelosh.com/).  Everything you need to get started really diving into customizing your `.vimrc` or potentially writing your own plugins you'll find there.

There are a few paths I'll offer for you:

* Use someone else's `.vimrc` wholesale
* Use my starter `.vimrc`
* Start from scratch and mix and match from sources you like

##### Use someone else's

A common strategy among new Vim users is to adopt an existing `.vimrc`, and there are plenty of great ones out there.  A few of my favorites are [Ben Orenstein](https://github.com/r00k/dotfiles/blob/master/vimrc), [Chris Hunt](https://github.com/chrishunt/dot-files/blob/master/.vimrc), and [Josh Cheek](https://github.com/JoshCheek/dotfiles/blob/master/vimrc).

If you choose to go this route I do have a piece of advice: make sure you understand the code before you use it.  I chose Josh's `.vimrc` when I started, made a point to comment out pieces that I didn't understand, and when I had time to read about them and grasp exactly what they did, I would finally uncomment them.  I think it's a valuable exercise to build your configuration over time as opposed to adopting potentially years of work wholesale from an existing `.vimrc`.  It's an easy way to feel overwhelmed and as a result not dive into any one piece in-depth.

If you choose to use one of the above `.vimrc` files, you have two options.  One, you can just create your own file by running `touch ~/.vimrc` from the command line, and simply copy/paste the contents in.  Two, you can clone the entire dotfiles repository, and copy the `.vimrc` file into your home directory.  An example using Josh Cheek's config:

```
git clone git@github.com:JoshCheek/dotfiles.git
cd dotfiles
cp vimrc ~/.vimrc
```

Two gotchas here:

* Make sure your config file eventually has the full path `~/.vimrc`, meaning it lives in your home folder, and has a leading `.` (making it a hidden file).  Notice in the example above the file is originally named without a dot, and we changed the filename to include one when we copied it.
* Be aware of which plugin manager they use.

##### Use my starter .vimrc

I'll use Vundle here as it makes the plugin installation process about as easy as it gets.

If you haven't yet, install Vundle:

```
$ git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

Here is the raw [blob](https://raw.githubusercontent.com/wfro/dotfiles/master/starter-vimrc) if you want to just copy/paste the contents in your own `.vimrc` file, and if you want to use the above method of cloning the whole dotfiles repo:

```
git clone https://github.com/wfro/dotfiles.git
cd dotfiles
cp starter-vimrc ~/.vimrc

# Now open the file to start installing plugins
vim ~/.vimrc
```

After you have the file open in vim, run `:PluginInstall` to install any plugins.

##### Start from scratch/mix and match

If you'd like to build your own with inspiration from multiple sources, simply `touch ~/.vimrc` if you don't have one yet and hack away!

## Part 4: COLOR THEMES!

As the icing on the cake we'll set a color theme.  Browse some themes [here](http://vimcolors.com/), though lately I've been a fan of [jellybeans](https://github.com/nanotech/jellybeans.vim).

With Pathogen:

```
cd ~/.vim/bundle
git clone git@github.com:nanotech/jellybeans.vim.git
```

With Vundle add the following to your plugins section (if you used my starter `.vimrc` this should already be taken care of):

```
Plugin 'nanotech/jellybeans.vim'
```

Then add this line to your `.vimrc`:

```
colorscheme jellybeans
```

Rinse and repeat for whatever colorsheme you're into at the time.

And that's it!  I will say that setting up Vim compared to something like Sublime Text is ...involved.  But it's all about tradeoffs.  Sublime Text gives you great functionality out of the box, Vim is Shangri-La for people who love to tinker and customize.
