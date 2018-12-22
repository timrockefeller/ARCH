---
title: Rust Windows环境搭建
date: 2018-12-22 10:22:11
tags: [coding]
---

该文章主要讲述在windows10+vscode下如何构建一个完整的rust编译调试环境，以及过程中可能出现的问题。

## Rust构造

rust 官方提供了一系列版本、包安装管理工具，如rustup和cargo。

## 基本组件

### rustup

#### 初始化

从[rustup.rs](https://rustup.rs/)官网下载_rustup-init.exe_（其他平台同理），这个软件是一个初始配置包，会帮助你下载选择版本的rustup和cargo。

打开软件后会显示如下信息：

```powershell
Welcome to Rust!

This will download and install the official compiler for the Rust programming
language, and its package manager, Cargo.

It will add the cargo, rustc, rustup and other commands to Cargo's bin
directory, located at:

  ~\.cargo\bin

This path will then be added to your PATH environment variable by modifying the
HKEY_CURRENT_USER/Environment/PATH registry key.

You can uninstall at any time with rustup self uninstall and these changes will
be reverted.

Current installation options:

   default host triple: x86_64-pc-windows-msvc
     default toolchain: stable
  modify PATH variable: yes

1) Proceed with installation (default)
2) Customize installation
3) Cancel installation
> _
```

默认安装于**用户目录**下。

> 该软件默认检查环境变量`CARGO_HOME`、`RUSTUP_HOME`，分别为`.cargo`、`.rustup`设置目录，按需修改（需要注意的是后期包的累积可能会占用巨量的存储空间）。

安装过程中请选择第二项 **“Customize installation”** ！注意到`default host triple`中设置的是`x86_64-pc-windows-msvc`，本文所使用的调试器是GDB，故需要修改为`x86_64-pc-windows-gnu`。`default toolchain`请选择`stable`，即稳定版。`nightly`为前瞻版，'beta'为测试版，实际使用时我们仍需使用到`nightly`版，后文会提到。

下载完成后可在`path`中检查添加`.cargo/bin`以使用各种命令。

输入`rustup --version`以检查是否安装成功。

#### setting.toml

> TOML(Tom's Obvious Minimal Language) 是一个想要打败yuml的标注语言，caogo项目也有使用。

该文件位于`.rustup/`下。

- `default_host_triple `: 第一次安装时设置，只能对该文件进行修改以改变默认值。
- `default_toolchain` : 默认使用的toolchain，可由`rustup default <toolchain name>`进行修改。

#### rustup命令

以下列出rustup的部分命令：

- `> rsutup show` : 列出现在使用的和已安装的rust版本。
- `> rustup update` : 更新所有已安装版本，由于`nightly`偶尔会爆肝日更，所以谨慎更新。
- `> rustup default `: 设置将要使用的版本。
- `> rustup component <sub>` : 检查(list)、安装(add)、移除(remove)组建。

#### 加速

此处使用科大源为例，修改PATH如下：

- `RUSTUP_DIST_SERVER` : `https://mirrors.ustc.edu.cn/rust-static`
- `RUSTUP_UPDATE_ROOT` : `https://mirrors.ustc.edu.cn/rust-static/rustup`

将同时加速cargo和rust，下载慢的可以体验一下w。

### Cargo

cargo既是一个类似于npm、pip的包管理软件，又是一个像marven一样的项目框架。一个`> cargo help`可以让你懂得它有多nb。

```
build       Compile the current package
check       Analyze the current package and report errors, but don't build object files
clean       Remove the target directory
doc         Build this package's and its dependencies' documentation
new         Create a new cargo package
init        Create a new cargo package in an existing directory
run         Build and execute src/main.rs
test        Run the tests
bench       Run the benchmarks
update      Update dependencies listed in Cargo.lock
search      Search registry for crates
publish     Package and upload this package to the registry
install     Install a Rust binary. Default location is $HOME/.cargo/bin
uninstall   Uninstall a Rust binary
```

更详细的介绍……[点❤我❤看❤文](http://wiki.jikexueyuan.com/project/rust-primer/cargo-projects-manager/cargo-projects-manager.html)。

## 集成于VS Code

再此之前，先设置一遍可能用到的环境变量。

- `RUST` : 某toolchain的目录，如`%USER%\.rust\toolchains\stable-x86_64-pc-windows-gnu`。
- `RUST_SRC_PATH` : 改版本rust的源码目录，如`%RUST%\lib\rustlib\src\rust\src`，若你的`rustlib`中没有`src`，请执行`> rustup cpmponent add rust-src`。
- `RUSTBINPATH` : `%CARGO_PATH%\bin`。

好，打开VS Code!

### 插件配置

![sp181222_141230](.\imgs\sp181222_141230.png)

商店搜索前两个就是几十万人下载的工具，两个都装上吧？前者是官方的包括rls在内的综合插件。后者虽然是第三方，但功能不比官方少。

你需要在settings.json文件中选择性键入以下内容(为第三方插件配置而不是官方)：

```json
    "rust.mode": "legacy",
    "rust.cargoHomePath": "%CARGO_HOME%",
    "rust.cargoPath":"%RUSTBINPATH%\\cargo.exe",
    "rust.racerPath":"%RUSTBINPATH%\\racer.exe",
    "rust.rls":"%RUSTBINPATH%\\rls.exe",
    "rust.rustfmtPath":"%RUSTBINPATH%\\rustfmt.exe",
    "rust.rustup":"%RUSTBINPATH%\\rustup.exe",
    "rust.rustLangSrcPath": "%RUST_SRC_PATH%",
    "rust.executeCargoCommandInTerminal": true,
```

很巧妙的是官方插件会为你提示是否提供权限安装nightly，请选择允许，并把你的default toolchain设置为__nightly__。

```powershell
> rustup default nightly
> rustup update
```