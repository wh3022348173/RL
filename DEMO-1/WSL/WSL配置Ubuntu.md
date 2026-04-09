### 不同盘符装ubuntu
```bash
# 升级 WSL 到最新版 
wsl --update 
# 重启 WSL 生效 
wsl --shutdown
# 创建自定义安装目录 
mkdir D:\WSL\Ubuntu
# 安装Ubuntu 24.04（先临时装到C盘默认位置） 
wsl --install -d Ubuntu-24.04
# 重启 WSL 生效 
wsl --shutdown
# 导出镜像到D盘（生成Ubuntu2504.tar文件） 
wsl --export Ubuntu-24.04 D:\WSL\Ubuntu.tar
# 注销 C 盘的默认 Ubuntu 24.04
wsl --unregister Ubuntu-24.04
# 导入到D盘，指定WSL2版本（关键） 
wsl --import Ubuntu-25.04 D:\WSL\Ubuntu D:\WSL\Ubuntu.tar --version 2
```
###  设置 Ubuntu 24.04 默认用户为 root（两种方法）
#### 方法1
```bash
# 格式：wsl --distribution 发行版名称 --user 用户名 
wsl --distribution Ubuntu-24.04 --user root
```
#### 方法2
```bash
# 在 WSL 终端内创建 / 修改 `wsl.conf` 配置文件
echo -e "[user]\ndefault=root" > /etc/wsl.conf
# 重启 WSL 生效 
wsl --shutdown
```
### 配置强化学习环境
1. 安装miniconda
[https://docs.anaconda.com/miniconda/install/](https://docs.anaconda.com/miniconda/install/)
```bash
curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash ~/Miniconda3-latest-Linux-x86_64.sh
```
 miniconda更换清华源
 [https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)

若在最后安装自定义过安装路径，则修改路径下的`.condarc`​文件
编辑`${PATH_TO_INSTALL_PATH}/.condarc`​
```
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
# 随后清除缓存  
conda clean -i
```
2. 创建conda虚拟环境并激活
```bash
# 创建python3.9虚拟环境，命名为deep_rl
conda create -n deep_rl python=3.9

# 后续使用该环境需要先激活
conda activate deep_rl

# 退出虚拟环境
conda deactivate 
```
3. 安装cudatoolkit
[https://developer.nvidia.com/cuda-12-1-1-download-archive?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_local](https://developer.nvidia.com/cuda-12-1-1-download-archive?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_local)
![[Pasted image 20251231122704.png|500]]

```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/13.1.0/local_installers/cuda-repo-wsl-ubuntu-13-1-local_13.1.0-1_amd64.deb
sudo dpkg -i cuda-repo-wsl-ubuntu-13-1-local_13.1.0-1_amd64.deb
sudo cp /var/cuda-repo-wsl-ubuntu-13-1-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cuda-toolkit-13-1
```
添加环境变量
```
# >>> cuda config >>>
# add nvcc compiler to path
export PATH=$PATH:/usr/local/cuda/bin
# add cuBLAS, cuSPARSE, cuRAND, cuSOLVER, cuFFT to path
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64
# <<< cuda config <<<
```
4. 安装pytorch
![[Pasted image 20251231122801.png|500]]
```bash
# 这里以cuda 13.0版本安装为例，需要更换为自己设备cuda的版本
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
```
5. 验证
```python
import torch # 如果pytorch安装成功即可导入
print(torch.cuda.is_available()) # 查看CUDA是否可用
print(torch.cuda.device_count()) # 查看可用的CUDA数量
print(torch.version.cuda) # 查看CUDA的版本号
```
