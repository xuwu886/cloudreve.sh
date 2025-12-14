# Cloudreve 一键启动管理脚本

一个功能完善的 Cloudreve 文件管理系统启动和管理脚本，支持 Pro 版和社区版，提供图形化菜单和命令行两种使用方式。

## ✨ 功能特性

### 核心功能
- ✅ **启动/停止/重启** - 一键管理 Cloudreve 服务
- ✅ **双版本支持** - 同时支持 Pro 版和社区版
- ✅ **License Key 管理** - 自动保存和加载 Pro 版授权密钥
- ✅ **进程监控** - 实时监控进程状态，支持自动重启
- ✅ **日志管理** - 实时查看日志、清空日志文件
- ✅ **状态查询** - 显示运行状态、PID、内存使用、运行时长等信息

### 高级功能
- 🔧 **systemd 服务** - 一键安装/卸载 systemd 服务，实现开机自启
- 🔧 **自动重启守护** - 创建后台守护进程，自动监控并重启 Cloudreve
- 🔧 **强制停止** - 安全停止所有 Cloudreve 进程（包括 systemd 服务）
- 🔧 **配置文件管理** - 自动保存版本选择和 License Key，下次启动无需重复输入

## 📋 系统要求

- Linux 系统（支持 systemd）
- Bash 4.0+
- Root 权限（部分功能需要）
- Cloudreve 可执行文件（需放在脚本同目录下）

## 🚀 快速开始

### 1. 准备工作

确保 `cloudreve` 可执行文件与脚本在同一目录：

```bash
# 示例目录结构
.
├── cloudreve.sh
└── cloudreve          # Cloudreve 可执行文件
```

### 2. 赋予执行权限

```bash
chmod +x cloudreve.sh
```

### 3. 运行脚本

**方式一：图形化菜单（推荐）**

```bash
./cloudreve.sh
```

**方式二：命令行参数**

```bash
# 启动
./cloudreve.sh start

# 停止
./cloudreve.sh stop

# 重启
./cloudreve.sh restart

# 查看状态
./cloudreve.sh status

# 查看日志
./cloudreve.sh log

# 手动监控
./cloudreve.sh monitor

# 强制停止所有进程
./cloudreve.sh force-stop
```

## 📖 使用说明

### 首次使用

1. **选择版本类型**
   - 首次启动时，脚本会提示选择 Cloudreve 版本（Pro 版或社区版）
   - 选择后可以保存到配置文件，下次启动无需重新选择

2. **输入 License Key（仅 Pro 版）**
   - 如果选择 Pro 版，需要输入 License Key
   - 可以选择保存到配置文件，下次启动自动加载

3. **启动服务**
   - 脚本会自动检查进程状态，避免重复启动
   - 启动成功后显示 PID 和日志文件位置

### 菜单功能说明

| 选项 | 功能 | 说明 |
|------|------|------|
| 1 | 启动 Cloudreve | 后台启动服务，自动检测版本和 License Key |
| 2 | 停止 Cloudreve | 安全停止所有进程，包括 systemd 服务 |
| 3 | 重启 Cloudreve | 先停止再启动 |
| 4 | 查看状态 | 显示运行状态、PID、内存、运行时长等 |
| 5 | 查看日志（实时） | 实时查看日志输出（Ctrl+C 退出） |
| 6 | 清空日志 | 清空日志文件内容 |
| 7 | 监控进程（手动） | 手动启动监控，每 10 秒检查一次 |
| 8 | 启用自动重启守护 | 创建 systemd 守护服务，每 30 秒检查一次 |
| 9 | 安装 systemd 服务 | 创建 systemd 服务，实现开机自启 |
| 10 | 删除 systemd 服务 | 移除 systemd 服务 |
| 11 | 查看 License Key | 查看已保存的 License Key |
| 12 | 修改 License Key | 更新 License Key 并可选重启服务 |
| 13 | 删除 License Key | 删除已保存的 License Key |
| 14 | 强制停止所有进程 | 强制停止所有 Cloudreve 进程 |
| 0 | 退出脚本 | 退出管理脚本 |

### 配置文件

脚本会在当前目录创建以下配置文件：

- `cloudreve_version.conf` - 保存版本类型（pro/community）
- `cloudreve_license.conf` - 保存 Pro 版 License Key（权限 600）
- `cloudreve.log` - 运行日志文件
- `/var/run/cloudreve.pid` - 进程 PID 文件

### systemd 服务

**安装服务：**

```bash
# 通过菜单选择选项 9，或直接运行
./cloudreve.sh
# 选择 9
```

**管理服务：**

```bash
# 启动
systemctl start cloudreve

# 停止
systemctl stop cloudreve

# 重启
systemctl restart cloudreve

# 查看状态
systemctl status cloudreve

# 查看日志
journalctl -u cloudreve -f

# 开机自启
systemctl enable cloudreve
```

### 自动重启守护

启用后，系统会创建一个守护进程，每 30 秒检查一次 Cloudreve 状态，如果进程停止会自动重启。

**启用守护：**

```bash
# 通过菜单选择选项 8
./cloudreve.sh
# 选择 8
```

**管理守护：**

```bash
# 停止守护
systemctl stop cloudreve-monitor

# 禁用守护
systemctl disable cloudreve-monitor

# 查看守护状态
systemctl status cloudreve-monitor
```

## ⚠️ 注意事项

1. **Root 权限**
   - 安装 systemd 服务和启用自动重启守护需要 Root 权限
   - 普通启动/停止操作不需要 Root 权限

2. **版本选择**
   - 首次使用必须选择版本类型
   - 版本选择会保存到配置文件，如需切换版本可删除配置文件重新选择

3. **License Key**
   - 仅 Pro 版需要 License Key
   - License Key 保存在 `cloudreve_license.conf`，权限为 600（仅所有者可读）
   - 建议定期备份配置文件

4. **进程冲突**
   - 脚本会自动检测 systemd 服务和手动启动的进程
   - 如果 systemd 服务正在运行，手动启动会失败（反之亦然）
   - 使用前请确保没有其他方式启动的 Cloudreve 进程

5. **日志文件**
   - 日志文件默认保存在脚本同目录下的 `cloudreve.log`
   - 日志会持续追加，建议定期清理或使用日志轮转工具

## 🔍 故障排查

### 启动失败

1. **检查可执行文件**
   ```bash
   ls -l ./cloudreve
   chmod +x ./cloudreve
   ```

2. **查看日志**
   ```bash
   tail -f cloudreve.log
   ```

3. **检查端口占用**
   ```bash
   netstat -tlnp | grep 5212
   ```

### 进程无法停止

1. **使用强制停止功能**
   ```bash
   ./cloudreve.sh force-stop
   ```

2. **手动停止**
   ```bash
   # 查找进程
   ps aux | grep cloudreve
   
   # 停止进程
   kill -9 <PID>
   ```

### systemd 服务问题

1. **重新加载配置**
   ```bash
   systemctl daemon-reload
   ```

2. **查看服务日志**
   ```bash
   journalctl -u cloudreve -n 50
   ```

## 📝 版本信息

- **脚本版本**: v1.0.0
- **作者**: 南栀（请勿去除版权信息）
- **更新日期**: 2025

## 📄 许可证

本脚本遵循原 Cloudreve 项目的许可证。

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📚 相关链接

- [Cloudreve 官方文档](https://docs.cloudreve.org)
- [Cloudreve GitHub](https://github.com/cloudreve/cloudreve)

---

**提示**: 使用本脚本前，请确保已阅读并理解 Cloudreve 的使用条款和许可证要求。

