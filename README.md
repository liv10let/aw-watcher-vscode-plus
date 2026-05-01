# aw-watcher-vscode-plus

[中文版 (Chinese)](README_zh.md)

VS Code extension for [ActivityWatch](https://activitywatch.net), the free and open-source time tracker. Tracks the projects, programming languages, and files you work on in VS Code.

This fork adds **HTTP Basic Auth support** for connecting to ActivityWatch servers behind an nginx reverse proxy.

## Features

Sends the following data to ActivityWatch:
- Current project name
- Programming language
- Current file name
- Current Git branch

## Installation

### From source (this fork)

```bash
git clone https://github.com/liv10let/aw-watcher-vscode-plus.git
cd aw-watcher-vscode-plus
npm install
npm run compile
```

Then symlink the folder to your VS Code extensions directory:

```bash
# Windows (PowerShell, as Admin)
New-Item -ItemType SymbolicLink -Path "$env:USERPROFILE\.vscode\extensions\aw-watcher-vscode-plus" -Target "path\to\aw-watcher-vscode-plus"

# macOS / Linux
ln -s /path/to/aw-watcher-vscode-plus ~/.vscode/extensions/aw-watcher-vscode-plus
```

### From marketplace (original, no Basic Auth)

Search for `aw-watcher-vscode` in the VS Code Extensions sidebar.

## Configuration

Open VS Code Settings (Ctrl+,) and search for `aw-watcher-vscode`, or edit `settings.json` directly:

```json
{
    "aw-watcher-vscode.host": "your-server.example.com",
    "aw-watcher-vscode.port": 5601,
    "aw-watcher-vscode.authUser": "your_username",
    "aw-watcher-vscode.authPassword": "your_password",
    "aw-watcher-vscode.maxHeartbeatsPerSec": 1
}
```

| Setting | Default | Description |
|---------|---------|-------------|
| `aw-watcher-vscode.host` | `""` | ActivityWatch server hostname. Leave empty for localhost. |
| `aw-watcher-vscode.port` | `0` | ActivityWatch server port. Set to 0 for default (5600). |
| `aw-watcher-vscode.authUser` | `""` | HTTP Basic Auth username |
| `aw-watcher-vscode.authPassword` | `""` | HTTP Basic Auth password |
| `aw-watcher-vscode.maxHeartbeatsPerSec` | `1` | Max heartbeats per second |

## Usage

1. Configure the server address and credentials in VS Code settings (see above)
2. Open any project in VS Code
3. The extension automatically sends heartbeat events to your ActivityWatch server

Use the **Reload ActivityWatch** command if VS Code was started before the AW server.

## Technical notes

### How Basic Auth is implemented

The `aw-client-js` library uses the native `fetch` API for HTTP requests. The `AWClient` constructor accepts an `auth` option and encodes it into a `Basic` authorization header:

```typescript
const clientOptions = {
    baseURL: `http://${host}:${port}`,
    auth: { username: authUser, password: authPassword }
};
this._client = new AWClient(clientName, clientOptions);
```

The constructor encodes `username:password` as Base64 and stores it. Each `_get`, `_post`, and `_delete` call attaches the `Authorization: Basic <base64>` header to the request.

### Differences from the Python watchers

Unlike the Python `aw-client` library where method patching is required, the JS client cleanly supports the `auth` option in its constructor — the Authorization header is added to every request automatically.

## Related projects

- [aw-watcher-window-plus](https://github.com/liv10let/aw-watcher-window-plus) — Window activity watcher with Basic Auth
- [aw-watcher-afk-plus](https://github.com/liv10let/aw-watcher-afk-plus) — AFK watcher with Basic Auth + background daemon

## License

MPL-2.0
