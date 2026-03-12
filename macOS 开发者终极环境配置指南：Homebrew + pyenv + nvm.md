# 🍏 macOS 开发者终极环境配置指南：Homebrew + pyenv + nvm

本文档旨在为 macOS 用户提供一套**优雅、纯净且互不干扰**的开发环境搭建标准流程。

**核心核心理念：** 永远不要修改 macOS 系统自带的 Python 或其他底层环境！我们将通过环境版本管理器（Version Managers）将开发环境隔离在用户目录中，彻底告别“路径混乱”与“权限报错（sudo 噩梦）”。

---

## 📋 目录
1. [准备工作](#1-准备工作)
2. [安装 Homebrew (系统级包管理器)](#2-安装-homebrew-系统级包管理器)
3. [配置 Python 环境 (pyenv)](#3-配置-python-环境-pyenv)
4. [配置 Node.js 环境 (nvm)](#4-配置-nodejs-环境-nvm)
5. [✅ 最终验证](#5--最终验证)

---

## 1. 准备工作

确保你的 macOS 已经安装了基础的命令行开发者工具。
打开 **终端 (Terminal)**，输入以下命令：

```bash
xcode-select --install

```

> **提示：** 如果系统提示“命令行工具已安装”，则可以直接进入下一步。

---

## 2. 安装 Homebrew (系统级包管理器)

[Homebrew](https://brew.sh/) 是 macOS 上无可替代的包管理神器，我们将用它来安装各种底层依赖。

在终端中执行官方一键安装脚本：

```bash
/bin/bash -c "$(curl -fsSL [https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh](https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh))"

```

> **注意：** 安装完成后，请仔细阅读终端最后的 `Next steps` 提示。通常你需要运行两行提供的命令，将 Homebrew 添加到系统路径中（特别是 Apple Silicon M系列芯片的 Mac）。

验证安装：

```bash
brew -v

```

---

## 3. 配置 Python 环境 (pyenv)

使用 `pyenv` 管理 Python 版本，可以随时切换不同版本，且所有包都安装在用户目录 `~/.pyenv` 下，绝不污染系统。

### 3.1 安装编译依赖库（关键！）

在从源码编译 Python 之前，**必须**先安装底层的 C 语言依赖库。如果不执行此步骤，后续安装 Python 时会报错（例如著名的 `_lzma` 模块缺失或 `sqlite3` 报错）。

```bash
brew install openssl readline sqlite3 xz zlib tcl-tk

```

### 3.2 安装 pyenv

```bash
brew install pyenv

```

### 3.3 配置环境变量

将 pyenv 注入到你的终端配置文件（以 macOS 默认的 `zsh` 为例）中。依次执行以下三行命令：

```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc

```

使其立即生效：

```bash
source ~/.zshrc

```

### 3.4 安装并全局切换 Python 版本

现在可以安装你需要的版本了（此处以 `3.11.8` 为例）：

```bash
# 下载并编译安装 Python 3.11.8
pyenv install 3.11.8

# 将其设置为全局默认版本
pyenv global 3.11.8

```

---

## 4. 配置 Node.js 环境 (nvm)

为了避免未来使用 `npm install -g` 全局安装包时出现权限问题，官方强烈建议使用 [nvm](https://github.com/nvm-sh/nvm) (Node Version Manager) 来安装 Node.js。

> **避坑指南：** 强烈建议使用官方脚本安装 nvm，**不要**使用 Homebrew 安装 nvm，以免环境变量冲突。

### 4.1 运行官方安装脚本

```bash
curl -o- [https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh](https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh) | bash

```

*脚本会自动下载 nvm 并将环境变量配置写入你的 `~/.zshrc` 文件中。*

### 4.2 刷新环境变量

```bash
source ~/.zshrc

```

### 4.3 安装 Node.js

使用 nvm 一键安装最新的 **LTS (长期支持版)**，它不仅稳定，还会自动为你安装好匹配的 `npm` 包管理器：

```bash
nvm install --lts

```

---

## 5. ✅ 最终验证

重新启动你的终端（或新开一个标签页），运行以下命令，检查你的“兵器库”是否已全部就绪：

```bash
# 1. 检查 Python 状态
python3 --version  # 预期输出: Python 3.11.8
which python3      # 预期路径: /Users/你的用户名/.pyenv/shims/python3

# 2. 检查 Node.js 状态
node -v            # 预期输出: v20.x.x (或最新的 LTS 版本号)
which node         # 预期路径: /Users/你的用户名/.nvm/versions/node/...

# 3. 检查 npm 状态
npm -v             # 预期输出: 10.x.x

```

🎉 **配置完成！** 现在你可以安全、自由地开始你的开发工作了（例如直接使用 `npm install -g asar` 而无需再加 `sudo`）。

```
