
## Grep
Nice setting for grepping in Vim
- `vim.opt.grepprg = "grep -HRIn $* ."` 
- H: filename
- R: recursive
- I: ignore binary
- n: line numbers
- now you can grep things by doing
	- `grep something` 
- then open the quickfix list 
	- `:copen` 
- move inside the quickfix list
	- `:cnext` 
	- `:cprev`
	- `:cc` 
	- `:cc #` -> # is some number
- Execute some command on all of the quickfix list
	- `:cdo [some command]`  
- and close the quickfix list
	- `:cclose` 

For a better keymapping you can do
- `vim.keymap.set("n", "<leader>gg", ":copen | :silent :grep ")` 
	- the command on the right of the pipe is executed first then the one on the left
	- This will grep things without opening the large and annoying window when grepping then automatically open up the quickfix list


## Commands I should remember
Toggle cap and uncap
- `~` 
Go back to selected text
- `gv` 
split the long line into multiple lines
- `gq` 
Go to the end or beginning of textline (not the end of the paragraph)
- `g$` and `g0`
Connect all (selected) multiple lines
- `gJ`
Execute the sort command on selected lines
- `<,>! sort` 
Take the previous replace and run it through the entire doc
- `g&` 
Show all the commands for `g` 
- `:help g` 
Select things in circle brackets (  )
- `vib`
Select things in curly brackets { }
- `viB` 
Append at the end of multiple lines 
- `ctrl+v` -> `$` -> `A` 
Indent whole file 
- `gg=G` 
Indent selected lines
- `V` -> select the lines -> `=` 
Suspend vim then bring back (to foreground)
- `ctrl+z` -> `fg` 
Open URL under cursor
- `gx` 
Open file under cursor
- `gf` 
Jump between marks in different files (you use capital letters for marking) i.e.,
- `mA` (for example the letter `A` )
Jump to line, for example, 40
- `:40` equivalent to `40G` 
Search all instances of word under cursor
- `*` 
Paste from register number 3
- `"3p` 
Yank to register number 7
- `"7y` 
List all marks 
- `:marks` 
Jump to next/previous line lower case mark
- `]'` / `['` 
Jump to next/previous lower case mark (exact location)
- `]` + tick (cannot type tick inside code) / `[` + tick
Jump to previous change in current buffer
- tick + `.` 
Jump to position where exited current buffer 
- tick + `"` 
Jump to position last edited in vim
- tick + `0` 
Delete all marks
- `:delmarks!` 
Delete mark `a`
- `:delmark a` 
Jump to next or previous in jump list
- `ctrl+o` / `ctrl+i`
List all jumps
- `:jumps` 
Go to older position in jump list
- `g;` 
Go to newer position in jump list
- `g,` 
Go to the 3rd older/newer location in jump list
- `3g;` / `3g,` 
Clear jump list
- `:clearjumps` 
Show change list
- `:changes` 

