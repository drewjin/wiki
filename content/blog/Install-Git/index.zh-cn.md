---
title: "安装 Git"
---

## 1. 在 Linux 上安装 Git

以 Debian/Ubuntu 为例：

```bash
sudo apt install git
```

## 2. 在 Windows 上安装 Git

安装 Scoop (参考: [在 Windows 上安装 Scoop](https://shusct.github.io/wiki/blog/install-scoop-on-windows/)) 后, 在终端中运行以下命令以安装 Git:

```powershell
scoop install main/git
```

安装完成后, 输入以下命令:

```bash
git bash
```

此时会额外弹出一个 PuTTY 窗口 (即 Git Bash); 由于这个 [Issue](https://github.com/gitextensions/gitextensions/issues/5073) 尚未被解决, **我们强烈建议在这个窗口中使用 Git 命令 (而非 PowerShell)**.