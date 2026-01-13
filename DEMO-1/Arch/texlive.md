```bash
# 管理员权限安装，包含所有核心组件，约2-3G左右，耐心等待下载 
sudo pacman -S texlive-core texlive-latexextra texlive-fontsextra texlive-bibtexextra texlive-langchinese
sudo pacman -S biber 
sudo pacman -S --needed texlive-core texlive-latexextra texlive-science texlive-publishers texlive-langchinese poppler-data
sudo pacman -Syu texlive-core texlive-latexextra texlive-xetex texlive-langchinese

sudo groupadd tex
sudo gpasswd -a lolita tex
sudo chown -R root:tex /var/lib/texmf && sudo chmod -R g+w /var/lib/texmf
newgrp tex
sudo fmtutil-sys --all
sudo mktexlsr && sudo texhash

```