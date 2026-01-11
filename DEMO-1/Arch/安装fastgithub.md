```bash
#创建临时目录并切换（可选，只是为了整洁）
mkdir -p ~/software && cd ~/software
#步骤 2：直接从官方 Github 下载最新版安装包
wget https://github.com/WangGithubUser/FastGitHub/releases/download/v2.1.5/fastgithub_linux-x64.zip
#或者
curl -O -L https://github.com/WangGithubUser/FastGitHub/releases/download/v2.1.5/fastgithub_linux-x64.zip
#步骤 3：解压下载好的压缩包
unzip fastgithub_linux-x64.zip
#步骤 4：进入解压目录 + 赋予执行权限
cd fastgithub_linux-x64 
chmod +x fastgithub
#步骤 5：移动程序到系统环境目录（全局可用）

把程序文件放到 `/usr/local/bin`（系统推荐的自定义软件目录，无需 sudo 也可放`~/.local/bin`），之后终端任意位置都能调用 `fastgithub` 命令：
sudo mv fastgithub /usr/local/bin/
#删除software
rm -rf ~/software
```
常用命令清单
```bash
# ✅ 启动服务（手动启动） 
sudo systemctl start fastgithub.service 
# ✅ 停止服务（优雅关闭，释放端口，无残留） 
sudo systemctl stop fastgithub.service 
# ✅ 重启服务（修改配置/网络异常时用，推荐） 
sudo systemctl restart fastgithub.service 
# ✅ 查看服务运行状态（排错首选） 
sudo systemctl status fastgithub.service 
# ✅ 设置开机自启（已配置，无需重复执行） 
sudo systemctl enable fastgithub.service 
# ✅ 取消开机自启（如需关闭开机启动） 
sudo systemctl disable fastgithub.service 
# ✅ 查看服务运行日志（排错专用，查看启动失败/运行异常原因） 
sudo journalctl -u fastgithub.service -f
```
## fastgithub
```bash
# 验证是否启动成功 
ps -aux | grep fastgithub 
# 手动停止FastGithub 
sudo pkill -9 fastgithub 
# 手动启动FastGithub 
sudo /usr/local/bin/fastgithub start > /dev/null 2>&1 &
```
