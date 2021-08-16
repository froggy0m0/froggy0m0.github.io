---
layout: post
title: .vimrc
categories: Memo
message: .vimrc 설정
---

```vi

syntax on
set title
set fileencodings=utf8,euc-kr
set number
set relativenumber
set numberwidth=8
set laststatus=2
set ruler
set background=dark
colorscheme desert  "pablo


set hlsearch
set incsearch
set ignorecase
"set smartcase

set autoindent
set cindent
set smartindent


set showmatch
set showcmd
set cursorline
set wildmenu

set smarttab
set shiftwidth=4
set tabstop=4
set softtabstop=4

set foldmethod=indent
set foldnestmax=1


set statusline=\ %<%l:%v\ [%P]%=%a\ %h%m%r\ %F\

map <F9> :! gcc % -o %<<CR>
map <F10> :! ./%<<CR>
map <F12> :wq<CR>
map! <F12> <ESC><F12>



"파일 수정시 마지막 편집 위치로"
au BufReadPost *
\ if line("'\"") > 0 && line("'\"") <= line("$") |
\ exe "norm g`\"" |
\ endif

"괄호 따옴표 자동완성"
inoremap " ""<left>
inoremap ' ''<left>
inoremap ( ()<left>
inoremap [ []<left>
inoremap { {}<left>
inoremap {<CR> {<CR>}<ESC>O
inoremap {;<CR> {<CR>};<ESC>O
inoremap (;<CR> ();<left><left>
```

