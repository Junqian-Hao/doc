# Mac下python多版本管理 - 掘金
1、安装homebrew
------------

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

```

Homebrew安装成功后，会自动创建目录 /usr/local/Cellar 来存放Homebrew安装的程序

2、安装pyenv
---------

```bash
brew update
brew install pyenv
pyenv -v 

```

3、安装&管理多个Python
---------------

```bash
pyenv install -l  
pyenv install 2.7.15
pyenv install 3.7.3
pyenv versions 

```

4、切换版本
------

```bash
pyenv global 3.7.3 

python -V  

pyevn global system  

pyenv local 3.7.3  

python -V  

pyenv local --unset  

pyenv shell 3.7.3  

python -V  

pyenv shell --unset  

```

5、切换不成功
-------

如果遇到切换之后，Python版本还是系统的默认版本的话，就需要配置一下环境变量，在 ~/.zshrc 或 ~/.bash_profile 文件最后写入：

```bash
export PYENV_ROOT=~/.pyenv
export PATH=$PYENV_ROOT/shims:$PATH
if which pyenv > /dev/null;
  then eval "$(pyenv init -)";
fi

```

使配置生效 source ~/.bash_profile