# Windows 无法启动（Native stack trace）

部分 Windows 用户在启动 MoeKoe Music 时可能出现“程序闪退/无窗口/无反应”，或在命令行启动时提示 `Native stack trace` 并退出。

该问题通常与 **系统运行库缺失/损坏、系统 DLL 异常、显卡驱动/硬件加速** 等环境因素有关（参考：Issue #835）。

## 常见现象

- 双击启动后立即退出，界面不显示。
- 任务管理器中进程一闪而过。
- 从安装目录命令行运行后出现 `Native stack trace`。

## 处理建议（按顺序尝试）

### 1) 临时关闭 GPU 硬件加速启动

在安装目录执行：

```bat
"MoeKoe Music.exe" --disable-gpu
```

如果这样可以启动，通常说明与显卡驱动/硬件加速有关，建议更新显卡驱动后再恢复正常启动方式。

### 2) 安装/修复 Microsoft Visual C++ 运行库

安装（或修复安装）最新版 **Visual C++ Redistributable 2015-2022**，建议同时安装：

- `VC_redist.x64.exe`
- `VC_redist.x86.exe`

安装完成后重启系统再尝试启动。

### 3) 修复 Windows 系统文件（系统 DLL 异常）

若系统组件/DLL 缺失或损坏，可能导致 Electron/Node 相关组件初始化失败。可在 **管理员权限** 终端中依次执行：

```bat
dism /online /cleanup-image /restorehealth
sfc /scannow
```

执行完成后重启系统再试。

### 4) 查看日志与系统事件

- 应用日志：`%appdata%\\moekoemusic\\logs`
- Windows 事件查看器：查看 “Application Error” 记录（Faulting module name / Exception code），便于定位是哪个系统组件导致崩溃。

### 5) 安全软件/系统精简导致的组件缺失

某些安全软件注入、系统精简版组件缺失、第三方“优化工具”误删系统文件，都可能导致启动异常。可以尝试：

- 暂时退出/卸载相关安全软件后测试
- 使用官方原版系统环境复现排查

## 仍无法解决？

建议在 GitHub 提交 Issue，并附上：

- `%appdata%\\moekoemusic\\logs` 的日志
- Windows 事件查看器中的 Faulting module name 与 Exception code
- 你的系统版本（例如 Windows 10 22H2）与应用版本号


## 相关问题链接
- [无法打开Moekoe music软件](https://github.com/MoeKoeMusic/MoeKoeMusic/issues/835)

