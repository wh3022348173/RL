miniconda够用
镜像源配置
# conda永久换国内源
### 方式一
```bash
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/ && conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r/ && conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2/ && conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/ && conda config --set show_channel_urls yes

```
## 方式二
```bash
# 打开conda的配置文件 
vim ~/.condarc
```
```yaml
channels: - defaults show_channel_urls: true default_channels: - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2 custom_channels: conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```
## 恢复默认
```bash
conda config --remove-key channels
```
# conda常用命令
```bash
conda activate pytorch_rl # 激活你的PyTorch环境 
conda deactivate # 退出当前环境 
conda info --envs # 查看所有conda环境 
conda clean -a -y # 清理conda缓存（释放磁盘空间，安装失败后必清） 
conda update conda -y # 更新conda本身
```
# Pip 永久换国内源
## 方法一
```bash
mkdir -p ~/.config/pip && echo -e "[global]\nindex-url = https://mirrors.aliyun.com/pypi/simple/\n[install]\ntrusted-host = mirrors.aliyun.com" > ~/.config/pip/pip.conf
```
## 方式二
```bash
# 创建pip配置目录+文件 
mkdir -p ~/.config/pip # 编辑配置文件 
vim ~/.config/pip/pip.conf
```
```ini
[global] index-url = https://pypi.tuna.tsinghua.edu.cn/simple 
[install] trusted-host = pypi.tuna.tsinghua.edu.cn
```
# pip常用命令
```bash
pip list # 查看当前环境安装的所有包 
pip install --upgrade 包名 # 更新指定包（比如pip install --upgrade gym） 
pip uninstall 包名 # 卸载包 
pip cache purge # 清理pip缓存
```