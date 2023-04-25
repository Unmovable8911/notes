# Part I vi and Vim Fundamentals
## 1 Introducing vi and Vim
- The `ex` *line editor* is the underlying editor of `vi`
- Your edit is stored in a *buffer* lives in your memory before you save your file
- Discard all changes and reloads the last saved version of the file `:e ENTER`
## 2 Simple editing
### 2.1 Moving around
| Keystroke        | Action                | Keystroke        | Action                |
| :---:            | ---                   | :---:            | ---                   |
| j / `CTRL-N` / - | move down             | k / `CTRL-P` / + | move up               |
| h                | move left             | l                | move right            |
| w                | move one word forward | W                | ignore punctuations   |
| b                | move one word backward | B               | ignore punctuations   |
| G / \<n>G | **G**oes to last line / nth line | gg | go to the first line |
| 0 (zero)         | beginning of the line | $                | end of the line       |

You can combine commands with *numbers* to repeate command *number* of times. For example,
if you type `5j`, the cursor will move five lines down.
### 2.2 Simple edits
- `i`: insert text before the cursor  
    `I`: Insert text before the first character of the current line  
- `a`: append text after the cursor  
    `A`: Append text at the end of the line
- `x`: delete the current character
- `c`: change. You can combine `c` with *numbers* and chunks (e.g. `w`)
- `d`: delete. You can combine `d` with *numbers* and chunks (e.g. `w`)  
    `dd`: delete the current line
- `r`: replace the current character  
    `R`: Replace text, start from the current character
### Bullet Points
- `wrapmargin` or `wm` option can automatically wrap a line when its length reaches the
value you specified.Usage:  
    `:set wm=100`, `:set nowm`, `:set nowrapmargin`
- `number` or `nu` option can add a line number before a line. Usage:   
    `:set number`, `:set nu`, `:set nonumber`