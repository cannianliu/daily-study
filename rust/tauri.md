# Tarui

tauri 是一个用于构建使用 web 技术的轻量级、高性能的桌面应用程序框架，支持跨平台，由 rust 语言实现，还在积极完善中
>官方文档： https://tauri.app/start/
## 环境搭建
### 系统依赖安装

```shell
sudo apt update

sudo apt install libwebkit2gtk-4.1-dev \
  build-essential \
  curl \
  wget \
  file \
  libxdo-dev \
  libssl-dev \
  libayatana-appindicator3-dev \
  librsvg2-dev
```

> 出现依赖安装冲突的问题，可以使用 aptitude 命令

```shell
sudo apt install aptitude

# 用法和apt一样，更加智能
sudo aptitude update
sudo aptitude upgrade
sudo aptitude install packagename
```

>安装存在依赖问题的包时，先选no，知道解决方案能够把包装上，再选yes
### rust环境
### node.js

安装方法见 [[Linux Enviroment]]
## 项目创建

```shell
# 安装 tauri-cli
cargo install tauri-cli --version "^2.0.0" --locked

# 安装tauri脚手架
cargo install create-tauri-app --locked
# 开始创建app项目
cargo create-tauri-app
# 选择项目的配置
# 这里官方推荐使用以下配置开始
# Choose which language to use for your frontend: TypeScript / JavaScript
# Choose your package manager: pnpm
# Choose your UI template: Vanilla
# Choose your UI flavor: TypeScript

# 创建完成后会在下面给出运行项目的命令
# 运行按上面的选项创建的项目
cd tauri-demo
pnpm install
pnpm tauri dev
# android开发
pnpm tauri android init
pnpm tauri android dev
```

运行后出现如下界面

![[Pasted image 20241103163935.png]]







