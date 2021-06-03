---
title: "How to install Vim Plugins?"
date: 2021-06-02T12:12:31+08:00
categories:
- Linux Tools
tags:
- Vim
keywords:
- Vim Plugins
summary: "Introduce useful plugins that I use, and how to install them."
---

In this post, I am going to introduce some useful plugins that I use,
and talk about how to install vim plugins in a easiest way.

# Vim Plugins
[Vim Awesome](https://vimawesome.com/) is a great website for you to explore 
all sort of vim plugins that you can use.

Below are some vim plugins that I use, and I just recorded the ways to install
them here. See more details about them in Vim Awesome.

## 1. [Vundle](https://github.com/VundleVim/Vundle.vim): A plugin manager
A plugin manager can manage all the plugins that you installed.
For example, you can use it install or delete a plugin easily.

  1. Clone the source code of Vundle to a specific directory.
  `git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim`

  2. Config the plugin in .vimrc

   ![Setting in .vimrc](https://i.imgur.com/pZeCX64.png)

   The most important thing is to add `.vim/bundle/Vundle.vim` into vim runtimepath,
   so that vim can execute Vundle at the first time.
   Please ignore line 33 right now, I'll discuss it later.

### Vundle command

   #### 1. Install a plugin
      1. Open up .vimrc. Add `Plugin [plugin name]` between `call vundle#begin()` and `call vundle#end()`
      (like line 37-44, but you don't have to install all these things like me, just
       install the plugins that you want)
      2. Open a vim page and type the command `:PluginInstall`

   #### 2. Remove a plugin
      1. Open up .vimrc. Mark off or remove the plugin that you want to remove.
      2. Open a vim page and type the command `:PluginClean`

   #### 3. Others

      :h vundle         - more details
      :PluginList       - lists configured plugins
      :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
      :PluginSearch foo - searches for foo; append `!` to refresh local cache
      :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal

### Note:
All plugins below can be installed by using Vundle command.
If you want to install a plugin manually(without Vundle), just follow the steps below:
(I use the plugin 'Fugitive' as an example)
1. Download .zip package for the plugin
2. Create a folder `mkdir -p ~/.vim/pack/fugitive/start`
3. Unzip the package and put it in the folder in step2
4. Add the path in .vimrc `set rtp+=~/.vim/pack/fugitive/start`
Just replace the 'fugitive' by your plugin names and you'll make it.
    
## 2. [NERDTree](https://github.com/preservim/nerdtre://github.com/preservim/nerdtree): A file system explorer

   By using NERDTree, you can view whole file directories on the left side while you open any file.

   ![NERDTree](https://i.imgur.com/k4DBqTf.png)

### NERDTree command

   #### 1. Open the NERDTree

      :NERDTreeToggle

   #### 2. Set shortcuts to open NERDTree

   Here, we use Ctrl+t as a shortcut, you could use F9,F7...,etc.
   Open up your .vimrc and add

      nnoremap <C-t> :NERDTreeToggle<CR>

   #### 3. more tips ([Docs](https://github.com/preservim/nerdtree/blob/master/doc/NERDTree.txt))

   vim config settings(add in your .vimrc as well)

      let g:NERDTreeMouseMode=3  "optional: single click to open files/directoris
      let NERDTreeWinPos=1       "set NERDTree window on the left side of window

   cmd to switch between a NERDTree window and a file window

      Ctrl+w

## 3. [Ctags](https://github.com/universal-ctags/ctags): a popular tool to trace code
Ctags can help you jump to the definition of a variables and functions.
We usually use it with Cscope.

### Ctags command
  #### 1. Generate tags
  Just goto the directory that you want to start tracing code and type down this command
  in a command line:

    ctags -R *

  Recurse into directories and generate ctags file in current folder.

  After executing this command, you will see a folder named 'tags' in current folder.
  Now you can use commands below to trace your code.

  Note that if you generate the tags in root folder, vim cannot find tags when you
  are in the inner folders. Just add this in your .vimrc to tell vim to search for tags
  upward to the root folder.
  `set tags=./tags,./TAGS,tags;~,TAGS;~`

  #### 2. Shortcuts to trace code

    Ctrl-] or Ctrl-Left_MouseClick       - jump to the definition of the tag
    Ctrl-t 或是 Ctrl-Right_MouseClick    - back to the position of tag
    :ts <tag>  <RET>                     - Search for a particular tag
    :tn                                  - Go to the next definition for the last tag
    :tp                                  - Go to the previous definition for the last tag
    :ts                                  - List all of the definitions of the last tag


## 4. [Cscope](https://github.com/brookhong/cscope.vim): another great tool to trace code

Cscope is a great tool that allows you to see where variables and functions have been used.

### Cscope command
  #### 1. Generate cscope db in a target directory
  Just goto the directory that you want to start tracing code and type down this command
  in a command line:

    cscope -Rbq
       -R: Recurse directories for files
       -b: Build the cross-reference only
       -q: Build an inverted index for quick symbol searching
           (Build cscope.in.out & cscope.po.out to increate searching speed)
       -k: Kernel mode, don't use /usr/include for #include files

  Note that Cscope uses related paths but ctags uses absolute paths.

  #### 2. Advanced tips to generate cscope db[1]
  ##### 1. Generate cscope.files
  By default, Cscope only parses C, lex and yacc files(i.e .c, .h, .l, .y)
  However, we might want to focus on some specific files like python, java or
  some specific subdirectories. In this case, we can use linux `find` command
  to make a list of the files that we are interested in and save them in cscope.files.
  For example: `find . -name '*.py' > cscope.files`

  ##### 2. Generate cscope.out
  Now, it's time to ask Cscope to generate a Cscope database for us.
  `cscope -Rbqk`
  Now, you'll see these files in current directory: cscope.in.out, cscope.po.out, cscope.out

  ##### 3. Add cscope.out
  When you open a file, we have to tell vim where's the cscope.out, so that we can use
  Cscope command to trace our code in this file.
  `:cs add /direcotry/../cscope.out`

  Once you complete these steps, you can use the command below to trace your code.

  #### 3. Shortcuts to trace code
  
    ctrl+\ s    "(Symbol) List the symbols, includes definitions and function calls.
    ctrl+\ g    "(Global) Find the definition. As same as [Ctrl+]] in ctags
    ctrl+\ c    "(Caller) List functions calling this function.
    ctrl+\ d    "(Called) List functions called by this function.
    ctrl+\ t    "(Text)   Find the text strings
    ctrl+\ f    "(File)   Find the file
    ctrl+\ i    "(Include)Find the files that include the file named by current string
    ctrl+\ e    "(egrep)  Find the egrep pattern
    
 or you could use `:cs cmd` to do the same thing
    :cs help  -for more info
    :cs add   -add a new database
    :cs find  -query for something(s|g|c|d|t|f|i|e)
    :cs reset -reinit the connections

  #### 4. Generate cscope files and add cscope.out automatically [2][3][4][5]
  I wrote some functions in .vimrc to speed up the process. Basically, my policy is:

    1. When I open a file by using vim, I can press F7 to run run_cscope.sh.
       It will generate a `cscope` folder, and a cscope database.
       Next, it will add cscope.out, so that I can use Cscope command to trace any code.
    2. Next time, when I open any file, .vimrc will find the `cscope.out` in cscope
       folder and add cscope.out again.

Run_cscope.sh
```
#!/bin/sh
DIR="/Users/ting/mm/cscope"
rm -rf ${DIR}
mkdir ${DIR}
TARGET_DIR="/Users/ting/mm"
find ${TARGET_DIR} -name "*.[chxsS]" -print > ${DIR}/cscope.files
cd ${DIR}
cscope -Rbq
```
.vimrc
```
" Add_cscope_out()
" When a user use vim to open any file, this function will be called automatically.
" If user ran 'cscope -Rbq' before, then the cscope.out will be added right away.
"
" Things that the function does:
" 1. find the folder named 'cscope'(start from current folder and search upwards)
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
" Triggered by a user when he/she opens any file and presses F7.
" The function will:
" 1. run a script to create cscope.in and cscope.out
"    files in a folder named 'cscope'
" 2. run Add_cscope_out()

function! Create_cscope_out()
	exe "! sh /Users/ireneluo/mm/Run_cscope.sh"
	call Add_cscope_out()
endfunction

"if already installed cscope, add cscope.out automatically
if has("cscope")
	silent call Add_cscope_out()
endif

nmap <silent> <F7> :call Create_cscope_out()<CR>
```

## 5. [Tagbar](https://github.com/preservim/tagbar)
Tagbar shows variables and function names in a individual window while you open a source code.
![Tagbar](https://i.imgur.com/5UGALLi.png)

  ## Shortcut to enable it
    nmap <F8> :TagbarToggle<CR>

## 6. Others (will be added in the future)
1. Fugitive
2. vim surround
3. Nerd Comment

## 7. Notes
AOSP Kernel can generate tags by executing:
`make ctag cscope ARCH=arm64` (if your platform is arm64)

## 8. Reference
[1] [Using Cscope on large projects (example: the Linux kernel)](http://cscope.sourceforge.net/large_projects.html)
[2] [Scripting the vim editor](https://developer.ibm.com/technologies/linux/articles/l-vim-script-1/)
[3] [Vim Reference Manual](https://vimhelp.org/eval.txt.html#finddir%28%29)
[4] [The Vim/Cscope Tutorial](http://cscope.sourceforge.net/cscope_vim_tutorial.html)
[5] [Vim + Cscope – 綁定快捷鍵及快速配置於大型Project](https://shaynechen.gitlab.io/vim-cscope/)
