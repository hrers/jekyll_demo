---
layout: post
title:  "IDEA vimrc 配置II"
date:   2022-10-23 16:50:34 +0800
categories: vim
---
* 目录
{:toc #markdown-toc}
``` properties
#好像是action需要用
let mapleader = ","
#映射
#Idea action 映射
#go to somewhere (g in normal mode for goto somewhere)
nnoremap ga :<C-u>action GotoAction<CR>
nnoremap gb :<C-u>action JumpToLastChange<CR>
nnoremap gc :<C-u>action GotoClass<CR>
nnoremap gd :<C-u>action GotoDeclaration<CR>
nnoremap gs :<C-u>action GotoSuperMethod<CR>
nnoremap gi :<C-u>action GotoImplementation<CR>
#频率高，定义好按的
nnoremap ff :<C-u>action GotoFile<CR>
nnoremap gm :<C-u>action GotoSymbol<CR>
nnoremap gu :<C-u>action ShowUsages<CR>
nnoremap gt :<C-u>action GotoTest<CR>
nnoremap gp :<C-u>action FindInPath<CR>
nnoremap gr :<C-u>action RecentFiles<CR>
nnoremap gh :<C-u>action Back<CR>
nnoremap gl :<C-u>action Forward<CR>

#打开maven视图 这样不行leader不清楚什么原因有时间可以研究一下
#nnoremap mv :<C-u>action ActivateMavenToolWindow<CR>
#nnoremap mv :<C-u>action ActivateMavenToolWindow<CR>
#显示git提交注解
nnoremap ta :<C-u>action Annotate<CR>
#打断点
nnoremap tb :<C-u>action ToggleLineBreakpoint<CR>
#打书签
nnoremap tm :<C-u>action ToggleBookmark<CR>
#切换窗口为项目目录
nnoremap tp :<C-u>action ActivateProjectToolWindow<CR>
#打开Terminal窗口
nnoremap tt :<C-u>action ActivateTerminalToolWindow<CR>
#打开Services窗口
nnoremap ts :<C-u>action ActivateServicesToolWindow<CR>
#打开Run窗口
# nnoremap tr :<C-u>action ActivateRunToolWindow<CR>
#打开structure窗口
nnoremap tr :<C-u>action ActivateStructureToolWindow<CR>
#切换分割窗口
nnoremap tw :<C-u>action NextSplitter<CR>
#Debug
nnoremap td :<C-u>action Debug<CR>
#Run
nnoremap tr :<C-u>action Run<CR>
#


#插入模式移动光标(暂时不适用，使用的是IDEA自定义快加减使用Alt+HJKL方式，同时可以上下下选择补全信息)
#Insert mode shortcut
#inoremap
#inoremap
#inoremap
#inoremap

#切换tab
nnoremap K gt
nnoremap J gT

#关闭窗口 映射Idea快捷键
#MacOs为shift+esc
#Win10 还不清楚

#所有的关闭操作
#==================================================
#关闭当前内容(tab)
nnoremap cc :<C-u>action CloseContent<CR>#
```