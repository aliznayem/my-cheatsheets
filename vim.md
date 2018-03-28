#### Commenting Out ####
Source: http://stackoverflow.com/questions/1676632/whats-a-quick-way-to-comment-uncomment-lines-in-vim

1. press Esc (to leave editing or other mode)
2. hit ctrl + v (visual block mode)
3. use the up/down arrow keys to select lines you want (it won't highlight everything - it's OK!)
4. Shift + i (capital I)
5. insert the text you want, i.e. '% '
6. press Esc.

#### Color On ####
````
cd ~
vim .vimrc
````
Add:
````
syntax on
````

#### Set Tab 4 Spaces ####
````
cd ~
vim .vimrc
````
Add:
````
set tabstop=4
````
#### Replace Words ####
Source: http://vim.wikia.com/wiki/Search_and_replace
````
:%s/foo/bar/g
````
Find each occurrence of 'foo' (in all lines), and replace it with 'bar'.
````
:s/foo/bar/g
````
Find each occurrence of 'foo' (in the current line only), and replace it with 'bar'.
````
:%s/foo/bar/gc
````
Change each 'foo' to 'bar', but ask for confirmation first.
````
:%s/\<foo\>/bar/gc
````
Change only whole words exactly matching 'foo' to 'bar'; ask for confirmation.
````
:%s/foo/bar/gci
````
Change each 'foo' (case insensitive due to the i flag) to 'bar'; ask for confirmation.
