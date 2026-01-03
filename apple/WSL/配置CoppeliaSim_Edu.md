### 步骤 1：进入 WSL 并定位安装包

打开 Windows 终端（或 PowerShell），进入 WSL Ubuntu 环境，然后导航到安装包所在目录：
```
# 进入WSL Ubuntu 
wsl 
# 导航到Windows D盘的PACKAGE文件夹（WSL中路径为/mnt/d/PACKAGE） 
cd /mnt/d/PACKAGE 
# 确认文件存在（输出文件列表，能看到CoppeliaSim的tar.xz文件则正常） 
ls -l
```
### 步骤 2：安装依赖（Ubuntu 24.04 必需）

CoppeliaSim 依赖部分系统库，先安装缺失的依赖：
```
# 更新软件源 sudo apt update # 安装核心依赖（64位系统+图形库+OpenGL等） sudo apt install -y libgl1-mesa-glx libgl1-mesa-dri libxcb-xinerama0 libxkbcommon-x11-0 libxcb-cursor0 \ libnss3 libatk-bridge2.0-0 libgtk-3-0 libgbm1 libasound2 libegl1-mesa # （可选）若后续运行提示缺少libssl，补充安装： sudo apt install -y libssl3
```