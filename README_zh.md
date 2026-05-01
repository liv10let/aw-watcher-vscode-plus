# aw-watcher-vscode-plus

[English](README.md)

[ActivityWatch](https://activitywatch.net) 的 VS Code 扩展，用于追踪你在 VS Code 中使用的项目、编程语言和文件。

本 fork 新增了 **HTTP Basic Auth 支持**，可连接 nginx 反向代理保护的 ActivityWatch 服务器。

## 功能

向 ActivityWatch 发送以下数据：
- 当前项目名称
- 编程语言
- 当前文件名
- 当前 Git 分支

## 安装

### 从源码安装（本 fork）

```bash
git clone https://github.com/liv10let/aw-watcher-vscode-plus.git
cd aw-watcher-vscode-plus
npm install
npm run compile
```

然后将项目目录软链接到 VS Code 扩展目录：

```bash
# Windows（PowerShell，管理员）
New-Item -ItemType SymbolicLink -Path "$env:USERPROFILE\.vscode\extensions\aw-watcher-vscode-plus" -Target "你的项目路径\aw-watcher-vscode-plus"

# macOS / Linux
ln -s /你的项目路径/aw-watcher-vscode-plus ~/.vscode/extensions/aw-watcher-vscode-plus
```

### 从插件市场安装（原版，不支持 Basic Auth）

在 VS Code 扩展侧边栏搜索 `aw-watcher-vscode`。

## 配置

打开 VS Code 设置（Ctrl+,）搜索 `aw-watcher-vscode`，或直接编辑 `settings.json`：

```json
{
    "aw-watcher-vscode.host": "your-server.example.com",
    "aw-watcher-vscode.port": 5601,
    "aw-watcher-vscode.authUser": "你的用户名",
    "aw-watcher-vscode.authPassword": "你的密码",
    "aw-watcher-vscode.maxHeartbeatsPerSec": 1
}
```

| 配置项 | 默认值 | 说明 |
|--------|--------|------|
| `aw-watcher-vscode.host` | `""` | ActivityWatch 服务器地址，留空表示 localhost |
| `aw-watcher-vscode.port` | `0` | ActivityWatch 服务器端口，0 表示默认值（5600） |
| `aw-watcher-vscode.authUser` | `""` | HTTP Basic Auth 用户名 |
| `aw-watcher-vscode.authPassword` | `""` | HTTP Basic Auth 密码 |
| `aw-watcher-vscode.maxHeartbeatsPerSec` | `1` | 每秒最大心跳数 |

## 使用方法

1. 在 VS Code 设置中配置服务器地址和认证信息（见上方）
2. 在 VS Code 中打开任意项目
3. 扩展会自动向 ActivityWatch 服务器发送心跳事件

如果 VS Code 在 AW 服务器之前启动，使用 **Reload ActivityWatch** 命令重新连接。

## 技术说明

### Basic Auth 实现方式

`aw-client-js` 库使用原生 `fetch` API 发送 HTTP 请求。`AWClient` 构造函数接受 `auth` 选项，将其编码为 `Basic` 认证请求头：

```typescript
const clientOptions = {
    baseURL: `http://${host}:${port}`,
    auth: { username: authUser, password: authPassword }
};
this._client = new AWClient(clientName, clientOptions);
```

构造函数将 `username:password` 编码为 Base64 并存储。每个 `_get`、`_post`、`_delete` 调用都会自动附带 `Authorization: Basic <base64>` 请求头。

### 与 Python 版 watcher 的区别

与 Python 的 `aw-client` 库需要替换方法实现不同，JS 客户端在构造函数中直接支持 `auth` 选项——认证请求头会自动添加到每个请求中。

## 相关项目

- [aw-watcher-window-plus](https://github.com/liv10let/aw-watcher-window-plus) — 窗口活动监控 watcher（带 Basic Auth）
- [aw-watcher-afk-plus](https://github.com/liv10let/aw-watcher-afk-plus) — AFK watcher（带 Basic Auth + 后台守护进程）

## 许可证

MPL-2.0
