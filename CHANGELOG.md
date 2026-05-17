# Change Log

#### 0.5.0 (aw-watcher-vscode-plus)

- Add HTTP Basic Auth support via `authUser` / `authPassword` settings
- Inline aw-client-js source (was a git submodule), add `auth` option to AWClient constructor
- Inline media assets (was a git submodule)
- Remove unused axios dependency (aw-client-js now uses native fetch)
- Update tsconfig: target es2018, add dom lib for fetch API types
- Add Chinese README (README_zh.md)

### 0.4.0

update submodules aw-client-js and media to latest

fix the extension to work with the latest aw-client:
- AppEditorActivityHeartbeat --> AppEditorEvent
- createBucket --> ensureBucket
- options object in AWClient constructor
- timestamp should be a Date not a string

#### 0.3.3

Fixed security vulnerability of an outdated dependency

#### 0.3.2

Added maxHeartbeatsPerSec configuration

### 0.3.0

Refined error handling and heartbeat logic

### 0.2.0

Refined error handling and README

### 0.1.0

Initial release of aw-watcher-vscode.

<!--- https://keepachangelog.com/en/1.0.0/ -->
