---
title: "在 Windows 上安装 Scoop"
---

Scoop 是一个命令行下的 Windows 应用安装工具, 类似于 Linux 下的 apt-get 或者 macOS 下的 Homebrew. Scoop 通过一个简单的命令行界面, 可以帮助你快速安装软件, 而不需要打开浏览器, 下载安装包, 解压安装等繁琐的操作.

## 1. 安装 PowerShell 7

目前 Windows 自带的 PowerShell 版本较低, 我们推荐使用 PowerShell 7 以获得更好的兼容性.

打开终端, 输入以下命令安装 PowerShell 7:

```powershell
winget install --id Microsoft.Powershell --source winget
```

重启终端, 定位到 `Settings`, 在 `Startup` 部分将 `Default profile` 设置为 `PowerShell 7`, 参考 Fig.1:

![](./pwsh-settings.png)

<p align="center">
Figure 1. Set PowerShell 7 as the default terminal.
</p>

重启终端即可默认打开 PowerShell 7; 输入以下命令改变 PowerShell 7 的执行策略以允许运行脚本:

```powershell
# Set Execution Policy for CurrentUser
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser -Force

# Set Execution Policy for Process
[System.Environment]::SetEnvironmentVariable('PSExecutionPolicyPreference', 'RemoteSigned', [System.EnvironmentVariableTarget]::User)
```

更改后, 重启终端以使设置生效.

## 2. 安装 Scoop 包管理器

在终端中运行以下命令以安装 Scoop:

```powershell
irm get.scoop.sh -outfile 'install.ps1'  # Download the install script
.\install.ps1  # Run the script
Remove-Item install.ps1  # Clean up the script
```
