fish = Friendly Interactive Shell
```bash
#官方源安装
sudo pacman -S fish --noconfirm
#安装最新开发版
yay -S fish-git --noconfirm


# 将 fish 设置为【默认终端 shell】
# 步骤 1：将 fish 加入系统合法 shell 列表
echo /usr/bin/fish | sudo tee -a /etc/shells
# 步骤 2：执行 chsh 命令修改默认 shell
chsh -s /usr/bin/fish

# ~/.config/fish/config.fish
~/.config/fish/config.fish
```

命令
```bash
# 1. 查看fish版本 
fish --version 
# 2. 查看当前shell 
echo $SHELL 
# 3. 加载配置文件（立即生效） 
source ~/.config/fish/config.fish 
# 4. 插件管理（fisher） 
fisher install 插件地址 
# 安装插件 
fisher uninstall 插件地址 
# 卸载插件 
fisher update 
# 更新所有插件 
fisher list 
# 查看已装插件 

# 5. 主题配置 
tide configure # 配置tide主题（图形化界面） 
# 6. 目录瞬移（z插件） 
z 目录关键词 # 比如 z doc → 跳转到~/Documents 
# 7. 查看fish帮助文档（超级详细） 
fish help
```