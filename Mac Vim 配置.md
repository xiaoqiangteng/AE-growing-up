# Mac Vim 配置

## Vim安装
`$ brew install vim`

## Vim 升级
这里有个Vim的[版本列表]{https://github.com/vim/vim/releases}，直接下载后解压即可。

cd到vim-8.0.XXX

设置编译参数，最后一个/usr/local是安装目录，可以改。

`./configure --with-features=huge --enable-pythoninterp=yes  --enable-cscope --enable-fontset --enable-perlinterp --enable-rubyinterp --with-python-config-dir=/usr/lib/python2.7/config --prefix=/usr/local`

编译和安装:

`$ make && make install`

这个时候Vim8.0已经安装好了，但是并没有取代内置的Vim7.3，需要设置别名。

`$ vim ~/.bash_profile // 如果用的是zsh就是 vim ~/.zshrc`

加入下面这行配置，这个和你的安装目录有关。

`$ alias vim='/usr/local/bin/vim'`

然后source一下让配置生效。

`$ source ~/.bash_profile`

这个时候就安装好了，并且取代了自带的Vim。

备注：

设置一下git的编辑器，否则git commit会失败。

`$ git config --global core.editor /usr/local/bin/vim`

## spf13-vim 安装
官网：

    https://github.com/spf13/spf13-vim

Requires Git 1.7+ and Vim 7.3+

    curl https://j.mp/spf13-vim3 -L > spf13-vim.sh && sh spf13-vim.sh
    
If you have a bash-compatible shell you can run the script directly:

    sh <(curl https://j.mp/spf13-vim3 -L)
    
通过官方文档，所有的用户自定义设置，都无需在vimrc中修改。

换句话说，不建议用户直接修改~/.vimrc文件。

如果要想自定义用户设置，可以通过创建~/.vimrc.local 和~/.gvimrc.local文件来实现。

显然前者用于设定普通vim，后者用于设定gvim。

将你个性化的配置信息写入文件，就可以覆盖掉spf13的默认配置。

## spf13-vim 快捷键

### Vim的多窗口模式管理

(1) vim中，默认的多窗口打开，是横向分割窗口。

进入vim编辑器以后，可以通过new命令，新建一个子窗口

    ：new  “新建一个未命名窗口

    ：new name "新建一个名为name的文

进入vim以后，也可以通过splite来进行横向窗口分

    :split name "在当前位置打开name文件 将原来文件向下移动

    :sp name "是splite的缩写 与split功能相同

进入vim后，也可以通过vsplit进行窗口纵向分割

    :vsplit name "在当前位置打开name文件 将原来文件向右移动

    :vs name "是vsplite的缩写 与vsplit功能相同
    
(2) Vim中的多窗口切换

vim中的多窗口切换可以通过Ctrl+w组合键加上方向选择键来实现

横向分割的窗口可以通过Ctrl+w+j/k或者Ctrl+w+(UP)/(DOWN)来实现上下窗口间切换

纵向分割的窗口可以通过Ctrl+w+h/l或者Ctrl+w+(LEFT)/(RIGHT)来实现上下窗口间切换

连续两次按下Ctrl+w也组合键可以实现多个窗口间的依次切换

(3) Vim中多窗口的调整

窗口的调整可以分为绝对值调整和增量式调整

绝对值调整如下：

    :resize num "将窗口的高度调整为num行

    :res num "resize的缩写模式 与resize实现同样的功能 　　

    :vertical resize num "将窗口的宽度调整为num列

    :verticalres num "verticalresize的缩写模式 与verticalresize实现同样的功能

增量式调整如下:

    :resize+num "将窗口的高度增加num行

    :resize-num "将窗口的高度减少num行

    :vertical resize+num "将窗口的宽度增加num列

    :vertical resize-num "将窗口的宽度减少num列

将resize简写为res能实现同样的功能。

(4) Vim中多窗口的关闭

vim中多窗口的关闭与单个窗口的关闭基本相似

    :q "退出当前所在的窗口

    :qa "退出所有的窗口


## spf13-vim 插件使用

### NERDTree
    	
    ,e/<C_E>，keydescriptionctrl+e打开/关闭文件浏览器j向下移动k向上移动o小写字母o，打开文件或者展开目录shift+c即大写字母C，当前选中目录作为根目录u上一层目录作为根目录:help NERDTreeNERDTree帮助手册.

### Ctags
1. 安装

    mac自带的ctags程序不是exuberant ctags， 所以使用时会出现问题，所以要重新安装一个；
    
    $ brew install exuberant ctags

2. 安装完， which ctags

    如果是/usr/bin/ctags,系统默认先看到我们安装的ctags

    打开~/根目录下的.profile，如果你也没发现有这个文件，没关系，创建一个！

    然后在里面添加：export PATH="/usr/local/bin:/usr/local/sbin:$PATH"

    再到终端执行：source ~/.profile

    然后再看看which ctags，如无意外，应该是/usr/local/bin/ctags

    最后在.vimrc配置文件添加：
    
    let Tlist_Ctags_Cmd="/usr/local/bin/ctags"
    
    let Tlist_Show_One_File=1

    let Tlist_Exit_OnlyWindow=1

    let Tlist_Use_Right_Window=1
    
3. 使用ctags编译项目tags文件

    终端cd 项目目录，然后执行：

    $ ctags -R

    你会发现目录中多了一个tags的文件，这个就是vim里面taglist会寻找的文件！

    在vim中对准某个对象调用的方法按control + ] 看看能否调到那个方法的定义！？

    control + t  返回原方法
    
### YouCompleteMe

1. 通过 Vundle 来安装 YCM（官方推荐）

在 vim 的配置文件 ~/.vimrc 中添加一行（在call vundle#begin() 和 call vundle#end() 之间）

    call vundle#begin()
    . . . 
    Plugin 'Valloric/YouCompleteMe’
    . . .
    call vundle#end()
    
然后保存运行 vim 命令 :PluginInstall 安装 需要特别注意的是这个时候可能等的时间会相当的长

2. 通过 Git 安装 YCM

如果你跟老夫一样，等待 Vundle 安装 YCM 等了好久终于貌似好像成功了，打开 vim 却发现 YouCompleteme unavailable : no module named future （当然没有遇到算你运气好），那么你应该考虑一下换用 Git 来安装 YCM：

    # 下载 （在 `～/.vim/bundle` 目录下）
    $ git clone --recursive [https://github.com/Valloric/YouCompleteMe.git](https://github.com/Valloric/YouCompleteMe.git)
    # 检查完整性（在 `～/.vim/bundle/YouCompleteMe` 目录下）
    $ git submodule update --init --recursive
    
3. 带 C-family languages 语义支持的版本

    cd ~/.vim/bundle/YouCompleteMe
    
    ./install.sh --clang-completer
    
4. 不带 C-family languages 语义支持的版本

    cd ~/.vim/bundle/YouCompleteMe
    
    ./install.sh --clang-completer
    
5. 带 C# 语义支持的版本

    cd ~/.vim/bundle/YouCompleteMe
    
    ./install.sh --omnisharp-completer
    
6. 带 Go 语言语义支持的版本

    cd ~/.vim/bundle/YouCompleteMe
    
    ./install.sh --gocode-completer
    
7. 配置 YCM

在.vimrc中添加配置

    " 自动补全配置
    set completeopt=longest,menu    "让Vim的补全菜单行为与一般IDE一致(参考VimTip1228)
    autocmd InsertLeave * if pumvisible() == 0|pclose|endif    "离开插入模式后自动关闭预览窗口
    inoremap <expr> <CR>       pumvisible() ? "\<C-y>" : "\<CR>"    "回车即选中当前项
    "上下左右键的行为 会显示其他信息
    inoremap <expr> <Down>     pumvisible() ? "\<C-n>" : "\<Down>"
    inoremap <expr> <Up>       pumvisible() ? "\<C-p>" : "\<Up>"
    inoremap <expr> <PageDown> pumvisible() ? "\<PageDown>\<C-p>\<C-n>" : "\<PageDown>"
    inoremap <expr> <PageUp>   pumvisible() ? "\<PageUp>\<C-p>\<C-n>" : "\<PageUp>"

    "youcompleteme  默认tab  s-tab 和自动补全冲突
     "let g:ycm_key_list_select_completion=['<c-n>']
    let g:ycm_key_list_select_completion = ['<Down>']
    "let g:ycm_key_list_previous_completion=['<c-p>']
    let g:ycm_key_list_previous_completion = ['<Up>']
    let g:ycm_confirm_extra_conf=0 "关闭加载.ycm_extra_conf.py提示

    let g:ycm_collect_identifiers_from_tags_files=1    " 开启 YCM 基于标签引擎
    let g:ycm_min_num_of_chars_for_completion=2    " 从第2个键入字符就开始罗列匹配项
    let g:ycm_cache_omnifunc=0    " 禁止缓存匹配项,每次都重新生成匹配项
    let g:ycm_seed_identifiers_with_syntax=1    " 语法关键字补全
    nnoremap <F5> :YcmForceCompileAndDiagnostics<CR>    "force recomile with syntastic
    "nnoremap <leader>lo :lopen<CR>    "open locationlist
    "nnoremap <leader>lc :lclose<CR>    "close locationlist
    inoremap <leader><leader> <C-x><C-o>
    "在注释输入中也能补全
    let g:ycm_complete_in_comments = 1
    "在字符串输入中也能补全
    let g:ycm_complete_in_strings = 1
    "注释和字符串中的文字也会被收入补全
    let g:ycm_collect_identifiers_from_comments_and_strings = 0

    nnoremap <leader>jd :YcmCompleter GoToDefinitionElseDeclaration<CR> " 跳转到定义处

8. 下载最新版本的libclang（3.9以上版本）

下载地址：http://llvm.org/releases/download.html

官方建议下二进制包：（别下错了）

    Clang for x86_64 Ubuntu 14.04 (.sig)
    Clang for x86_64 Ubuntu 16.04 (.sig)
    Clang for Mac OS X (.sig)
    
9. 正式编译安装ycm_core

    cd ~
    
    mkdir ycm_build
    
    cd ycm_build
    
下一步生成makefile，这一步很重要，有点复杂。

10. 如果不需要C族语言的语义支持，在ycm_build目录下执行：

cmake -G "Unix Makefiles" . ~/.vim/bundle/YouCompleteMe/third_party/ycmd/cpp

11. 如果需要C族语言的语义支持，还得分几种情况：

(1) 从llvm的官网下载了LLVM+Clang的二进制包

解压到：~/ycm_temp/llvm_root_dir

    该目录下有bin, lib, include等文件夹
    
(2) 然后执行:

    cmake -G "Unix Makefiles" -DPATH_TO_LLVM_ROOT=~/ycm_temp/llvm_root_dir . ~/.vim/bundle/YouCompleteMe/third_party/ycmd/cpp
    
12. 如果想用系统的libclang

    cmake -G "Unix Makefiles" -DUSE_SYSTEM_LIBCLANG=ON . ~/.vim/bundle/YouCompleteMe/third_party/ycmd/cpp
    
13. 如果想用自定义的libclang

    cmake -G "Unix Makefiles" -DEXTERNAL_LIBCLANG_PATH=/path/to/libclang.so . ~/.vim/bundle/YouCompleteMe/third_party/ycmd/cpp
    
    /path/to/libclang.so这部分填入你自己编译libclang的路径
    
14. 生成ycm_core

    cmake --build . --target ycm_core --config Release

至此，YouCompleteMe已经算是安装成功！

注意：这时候，ycm_build目录可以删除啦！

安装成功后，ycm_build以及ycm_temp目录都可以删除，不影响YouCompleteMe插件的使用。

15. .ycm_extra_conf.py中的配置

(1) 用命令查看库路径

    echo | clang -v -E -x c++ -
    
    结果可能如下：
    clang version 3.6.2 (tags/RELEASE_362/final)
    Target: i386-pc-linux-gnu
    Thread model: posix
    Found candidate GCC installation: /usr/lib/gcc/i686-redhat-linux/4.4.4
    Found candidate GCC installation: /usr/lib/gcc/i686-redhat-linux/4.4.7
    Found candidate GCC installation: /usr/local/bin/../lib/gcc/i686-pc-linux-gnu/4.8.1
    Selected GCC installation: /usr/local/bin/../lib/gcc/i686-pc-linux-gnu/4.8.1
    Candidate multilib: .;@m32
    Selected multilib: .;@m32
    太长了，这里省略一部分中间内容；.........表示生咯的内容
    "/usr/local/bin/clang" -cc1 -triple ......... -mstackrealign -fobjc-runtime=gcc  directory "/include"
    #include "..." search starts here: 
    这里没有显示任何东西,所以不需要包含任何路径
    #include <...> search starts here:
    这里就是需要包含的路径下面这些都是需要包含的路径
    /usr/local/bin/../lib/gcc/i686-pc-linux-gnu/4.8.1/../../../../include/c++/4.8.1
    /usr/local/bin/../lib/gcc/i686-pc-linux-gnu/4.8.1/../../../../include/c++/4.8.1/i686-pc-linux-gnu
    /usr/local/bin/../lib/gcc/i686-pc-linux-gnu/4.8.1/../../../../include/c++/4.8.1/backward
    /usr/local/include
    /usr/local/bin/../lib/clang/3.6.2/include
    /usr/include
    End of search list.
    # 1 "<stdin>"
    # 1 "<built-in>" 1
    # 1 "<built-in>" 3
    # 318 "<built-in>" 3
    # 1 "<command line>" 1
    # 1 "<built-in>" 2
    # 1 "<stdin>" 2

(2) 整理上述内容，并添加到flag中

将以上内容复制出来,修改成如下:

     '-isystem',
     '/usr/local/bin/../lib/gcc/i686-pc-linux-gnu/4.8.1/../../../../include/c++/4.8.1',
     '-isystem',
     '/usr/local/bin/../lib/gcc/i686-pc-linux-gnu/4.8.1/../../../../include/c++/4.8.1/i686-pc-linux-gnu',
     '-isystem',
     '/usr/local/bin/../lib/gcc/i686-pc-linux-gnu/4.8.1/../../../../include/c++/4.8.1/backward',
     '-isystem',
     '/usr/local/include',
     '-isystem',
     '/usr/local/bin/../lib/clang/3.6.2/include',
     '-isystem',
     '/usr/include',
     
     补全 C 语言全局函数问题(vim ~/.vimrc文件修改)
     
     默认情况下输入 ., ->, :: 之后会触发补全函数和类， 但是默认情况下是不补全全局函数的，所以 C 语言中的 printf 之类的函数就无法补全
     
     解决办法就是手动调用补全，对应的 YCM 函数是 ycm_key_invoke_completion，将其绑定到快捷键 let g:ycm_key_invoke_completion = '<C-a>'（默认是 <C-Space>）
