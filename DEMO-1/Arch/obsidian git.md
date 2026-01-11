## 共享文件夹
```bash
#查看当前远程链接：
git remote -v
#如果是 `https://github.com/用户名/仓库名.git` 格式，替换为 SSH 格式：
git remote set-url origin git@github.com:用户名/仓库名.git
git remote set-url origin git@github.com:wh3022348173/RL.git
#测试 SSH 连接
ssh -T git@github.com
```
**你的本地 Arch 系统第一次连接[github.com](https://github.com)的 SSH 服务，无法确认对方主机的真实性**
```bash
#一键确认 GitHub 主机密钥
ssh-keyscan github.com >> ~/.ssh/known_hosts

```