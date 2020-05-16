# **VIM Tutorial** (`$ vimtutor`)

## Chapter 0
- Personal configuration of VIM: `~/.vimrc` -> Check Chapter 7
- There are 3 stages in VIM:
  1. Command
  2. Insert = `i`
  3. View = `v`
- You can escape from each stage with `esc` button.
- Move cursor in command mode:
  - Left arrow = `h`
  - Right Arrow = `l`
  - Up Arrow = `k`
  - Down Arrow = `j`
- `Ctrl+b` = Page up
- `Ctrl+f` = Page down
- `:q` = [quit]
- `:q!` = [quit] without save
- `:qw` = [quit] with save
- `:w` = [write] save
- `:x` = `:wq` = Write and Exit

## Chapter 1
- `a` = [append] -> Start from a character 
- `A` = [Append] -> Go to end of line and append
- `x` = Delete a character under the cursor
  - `xp` = Substitute character under cursor with next character
- `X` = Delete a character backward

## Chapter 2
- `d` = [Delete] + KEYWORD
  - `w` = [Word] + space
  - `e` = [End] -> from place of cursor to the end of word
  - `$` = to the end of line
- `w` = [Word] -> Go to next word
- `e` = [End] -> Go to end of word
- `0` or `^` = Go to beginning of line
- `$` = Go to end of line
- Combination (Operator[number]motion):
  - `d3w` = Delete next 3 words
  - `d$` = Delete whole line from cursor's position to the end of line
  - `ce` = [change] until the end of word + enter Insert mode
- `dd` = delete whole line without consideration of cursor's position
  - `yy` = [Yank Yank] paste the deleted line in place of cursor
- `u` = [undo] undo a single character
- `U` = [Undo] undo a whole line changes
- `Ctrl+r` = [redo]

## Chapter 3
- `P` = [Put] previously deleted text under/after the cursor
- `rX` = [replace] replace the character under cursor with X

## Chapter 4
- `Ctrl+g` = Show line numbers and your cursor's position
- `gg` = Go to top of file
- `G` = Go to end of file
- `NUM+G` = Go to line number NUM
- `NUM+H` = Go to NUMth line from top of screen
- `NUM+L` = Go to NUMth line from bottom of screen
- Search
  - `/` = Forward search
  - `?` = Backward search
  - `n` = Go to next word in search
  - `N` = Go to previous word in search
  - `Ctrl+O` = Go backward to where you came from
  - `Ctrl+I` = Go forward from where you went with `Ctrl+O`
  - `/KEYWORD\c` = `\c` will ignore case sensitivity search for just a single time
- `%` = Matching `(), {}, []` in current line
- Substitution
  - `:s/OLD/NEW` = To substitute NEW for the first OLD in a line
  - `:s/OLD/NEW/g` = To substitute NEW for all 'OLD's on a line
  - `:#,#s/OLD/NEW/g`
    - `#,#` = are line numbers. From line # to line #
    - `g` = Globally (change all occurrences) in line
  - `:%s/OLD/NEW/g`
    - `%` = Whole file without considering specific line numbers
  - `:%s/OLD/NEW/gc`
    - `c` = Ask for confirmation whether to change it or not

## Chapter 5
- `:! COMMAND` = Execute the COMMAND with shell and show the result
- `:.! COMMAND` = Execute the command and import command's output in VIM
- `:w FILENAME` = Save this file to another file named FILENAME
- `v` = Visual mode for selecting -> press `v` and select what you want with arrow keys, then press `:` and you can see `:'<,'>` on bottom. Finally you can decide what to do with your selection. For instance, you can save the selected part to another file and name it TEST. -> `:'<,'>w TEST`
- `V` = Visual mode with line selection
- `v >` = Forward indent for selected line(s)
- `v <` = Backward indent for selected line(s)
- `Ctrl+v` = Visual mode with block selection
- `:r FILENAME` = Retrieves file FILENAME from your hard disk and puts it below the cursor position.
- `:r !COMMAND` = Reads the output of your COMMAND and puts it below the cursor position.

## Chapter 6
- `o` = [open] a line below the cursor + Enter Insert mode
- `O` = [Open] a line above the cursor + Enter Insert mode
- Append commands
  - `a` = [append] from next character of cursor
  - `A` = [Append] from end of line
  - `i` = [insert] exactly at the place of cursor
- `R` = [Replace] mode is like insert mode, but every typed character deletes an existing character.
- Copy and Paste
  1. Select your text in visual mode (v)
  2. `y` = [yank] copy your selected text
     - `yw` = [Yank Word] Copy the word under the cursor
     - `yy` = [Yank Yank] Copy the whole line including the newline character at the end of the line.
  3. `p` = [paste] wherever you want
     
## Chapter 7
- `:help`
  - `Ctrl+w + Ctrl+w` = (Hold down Ctrl button and press w button 2 times) to jump from one window to another.
  - `:q` = Close help window
- `:help KEYWORD` = Find help on your KEYWORD
- VIMRC
  - Most of the vim features are disabled by default. You have to enable them manually by creating a configuration file named `vimrc`
  - If you wanna add plugins, remember that they are only load during initialization.
  - You can see example of `vimrc` with this command in vim `:r $VIMRUNTIME/vimrc_example.vim`
  - You can personalize your `vimrc` based on your programming language.
  - Some personal vimrc configurations:
    - `:set number` = Show line numbers
    - `:set autoindent` = Enable auto-indentation
    - `:set hlsearch` = Enable highlight in search
    - `:set ic` = Ignore case-sensitive in search
    - `:set is` = Show partial matches for a search phrase
  - `:set noX` = You can disable set `X`
- Completion
  - `:set nocp` = To make sure Vim is not in compatible mode
  - Use `:` and choose a character, then press `Ctrl+d` to show a list of commands that start with your chosen character. You can even complete your command with `tab` button if it's unique.
  - It's pretty helpful in `:help` or large files which you need different commands to complete your work.

## Extended Chapter
- Move cursor with mouse instead of arrow keys: `:set mouse=a`
- Soft text wrapping: `:set wrap linebreak nolist`
- Activate syntax highlighting: `syntax on`
- Set tab for X spaces: `:set shiftwidth=X`
- `J` = [Join] this line with the next line
- `.` = Repeat the latest command
- `%` = Go to the other side of bracket, curly bracket, or parenthesis in command mode.
- `:Explore` = Access file manager from inside VIM
- `:Vexplore` = Vertically split window and access file manager
- `:Sexplore` = Horizontally split window and access file manager