---
title: "Write_vimrc"
date: 2021-06-16T20:19:42+08:00
categories:
- category
- subcategory
tags:
- tag1
- tag2
keywords:
- tech
#thumbnailImage: //example.com/image.jpg
---

```
set number
set autowrite "auto save
"set tabstop=4 "how many spaces should be inserted in a tab

"a tab will be replaced by 1 or more space, usually use with tabstop
"set expandtab

set mouse=a "enable mouse-reporting function at every mode
set hlsearch
set clipboard=unnamed

"insert indent automatically according to the language
"set cindent

"an indent should be with 4 spaces
"set shiftwidth=4

"let mapleader="\<Space>"
" line highlight
highlight LineNr ctermfg=grey ctermbg=White
hi CursorLine cterm=none ctermbg=DarkMagenta ctermfg=White
syntax on

"---set for powerline
set laststatus=2
let g:airline_powerline_fonts = 1 "enable powerline-fonts

"set for solarized
syntax enable
set background=dark
set t_Co=16
let g:solarized_termcolors=256
colorscheme solarized

"ctags can search tags upwards to the root folder
set tags=./tags,./TAGS,tags;~,TAGS;~

"---start the setting of vundle
set nocompatible              " be iMproved, required(disable vi support)
filetype off                  " required

"set the runtime path to include Vundle and initialize
set rtp+=~/.vim/pack/fugitive/start
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()

Plugin 'gmarik/Vundle.vim'
Plugin 'scrooloose/nerdtree'
Plugin 'vim-airline/vim-airline'
Plugin 'vim-airline/vim-airline-themes'
Plugin 'altercation/vim-colors-solarized'
Plugin 'majutsushi/tagbar'
Plugin 'chazy/cscope_maps'
Plugin 'universal-ctags/ctags'


call vundle#end()            " required
filetype plugin indent on    " required
"---end the setting of vundle

" Add_cscope_out()
" When a user use vim to open any file,
" this function will be called automatically.
" If user ran 'cscope -Rbq' before, then the
" cscope.out will be added right away.
"
" Things that the function does:
" 1. find the folder named 'cscope'
" 2. if the folder is non empty, we add cscope.out into vim
function! Add_cscope_out()
	let cscope_folder=finddir("cscope", ".;")
	if !empty(cscope_folder)
		let cscope_out=cscope_folder."/cscope.out"
		if !empty(cscope_out) && filereadable(cscope_out)
			exe "cs add " cscope_out
		endif
	endif
endfunction

" Create_cscope_out()
" Triggered by a user when he/she opens any file
" and presses F7.
" The function will:
" 1. run a script to create cscope.in and cscope.out
"    files in a folder named 'cscope'
" 2. run Add_cscope_out()
function! Create_cscope_out()
	exe "! sh /Users/ireneluo/mm/Run_cscope.sh"
	"exe echo \"create cscope db\""
	"exe echo \"add cscope.out\""
	call Add_cscope_out()
endfunction

"if already installed cscope,
"add cscope.out automatically
if has("cscope")
	silent call Add_cscope_out()
endif


"todo: 
" 1. use dynamic path to place cscope file
" refine the if-else section


"---hotkey
"----------for NERDTree-------------
nmap <silent> <F9> :NERDTreeToggle<CR>
nmap <silent> <F8> :TagbarToggle<CR>
"----------for Cscope-------------
nmap <silent> <F7> :call Create_cscope_out()<CR>
nmap zs :cs find s <C-R>=expand("<cword>")<CR><CR>
nmap zg :cs find g <C-R>=expand("<cword>")<CR><CR>
nmap zc :cs find c <C-R>=expand("<cword>")<CR><CR>
nmap zt :cs find t <C-R>=expand("<cword>")<CR><CR>
nmap ze :cs find e <C-R>=expand("<cword>")<CR><CR>
nmap zf :cs find f <C-R>=expand("<cfile>")<CR><CR>
nmap zi :cs find i <C-R>=expand("<cfile>")<CR><CR>
nmap zd :cs find d <C-R>=expand("<cword>")<CR><CR>
```

