
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

