---
layout: post
title: "Notes from Practical Vim"
date: 2018-12-27 11:00:00 
comments: true
description: "Notes from Practical Vim"
categories:
- books
- editor
- vim
permalink: 2018-12-27-practical-vim

---

<img src="{{ site.url}}/assets/images/books/practical_vim.jpg" alt="drawing" width="200"/>

Some of the content of this book I already knew, but it was good to understand them in a deeper level and how can 
I extend the usage from most of the basic commands that I have been using before.

* the `.` command let us repeat the last change. It is the most powerful and versatile command vim
* the `>G` command increases the indentation from the current line until the end of the file
* the `x`, `dd` and `>` commands are all executed from Normal mode, but we also create a change each time we dip into
Insert mode. From the moment we enter in `IM`, vim records every keystroke
* `A` to insert in the end of the current line
* the `;` command will repeat the last search that the `f` command performed
* to perform an substitution in a file `:s/target/replacement` 
* `*` searchs for the next occurrence of the word under the cursor
* vim records our keystrokes until we leave `IM`
* the author mentions the `dot formula`: one keystroke to move, one keystroke to execute
* `dw` deletes a word
* `dw` deletes a word and a space
* list of vim `operators`:
    they can operate on a single character `dl`, a complete word `daw`, or an entire paragraph `dap`:
    * `c`: change
    * `d`: delete
    * `y`: yank into register
    * `g~`: swap case
    * `gu`: make lowercase
    * `gU`: make uppercase
    * `>`: shift right
    * `<`: shift left
    * `=`: autoindent
* grammar: an action is composed from an `operator` + `motion`
* when invoking the operator in duplication (`dd` per example) it acts upon the current line
* you can add operators to vim: take the `commentary.vim` plugin. The operator it's `gc`, so `gcc` comments the current
line
* editing from the `IM`: 
    * `ctrl + h`: delete back one char
    * `ctrl + w`: delete back one word
    * `ctrl + u`: delete back to start of the line
* `viw`: select inner word in visual mode
* to enter in `IM`: `i` it's for before the current char and `a` (append) after the current char
* when we open a file, vim creates a new buffer and loads it into the current window

# inline search

* `f{char}` searchs for the next char occurrence 
* `F{char}` for previous char occurrence 
* `t{char}` searchs for the char before next char occurrence 
* `T{char}` searchs for the char before previous char occurrence 
* `;` repeat the last character-search command
* `,` reverse the last character-search command

# motions for lines

* `j`: down one real line
* `gj`: down one display line
* `k`: up one real line
* `gk`: up one display line
* `0`: to first character of the real line
* `g0`: to first character of the display line
* `ˆ`: to first nonblank character of the real line
* `gˆ`: to first nonblank character of the display line
* `$`: to end of the real line
* `g$`: to end of the display line

# motions for words

* `w`: forward to start of next word
* `b`: backward to start of current/previous word
* `e`: forward to end of current/next word
* `ge`: backward to end of previous word
* append to end of word: `ea`
* append to end of previous word: `gea`
* definition of a `WORD`: sequence of nonblank characters separated with whitespaces
* vim's text objects consist of two characters, the first of which is always either `i` or `a`. In general, we can say
that the text objects prefixed with `i` select inside the delimiters, whereas those that are prefixed with `a` select
everything including the delimiters
* `ci"`: change inside the double quotes
* `d{motion}` tends to work well with `aw`, `as` and `ap` whereas the `c{motion}` command works better with `iw` and
similar

# other motions

* `"`: position before the last jump within current file
* `%`: jumps between opening and closing sets of `(), {}, []`
* `<Leader>f{char}`: my leader key is `,`, so `,f{char}` would show the keys that I can press to go to `{char}`. This
also works with `AceJump` IntelliJ plugin with you configure the keys properly.

# surround.vim plugin

* `cs"'`: change surround from `"` to `'`
* `ds'`: delete surround `'`
* `ysiw[`: add surround in an inner word with `[`

# windows management

* `ctrl + w + s`: split current window horizontally
* `ctrl + w + v`: split current window vertically
* `ctrl + w + w`: cycle between open windows
* `ctrl + w + h`: focus the window to the left
* `ctrl + w + j`: focus the window below
* `ctrl + w + k`: focus the window above
* `ctrl + w + l`: focus the window right
* `ctrl + w + c`: close the active window
* `ctrl + w + T`: move the current window into its own tab
* `{N}gt`: switch to tab page number {N}
* `gt`: switch to the next tab page
* `gT`: switch to the previous tab page

# visual mode

* `v` enables character-wise visual mode
* `V` enables line-wise visual mode
* `ctrl + v` enables block-wise visual mode
* `gv` reselects the last visual selection
* the `.` command in visual mode acts on the same amount of text as was marked by the most recent visual selection. But
doesn't work so well sometimes
* `vit`: select the inner contents of a tag

# copy and paste

* in Vim's terminology, we don't deal with the clipboard but instead with registers
* the `x` command cuts the character under the cursor, placing a copy of it in the unnamed register after the cursor
position
* `yyp`: copy and paste line
* "Oops! I clobbered my yank": you had something to paste, but before pasting it you had to cut something and then your
previously yanked text was replaced by the cut that you did
* vim's delete command is equivalent to the standard cut operation
* black hole register is addressed by the `-` symbol, so `_d{motion` performs a true deletion
* delete, yank and put commands interact with one of Vim's registers. We can specify which register we want to use
by prefixing the command with `"{register}`. If we don't specify the register, then Vim will use the unnamed register
* example: cut the current line into register `b` with `"bdd` and paste it with `"bp`
* when we use `y{motion}` command, the specified text is copied not only into the unnamed register but also into the
yank register, which is addressed by the `0` symbol. The yank register is reliable, whereas the unnamed register is
volatile. We can cut something and when using `"0p` we would still have what we want
* copying and paste from clipboard:
    * if we capture text in an external application, then we can paste it inside Vim using `"+p` command
    * if we want to copy text from vim to an external application, then we have to use the register `"+`
* the visual selection in the document swaps places with the text in the register
* `p`: pastes after the cursor position
* `P`: pastes before the cursor position

# macros

* when recording a macro, ensure that every command is repeatable
* `w, b, e` is better for macros than `h, j, k, l`
* `q{register}` (you don't need the `"`) to start recording a macro. The word `recording` should appear in the status
* `q` is a good register for a macro, so you just need to press `qq` to start recording and then a final `q` to finish
* `@{register}` command executes the macro in the given register
* `@@` repeats the that was invoked most recently
* to append commands in a macro: if you macro was `q`, then use `Q`. Per example `qQ` to start appending commands in the
previous macro that you recorded using `qq`

# search and replace

* `%s/going/rolling/g`: it will search and replace in the whole file the word `going` for `rolling`
* `%s/going/rolling/gc`: it will ask every time that it finds a `going` if we want to replace it for `rolling`
















