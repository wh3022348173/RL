arch Linux 官方源没有 Anaconda/Miniconda，且 Anaconda 体积大（自带大量无用包），**强烈推荐安装 Miniconda3**（仅含 conda 核心 + python，体积小、无冗余），二者配置方式完全一致，Miniconda 够用 99% 的开发场景。
```bash
yay -S miniconda3
```
解决 Arch + Conda 的【base 环境自动激活】问题
```bash
conda config --set auto_activate_base false
```
解决 fish-shell 下 `conda: Unknown command` + `conda.fish 不存在` 问题
方案一：「临时生效」一键启用 conda（fish 专用，立即能用，优先做）
```bash
set PATH /opt/miniconda3/bin $PATH
```
方案二：「永久生效」一键配置 fish 环境变量
```bash
echo 'set PATH /opt/miniconda3/bin $PATH' >> ~/.config/fish/config.fish
source ~/.config/fish/config.fish
```

Miniconda 常用操作
```bash
# 1. 查看conda版本 
conda --version 
# 2. 查看所有conda虚拟环境 
conda env list 
# 3. 创建虚拟环境（示例：python3.10，环境名test_env） 
conda create -n test_env python=3.10 -y 
# 4. 激活环境（fish专用正确写法） 
source /opt/miniconda3/bin/activate test_env 
# 5. 查看当前环境的python版本（确认是conda的独立python） 
python --version 
# 6. 安装python包（示例：安装numpy） 
conda install numpy -y 
# 7. 退出当前环境 
conda deactivate 
# 8. 删除无用的虚拟环境 
conda remove -n test_env --all -y 
# 9. 更新conda本身 
conda update conda -y
```