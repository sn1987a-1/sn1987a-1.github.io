---

title: WSL2美化
comments: true
toc: true
categories:
  - WSL2
tags:
  - 美化
  - Unix
abbrlink: 15684
cover: https://user-images.githubusercontent.com/74918703/155833471-10c2dd15-9579-4516-a86b-539be619066d.png
date: 2022-1-21 22:12:20

---

#  

# WSL2美化

本文简要记录基于Windows11对WSL的终端进行美化的主要步骤。

环境：Ubuntu 20.04，Windows Terminal(WT)

主要工具和插件：zsh，oh my zsh, povwerlevel10k(powerlevel9k也可以),autozsh-autosuggestions , zsh-syntax-highlig

下图是我的美化结果。

![image-20220122222259548](https://user-images.githubusercontent.com/74918703/152648068-cc3de98c-40c3-401e-983b-a32860435f16.png)

## 对Windows terminal的外观进行美化

在微软应用商店搜索Windows terminal即可下载最新版本，如果不想用Windows Terminal，也可以下载另外一个跨平台终端——Tabby Terminal，[点击下载]([Tabby - a terminal for a more modern age](https://tabby.sh/))，配置方案也类似，但亲测效果不如WT。

### 修改默认配置

打开Windows Terminal，点击上方栏中“v”按钮，选择侧边栏中的“setting.json”文件并打开，后文中对WT的配置均对该文件进行修改（可以用VScode打开）。

例如，若要规定默认打开的界面是WSL2/WSL的界面，即可在actions一栏进行修改：

```c
 "defaultProfile": "{07b52e3e-de2c-5db4-bd2d-ba144ed6c273}",//括号内对应的序列可在setting.json文件里查找到Ubuntu对应的GUID
```

### 修改配色方案

在setting.json最后部分有schemes一栏，代表WT的配色方案，每个配色方案的name项即为名称，系统默认提供了一部分配色方案以及名称，但都不是很好看，将自定义配色添加到schemes底下即可新增配色方案，[自定义配色网站]([Windows Terminal Themes](https://windowsterminalthemes.dev/))提供了较多推荐的配色，可以直接复制。

以下为我选择的配色：

```c
{
      "name": "ChallengerDeep",
      "black": "#141228",
      "red": "#ff5458",
      "green": "#62d196",
      "yellow": "#ffb378",
      "blue": "#65b2ff",
      "purple": "#906cff",
      "cyan": "#63f2f1",
      "white": "#a6b3cc",
      "brightBlack": "#565575",
      "brightRed": "#ff8080",
      "brightGreen": "#95ffa4",
      "brightYellow": "#ffe9aa",
      "brightBlue": "#91ddff",
      "brightPurple": "#c991e1",
      "brightCyan": "#aaffe4",
      "brightWhite": "#cbe3e7",
      "background": "#1e1c31",
      "foreground": "#cbe1e7",
      "selectionBackground": "#cbe1e7",
      "cursorColor": "#fbfcfc"
    },
```

添加完配色方案后，还应该对profiles部分进行修改，以便于使用最新配色方案。如果只需要对虚拟机部分添加如下文本，可以只修改name为“Ubuntu-xx.xx”的部分（当然其他部分也只是复制粘贴）。

```c
    "colorScheme": "ChallengerDeep",
```



### 修改字体

Windows原装字体不支持很多符号的显示，这里推荐修改默认字体

比较简单的，可以在微软官方[下载]([Windows ターミナル Cascadia Code | Microsoft Docs](https://docs.microsoft.com/ja-jp/windows/terminal/cascadia-code))Cascadia Code PL字体，或者是下载文件后右键单击该字体对应的.otf/.ttf文件并选择安装。

当然有功能更加强大，应用更加广泛的字体Nerd Fond(Hack Nerd Fond)，包含了更多字符库，[点击下载]([ryanoasis/nerd-fonts: Iconic font aggregator, collection, & patcher. 3,600+ icons, 50+ patched fonts: Hack, Source Code Pro, more. Glyph collections: Font Awesome, Material Design Icons, Octicons, & more (github.com)]( https://github.com/ryanoasis/nerd-fonts ))。

安装完成后，同样在profiles目录的Ubuntu-xx.xx里修改：

```c
     "fontFace": "Hack Nerd Fond"
     "fontSize": 10,
```

### 设置背景和透明效果

均是在perfiles目录内

添加背景图：

```c
     "backgroundImage": "E:\\wallpaper\\wp3.jpg",//背景的地址
```

添加透明效果（0~1，越小表示越透明）

```c
     "acrylicOpacity": 0.8,
     "useAcrylic": true
```

指定启动时的默认路径：

```c
		"startingDirectory": "./",
```

这样Windows Terminal基本就配置好了。



## 美化WSL2

步骤：

- 将原有的shell替换为zsh
- 安装oh my zsh
- 关键字高亮以及自动填充插件
- 安装powerlevel10k

安装完自动填充以及高亮插件后对文件zshrc进行的主要添加为：

```c
source /usr/share/zsh-autosuggestions/zsh-autosuggestions.zsh
source /usr/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```



这里主要从安装powerlevel10k开始记录。

在命令行输入：

```c
sudo vim ~/.zshrc
```

打开配置文件后，找到THEME一行，修改为：

```c
ZSH_THEME="powerlevel10k/powerlevel10k"
```

之后重启或输入命令

```c
p10k configure
```

之后填写弹出的问卷即可自定义并保存当前配置文件。如果需要使用其他路径的文件，可以使用source命令进行导入。
