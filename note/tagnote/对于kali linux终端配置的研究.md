---
tags:
  - 技巧
  - shell
---
由于长时间对于kali linux美化的不满，我开始零零碎碎的对kali linux的终端进行更改，但结果总是不尽人意，以下是我的一些收获。

### 提示符
首先kali那个绿色的，长的像"虾头"一样的东西叫做提示符，通过变量`PROMPT`修改
临时修改提示符：
```
PROMPT='%n@%m:%~> '
```
带颜色的：
```
PROMPT='%F{green}%n%f@%F{blue}%m%f:%F{yellow}%~%f%(!.#.>) '
```
kali的小标志
```
㉿ ⚙
```

**kali .zshrc的提示符代码**

```
PROMPT=$'%F{%(#.blue.green)}┌──${debian_chroot:+($debian_chroot)─}${VIRTUAL_ENV:+($(basename $VIRTUAL_ENV))─}(%B%F{%(#.red.blue)}%n'$prompt_symbol$'%m%b%F{%(#.blue.green)})-[%B%F{reset}%(6~.%-1~/…/%4~.%5~)%b%F{%(#.blue.green)}]\n└─%B%(#.%F{red}#.%F{blue}$)%b%F{reset} '

```

```
PROMPT=$'${debian_chroot:+($debian_chroot)}${VIRTUAL_ENV:+($(basename $VIRTUAL_ENV))}%B%F{%(#.red.blue)}%n@%m%b%F{reset}:%B%F{%(#.blue.green)}%~%b%F{reset}%(#.#.$) ' 
```

```
PROMPT=$'${debian_chroot:+($debian_chroot)}${VIRTUAL_ENV:+($(basename $VIRTUAL_ENV))}%B%F{red}%n@%m%b%F{reset}:%B%F{blue}%~%b%F{reset}%(#.#.$) '
```

**右提示符**
```
# Right-side prompt with exit codes and background processes                       #RPROMPT=$'%(?.. %? %F{red}%B⨯%b%F{reset})%(1j. %j %F{yellow}%B⚙%b%F{reset}.)'
```
在扒代码的过程中，我发现了一段被注释的代码，根据ai的说法，这个是右提示符，就是根据输入命令的状态呈现不同的符号，效果如下：
![[Pasted image 20250320102530.png]]

最后`ctrl+p`就能修改为一般的提示符:)
### 配色
kali自带的qterminal应该是不能改配色的，建议下一个`gnome-terminal`(这个可以改配色)


### 菜单栏
暂时没有什么办法
gnome tweak


