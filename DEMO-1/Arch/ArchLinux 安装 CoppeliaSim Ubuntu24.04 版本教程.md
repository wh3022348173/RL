一、手动下载 Ubuntu24.04 版本安装包

    官方下载地址（选择对应版本）
        教育版（免费）：https://downloads.coppeliarobotics.com/V4_5_1/CoppeliaSim_Edu_V4_5_1_Ubuntu24_04.tar.xz
        注：如果需要其他版本，可访问 CoppeliaSim 官网下载页，选择 Ubuntu 24.04 版本
    手动下载到本地（推荐下载到 ~/Downloads 目录）
        浏览器打开上述链接，直接下载；
        或用 fish shell 命令下载：
        fish

        cd ~/Downloads
        wget https://downloads.coppeliarobotics.com/V4_5_1/CoppeliaSim_Edu_V4_5_1_Ubuntu24_04.tar.xz

二、手动解压到指定目录（和之前路径一致）
继续使用 ${HOME}/CoppeliaSim 作为安装根目录，避免修改环境变量路径，执行以下 fish 命令：
fish
```bash
# 1. 创建安装根目录（如果已存在，跳过此步）
mkdir -p ~/CoppeliaSim

# 2. 解压压缩包到目标目录（--strip-components 1 去掉外层文件夹）
# 替换 压缩包文件名 为你实际下载的文件名，比如 CoppeliaSim_Edu_V4_5_1_Ubuntu24_04.tar.xz
tar -xf ~/Downloads/CoppeliaSim_Edu_V4_5_1_Ubuntu24_04.tar.xz -C ~/CoppeliaSim --strip-components 1

# 3. 赋予启动脚本执行权限
chmod +x ~/CoppeliaSim/coppeliaSim.sh
# 4.启动
cd ~/CoppeliaSim && ./coppeliaSim.sh
```

Linux 桌面环境会扫描两个目录下的 `.desktop` 文件，自动生成菜单快捷方式：
- **用户专属（推荐）**：`~/.local/share/applications/` → 仅当前用户可见，无需 sudo 权限
- **系统全局**：`/usr/share/applications/` → 所有用户可见，需要管理员权限
## 创建 Desktop 桌面项文件

### 步骤 1：创建并编辑 `.desktop` 文件

在 fish 终端执行以下命令，创建并打开 CoppeliaSim 的桌面项文件：
```bash
# 进入用户桌面项目录 
cd ~/.local/share/applications/ 
# 创建并编辑 coppeliasim.desktop 文件
vim coppeliasim.desktop
```
```ini
[Desktop Entry] 
Type=Application 
Name=CoppeliaSim 
Comment=Robot Simulation Software (Ubuntu24.04 Version) 
Exec=/home/lolita/CoppeliaSim/coppeliaSim.sh 
Icon=/home/lolita/CoppeliaSim/icons/CoppeliaSim.ico 
Terminal=false 
Categories=Development;Robotics;Simulation; 
Keywords=robot;simulation;coppeliasim; 
StartupWMClass=CoppeliaSim
```
