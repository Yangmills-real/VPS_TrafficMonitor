# 🚥 TrafficMonitor

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![Paramiko](https://img.shields.io/badge/SSH-Paramiko-orange)
![License](https://img.shields.io/badge/License-MIT-green)

一款专为 VPS 玩家打造的轻量级桌面流量监控悬浮窗。无需登录网页后台，直接通过 SSH 实时读取服务器网卡流量。
A lightweight desktop traffic monitoring widget designed for VPS enthusiasts. No need to log into web panels; it reads real-time network interface traffic via SSH.

> ⚠️ **本工具需要 SSH 连接到你的服务器。代码里不含任何服务器信息**，首次运行会弹图形向导让你填。

---

## 功能

- 📈 实时流量：已用 / 总量 / 剩余 / 今日增量
- 🔌 SSH 长连接 + 心跳，断线自动退避重连
- 🪟 无边框、半透明、整窗拖拽
- ⌨️ 全局快捷键沉底/置顶（默认 `Ctrl+Shift+M`，可改）
- 💾 配置持久化（服务器信息存在本机，不进代码）

---

## 运行

```bash
pip install paramiko
python traffic_monitor.py
```

首次运行会弹出**配置向导**：

| 字段 | 说明 |
|---|---|
| 服务器地址 | IP 或域名 |
| SSH 端口 | 默认 22 |
| 用户名 | 登录名，如 `root` / `ubuntu` / `deploy` |
| 私钥文件 | 点「浏览…」选私钥，如 `~/.ssh/id_ed25519` |
| 网卡名 | 一般 `eth0`，不确定先填 `eth0` |
| 刷新间隔 | 秒，默认 10 |
| 全局快捷键 | **点一下输入框，直接按组合键** |

填完点「保存并运行」即可。

---

## 配置文件

自动生成，无需手动创建：

| 系统 | 路径 |
|---|---|
| Windows | `%APPDATA%\TrafficMonitor\config.json` |
| Linux | `~/.config/TrafficMonitor/config.json` |
| macOS | `~/Library/Application Support/TrafficMonitor/config.json` |

```json
{
  "host": "你的服务器IP",
  "port": 22,
  "user": "你的用户名",
  "key_file": "C:\\Users\\你\\.ssh\\id_ed25519",
  "iface": "eth0",
  "refresh_sec": 10,
  "hotkey": "ctrl+shift+m"
}
```

**重新配置**：删掉这个文件，重启程序就会再次弹出向导。

---

## 修改全局快捷键

### 方式一：向导里按键捕获

删掉 `config.json` 重开程序 → 在「全局快捷键」框点一下 → 直接按组合键 → 保存。

### 方式二：手动改配置

```json
{ "hotkey": "ctrl+alt+m" }
```

规则：全小写、`+` 连接、修饰键只能是 `ctrl`/`shift`/`alt`/`win`、主键只能是**一个字母或数字**。
`""` 表示不启用。格式不对会自动退回默认 `ctrl+shift+m`。

### ❗ 快捷键冲突怎么办

注册失败时程序会**自动降级成按键轮询**，功能照常可用。

彻底解决：换成罕见组合（如 `ctrl+alt+shift+k`），或设成 `""` 关掉，改用界面上的 `⬇` 按钮。

常见被占用：`Ctrl+Shift+1`（微信）、`Ctrl+Alt+A`（QQ）、`Win+Shift+S`（系统截图）。

---

## ⚠️ 安全提示

- **不要把 `config.json` 提交到 Git**（里面有你的服务器地址和用户名）
- 私钥文件请保持在 `~/.ssh/` 下，**不要**拷到项目目录
- 建议服务器上用**专用低权限账号**跑监控，不要直接用 root

---

## License

MIT
