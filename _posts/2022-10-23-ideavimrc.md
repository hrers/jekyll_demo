---
layout: post
title:  "IDEA vimrc 配置"
date:   2022-10-23 16:50:34 +0800
categories: vim
---
* 目录
{:toc #markdown-toc}
```properties
""" Map leader to space ---------------------
let mapleader=" "

""" Plugins  --------------------------------
set surround
set multiple-cursors
set commentary
set argtextobj
set easymotion
set textobj-entire
set ReplaceWithRegister

""" Plugin settings -------------------------
let g:argtextobj_pairs="[:],(:),<:>"

""" Common settings -------------------------
set showmode
set so=5
set incsearch
set nu

""" Idea specific settings ------------------
set ideajoin
set ideastatusicon=gray
set idearefactormode=keep

""" Mappings --------------------------------
map <leader>f <Plug>(easymotion-s)
map <leader>e <Plug>(easymotion-f)

map <leader>d <Action>(Debug)
map <leader>r <Action>(RenameElement)
map <leader>c <Action>(Stop)
map <leader>z <Action>(ToggleDistractionFreeMode)

map <leader>s <Action>(SelectInProjectView)
map <leader>a <Action>(Annotate)
map <leader>h <Action>(Vcs.ShowTabbedFileHistory)
map <S-Space> <Action>(GotoNextError)

map <leader>b <Action>(ToggleLineBreakpoint)
map <leader>o <Action>(FileStructurePopup)

    " built in search looks better
nnoremap / :action Find<CR>
" but preserve ideavim search
nnoremap <Space>/ /

nnoremap <Space>gc :action GotoClass<CR>
nnoremap <Space>ga :action GotoAction<CR>

nnoremap <Space>fp :action ShowFilePath<CR>
nnoremap <Space>pm :action ShowPopupMenu<CR>
```

