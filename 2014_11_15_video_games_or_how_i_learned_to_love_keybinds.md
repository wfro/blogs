As a programmer, or even as a frequent computer user, you need keybinds.  Trust me, they are amazing.  In most situations they are just flat out faster.  You should use keybinds to navigate your operating system, web browsers, text editors, and anything else you can imagine where you spend unecessary energy on mouse movements.

Here I thought I'd share some strategies I use to learn keybinds.  Any insight here wasn't just gleaned from my programming days, it's been slowly ingrained in me over time through none other than the magic of videogames. Some gaves you would have 50, 70, even 100+ abilities that you needed to keep track of and be able to use on demand with as little delay as humanly possible.

With competitive games in particular the motivation for putting up with the hassle of learning keybinds is obvious.  There is a clear goal that they help you reach: be better.  Win more.  Tell people just how bad you think they are in all chat more frequently (don't do this last one).

With programming I've found it's a little bit more abstract, but the general concept holds true for both.  As a developer things like your OS and your editors are your tools.  They facilitate the goal of producing good code and you want your actual physical production to keep up with your thinking.

The process is relatively straightforward:

1) Force yourself to use them.  Don't cheat.
2) Have a master list of keybinds a.k.a cheatsheet.  Build this up over time.
3) Have a few that you keep on hand (sometimes I have an index card that I'll keep with my computer) that you plan on deliberately practicing.
4) Profit.

Long term, 2 and 3 are the most important.  The first usually takes some time to get over, but once you cross that bridge you'll likely stay there forever.

The general idea is to not take on too many new bindings at once.  Start with a few, get the muscle memory down, and make sure you add them to the cheatsheet.  Rinse and repeat.

If you're new to your tools here are a few common bindings that would be a great place to start to build up a cheatsheet.  Keep in mind these are for OSX, but the windows/linux equivalents should be straightforward, often replacing cmd with Ctrl.

###### Sublime Text

* **ctrl-shift-K** => delete (kill) line
* **cmd-enter** and **cmd-shift-enter** => insert line below and above respectively
* **cmd-p** => fuzzy file search

###### Web Browers

* **cmd-t** => open new tab
* **cmd-w** => close tab
* **cmd-option-i** => chrome dev tools

###### iTerm2

* **cmd-1-9** => 'command 1 through 9' quickly reach specific tabs
* **cmd-d** => vertical split terminal window
* **cmd-[, cmd-]** => Swap between terminal windows

###### Vim

Vim deserves its own section, especially now that I've had some time to grow my collection of keybinds.

It can help to see examples from more experienced developers, so if you're feeling ambitious check out [Ben Orenstein's .vimrc](https://github.com/r00k/dotfiles/blob/master/vimrc) and look for the huge block of commands prefixed with `map <Leader> ...`. It's also worth checking out [this talk](https://www.youtube.com/watch?v=SkdrYWhh-8s) he gave on breaking out of low-intermediate to intermediate level vim.  Unsurprisingly, keybinds are a huge part of that.

Here are some that I've picked up:

```VimL
" insert pry on a new line and return to normal mode
map <Leader>p orequire "pry"; binding.pry<esc>
" open rails database schema
map <Leader>db :e db/schema.rb<cr>
" insert console.log() on a new line and place cursor inside parentheses
map <Leader>cl oconsole.log();<esc>hi
" insert JavaScript debugger on a new line and save
map <Leader>de odebugger;<esc>:w %<cr>
" Open a model using rails-vim, variants for controllers, specs, etc. also exist
map <Leader>em :Emodel<Space>
```
