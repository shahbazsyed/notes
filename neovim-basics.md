# NeoVim Basics
This guide covers essential concepts and commands for efficient text editing in [neovim](https://neovim.io/). I also use [neovide](https://neovide.dev/installation.html) as the GUI together with [nvChad](https://nvchad.com/) for configuration which provides a beginner friendly setup with useful plugins and keybindings. Most of the shortcuts work in plain vim as well. 

## Modes
* **Normal Mode:**
  * This is your primary mode for navigation, executing commands, and managing text.
  * Use `<Esc>` to return to normal mode from any other mode.
* **Insert Mode:**
  * Use `i`, `a`, `o`, `I`, `A`, or `O` to enter insert mode and type text:
    * `i` Insert before the cursor.
    * `a` Append after the cursor.
    * `o` Open a new line below.
    * `I` Insert at the beginning of the line.
    * `A` Append at the end of the line.
    * `O` Open a new line above.
* **Command-Line Mode:**
  * Enter this mode with `:` to execute commands (e.g., `:w`, `:q`, `:s`, `:set`).

* **Visual Mode:**
  * Use `v` (character-wise), `V` (line-wise), or `<Ctrl+v>` (block-wise) to select text.
  * After selection, perform actions like `d` (delete), `y` (yank), or `c` (change).

## Navigation
* **Basic Movement:**
  * `h`, `j`, `k`, `l` to navigate Left, Down, Up, Right respectively.
  * `0` Beginning of line.
  * `$` End of line.
  * `w` Jump forward one word.
  * `b` Jump backward one word.
  * `e` Jump to the end of the current word.
  * `W` Jump forward by a "multi-word" where it considers text connected by hyphens as a single word.
  * `B` Jump backward by a "multi-word" where it considers text connected by hyphens as a single word.
* **Sentences:**
  * `(` Move to the beginning of the previous sentence.
  * `)` Move to the beginning of the next sentence.
  * `j)` Move to the beginning of the next sentence, potentially moving to a new line.
  * `k(` Move to the beginning of the previous sentence, potentially moving to a new line.
* **File Navigation:**
  * `G` Go to the bottom of the file.
  * `gg` Go to the top of the file.
  * `[number]G` Jump to a specific line number.
  * `:[number]` Jump to a specific line number.
* **Combining Numbers:**
  * Combine numbers with motion keys (e.g., `10j`, `5w`) for faster movement.

* **Bookmarks**
  * `m[symbol]` Mark the current location with the specified symbol (e.g.: ma).
  * `'symbol]` Go to the marked location (e.g.: 'a).


## Editing
* **Basic Actions:**
  * `i` Insert before the cursor.
  * `a` Append after the cursor.
  * `o` Open a new line below the current one.
  * `d` Delete.
  * `y` Yank (copy).
  * `p` Paste below the cursor.
  * `P` Paste above the cursor.
  * `u` Undo.
  * `<Ctrl + r>` Redo.
* **Line Operations:**
  * `dd` Delete the current line.
  * `yy` Yank (copy) the current line.
  * `cc` Change the current line (delete and enter insert mode).
  * `D` Delete to the end of the line.
  * `C` Change to the end of the line (delete and enter insert mode).
* **Word Operations:**
  * `diw` Delete inside word.
  * `ciw` Change inside word.
* **Text Deletion:**
  * `d0` Delete to the beginning of the line.
  * `d$` Delete to the end of the line.
* **Change Operations:**
  * `ct[symbol]` Change everything till the next occurrence of that symbol.
  * `ci"` Change inside double quotes.
  * `cip` Change the current paragraph.
* **Text Objects:**
  * `i[symbol]` Select the text inside the specified delimiters.
  * `a[symbol]` Select the text including the specified delimiters (e.g.: da( deletes everything inside the parenthesis and also the parenthesis).
* **Indentation:**
  * `>>` Indent right.
  * `<<` Indent left.
  * `==` Auto-indent the current line.
  * `gg=G` Auto-indent the entire file.

## Searching & Replacing
* **Search:**
  * `/term` Search forward for "term".
  * `?term` Search backward for "term".
  * `n` Next search result.
  * `N` Previous search result.
  * `#` Search forward for the next occurrence of the currently selected token.
* **Replace:**
  * `:%s/old/new/g` Replace all occurrences of "old" with "new" in the file.
  * `:%s/old/new/gi` Case-insensitive replacement.
  * `:%s/old/new/gc` Replace with confirmation for each occurrence.

## Intermediate
* **Registers:**
  * Temporary storage locations for text or commands.
    * `"0`: Most recently yanked text.
    * `"1` to `"9"`: Most recently deleted text.
    * `"a` to `"z"`: Named registers for user-defined content.
  * Use `[register]p` or `[register]P` to paste from a specific register.
  * Use `"ayy` to yank a line into register a.
  * Use `:y A` or `Ay` to append text to register a.
* **Macros:**
  * `qa` Start recording a macro named "a".
  * `q` Stop recording.
  * `@a` Play back macro "a".

## File Management
* `:w` Save the current file.
* `:wq` Save and quit the current file.
* `:q!` Quit without saving the current file.
* `:!command` Execute a shell command (e.g., :!ls).
* `:reg` View all the registers.


## Parentheses
* `%` Jump between matching parentheses.
* `?(` and `N`: Navigate backward in the file by open parenthesis.
* `d%` Delete everything inside a parenthesis and the parenthesis itself.


## Window Management
* **Splitting Windows:**
  * You can split the current window horizontally or vertically to view multiple files at once.
    * **Vertical Split:** Use `Ctrl-w v` to create a new vertical split window.
    * **Horizontal Split:** Use `Ctrl-w s` to create a new horizontal split window.
* **Navigating Windows:**
  * Once you have split windows, navigate using these shortcuts:
    * `Ctrl-w h` Move to the window on the left.
    * `Ctrl-w j` Move to the window below.
    * `Ctrl-w k` Move to the window above.
    * `Ctrl-w l` Move to the window on the right.

## Buffer Management
* **Toggling Between Two Files:**
  * When you have two files open in different buffers, use `Ctrl-6` (or `Ctrl-^`) to quickly toggle between them.

## File Access
* **Opening Recent Files via Telescope:**
  * Use `<leader> fo` to open Telescope and search through recent files.
* **Finding Files in the Current Folder:**
  * Use `<leader> ff` to open Telescope and search through files in the current folder.

## Others
* **Font:** Set the font in your init.lua file: vim.opt.guifont = "SpaceMono Nerd Font:h12".
* Use the command YpVr- to underline text. This command copies the line, pastes it below, and replaces the pasted line with hyphens.
* `.` The period key repeats the last action you performed.