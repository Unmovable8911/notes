Before we get started, create the following directory structure in your *home* directory  
```
.vim/
|- autoload/
|- backup/
|- colors/
|- plugged/
```
Create `.vimrc` in your *home* directory
# 00 Basic Settings
## 00x00 Most basic settings
The following settings are the most basic and mostly used ones, and it is highly recommended to apply them.
*Each setting has a comment precceeding it, which describes what the setting does.*
```
" Enable file type detection.
filetype on

" Turn syntax highlighting on.
syntax on

" Add numbers to each line on the left-hand side.
set number

" Set shift width to 4 spaces.
set shiftwidth=4

" Set tab width to 4 columns.
set tabstop=4

" Use space characters instead of tabs.
set expandtab

" While searching though a file incrementally highlight matching characters as you type.
set incsearch

" Show matching words during a search.
set showmatch
```
## 00x01 Highlighting cursor position
```
" Highlight cursor line underneath the cursor horizontally.
set cursorline

" Highlight cursor line underneath the cursor vertically.
set cursorcolumn
```
## 00x02 Enable auto completion
```
" Enable auto completion menu after pressing TAB.
set wildmenu

" Make wildmenu behave like similar to Bash completion.
set wildmode=list:longest

" There are certain files that we would never want to edit with Vim.
" Wildmenu will ignore files with these extensions.
set wildignore=*.docx,*.jpg,*.png,*.gif,*.pdf,*.pyc,*.exe,*.flv,*.img,*.xlsx
```
## 00x03 Uncommon settings
The following settings are solely optional, apply them accoding to your own requirement. However, you will need to enable plugins if you are going to install vim plugins.
```
" Disable compatibility with vi which can cause unexpected issues.
set nocompatible

" Enable plugins and load plugin for the detected file type.
filetype plugin on

" Load an indent file for the detected file type.
filetype indent on

" Do not save backup files.
set nobackup

" Do not let cursor scroll below or above N number of lines when scrolling.
set scrolloff=10

" Do not wrap lines. Allow long lines to extend as far as the line goes.
set nowrap

" Ignore capital letters during search.
set ignorecase

" Override the ignorecase option if searching for capital letters.
" This will allow you to search specifically for capital letters.
set smartcase

" Show partial command you type in the last line of the screen.
set showcmd

" Show the mode you are on the last line.
set showmode

" Use highlighting when doing a search.
set hlsearch

" Set the commands to save in history default number is 20.
set history=1000
```
# 01 Install plugins
There are two ways to install plugins for vim, manually or through a *plugin manager*. It is recommended to use a *plugin manager* since it's much more convenient to achieve.  
That is to say, you must install a *plugin manager* first. There are many *plugin managers* you can choose, for example, `vim-plug`, `Pathogen`, `Vundle`, `Dein.vim`, etc. Here, we are going to use `vim-plug` plugin manager.
## 01x00 Install `vim-plug`
Unix, Linux  
```
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```  
Windows  
```
iwr -useb https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim |`
    ni $HOME/vimfiles/autoload/plug.vim -Force
```
## 01x01 Edit `.vimrc`
Add these two line to your `.vimrc`.  
```
call plug#begin('~/.vim/plugged')



call plug#end()
```  
All your *plugins* are going to be listed between these two lines.
## 01x02 Install *plugins*
