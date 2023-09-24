---
title: 离线搭建前端团队开发环境
categories: environment
tags:
  - Nexus
  - Docker
  - Nginx
  - Mirror
  - VS Build Tools
  - DNS
  - SSL

excerpt: 详细介绍在内网办公情况下，如何快速搭建一套适合前端团队的开发环境。其中包含npm包管理仓库、binary镜像、使用nvm进行nodejs版本管理、node-gyp需要的Python以及VS Build Tools 离线安装。搭建内网DNS服务器，Https证书签发。
date: 2023-09-21 20:33:36
cover: "https://th.bing.com/th/id/R.bc8546c959a84172beb6b8cddc5dd11e?rik=zK7w8yESRgPA6w&riu=http%3a%2f%2fwww.ttlsa.com%2fwp-content%2fuploads%2f2015%2f09%2fdocker-friends.png&ehk=LTCiZYbyTQQzTYgzZCANSCY05jw34tbJVQWxDgrQC5g%3d&risl=&pid=ImgRaw&r=0"
---

# 搭建思路

利用 `docker` 部署 Nexus，作`npm`Mirror, 使用 `Nginx` 搭建 NodeJs binary镜像站点。离线安装 `Microsoft Visual Studio Build Tools`。搭建内网`DNS`服务器。搭建内网 `SSL` 证书签发服务器，在内网使用 https。使用 `NVM`进行nodejs版本管理。

## 环境准备

1、Linux 服务器

2、连接公网的电脑

### 服务器配置

#### 安装 Docker

#### 部署 Nexus

#### 部署 Nginx

#### 部署 DNS服务器

#### 部署 SSL 服务器

### 客户端配置

#### NVM配置

#### VS Build Tools安装

#### NodeJs配置

#### DNS配置

#### Https配置

