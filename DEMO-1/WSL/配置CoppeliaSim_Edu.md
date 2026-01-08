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
# 更新软件源 
sudo apt update 
# 安装核心依赖（64位系统+图形库+OpenGL等） 
sudo apt install -y libgl1-mesa-glx libgl1-mesa-dri libxcb-xinerama0 libxkbcommon-x11-0 libxcb-cursor0 \ libnss3 libatk-bridge2.0-0 libgtk-3-0 libgbm1 libasound2 libegl1-mesa 
# 仅安装CoppeliaSim运行必需的依赖（无音频库） 
sudo apt install -y libgl1 libegl1 libglx0 libglvnd0 libgl1-mesa-dri \ libxcb-xinerama0 libxkbcommon-x11-0 libxcb-cursor0 \ libnss3 libatk-bridge2.0-0 libgtk-3-0 libgbm1
# （可选）若后续运行提示缺少libssl，补充安装：
 sudo apt install -y libssl3
```
### 步骤 3：解压安装包

建议将 CoppeliaSim 解压到`/opt`（系统级）或`~/opt`（用户级），这里以用户级为例（权限更友好）：
```
# 创建自定义安装目录（若不存在） 
mkdir -p ~/opt/coppeliasim 
# 解压tar.xz包到目标目录（替换文件名与实际一致，注意大小写） 
tar -xvf CoppeliaSim_Edu_V4_10_0_rev0_Ubuntu24_04.tar.xz -C ~/opt/coppeliasim --strip-components=1 # 确认解压成功（输出目录内容，能看到coppeliaSim.sh则正常） 
ls -l ~/opt/coppeliasim/
```
### 步骤 4：配置运行权限与快捷方式

#### 方式 1：直接运行（临时）
```
# 进入CoppeliaSim目录 
cd ~/opt/coppeliasim 
# 添加执行权限 
chmod +x coppeliaSim.sh 
# 启动
CoppeliaSim ./coppeliaSim.sh
```
#### 方式 2：创建全局快捷方式（永久）

为了在任意目录直接运行`coppeliasim`，创建软链接到系统`PATH`目录：
```
# 创建软链接（/usr/local/bin是系统默认PATH，无需修改环境变量） 
sudo ln -s ~/opt/coppeliasim/coppeliaSim.sh /usr/local/bin/coppeliasim 
# 赋予执行权限 
sudo chmod +x /usr/local/bin/coppeliasim
```
### 步骤 5：解决 WSL 图形显示问题（关键）

WSL 本身无图形界面，需配置 Windows 端的 X Server 来显示 CoppeliaSim 窗口：

#### 1. 安装 X Server（Windows 端）

推荐使用：

- **VcXsrv**：官网下载 [https://sourceforge.net/projects/vcxsrv/](https://sourceforge.net/projects/vcxsrv/)，安装后运行`XLaunch`，配置：
    - 勾选 “Disable access control”（关键）；
    - 其余默认，点击 “Finish”。

#### 2. 配置 WSL 的 DISPLAY 环境变量

在 WSL 中执行（临时生效，关闭终端后失效）：
```
# Windows 10/11 WSL 2 固定配置（替换<Windows_IP>为你的Windows本机IP，或用wslhost.local） 
export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0.0 
# 可选：禁用OpenGL硬件加速（若启动后黑屏/闪退） 
export LIBGL_ALWAYS_INDIRECT=1
```
**永久生效**：将上述命令写入`~/.bashrc`（或`~/.zshrc`，若用 zsh）：
```
# 编辑
bashrc nano ~/.bashrc 
# 在文件末尾添加： 
export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0.0 export LIBGL_ALWAYS_INDIRECT=1 
# 保存并生效 
source ~/.bashrc
```
### 步骤 6：验证运行

在 WSL 中执行：
```
coppeliasim
```