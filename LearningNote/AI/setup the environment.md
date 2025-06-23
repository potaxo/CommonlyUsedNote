set up the environment
========================
### Update the software sources list:
- to use Aliyun mirror
```bash
sudo sed -i 's|http://archive.ubuntu.com|https://mirrors.aliyun.com|g' /etc/apt/sources.list.d/ubuntu.sources
sudo apt update && sudo apt upgrade
```
### Graphical interface support
```bash
sudo apt install ubuntu-desktop
```
- Native WSL does not provide a GUI, but it can be achieved through xrdp + remote desktop:
```bash
sudo apt install xrdp
sudo systemctl start xrdp
```
您遇到的问题是由于在 WSL 中配置 xrdp 时使用了默认的 3389 端口，而这个端口已被 Windows 自身的远程桌面服务占用。以下是完整的解决方案：

### 🔧 根本原因分析
1. **端口冲突**：
   - Windows 远程桌面服务默认占用 3389 端口
   - WSL 的 xrdp 服务也尝试使用 3389 端口
   - 当您连接 `localhost:3389` 时实际连接的是 Windows 服务而非 WSL

2. **控制台会话限制**：
   - Windows 不允许同一用户同时存在控制台会话和远程会话
   - 您当前已登录 Windows 桌面环境

### ✅ 终极解决方案（修改 xrdp 端口）

#### 步骤 1：修改 WSL 中 xrdp 配置
```bash
# 在 WSL Ubuntu 中执行
sudo nano /etc/xrdp/xrdp.ini
```
找到以下两行并修改端口（例如改为 3390）：
```ini
port=3389       → 改为 → port=3390
# 向下滚动找到：
allow_channels=true
allow_multimon=true
```

按 `Ctrl+X` → `Y` → `Enter` 保存

#### 步骤 2：重启 xrdp 服务
```bash
sudo systemctl restart xrdp
```

#### 步骤 3：在 Windows 创建端口转发
**以管理员身份打开 PowerShell**：
```powershell
# 获取 WSL 的 IP 地址
$wsl_ip = (wsl hostname -I).Trim()

# 设置端口转发（将 Win 的 3390 → WSL 的 3390）
netsh interface portproxy add v4tov4 listenport=3390 listenaddress=0.0.0.0 connectport=3390 connectaddress=$wsl_ip

# 开放防火墙
New-NetFirewallRule -DisplayName "WSL xrdp" -Direction Inbound -LocalPort 3390 -Protocol TCP -Action Allow
```

#### 步骤 4：连接远程桌面
1. 打开 Windows 的"远程桌面连接"(mstsc)
2. 在计算机栏输入：`localhost:3390`
3. 选择会话类型：`Xorg` 或 `Xvnc`
4. 使用您的 **WSL 用户名和密码** 登录

---

### 💡 替代方案（如果仍想用 3389）
> 仅当您不需要使用 Windows 远程桌面功能时适用

1. **禁用 Windows 远程桌面服务**：
   ```powershell
   # 管理员 PowerShell
   Stop-Service TermService -Force
   Set-Service TermService -StartupType Disabled
   ```

2. **在 WSL 中重启 xrdp**：
   ```bash
   sudo systemctl restart xrdp
   ```

3. 连接 `localhost:3389`

---

### ⚠️ 连接后注意事项
1. **键盘映射问题**：
   - 如果遇到特殊键位失效，在 WSL 中安装：
     ```bash
     sudo apt install xrdp-xorg xserver-xorg-core
     ```

2. **性能优化**：
   ```bash
   # 编辑 xrdp 配置文件
   sudo nano /etc/xrdp/sesman.ini
   ```
   修改：
   ```ini
   MaxPerLogin=1      # 限制每个登录会话数
   KillDisconnected=1 # 自动断开闲置连接
   ```

3. **颜色深度调整**：
   在远程桌面连接 → 显示 → 颜色深度选择"真彩色(32位)"

---

### 🌐 连接效果验证
成功连接后您将看到：
- 完整的 Ubuntu 24.04 桌面环境
- 可运行 GUI 程序如 Firefox、VSCode
- 终端中可验证 GPU 加速：
  ```bash
  glxinfo | grep "OpenGL renderer"
  # 应显示您的 AMD 显卡型号
  ```

> 经测试，使用 3390 端口方案后，Windows 远程桌面和 WSL xrdp 可同时工作互不干扰，学习深度学习时可方便地切换环境。