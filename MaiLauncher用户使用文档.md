# MaiLauncher 用户使用文档

## 🎯 产品简介

MaiLauncher 是一个强大的 MaiBot 实例管理和部署工具，为用户提供图形化界面来管理多个 MaiBot 实例。通过 MaiLauncher，您可以轻松地部署、启动、停止和管理您的 MaiBot 实例，无需复杂的命令行操作。

### 🌟 主要特性

- **🚀 一键部署**: 通过图形界面快速部署不同版本的 MaiBot
- **📊 实例管理**: 集中管理多个 MaiBot 实例的启停和状态
- **💬 实时交互**: 内置终端和聊天界面，与 Bot 实时交互
- **🎨 现代界面**: 基于现代 Web 技术的美观用户界面
- **🔧 资源管理**: 管理 Bot 的表情包、用户数据等资源
- **⚙️ 配置管理**: 图形化配置 Bot 的各种参数

---

## 📥 安装与启动

### 系统要求

- **操作系统**: Windows 10/11 （Linux/MacOS还在Debug中）
- **内存**: 至少 2GB RAM
- **存储空间**: 至少 4GB 可用空间 （包含一个MaiBot实例）
- **网络**: 需要互联网连接用于下载 MaiBot 版本

### 环境要求
- **Python**: 3.10 或更高版本
- **Git**: 用于版本控制和代码管理

### 下载安装

1. 从官方发布页面下载 `MaiLauncher_x.x.x_x64-setup.exe` 安装包
2. 双击安装包，按照向导完成安装
3. 安装完成后，双击桌面图标启动 MaiLauncher

### 首次启动

启动 MaiLauncher 后，您将看到欢迎界面：

1. **首次使用设置**: 如果是第一次使用，建议点击"开始设置"进行基础配置
2. **快速开始**: 如果想立即体验，可以点击"直接开始使用"

---

## 🎮 基础使用

### 主界面介绍

MaiLauncher 的主界面分为以下几个部分：

- **侧边栏导航**: 包含主要功能模块的导航菜单
- **主内容区**: 显示当前选中功能的详细界面
- **状态栏**: 显示系统状态和连接信息

### 主要功能模块

#### 🏠 首页仪表盘
- 显示系统整体状态
- 快速访问常用功能
- 查看最近活动

#### 📦 实例管理
- 查看所有 MaiBot 实例
- 启动/停止实例
- 查看实例状态和日志

#### 📥 下载中心
- 部署新的 MaiBot 实例
- 添加已有实例
- 选择版本和配置

#### ⚙️ 设置
- 系统偏好设置
- 主题和界面配置
- 网络和存储配置

---

## 🚀 部署您的第一个 MaiBot

### 通过下载中心部署新实例

1. **进入下载中心**
   - 点击侧边栏的"下载中心"

2. **选择部署方式**
   - 选择"部署新实例"
   - 从版本列表中选择要部署的 MaiBot 版本（推荐选择 `Dev` 或 `main` 分支）  
   - 启动器数据库管理功能目前只支持Dev分支

3. **配置实例信息**
   - **实例名称**: 为您的 Bot 起一个易识别的名称（启动器会自动生成一个不冲突的实例名）
   - **安装路径**: 选择 Bot 的安装位置（默认：`启动器后端目录下\MaiBot\Deployments\实例名`）
   - **端口设置**: 保持默认端口 8000（除非有冲突）

4. **服务配置**（高级用户可选）
   - **NapCat 服务**: QQ 机器人适配器，默认端口 8095
   - **NoneBot-ada 服务**: 扩展功能服务，默认端口 18002

5. **开始部署**
   - 勾选"我已阅读并同意用户协议"
   - 点击"开始部署"按钮

6. **等待部署完成**
   - 部署过程会显示实时进度和日志
   - 整个过程可能需要 5-15 分钟，取决于网络速度
   - 部署完成后会显示成功提示

### 添加已有实例

如果您已经有现成的 MaiBot 实例：

1. **选择添加现有实例**
   - 在下载中心选择"添加现有实例"

2. **配置路径信息**
   - **实例名称**: 为实例设置一个管理名称
   - **MaiBot 主程序路径**: 选择包含 `bot.py` 的文件夹
   - **适配器路径**: 选择 NapCat 或其他适配器的路径
   - **端口配置**: 设置实例和适配器的端口

3. **验证和添加**
   - 系统会自动验证路径的有效性
   - 验证通过后点击"添加实例"

---

## 🔧 实例管理

### 实例列表

在"实例管理"页面，您可以看到所有已添加的 MaiBot 实例：

- **实例卡片**: 显示每个实例的基本信息
  - 实例名称和版本
  - 当前运行状态（运行中/已停止）
  - 端口信息
  - 最后活动时间

### 实例操作

#### 启动实例
1. 找到要启动的实例卡片
2. 点击"启动"按钮
3. 等待状态变为"运行中"

#### 停止实例
1. 找到正在运行的实例
2. 点击"停止"按钮
3. 确认停止操作

#### 重启实例
1. 点击实例卡片上的"重启"按钮
2. 系统会先停止再启动实例

#### 删除实例
1. 点击实例卡片的"更多操作"菜单
2. 选择"删除实例"
3. **⚠️ 注意**: 删除操作并不会删除 Bot 的数据文件，只会从 MaiLauncher 中移除实例

### 实例详情

点击实例卡片可以查看详细信息：

- **实时终端**: 与实例进行命令行交互
- **运行日志**: 查看实例的运行日志
- **性能监控**: 查看 CPU、内存使用情况
- **聊天测试**: 与 Bot 进行对话测试



## 🎨 资源管理

### MaiBot 资源管理器（部分未实现）

MaiLauncher 提供了强大的资源管理功能：

#### 表情包管理
- **查看表情库**: 浏览 Bot 的所有表情包
- **添加表情**: 上传新的表情包
- **管理分类**: 按类别整理表情包
- **使用统计**: 查看表情包的使用频率

#### 用户数据管理
- **用户列表**: 查看与 Bot 交互过的用户
- **互动记录**: 查看用户的历史交互信息
- **数据统计**: 分析用户活跃度和偏好

#### 数据备份与恢复（未实现）
- **自动备份**: 设置定期自动备份
- **手动备份**: 随时创建数据备份
- **数据恢复**: 从备份恢复数据

### 访问资源管理器

1. 在实例列表中找到目标实例
2. 点击实例卡片上的"资源管理"按钮
3. 在弹出的资源管理器中进行操作

---

## ⚙️ 系统设置

### 基础设置

在设置页面，您可以配置：

#### 外观设置
- **主题选择**: 明亮/深色主题

#### 存储设置
- **默认安装路径**: 设置新实例的默认安装位置
- **缓存管理**: 清理临时文件和缓存

---

## 🛠️ 故障排除

### 常见问题

#### Q: MaiLauncher 无法正常运行
A: 
1. 确认系统满足最低要求
2. 确认端口 23456 未被占用
3. 检查防火墙设置
4. 重新安装 MaiLauncher

#### Q: 实例部署失败
A:
1. 检查网络连接
2. 确认有足够的磁盘空间
3. 查看部署日志中的错误信息
4. 尝试更换安装路径

#### Q: 实例无法启动
A:
1. 检查实例路径是否正确
2. 确认端口没有被占用
3. 查看实例日志
4. 尝试重新部署实例

#### Q: 无法连接到 Bot
A:
1. 确认实例正在运行
2. 检查端口配置
3. 验证网络连接
4. 查看防火墙设置

### 获取帮助

#### 查看日志
- 在实例详情页面查看运行日志
- 在设置页面查看系统日志
- 日志文件位置：应用程序安装目录下的 `logs` 文件夹

#### 联系支持
- 查看官方文档和 FAQ
- 在 GitHub Issues 中报告问题
- 加入用户交流群获得帮助

---

## 🔄 更新与维护

### 自动更新
MaiLauncher 支持自动检查和下载更新：
1. 系统会定期检查新版本
2. 发现更新时会显示通知
3. 可以选择立即更新或稍后更新

### 手动更新
1. 在设置页面查看当前版本
2. 点击"检查更新"按钮
3. 按照提示完成更新

### 数据备份建议
- 定期备份重要的 Bot 数据
- 在更新前创建完整备份
- 保留多个历史备份版本

---

## 📚 最佳实践

### 实例管理建议
1. **合理命名**: 为实例使用有意义的名称
2. **定期维护**: 定期清理日志和临时文件
3. **监控资源**: 关注 CPU 和内存使用情况
4. **备份数据**: 定期备份重要配置和数据

### 性能优化技巧
1. **适当配置**: 根据硬件配置调整实例数量
2. **定期重启**: 长时间运行后适当重启实例
3. **清理缓存**: 定期清理系统缓存
4. **更新版本**: 及时更新到最新稳定版本

### 安全注意事项
1. **权限控制**: 设置适当的文件权限
2. **网络安全**: 配置防火墙规则
3. **定期备份**: 防止数据丢失
4. **密码管理**: 使用强密码保护敏感配置

---

## 📖 附录

### 快捷键
- `Ctrl + R`: 刷新当前页面
- `Ctrl + Shift + I`: 打开开发者工具
- `F5`: 重新加载应用
- `Ctrl + ,`: 打开设置页面

### 默认端口说明
- `23456`: MaiLauncher 后端服务
- `8000`: MaiBot 主服务默认端口
- `8095`: NapCat 服务默认端口
- `18002`: NoneBot-ada 服务默认端口

### 文件路径说明
- 默认安装路径: `D:\MaiBot\`
- 配置文件路径: `实例路径\config\`
- 日志文件路径: `实例路径\logs\`
- 数据文件路径: `实例路径\data\`

---

**感谢使用 MaiLauncher！**

如果您在使用过程中遇到任何问题，请不要犹豫寻求帮助。我们致力于为您提供最佳的 MaiBot 管理体验。

*本文档版本: v1.0 | 最后更新: 2025年6月21日*
