---
title: 重构VSCode：打造高效开发环境

date: 2025-07-20

tags:
- VSCode
- 开发工具

excerpt: "vscode深度定制，打造高效开发环境"
---

## 0.前言

VSCode是由微软开发的一款开源代码编辑器。具有跨平台、可拓展等优势。相对于Jetbrains家族的IDEs来说更为轻量，并且一个配置得当的VSCode可以进行全栈开发。

本文旨在记录VSCode配置的过程。

文章为了方便， 将直接给出 `settings.json` 的配置代码而不是在设置中进行手动点击。

所有插件都可以在插件商店中搜索，我也会给出每个插件的网址。

### 打开`settings.json` 的方法

`CTRL + ,` 打开设置界面。在右上角有一个“打开设置（json）”的图标，单机即可打开。

所有的配置文件按照 `json` 格式编写，全部写在一对大括号内，文中出现的代码不包括大括号。

## 1.基础设置

### 添加中文插件

`CTRL + Shift + x` 打开插件，搜索chinese，选择中文简体安装，选择右下角弹框中的`Change Language and Restart` 或手动重启。

插件地址：[Chinese (Simplified) (简体中文) Language Pack for Visual Studio Code - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=MS-CEINTL.vscode-language-pack-zh-hans)

### 设置字体大小于样式

由于我的显示器较大所以我设置的字体比较大，可以更具自己的习惯进行修改。

```json
"editor.fontSize": 25, //设置字体大小
"editor.fontFamily": "Monaspace Radon Frozen, monospace",
```

Monaspace字体下载连接：[Monaspace](https://monaspace.githubnext.com/)

### 设置光标样式

```json
"editor.cursorBlinking": "phase",
"editor.cursorSmoothCaretAnimation": "on",//开启光标平滑动画
"editor.cursorStyle": "line", 
```

### 设置主题

```json
"workbench.colorTheme": "Noir Vira",
```

NoirVira是一款黑色风格的主题，我十分推荐，因为是我自己开发的。

主题地址：[Noir Vira - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=NoirVira.noir-vira)

### 控制台设置

```json
"terminal.integrated.fontSize": 20, //控制台字体
"terminal.integrated.cursorBlinking": true, //开启控制台光标闪烁
```

### 折行设置

当代码的某一行过长我们希望能够自动折叠这行代码。

```json
"editor.wordWrap": "on",              // 打开自动换行
"editor.wordWrapColumn": 80,          // 控制在哪一列自动换行（可选）
"editor.wrappingIndent": "same"       // 换行后保持缩进
```

## 2.插件系统搭建

### 前端插件

- **Auto Rename Tag**：修改 HTML 标签时自动重命名闭合标签
  
  - 插件地址：[Auto Rename Tag - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag)
- **CSS Peek**：可点击 CSS class 直接跳转定义
  
  - 插件地址：[CSS Peek - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=pranaygp.vscode-css-peek)
- **Live Server**：动态预览前端页面
  
  - 插件地址：[Live Server - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)
- **Console Ninja**：在VSCode中显示console.log信息
  
  - 插件地址：[Console Ninja - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=WallabyJs.console-ninja)

### 代码增强插件

- **Prettier**：格式化代码（快捷键`Shift + ALT + f`）
  
  - 插件地址：[Prettier - Code formatter - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- **Code Spell Checker**：自动检查拼写错误
  
  - 插件地址：[Code Spell Checker - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)
- **Markdown Preview Enhanced**：markdown预览插件
  
  - 插件地址：[Markdown Preview Enhanced - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=shd101wyy.markdown-preview-enhanced)
- **TODO Highlight**：高亮TODO
  
  - 插件地址：[TODO Highlight - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight)
- **Code Runner**：在VSCode中直接运行代码
  
  - 插件地址：[Code Runner - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=formulahendry.code-runner)

### 编程语言插件

- **Python**: [Python - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  
- **JavaScript/TypeScript**：内置支持。
  
- **C/C++**：
  
  - [C/C++ Extension Pack - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools-extension-pack)
    
  - [C/C++ - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
    
- **Go**: [Go - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=golang.Go)
  
- **PHP**: [PHP - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=DEVSENSE.phptools-vscode)
  

### 美化插件

- **indent-rainbow**：然缩进具有颜色
  
  - 插件地址：[indent-rainbow - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=oderwat.indent-rainbow)
- **Material Icon Theme**：好看的文件图标
  
  - 插件地址：[Material Icon Theme - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme)
- **Trailing Spaces**：突出尾部空格
  
  - 插件地址：[Trailing Spaces - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=shardulm94.trailing-spaces)
    
- **Path Intellisense**：自动补全文件名
  
  - 插件地址：[Path Intellisense - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense)
    
  - 替换JS和TS中的路径补全，在`settings.js`中写入：
    
    ```json
    "typescript.suggest.paths": false,
    "javascript.suggest.paths": false,
    ```
    

### 动画插件

安装**VSCode Animations**插件，根据右下角提示一直点击确认就可以获得一个丝滑的VSCode。

**注意：如果重启也不能让动画设置生效请可尝试重启电脑。**

如果想给鼠标也添加动画，在settings.js中添加：

```json
"animations.CursorAnimation": true
```

## 3.远程开发于容器开发

如果你在服务器（比如 Ubuntu Server）上开发，或者在 Docker 容器里跑项目，VSCode 的远程功能非常方便。

### 远程 SSH 连接服务器

安装插件：`Remote - SSH`  
插件地址：[Remote - SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)

按下 `F1` → 输入并选择 `Remote-SSH: Connect to Host...` → 添加主机。

### 使用 Docker 容器开发

安装插件：`Dev Containers`
插件地址：[Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

按下 `F1` → 输入并选择 `Dev Containers: Attach to Running Container...` → 选择容器即可进入。

## 4.Git 工作流

VSCode 原生就支持 Git，只要你本地已经安装了 `git`，就可以直接使用。

### 初始化 Git 仓库

点击左侧菜单的 `源代码管理（分支图标）` → 点击 `初始化仓库`。

或者直接打开终端输入：

bash

复制编辑

`git init`

### 常用操作

- **提交代码**  
  修改完后点击左侧 Git 图标，填写提交信息，点击✅就能提交。
  
- **查看修改**  
  文件左边会显示绿色/红色标记，点进去可以看到修改的内容。
  
- **推送到远程仓库**
  

先绑定远程仓库地址：

bash

复制编辑

`git remote add origin git@github.com:你的用户名/你的项目.git`

然后执行：

bash

复制编辑

`git push -u origin main`

### Git 插件推荐

- **GitLens**：更详细的提交记录、作者信息查看  
  插件地址：[GitLens - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)

## 总结

VSCode 是一款高度可定制的编辑器，一个配置得当的 VSCode 足以覆盖大多数开发需求。

本文从**基础设置**、**插件体系**、**远程开发**、**Git 集成**等方面，梳理了完整的定制过程，重点突出**颜值 ✚ 效率 ✚ 多语言适配**。

你可以按需取用，也可以在此基础上打造属于你自己的开发环境。

> 工具只是手段，真正提升效率的，是你对工具的理解与掌控。
