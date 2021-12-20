---
title: CEF3 Windows 编译 4664.110 增加H.264支持
categories: CEF3
tags:
  - CEF3
  - CEFSharp
  - C#
excerpt: 本文记录在windows平台下，编译最新版本 CEF（Chromium Version:96.0.4664.110）过程 ，包含详细的步骤和常见问题，编译完成后的 CEF 具备完整功能的 cef_sandbox.lib 和完整的多媒体功能（如常用的 MP3 MP4 FLV AVI 等）支持。
date: 2021-10-25 20:33:36
cover: "https://hbimg.huabanimg.com/e82770c95fdefcf35246432658f85d45a12a4ce75c2f-L5VcKL_fw658/format/webp"
---

# CEF3 Windows 编译 4664.110 增加 H.264 支持

本文记录在 windows 平台下，编译最新版本 CEF（Chromium Version: 96.0.4664.110）过程 ，包含详细的步骤和常见问题，编译完成后的 CEF 具备完整功能的 cef_sandbox.lib 和完整的多媒体功能（如常用的 MP3 MP4 FLV AVI 等）支持。

官网地址：[分支构建](https://bitbucket.org/chromiumembedded/cef/wiki/BranchesAndBuilding.md#markdown-header-development)，有最新的编译需求环境，请根据不同的版本分支进行选择。

## 环境准备

1、建议编译机器尽可能选择高性能的，以及足够的硬盘空间（200G 以上）；

2、腾讯云、阿里云、AWS 等 Windows Server 2019 x64 英文版。

- 也可以是稳定的 VPN，还是使用按量付费的服务器稳定。100M 带宽
- 中文编译会出现 GBK 不存在的问题。
- 下载代码的过程中可以选择配置低的
- 编译过程中 CPU 更改到 64 核。48 核足够，64 核跑不满

3、安装 [Microsoft Visual Studio 2019](https://aka.ms/vs/16/release/vs_community.exe)；

- 安装的时候选择上 MFC
- 确保安装 Windows 10 SDK 10.0.19041
- 安装完成后，在设置/应用程序管理/Window 10 SDK 右键 Modify，勾选 change 后，选中 Debug Tools for windows。单击 change

4、GIT

### 创建工作目录

```bash
c:\code\automate
c:\code\chromium_git
```

### 下载 depot_tools.zip

在[项目分支 CHROMIUM_BUILD_COMPATIBILITY](https://bitbucket.org/chromiumembedded/cef/src/4664/CHROMIUM_BUILD_COMPATIBILITY.txt) 下找到 depot_tools_checkout 的值。4664 的为 e023d44820，后续要 checkout 出来。

```Bash
cd c:\code\
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
cd depot_tools
git checkout e023d44820 # 这个值为上面 CHROMIUM_BUILD_COMPATIBILITY 中 depot_tools_checkout 对应的值
.\update_depot_tools.bat #运行 "update_depot_tools.bat" 自动安装 Python 与 Git.
git add .
git config --global user.name "name"
git config --global user.email "mail@gmail"
git commit -m "安装依赖" # 把变动提交到git暂存，否则后续更新代码的时候报错
```

### 下载 automate-git.py

从官方下载[automate-git.py](https://bitbucket.org/chromiumembedded/cef/src/4664/tools/automate/automate-git.py)文件然后放到/code/automate/目录里。这里是特指 4664 分支版本，实际上是需要根据你想编译的分支，将 cef 的具体某个分支里的/tools/automate/automate-git.py 文件拷贝到上述目录里去。
链接规则：<https://bitbucket.org/chromiumembedded/cef/src/==4664==/tools/automate/automate-git.py>。其中4664为分支名称，在右侧选择Open raw，保存到本地就可以了。

### 设置系统变量

Path 中增加 c:\code\depot_tools；并且置顶。关闭 CMD 命令行，重新打开。

### 下载 chromium 源码

⚠️ 一定不要开启 ==set GN_DEFINES=is_component_build=true== CEF 官方说明 CEF 二进制分发不支持 component_build 详细说明见 CEF [Build Notes](https://bitbucket.org/chromiumembedded/cef/wiki/BranchesAndBuilding.md) 下的 Build Notes。

Component builds are supported by 3202 branch and newer and significantly reduce link time. Add is_component_build=true to GN_DEFINES in combination with the above VS-version-specific values. Component builds cannot be used to create a CEF binary distribution. See issue [#1617](https://bitbucket.org/chromiumembedded/cef/issues/1617#comment-38074395) for details.

```bash
cd c:\code\chromium_git
set DEPOT_TOOLS_WIN_TOOLCHAIN=0
set GN_ARGUMENTS=--ide=vs2019 --sln=cef --filters=//cef/*
python ..\automate\automate-git.py --download-dir=c:\code\chromium_git --depot-tools-dir=c:\code\depot_tools --no-distrib --no-build --branch=4664
```

耐心等待代码下载完成。

### 激活 ffmpeg 内部解码器及配置

通过修改 ffmpeg 的配置文件，以支持更多的多媒体编解码，修改编解码宏为 1 即可。
配置文件路径：

- code/chromium_git/chromium/src/third_party/ffmpeg/chromium/config/Chrome/==win==/x64/config.h
  _ win 为要编译的平台
  - x64 为要编译的 CPU 架构

Config 配置文件如下：
[config.h](https://gist.github.com/neostfox/0987b1d30a384d467db3a5ba166b4dfe)
在云服务器升级服务器到 64 核 128G 或者 32 核 64G

```bash
set DEPOT_TOOLS_WIN_TOOLCHAIN=0
set GN_ARGUMENTS=--ide=vs2019 --sln=cef --filters=//cef/*
set GN_DEFINES=ffmpeg_branding=Chrome proprietary_codecs=true is_official_build=true
cd c:\code\chromium_git\chromium\src\cef
call cef_create_projects.bat # 创建工程文件
cd c:/code/chromium_git/chromium/src #编译CEF
ninja -C out/Release_GN_x86 cef
cd c:/code/chromium_git/chromium/src #编译sandbox
ninja -C out/Release_GN_x86_sandbox cef_sandbox
cd c:/code/chromium_git/chromium/src/cef/tools
.\make_distrib.bat --ninja-build --minimal
```
