---
layout: post
title: 리눅스 셋팅 
categories: Memo
message: Ubuntu 18.04
---

## Linux Setting

### vim

#### vim 최신 업데이트

```bash
sudo apt-get update
........................
........................
........................
sudo apt-get install vim
```



#### colorscheme 

<https://github.com/tomasiser/vim-code-dark>

사용한 colorshceme 경로 <https://github.com/tomasiser/vim-code-dark/blob/master/colors/codedark.vim>

__raw 로 clone (airline 안썻음) __

__path__ : ~/.vim/colors `codedark테마` 사용

#### bundle 설치 

<https://github.com/VundleVim/Vundle.vim>

```bash
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```



#### .vimrc

 update ....  21-08-27

```bash
syntax on
set title
set fileencodings=utf8,euc-kr
set number
set relativenumber
set numberwidth=8
set laststatus=2
set ruler

set background=dark
colorscheme codedark    "https://github.com/tomasiser/vim-code-dark
""colorscheme jellybeans
set t_Co=256
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
set shiftwidth=4 et
set tabstop=4
set softtabstop=4


"접기"
"set foldmethod=indent
"set foldnestmax=1


set statusline=\ %<%l:%v\ [%P]%=%a\ %h%m%r\ %F\


set backup
set backupdir=~/vim/backup "백업 파일이 저장될 경로"
set directory=~/vim/tmp	"스왑 파일이 저장될 경로"

"파일 수정시 마지막 편집 위치로"
"작동이 안될시 ~/.viminfo 소유권자를 바꿔줌"
"sudo chown froggy:froggy ~/.viminfo"
au BufReadPost *
\ if line("'\"") > 0 && line("'\"") <= line("$") |
\ exe "norm g`\"" |
\ endif

""map <F9> :! gcc % -o %<<CR>
""map <F10> :! ./%<<CR>
map <F12> :wq<CR>
map! <F12> <ESC><F12>
"map <S-F10> :python3 %


""Python
au FileType python map <S-F10> : !python3 %<CR>


"" C
au FileType c map <F9> :! gcc % -o %<<CR>
au FileType c map <F10> :! ./%<<CR>
au FileType c inoremap <C-o> <C-o>$<right>;

""괄호 따옴표 자동완성"

inoremap " ""<left>
inoremap ' ''<left>
inoremap ( ()<left>
inoremap [ []<left>
inoremap { {}<left>
inoremap {<CR> {<CR>}<ESC>O
inoremap {;<CR> {<CR>};<ESC>O
"inoremap (;<CR> ();<left><left>

"airline "
noremap <leader>q :bp<CR>
nnoremap <leader>w :bn<CR>

set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()

	Plugin 'VundleVim/Vundle.vim'
    Plugin 'nathanaelkane/vim-indent-guides'
    Plugin 'vim-airline/vim-airline'
    Plugin 'vim-airline/vim-airline-themes'

    Plugin 'Vimjas/vim-python-pep8-indent'
    ""Plugin 'hynek/vim-python-pep8-indent'
    filetype plugin indent on 

call vundle#end()

let g:indent_guides_enable_on_vim_startup = 1
let g:indent_guides_start_level = 2
let g:indent_guides_guide_size = 1
let g:indent_guides_auto_colors  =  0
hi IndentGuidesOdd  guibg=red   ctermbg=237 "16 == black
hi IndentGuidesEven guibg=green ctermbg=237

highlight Normal ctermfg=255 "https://jonasjacek.github.io/colors/"

let g:airline#extensions#tabline#enabled = 1 " turn on buffer list
""let g:airline_theme=''

```



### pwngdb

<https://github.com/pwndbg/pwndbg>

```bash
git clone https://github.com/pwndbg/pwndbg
cd pwndbg
./setup.sh
```

### pwntools

 <https://github.com/Gallopsled/pwntools>

```bash
apt-get update
apt-get install python3 python3-pip python3-dev git libssl-dev libffi-dev build-essential
python3 -m pip install --upgrade pip
python3 -m pip install --upgrade pwntools
```

#### bash

```bash
export HISTTIMEFORMAT="%Y.%m.%d  %H:%M:%S -> "
# export HISTTIMEFORMAT="%Y.%m.%d  %T -> "
```

#### core 파일 생성

```bash
ulimit -c unlimited
```

