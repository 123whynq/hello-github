# Git & GitHub 上手笔记

本机环境已配置完成，记录在此备查。配置日期：2026-11-17

## 一、已完成的配置

| 项目 | 值 |
|---|---|
| GitHub 账号 | 123whynq |
| 邮箱（提交署名） | 357701644+123whynq@users.noreply.github.com |
| SSH 密钥 | `~/.ssh/id_ed25519`（ed25519，无口令） |
| 远程仓库 | `git@github.com:123whynq/hello-github.git` |
| git 版本 | 2.55.0.windows.3 |
| OpenSSH | 10.3p1 |

已写入的 git 全局配置：

```
user.name        = 123whynq
user.email       = 357701644+123whynq@users.noreply.github.com
init.defaultBranch = main
core.autocrlf    = true
core.sshCommand  = "C:/Windows/System32/OpenSSH/ssh.exe" -o UserKnownHostsFile="..." -o IdentityFile="..."
```

## 二、日常三步

```bash
git status                  # 看现在改了什么
git add .                   # 暂存
git commit -m "改了什么"     # 提交
git push                    # 推到 GitHub
```

## 三、本机特有的两个坑

### 坑 1：中文用户名导致 SSH 路径乱码

用户名 `商文杰` 含中文，Git 自带 bash 在把路径传给 ssh 时会按 GBK 解码，`C:\Users\商文杰\.ssh` 变成 `C:\Users\\311\314\316\304\275\334\.ssh`，导致：

- `Host key verification failed`
- `Permission denied (publickey)`
- `Could not create directory .../.ssh`

**解决办法**（已写入全局配置）：让 git 调用 Windows 原生 ssh，并显式传绝对路径：

```bash
git config --global core.sshCommand '"C:/Windows/System32/OpenSSH/ssh.exe" -o UserKnownHostsFile="C:/Users/商文杰/.ssh/known_hosts" -o IdentityFile="C:/Users/商文杰/.ssh/id_ed25519"'
```

注意：路径要用正斜杠 `/`，不要用反斜杠。

### 坑 2：直接 curl github.com 不通

本机走系统代理 `127.0.0.1:60472` 才能访问 github.com，直连报 `ERR_CONNECTION_ABORTED`。
浏览器访问 GitHub 报「无法访问此网站」时，先刷新，多半是代理抖了一下，不是账号问题。

另外 GitHub 对该出口 IP 有风控，注册/登录偶尔弹人机验证。滑动验证容易失败，**改用音频验证**（点播放器右侧喇叭，听 6 位数字）成功率更高。

## 四、验证 SSH 是否正常

```powershell
ssh -T git@github.com
```

正常返回：

```
Hi 123whynq! You've successfully authenticated, but GitHub does not provide shell access.
```

## 五、还没做的（按需再说）

- 装 `gh` CLI（可用命令行建仓库、提 PR，比网页快）
- 配 GPG 签名（提交显示 Verified 徽章）
- 多账号共存（工作号 + 个人号）
- 配 `.gitignore` 模板（Node / Python 项目常用）
