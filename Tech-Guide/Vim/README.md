# **VIM Tutorial** (`$ vimtutor`)

## Chapter 0
- Personal configuration of VIM: `~/.vimrc` -> Check Chapter 7
- There are 3 stages in VIM:
  1. Command
  2. Insert = <kbd>i</kbd>
  3. View = <kbd>v</kbd>
- You can escape from each stage with <kbd>esc</kbd> button.
- Move cursor in command mode:
  - Left arrow = <kbd>h</kbd>
  - Right Arrow = <kbd>l</kbd>
  - Up Arrow = <kbd>k</kbd>
  - Down Arrow = <kbd>j</kbd>
- `Ctrl+b` = Page up
- `Ctrl+f` = Page down
- `:q` = [quit]
- `:q!` = [quit] without write/save
- `:qw` = [quit] with save
- `:w` = [write] save
- `:x` = `:wq` = <kbd>Z</kbd><kbd>Z</kbd> = Write and Exit

## Chapter 1
- <kbd>a</kbd> = [append] -> Start from a character 
- <kbd>A</kbd> = [Append] -> Go to end of line and append
- <kbd>x</kbd> = Delete a character under the cursor
  - <kbd>x</kbd><kbd>p</kbd> = Substitute character under cursor with next character
- <kbd>X</kbd> = Delete a character backward

## Chapter 2
- <kbd>d</kbd> = [Delete] + KEYWORD
  - <kbd>w</kbd> = [Word] + space
  - <kbd>e</kbd> = [End] -> from place of cursor to the end of word
  - `$` = to the end of line
- <kbd>w</kbd> = [Word] -> Go to next word
- <kbd>e</kbd> = [End] -> Go to end of word
- <kbd>0</kbd> or `^` = Go to beginning of line
- `$` = Go to end of line
- Combination (Operator[number]motion):
  - <kbd>d</kbd><kbd>3</kbd><kbd>w</kbd> = Delete next 3 words
  - `d$` = Delete whole line from cursor's position to the end of line
  - <kbd>c</kbd><kbd>e</kbd> = [change] until the end of word + enter Insert mode
- <kbd>d</kbd><kbd>d</kbd> = delete whole line without consideration of cursor's position
  - <kbd>y</kbd><kbd>y</kbd> = [Yank Yank] paste the deleted line in place of cursor
- <kbd>u</kbd> = [undo] undo a single character
- <kbd>U</kbd> = [Undo] undo a whole line changes
- `Ctrl+r` = [redo]

## Chapter 3
- <kbd>P</kbd> = [Put] previously deleted text under/after the cursor
- <kbd>r</kbd><kbd>X</kbd> = [replace] replace the character under cursor with X

## Chapter 4
- `Ctrl+g` = Show line numbers and your cursor's position
- <kbd>g</kbd><kbd>g</kbd> = Go to top of file
- <kbd>G</kbd> = Go to end of file
- `NUM+G` = Go to line number NUM
- `NUM+H` = Go to NUMth line from top of screen
- `NUM+L` = Go to NUMth line from bottom of screen
- Search
  - <kbd>/</kbd> = Forward search
  - <kbd>?</kbd> = Backward search
  - <kbd>n</kbd> = Go to next word in search
  - <kbd>N</kbd> = Go to previous word in search
  - `Ctrl+O` = Go backward to where you came from
  - `Ctrl+I` = Go forward from where you went with `Ctrl+O`
  - `/KEYWORD\c` = `\c` will ignore case sensitivity search for just a single time
- `%` = Matching `(), {}, []` in current line
- Substitution
  - `:s/OLD/NEW` = To substitute NEW for the first OLD in a line
  - `:s/OLD/NEW/g` = To substitute NEW for all 'OLD's on a line
  - `:#,#s/OLD/NEW/g`
    - `#,#` = are line numbers. From line # to line #
    - <kbd>g</kbd> = Globally (change all occurrences) in line
  - `:%s/OLD/NEW/g`
    - `%` = Whole file without considering specific line numbers
  - `:%s/OLD/NEW/gc`
    - <kbd>c</kbd> = Ask for confirmation whether to change it or not

## Chapter 5
- `:! COMMAND` = Execute the COMMAND with shell and show the result
- `:.! COMMAND` = Execute the command and import command's output in VIM
- `:w FILENAME` = Save this file to another file named FILENAME
- <kbd>v</kbd> = Visual mode for selecting -> press <kbd>v</kbd> and select what you want with arrow keys, then press <kbd>:</kbd> and you can see `:'<,'>` on bottom. Finally you can decide what to do with your selection. For instance, you can save the selected part to another file and name it TEST. -> `:'<,'>w TEST`
- <kbd>V</kbd> = Visual mode with line selection
- <kbd>v</kbd><kbd>></kbd> = Forward indent for selected line(s)
- <kbd>v</kbd><kbd><</kbd> = Backward indent for selected line(s)
- `Ctrl+v` = Visual mode with block selection
- `:r FILENAME` = Retrieves file FILENAME from your hard disk and puts the FILENAME content below the cursor position.
- `:r !COMMAND` = Reads the output of your COMMAND and puts it below the cursor position.

## Chapter 6
- <kbd>o</kbd> = [open] a line below the cursor + Enter Insert mode
- <kbd>O</kbd> = [Open] a line above the cursor + Enter Insert mode
- Append commands
  - <kbd>a</kbd> = [append] from next character of cursor
  - <kbd>A</kbd> = [Append] from end of line
  - <kbd>i</kbd> = [insert] exactly at the place of cursor
- <kbd>R</kbd> = [Replace] mode is like insert mode, but every typed character deletes an existing character.
- Copy and Paste
  1. Select your text in visual mode (v)
  2. <kbd>y</kbd> = [yank] copy your selected text
     - <kbd>y</kbd><kbd>w</kbd> = [Yank Word] Copy the word under the cursor
     - <kbd>y</kbd><kbd>y</kbd> = [Yank Yank] Copy the whole line including the newline character at the end of the line.
  3. <kbd>p</kbd> = [paste] wherever you want
     
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
  - Use <kbd>:</kbd> and choose a character, then press `Ctrl+d` to show a list of commands that start with your chosen character. You can even complete your command with <kbd>TAB</kbd> button if it's unique.
  - It's pretty helpful in `:help` or large files which you need different commands to complete your work.

## Extended Chapter
- Move cursor with mouse instead of arrow keys: `:set mouse=a`
- Soft text wrapping: `:set wrap linebreak nolist`
- Activate syntax highlighting: `syntax on`
- Set tab for X spaces: `:set shiftwidth=X`
- <kbd>J</kbd> = [Join] this line with the next line
- <kbd>.</kbd> = Repeat the latest command
- `%` = Go to the other side of bracket, curly bracket, or parenthesis in command mode.
- `:Explore` = Access file manager from inside VIM
- `:Vexplore` = Vertically split window and access file manager
- `:Sexplore` = Horizontally split window and access file manager
