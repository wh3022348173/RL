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
#启动 FastGithub（后台静默运行，无日志、不占终端，推荐）
sudo /usr/local/bin/fastgithub & disown
#检查 FastGithub 是否运行成功（验证启动，替代无效的 systemctl 命令）
ps -aux | grep fastgithub
#停止 FastGithub（彻底关闭加速服务，最常用）
sudo pkill -9 fastgithub
#临时前台启动（调试 / 看运行日志用，不推荐日常用）
sudo /usr/local/bin/fastgithub start
#查看 FastGithub 版本（验证程序是否正常识别）
sudo /usr/local/bin/fastgithub --version
#信任 FastGithub 证书（解决浏览器访问 Github 提示「隐私错误 / 不安全」）
sudo /usr/local/bin/fastgithub cert trust
#查找 FastGithub 程序位置（确认路径无误，防止路径失效）
which fastgithub
```