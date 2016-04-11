Here's a rough outline of what we're about to accomplish: we'll install [tmux](http://tmux.sourceforge.net/), add some cool configurations, and setup Vim/tmux to run Ruby/JavaScript/whatever tests using Vim keybinds.  Cool.  Let's do some things to some stuff.

Posts on Vim/tmux and how to supercharge your TDD workflow aren't in short supply, but I thought I'd share my own setup as I ended up pulling from multiple sources to find something I liked.

The general idea is that you should be moving as much of your workflow into your editor as possible.  The time it takes to switch from editor to terminal, type in whatever you need for your tests, and hit return adds up quickly.  What we're about to go over is one solution to this, something like [Guard](https://github.com/guard/guard) is another.  I use Guard often, but sometimes I want more control over running my test suite as opposed to having them run every time changes are saved.

Before we get started, you should have Vim installed, and a way to install plugins (check out a previous [post](http://willfaurot.com/first-time-vim-setup-101/) if you're new to Vim).

Let's enter the matrix.

## 1) Install tmux

If you're on OS X, install tmux using [homebrew](http://brew.sh/):
```
$ brew update
$ brew install tmux
```

Start a tmux session by running `$ tmux` from the command line.

If you're running Linux, check your package manager's official repositories, though keep in mind the official repos often keep outdated versions (we're looking for 1.9a as of 1/15/15).

You can run the following to ensure that the installation succeeded and you have the correct version:

```
$ tmux -V
tmux 1.9a
```

Tmux stands for "terminal multiplexer", and keep in mind this won't be an in-depth look into its features.  If you're brand new to tmux, [here](https://gist.github.com/MohamedAlaa/2961058) is a cheat sheet, [here](https://danielmiessler.com/study/tmux/) is an excellent beginner's tutorial, and if you're feeling particularly bold, [here](https://pragprog.com/book/bhtmux/tmux) is a book written by [Brian P. Hogan](http://bphogan.com/).  What we care about today is that tmux allows us to use Vim to send commands to a separate terminal pane without leaving the comfort of our favorite text editor (we'll go into this in depth in a minute).

## 2) .tmux.conf (optional)

If you'd just like to get to the running tests bit, feel free to skip this section.

Tmux is great and fully functional out of the box, as is Vim.  But like Vim, adding your own custom configurations can greatly enhance your experience.

Here is my current [.tmux.conf](https://gist.github.com/wfro/2c292e931e3caa97a0ce) as a gist.  Create a new file by running `$ touch ~/.tmux.conf`, and copy the contents into the new file.  There are some optional settings commented out, feel free to give them a try.  Similar to `.vimrc`, make sure it lives in your home folder.  Full credit to [Chris Hunt](https://github.com/chrishunt/dot-files)/[Ben Orenstein](https://github.com/r00k/dotfiles) for their tmux configurations (which I've used nearly wholesale).  My only addition was to add Ctrl-HJKL for tmux pane switching with vim awareness as per [this Thoughtbot article](http://robots.thoughtbot.com/seamlessly-navigate-vim-and-tmux-splits).


## 3) Basic tmux Usage

Here are the few basic commands you'll need to get started with tmux (at least up to the point of being able to get this test workflow working).

* Create a new session by running `tmux` from the command line.
* `C-b` is your prefix key, meaning you'll run this before most tmux commands.
* `C-b %` - vertical split pane
* `C-b "` - horizontal split pane
* `C-b o` - move to next pane.
* `C-b q <number>` - go to a specific pane (tmux will show you a number for each pane after you hit `C-b q`)
* `C-b :resize-pane -L 10` - resize whatever pane your cursor is currently in.  Replace the -L flag with -R, -U, -D (right, up, down), and however many units you want to resize by.  If you chose to use my `.tmux.conf`, you can accomplish this with `C-b H (or JKL)`, where each letter will resize the pane corresponding to Vim's movement scheme.

## 4) Install Vim Plugins (vimux/turbux)

There are several options for interacting with tmux from Vim, but we'll start by using a combination of Vimux and Turbux.

* [Vimux](https://github.com/benmills/vimux) - This will allow us to interact with tmux from Vim.  Have a look at the help file (in `docs/vimux.txt`), but the big thing we care about is `VimuxRunCommand()`.  This function will send a command to a separate tmux pane if you have one open, or create a 20% tall horizontal pane and run the command in that if you don't.
* [Turbux](https://github.com/jgdavey/vim-turbux) - A Ruby-specific plugin for running tests.  Keep in mind this relies on Vimux (or one of its alternatives such as tslime).

Turbux gives you two default bindings to run tests in a separate tmux pane:

```
<Leader>t " Run test file
<Leader>T " Run focused test (whichever test your cursor currently resides in)
```

Turbux will infer what testing framework to use based on filename.  It currently supports Minitest, RSpec, and Cucumber.

It's also particularly awesome with [vim-rails](https://github.com/tpope/vim-rails).  You can also use turbux's keybinds in implementation files, not just the test files themselves.  For example (using rspec): if you have an `Order` model and run `,t` inside of `app/models/order.rb`, turbux would be able to infer the test file and run `spec/models/order_spec.rb`

Check out [this StackOverflow post](http://stackoverflow.com/questions/1764263/what-is-the-leader-in-a-vimrc-file) if you're unfamiliar with Leader keybinds.  Leader is bound to `\` by default in vim.  This is a little awkward to reach, and I personally rebind it to `,` by putting `let mapleader = ","` in my `.vimrc`.

Install these using your preferred plugin manager such as [Vundle](https://github.com/gmarik/Vundle.vim) or [Pathogen](https://github.com/tpope/vim-pathogen).

## 5) Let's Run Some (Ruby) Tests

If you've installed everything correctly up to this point you have everything you need to run Ruby tests with Vim and tmux.

Before we get started you need to decide what you want your split windows to look like (of course you can always try different setups and see what you like best).  Vimux by default will create a 20% tall horizontal pane below Vim if no other panes are open.  If you'd like something other than that, create and adjust a pane using the previously mentioned commands.  Vimux will send commands to this pane instead of creating a new one for you.  My best piece of advice is to play around with it and see what you like.

Here are step by step instructions, ignore the step where you create a second pane if you'd like to use the Vimux default:

* Start a new tmux session by running `tmux` from the command line
* Split your current tmux pane using either `C-b %` (vertical) or `C-b "` (horizontal)
* In one pane, open up a Ruby test file in Vim (i.e. a file ending in _test.rb or _spec.rb)
* Make sure the remaining empty terminal pane is in the correct directory (where the test files can be found).
* Run `<Leader>t` to run tests in the whole file
* Run `<Leader>T` with your cursor inside a single test definition for a focused test

Assuming you rebound your Leader to be comma by puttting `let mapleader = ","` in your `.vimrc`, the sequences would be `,t` and `,T`.

If all goes according to plan, you should see the right tests run in a separate tmux pane.  And hopefully you're one step closer to feeling like [this guy](http://i.imgur.com/2ITPC1u.jpg).

## 6) Adding Your Own Vimux Commands

Turbux is a handy plugin, but it only supports Ruby.  The great thing about Vimux is that it lets you send any command you want to a tmux pane via `VimuxSendCommand()`.  Here is an example for running [jasmine-node](https://github.com/mhevery/jasmine-node) tests, run this directly from Vim's command line:

```
:call VimuxRunCommand("clear; jasmine-node --verbose " . bufname("%"))
```

`clear` clears (duh) the content in your terminal.  Then we invoke `jasmine-node` with the `--verbose` flag (prints your spec descriptions and other extra tidbits, optional of course).  Finally, `bufname("%")` will resolve to your current Vim buffer.

Running this manually would become annoying extremely quickly, so you can add a keybind to your `.vimrc` like so:

```
map <Leader>rj :w %<cr>:call VimuxRunCommand("clear; jasmine-node --verbose " . bufname("%"))<cr>
```

Here is another trusty `<Leader>` bind, so if you have yours bound to comma, the full sequence would be `,rj`.  This command saves your current Vim buffer as well before running `VimuxRunCommand`.

At this point, you have everything you need to send whatever you want to a tmux pane within Vim: just pass in a command to `VimuxRunCommand`.

This concludes our journey in Vim/tmux land.  Hopefully you were able to get this far painlessly, but of course there is an endless number of things that can go wrong in the act of setting up new tools/toys.  Always feel free to ask questions/criticize/make suggestions/lavishly praise me on [Twitter](https://twitter.com/will_faurot).
