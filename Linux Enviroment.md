# Linux

> 系统：Ubuntu 24.04
> 推荐先安装 gcc 等编译工具 sudo apt install build-essential
## ZSH 配置
zsh 是一个类bash的shell，比bash更加强大，功能更加丰富，配置比较复杂

通常与Oh My Zsh配合使用，方便管理插件和配置

内置了许多主题，可以挑个自己喜欢的，或者设为 random，`zsh` 命令可以刷新主题

### 插件推荐

1、git ：内置插件，默认使用

2、zsh-autosuggestions：自动补全

3、zsh-syntax-highlighting：语法高光

4、z：内置插件，快捷跳转到之前访问过的目录 `z path`

5、extra：内置插件，方便解压文件 `x file`
### 安装命令

```shell
sudo apt update
sudo apt install zsh
sudo apt install git
# ohmyzsh 安装
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
# 插件安装
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
# 修改配置文件
vim ~/.zshrc
# plugins 改为 plugins=(git zsh-autosuggestions zsh-syntax-highlighting z extract)
# 配置生效
source ~/.zshrc
# zsh设为默认
chsh -s $(which zsh)
```

## github 配置

本地创建ssh密钥对
```shell
# 替换为自己的邮箱
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
# 一路回车
cat ～/.ssh/id_rsa.pub
# 复制公钥
```

登陆github账户，进入Settings > SSH and GPG keys，点击"New SSH key"，添加公钥和注释

```shell
# 验证ssh是否添加成功
ssh -T git@github.com

# 正常输出如下：
# Hi cannianliu! You've successfully authenticated, but GitHub does not provide shell access.
```

## Rust 安装
参考 https://course.rs/first-try/installation.html

```shell
# 通过rustup安装rust
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
# 验证是否安装成功
rustc -V
cargo -V

# cargo官方仓库访问比较慢，使用crm自动添加镜像
cargo install crm
crm best
```
## Node.js 安装
用于 web 前端开发

```shell
# 安装 nvm (Node 版本管理器)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
# 设置镜像源
NVM_NODEJS_ORG_MIRROR="https://npmmirror.com/mirrors/node/"
# 下载并安装 Node.js（可能需要重启终端）
nvm install 22
# 验证环境中是否存在正确的 Node.js 版本
node -v # 应该打印 `v22.11.0`
# 验证环境中是否存在正确的 npm 版本
npm -v # 应该打印 `10.9.0
# 启用如pnpm的其他包管理器
corepack enable

# 设置淘宝镜像源
pnpm config set registry https://registry.npmmirror.com
```


