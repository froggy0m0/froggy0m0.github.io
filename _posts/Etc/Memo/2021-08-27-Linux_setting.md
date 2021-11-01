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



#### bundle 설치 

<https://github.com/VundleVim/Vundle.vim>

__Install__ : PluginInstall

__remove__ : PluginClean

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
set wildmode=longest:full,full

set smarttab
set shiftwidth=4
set tabstop=4
set softtabstop=4

"접기"
"set foldmethod=indent
"set foldnestmax=1


if &term =~ '256color'
	" Disable Background Color Erase (BCE) so that color schemes
	" work properly when Vim is used inside tmux and GNU screen.
	 set t_ut=
endif

set statusline=\ %<%l:%v\ [%P]%=%a\ %h%m%r\ %F\


set backup
set backupdir=~/vim/backup "백업 파일이 저장될 경로"
set directory=~/vim/tmp	"스왑 파일이 저장될 경로"


command! Wq :wq
command! W :w

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

filetype plugin indent on 

""공통 괄호 따옴표 자동완성"
au FileType python,c inoremap " ""<left>
au FileType python,c inoremap ' ''<left>
au FileType python,c inoremap ( ()<left>
au FileType python,c inoremap [ []<left>
au FileType python,c inoremap { {}<left>
au FileType python,c inoremap {<CR> {<CR>}<ESC>O
au FileType python,c inoremap {;<CR> {<CR>};<ESC>O
"inoremap (;<CR> ();<left><left>



""Python
au FileType python map <S-F10> : !python3 %<CR>


"" C
au FileType c map <F9> :! gcc % -o %<<CR>
au FileType c map <F10> :! ./%<<CR>
au FileType c inoremap <C-o> <C-o>$<right>;


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

call vundle#end()

au filetype python :IndentGuidesEnable
"let g:indent_guides_enable_on_vim_startup = 1
let g:indent_guides_start_level = 2
let g:indent_guides_guide_size = 1
let g:indent_guides_auto_colors  =  0
hi IndentGuidesOdd  guibg=red   ctermbg=237 "16 == black
hi IndentGuidesEven guibg=green ctermbg=237

highlight Normal ctermfg=255 "https://jonasjacek.github.io/colors/"

"airline "
noremap <leader>q :bp<CR>
nnoremap <leader>w :bn<CR>

let g:airline#extensions#tabline#enabled = 1 
"let g:airline#extensions#tabline#enabled = 1 " turn on buffer list
""let g:airline_theme=''

filetype plugin indent on 

au FileType c setl ofu=ccomplete#CompleteCpp


```

#### colorscheme 

<https://github.com/tomasiser/vim-code-dark>

__color path__ : ~/.vim/colors/codedark.vim

~~__airline path__~~ : ~/.vim/bundle/vim-airline-themes/autoload/airline/themes/codedark.vim



### pwngdb

<https://github.com/pwndbg/pwndbg>

```bash
git clone https://github.com/pwndbg/pwndbg
cd pwndbg
./setup.sh
```

### ROPgadget

```
python3 -m pip install ROPgadget --user
```

~~__update-alternatives 에 3.6 넣어보고 ROPgadget을 install 해보던가 ROPgadget 명령어를 실행해보자__~~

__~~python install 전이니까 다시해보기 ~~~__

error1 `Requirement already satisfied ~~`



> sudo apt-get install python3.7		## 3.7설치 (기존 3.6.9)
>
> $ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.7 2 #3설치한python3.7을 Priority를 설정
>
> $ sudo update-alternatives --config python	##설정된거 확인
>
> $ python3 -m pip install ROPgadget --user	## 이후 ROPgadget 설치!
>

~~음 기존 python3.6.9에 문제가있는건가 ?~~

### pwntools

 <https://github.com/Gallopsled/pwntools>

```bash
apt-get update
apt-get install python3 python3-pip python3-dev git libssl-dev libffi-dev build-essential
python3 -m pip install --upgrade pip		##오류뜨면 PATH 에 경로 추가하기
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

