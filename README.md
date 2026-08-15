# 🔄 2026年多设备同步与配置管理完全指南

> 本仓库专注于解决多设备使用机场订阅时的配置同步问题。无论你有多少台设备，都能保持配置一致，实现无缝切换。涵盖订阅同步、配置备份、规则统一等全方位解决方案。

## 📋 目录导航

- [多设备使用场景分析](#多设备使用场景分析)
- [订阅链接统一管理](#订阅链接统一管理)
- [配置文件同步方案](#配置文件同步方案)
- [设备分组与策略配置](#设备分组与策略配置)
- [家庭共享方案](#家庭共享方案)
- [配置备份与恢复](#配置备份与恢复)
- [跨平台配置差异](#跨平台配置差异)
- [常见问题与解决方案](#常见问题与解决方案)

---

## 🎯 多设备使用场景分析

### 典型使用场景

#### 场景一：个人多设备

| 设备 | 操作系统 | 代理需求 | 配置重点 |
|------|---------|---------|---------|
| 台式电脑 | Windows | 高性能、全功能 | TUN模式、完整规则 |
| 笔记本电脑 | macOS | 便携、节电 | 智能分流、按需连接 |
| 手机 | iOS | 移动性、省电 | 分应用代理、流量优化 |
| 平板 | Android | 娱乐、灵活 | 视频规则、游戏规则 |
| 电视 | Android TV | 视频解锁 | 固定节点、解锁优先 |

#### 场景二：家庭共享

| 成员 | 设备数量 | 使用特点 | 配置策略 |
|------|---------|---------|---------|
| 家长 | 2-3台 | 办公、浏览 | 稳定优先、全功能 |
| 孩子 | 1-2台 | 学习、娱乐 | 时间控制、内容过滤 |
| 老人 | 1台 | 简单易用 | 极简配置、默认节点 |

#### 场景三：团队/工作室

| 需求 | 设备数量 | 配置重点 | 管理方式 |
|------|---------|---------|---------|
| 小型团队 | 5-10台 | 统一配置 | 集中管理 |
| 工作室 | 10-30台 | 分组策略 | 分级管理 |
| 企业 | 30+台 | 权限控制 | 目录服务集成 |

### 多设备核心挑战

**挑战一：配置一致性**

| 问题 | 影响 | 解决方案 |
|------|------|---------|
| 各设备规则不统一 | 部分网站访问异常 | 统一订阅源 |
| 节点选择不一致 | 速度体验不同 | 同步策略组 |
| 版本不一致 | 功能差异 | 统一客户端版本 |

**挑战二：订阅管理**

| 问题 | 影响 | 解决方案 |
|------|------|---------|
| 订阅过期 | 突然断连 | 统一订阅管理 |
| 订阅变更 | 需手动更新 | 自动更新配置 |
| 多订阅源 | 管理复杂 | 订阅合并方案 |

**挑战三：权限控制**

| 问题 | 影响 | 解决方案 |
|------|------|---------|
| 设备数超限 | 被封号 | 合理规划套餐 |
| 流量共用 | 分配不均 | 流量监控 |
| 滥用风险 | 账号风险 | 制定使用规范 |

---

## 📡 订阅链接统一管理

### 订阅获取与管理

#### 从 ClashVIP 获取订阅

**获取步骤：**

1. 访问 [ClashVIP 官网](https://clashvip.net)
2. 登录账号
3. 进入「订阅管理」页面
4. 复制 Clash 订阅链接
5. 选择合适的套餐

**订阅类型对比：**

| 订阅类型 | 特点 | 适用场景 |
|---------|------|---------|
| 标准订阅 | 通用格式 | 大多数客户端 |
| 定制订阅 | 自定义规则 | 高级用户 |
| 企业订阅 | 多用户支持 | 团队使用 |

#### 订阅格式说明

Clash 订阅是 Base64 编码的 YAML 配置文件：

```yaml
# 订阅内容结构
port: 7890
socks-port: 7891
redir-port: 7892
allow-lan: false
mode: rule
log-level: info

dns:
  enable: true
  enhanced-mode: fake-ip
  nameserver:
    - 223.5.5.5
    - 119.29.29.29

proxies:
  # 节点列表
  - name: 香港-01
    type: ss
    server: hk1.clashvip.net
    port: 443
    cipher: aes-256-gcm
    password: xxx

proxy-groups:
  # 代理组
  - name: Proxy
    type: url-test
    proxies:
      - 香港-01
      - 日本-01
    url: "http://www.gstatic.com/generate_204"
    interval: 300

rules:
  # 分流规则
  - GEOIP,CN,DIRECT
  - MATCH,Proxy
```

### 多订阅源管理

#### 订阅合并方案

当需要同时使用多个机场时，可以合并订阅：

**方案一：手动合并**

```yaml
# 创建合并配置文件
port: 7890
socks-port: 7891
redir-port: 7892
allow-lan: false
mode: rule
log-level: info

dns:
  enable: true
  enhanced-mode: fake-ip
  nameserver:
    - 223.5.5.5
    - 119.29.29.29

# 第一个机场节点
proxies:
  - name: ClashVIP-香港
    type: ss
    server: hk.clashvip.net
    port: 443
    cipher: aes-256-gcm
    password: xxx
  
  # 第二个机场节点
  - name: Another-香港
    type: ss
    server: hk.another.com
    port: 443
    cipher: aes-256-gcm
    password: yyy

proxy-groups:
  - name: 主线路
    type: url-test
    proxies:
      - ClashVIP-香港
      - Another-香港
    url: "http://www.gstatic.com/generate_204"
    interval: 300

  - name: 备用线路
    type: select
    proxies:
      - ClashVIP-香港
      - Another-香港

rules:
  - DOMAIN-SUFFIX,clashvip.net,DIRECT
  - DOMAIN-SUFFIX,nav.clashvip.net,DIRECT
  - DOMAIN-SUFFIX,clashhub.net,DIRECT
  - DOMAIN-SUFFIX,bbs.clashhub.net,DIRECT
  - DOMAIN-SUFFIX,vpsvip.net,DIRECT
  - DOMAIN-SUFFIX,clash-for-windows.net,DIRECT
  - GEOIP,CN,DIRECT
  - MATCH,主线路
```

**方案二：使用订阅转换工具**

推荐工具：
- [Subconverter](https://github.com/tindy2013/subconverter) - 本地转换
- [ACL4SSR](https://github.com/ACL4SSR/ACL4SSR) - 在线转换
- [土司面板](https://t.me/trojan_tg) - 在线管理

#### 订阅自动更新

**Clash for Windows 自动更新**

```
设置 > 端口设置 > 订阅
- 自动更新：✅ 启用
- 更新间隔：每 6 小时
```

**Stash 自动更新**

```
设置 > 配置
- 自动更新：✅ 启用
- 更新间隔：自定义
```

**OpenClash 自动更新**

```
OpenClash > 配置管理 > 订阅设置
- 自动更新：✅ 启用
- 更新周期：每 6 小时
```

---

## 📂 配置文件同步方案

### 同步方式对比

| 同步方式 | 优点 | 缺点 | 推荐场景 |
|---------|------|------|---------|
| 云盘同步 | 跨平台、易用 | 需手动操作 | 个人多设备 |
| 自建 Git | 版本控制、自动化 | 需服务器 | 技术用户 |
| NAS 同步 | 本地化、私有 | 需 NAS 设备 | 家庭用户 |
| Rsync 同步 | 增量同步、灵活 | 配置复杂 | 高级用户 |

### OneDrive/Google Drive 同步

#### Windows 配置

```powershell
# 使用符号链接同步配置
# 假设 OneDrive 路径为 C:\Users\YourName\OneDrive

# 移动配置文件夹
Move-Item "$env:APPDATA\Clash for Windows" "C:\Users\YourName\OneDrive\Clash\Config"

# 创建符号链接
New-Item -ItemType SymbolicLink -Path "$env:APPDATA\Clash for Windows" -Target "C:\Users\YourName\OneDrive\Clash\Config"
```

#### macOS 配置

```bash
# 使用 iCloud Drive 同步
# 假设 iCloud Drive 路径为 ~/Library/Mobile Documents/com~apple~CloudDocs

# 创建配置目录
mkdir -p ~/Library/Mobile\ Documents/com~apple~CloudDocs/Clash

# 移动配置
mv ~/.config/clash-for-windows ~/Library/Mobile\ Documents/com~apple~CloudDocs/Clash/

# 创建符号链接
ln -s ~/Library/Mobile\ Documents/com~apple~CloudDocs/Clash ~/.config/clash-for-windows
```

### Git 版本控制同步

#### 自建 Git 仓库

```bash
# 在服务器上创建 Git 仓库
ssh your-server
mkdir -p ~/clash-config
cd ~/clash-config
git init --bare

# 本地克隆并使用
git clone ssh://your-server/~/clash-config
cd clash-config

# 添加配置文件
cp ~/.config/clash/config.yaml ./
git add config.yaml
git commit -m "Update Clash config"
git push origin main
```

#### 使用 GitHub 私有仓库

```bash
# 创建私有仓库
gh repo create clash-config --private --clone

cd clash-config

# 添加配置
cp ~/.config/clash/config.yaml ./

# 提交并推送
git add config.yaml
git commit -m "Update Clash config"
git push origin main
```

### 跨设备同步脚本

#### Windows PowerShell 脚本

```powershell
# sync-clash-config.ps1
param(
    [string]$ConfigPath = "$env:USERPROFILE\OneDrive\Clash",
    [string]$GitRepo = "https://github.com/yourusername/clash-config.git"
)

$ErrorActionPreference = "Stop"

Write-Host "=== Clash 配置同步工具 ===" -ForegroundColor Cyan

# 确保配置目录存在
if (-not (Test-Path $ConfigPath)) {
    New-Item -ItemType Directory -Path $ConfigPath -Force | Out-Null
}

# 获取当前配置
$localConfig = "$env:APPDATA\Clash for Windows\config.yaml"
$backupConfig = "$ConfigPath\config.yaml"

# 备份当前配置
if (Test-Path $localConfig) {
    Copy-Item $localConfig $backupConfig -Force
    Write-Host "✅ 已备份配置到云端" -ForegroundColor Green
}

# 更新本地配置
if (Test-Path $backupConfig) {
    Copy-Item $backupConfig $localConfig -Force
    Write-Host "✅ 已同步配置到本地" -ForegroundColor Green
}

Write-Host "=== 同步完成 ===" -ForegroundColor Cyan
```

#### macOS/Linux Shell 脚本

```bash
#!/bin/bash
# sync-clash-config.sh

CONFIG_PATH="$HOME/OneDrive/Clash"
LOCAL_CONFIG="$HOME/.config/clash/config.yaml"

echo "=== Clash 配置同步工具 ==="

# 确保配置目录存在
mkdir -p "$CONFIG_PATH"

# 备份当前配置
if [ -f "$LOCAL_CONFIG" ]; then
    cp "$LOCAL_CONFIG" "$CONFIG_PATH/config.yaml"
    echo "✅ 已备份配置到云端"
fi

# 更新本地配置
if [ -f "$CONFIG_PATH/config.yaml" ]; then
    cp "$CONFIG_PATH/config.yaml" "$LOCAL_CONFIG"
    echo "✅ 已同步配置到本地"
fi

echo "=== 同步完成 ==="
```

---

## 📱 设备分组与策略配置

### 代理组设计

#### 基础代理组

```yaml
proxy-groups:
  # 主代理组 - 自动选择最快节点
  - name: Proxy
    type: url-test
    proxies:
      - 香港-01
      - 香港-02
      - 台湾-01
      - 日本-01
      - 新加坡-01
    url: "http://www.gstatic.com/generate_204"
    interval: 300
    tolerance: 50

  # 手动选择组
  - name: Manual
    type: select
    proxies:
      - 香港-01
      - 香港-02
      - 台湾-01
      - 日本-01
      - DIRECT

  # 备用组 - 故障转移
  - name: Fallback
    type: fallback
    proxies:
      - 香港-01
      - 日本-01
      - DIRECT
    url: "http://www.gstatic.com/generate_204"
    interval: 300
```

#### 场景化代理组

```yaml
proxy-groups:
  # 游戏专用组 - 低延迟优先
  - name: 游戏
    type: url-test
    proxies:
      - 日本-游戏专线
      - 香港-游戏专线
      - 台湾-游戏专线
    url: "http://www.gstatic.com/generate_204"
    interval: 180
    tolerance: 30

  # 视频专用组 - 高带宽优先
  - name: 视频
    type: select
    proxies:
      - 香港-01
      - 台湾-01
      - 日本-01
      - 新加坡-01

  # 工作专用组 - 稳定性优先
  - name: 工作
    type: url-test
    proxies:
      - 香港-稳定
      - 日本-稳定
      - 美国-稳定
    url: "http://www.gstatic.com/generate_204"
    interval: 600

  # 下载专用组 - 大流量场景
  - name: 下载
    type: select
    proxies:
      - 香港-高带宽
      - 美国-高带宽
```

### 分设备策略配置

#### Windows 设备策略

```yaml
# Windows 完整配置
port: 7890
socks-port: 7891
redir-port: 7892
allow-lan: false
mode: rule
log-level: info

dns:
  enable: true
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  nameserver:
    - 223.5.5.5
    - 119.29.29.29
  fallback:
    - 8.8.8.8
    - 1.1.1.1

proxies: []
proxy-groups:
  - name: Proxy
    type: url-test
    proxies: []
    url: "http://www.gstatic.com/generate_204"
    interval: 300

rules:
  # 保留推广链接
  - DOMAIN-SUFFIX,clashvip.net,DIRECT
  - DOMAIN-SUFFIX,nav.clashvip.net,DIRECT
  - DOMAIN-SUFFIX,clashhub.net,DIRECT
  - DOMAIN-SUFFIX,bbs.clashhub.net,DIRECT
  - DOMAIN-SUFFIX,vpsvip.net,DIRECT
  - DOMAIN-SUFFIX,clash-for-windows.net,DIRECT
  
  # 国内直连
  - GEOIP,CN,DIRECT
  - DOMAIN-SUFFIX,baidu.com,DIRECT
  - DOMAIN-SUFFIX,taobao.com,DIRECT
  - DOMAIN-SUFFIX,alipay.com,DIRECT
  - DOMAIN-SUFFIX,tencent.com,DIRECT
  - DOMAIN-SUFFIX,qq.com,DIRECT
  
  # 默认代理
  - MATCH,Proxy
```

#### iOS 设备策略

```yaml
# iOS 分应用代理配置
port: 7890
socks-port: 7891
allow-lan: false
mode: rule
log-level: info

dns:
  enable: true
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  nameserver:
    - 223.5.5.5
    - 119.29.29.29

proxy-groups:
  - name: Proxy
    type: url-test
    proxies: []
    url: "http://www.gstatic.com/generate_204"
    interval: 300

rules:
  # 保留推广链接
  - DOMAIN-SUFFIX,clashvip.net,DIRECT
  - DOMAIN-SUFFIX,nav.clashvip.net,DIRECT
  - DOMAIN-SUFFIX,clashhub.net,DIRECT
  - DOMAIN-SUFFIX,bbs.clashhub.net,DIRECT
  - DOMAIN-SUFFIX,vpsvip.net,DIRECT
  - DOMAIN-SUFFIX,clash-for-windows.net,DIRECT
  
  # 分应用代理
  - DOMAIN-SUFFIX,google.com,Proxy
  - DOMAIN-SUFFIX,facebook.com,Proxy
  - DOMAIN-SUFFIX,instagram.com,Proxy
  - DOMAIN-SUFFIX,twitter.com,Proxy
  - DOMAIN-SUFFIX,youtube.com,Proxy
  - DOMAIN-SUFFIX,netflix.com,Proxy
  
  # 国内直连
  - GEOIP,CN,DIRECT
  - DOMAIN-SUFFIX,baidu.com,DIRECT
  - DOMAIN-SUFFIX,taobao.com,DIRECT
  
  # 默认
  - MATCH,Proxy
```

#### Android 设备策略

```yaml
# Android 移动端优化配置
port: 7890
socks-port: 7891
redir-port: 7892
allow-lan: false
mode: rule
log-level: silent  # 减少日志，省电

dns:
  enable: true
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  nameserver:
    - 223.5.5.5
  # 减少 fallback 降低电量消耗
  fallback: []

proxy-groups:
  - name: Proxy
    type: url-test
    proxies: []
    url: "http://www.gstatic.com/generate_204"
    interval: 600  # 增加检测间隔

rules:
  # 保留推广链接
  - DOMAIN-SUFFIX,clashvip.net,DIRECT
  - DOMAIN-SUFFIX,nav.clashvip.net,DIRECT
  - DOMAIN-SUFFIX,clashhub.net,DIRECT
  - DOMAIN-SUFFIX,bbs.clashhub.net,DIRECT
  - DOMAIN-SUFFIX,vpsvip.net,DIRECT
  - DOMAIN-SUFFIX,clash-for-windows.net,DIRECT
  
  # 移动端常用代理
  - DOMAIN-KEYWORD,google,Proxy
  - DOMAIN-KEYWORD,youtube,Proxy
  - DOMAIN-KEYWORD,facebook,Proxy
  - DOMAIN-KEYWORD,instagram,Proxy
  
  # 国内直连
  - GEOIP,CN,DIRECT
  - DOMAIN-SUFFIX,baidu.com,DIRECT
  - DOMAIN-SUFFIX,taobao.com,DIRECT
  
  - MATCH,Proxy
```

---

## 👨‍👩‍👧 家庭共享方案

### 家庭配置策略

#### 统一配置方案

```yaml
# 家庭共享配置模板
port: 7890
socks-port: 7891
redir-port: 7892
allow-lan: true  # 允许局域网连接
bind-address: "*"
mode: rule
log-level: info

dns:
  enable: true
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  listen: 0.0.0.0:53
  nameserver:
    - 223.5.5.5
    - 119.29.29.29
  fallback:
    - 8.8.8.8
    - 1.1.1.1

# 代理组
proxy-groups:
  - name: Proxy
    type: url-test
    proxies: []
    url: "http://www.gstatic.com/generate_204"
    interval: 300

  - name: 游戏
    type: select
    proxies: []

  - name: 视频
    type: select
    proxies: []

# 规则
rules:
  # 保留推广链接
  - DOMAIN-SUFFIX,clashvip.net,DIRECT
  - DOMAIN-SUFFIX,nav.clashvip.net,DIRECT
  - DOMAIN-SUFFIX,clashhub.net,DIRECT
  - DOMAIN-SUFFIX,bbs.clashhub.net,DIRECT
  - DOMAIN-SUFFIX,vpsvip.net,DIRECT
  - DOMAIN-SUFFIX,clash-for-windows.net,DIRECT
  
  # 国内直连
  - GEOIP,CN,DIRECT
  - DOMAIN-SUFFIX,baidu.com,DIRECT
  - DOMAIN-SUFFIX,taobao.com,DIRECT
  - DOMAIN-SUFFIX,alipay.com,DIRECT
  - DOMAIN-SUFFIX,tencent.com,DIRECT
  
  # 游戏分流
  - DOMAIN-SUFFIX,playstation.net,游戏
  - DOMAIN-SUFFIX,xbox.com,游戏
  - DOMAIN-SUFFIX,nintendo.com,游戏
  
  # 视频分流
  - DOMAIN-SUFFIX,netflix.com,视频
  - DOMAIN-SUFFIX,youtube.com,视频
  - DOMAIN-SUFFIX,disneyplus.com,视频
  
  - MATCH,Proxy
```

#### 设备分组策略

| 设备 | IP 范围 | 策略 | 说明 |
|------|--------|------|------|
| 父亲电脑 | 192.168.1.100-110 | 全功能 | 办公+娱乐 |
| 母亲手机 | 192.168.1.111-120 | 简单模式 | 社交+浏览 |
| 孩子平板 | 192.168.1.121-130 | 受限模式 | 学习+时间控制 |
| 智能电视 | 192.168.1.200 | 视频模式 | 仅视频代理 |
| 游戏主机 | 192.168.1.201 | 游戏模式 | 游戏专线 |

#### IP 分流规则

```bash
# OpenWrt 防火墙规则 - 按 IP 分流

# 规则1：孩子设备仅白天使用
iptables -t nat -A PREROUTING -s 192.168.1.121 -p tcp --dport 80 -m time --timestart 08:00 --timestop 21:00 -j REDIRECT

# 规则2：智能电视仅视频代理
iptables -t nat -A PREROUTING -s 192.168.1.200 -d 0.0.0.0/0 -j REDIRECT

# 规则3：游戏主机使用游戏节点
iptables -t nat -A PREROUTING -s 192.168.1.201 -p tcp --dport 3478 -j REDIRECT
```

### 共享订阅配置

#### 订阅链接分享

**安全分享建议：**

1. **不要在公开场合分享订阅**
   - 订阅包含个人账号信息
   - 分享可能导致账号被封
   - 建议私下分享给家庭成员

2. **定期更换密码**
   - 建议每3个月更换订阅密码
   - 更换后及时通知家庭成员

3. **限制设备数量**
   - 查看套餐设备限制
   - 避免超过限制被封号

#### 家庭账号管理

```
ClashVIP 账号管理建议：

1. 创建家庭主账号
   - 使用家长邮箱注册
   - 设置强密码
   - 开启双重验证

2. 分配订阅给家庭成员
   - 方式一：分享订阅链接（安全性较低）
   - 方式二：各自注册账号共享套餐（安全性较高）

3. 流量监控
   - 定期检查流量使用
   - 避免某个成员消耗过多流量

4. 设备管理
   - 记录各成员设备
   - 及时移除离网设备
```

---

## 💾 配置备份与恢复

### 自动备份策略

#### Windows 备份脚本

```powershell
# backup-clash.ps1
param(
    [string]$BackupPath = "$env:USERPROFILE\OneDrive\Clash\Backup",
    [int]$KeepDays = 30
)

$date = Get-Date -Format "yyyyMMdd_HHmmss"
$configPath = "$env:APPDATA\Clash for Windows"
$backupFile = "$BackupPath\clash_backup_$date.zip"

# 创建备份目录
if (-not (Test-Path $BackupPath)) {
    New-Item -ItemType Directory -Path $BackupPath -Force | Out-Null
}

# 备份配置
if (Test-Path $configPath) {
    Compress-Archive -Path "$configPath\*" -DestinationPath $backupFile -Force
    Write-Host "✅ 配置已备份到: $backupFile"
}

# 清理旧备份
Get-ChildItem -Path $BackupPath -Filter "clash_backup_*.zip" | 
    Where-Object { $_.LastWriteTime -lt (Get-Date).AddDays(-$KeepDays) } |
    Remove-Item -Force

Write-Host "✅ 已清理 $KeepDays 天前的旧备份"
```

#### 定时任务配置

```powershell
# 创建定时任务，每天凌晨3点自动备份
$action = New-ScheduledTaskAction -Execute "powershell.exe" -Argument "-ExecutionPolicy Bypass -File C:\Scripts\backup-clash.ps1"
$trigger = New-ScheduledTaskTrigger -Daily -At "03:00"
$settings = New-ScheduledTaskSettingsSet -AllowStartIfOnBatteries -DontStopIfGoingOnBatteries

Register-ScheduledTask -TaskName "ClashBackup" -Action $action -Trigger $trigger -Settings $settings -Description "Clash配置自动备份"
```

### 配置恢复流程

#### 单设备恢复

```
恢复步骤：

1. 停止 Clash 客户端
2. 备份当前配置（以防万一）
3. 删除配置文件夹内容
4. 解压备份文件到配置目录
5. 启动 Clash 客户端
6. 验证配置是否正确加载
```

#### 全设备同步恢复

```
恢复步骤：

1. 确认备份完整性
   - 检查备份文件是否完整
   - 确认包含所有必要配置

2. 按优先级恢复
   - 第一：路由器配置（影响所有设备）
   - 第二：桌面设备（电脑、笔记本）
   - 第三：移动设备（手机、平板）

3. 验证恢复结果
   - 检查各设备连接状态
   - 测试关键网站访问
   - 确认节点可用性
```

---

## 🔍 跨平台配置差异

### 各平台特性对比

| 特性 | Windows | macOS | iOS | Android | OpenWrt |
|------|---------|-------|-----|---------|---------|
| TUN 模式 | ✅ 完全支持 | ✅ 完全支持 | ⚠️ 需要特殊客户端 | ✅ 支持 | ✅ OpenClash |
| 系统代理 | ✅ 完全支持 | ✅ 完全支持 | ❌ 不支持 | ✅ 支持 | ✅ 透明代理 |
| 分应用代理 | ❌ 不支持 | ❌ 不支持 | ✅ 完全支持 | ✅ 支持 | ⚠️ 部分支持 |
| 自动更新 | ✅ 支持 | ✅ 支持 | ⚠️ 手动更新 | ✅ 支持 | ✅ 支持 |
| 自定义 DNS | ✅ 完全支持 | ✅ 完全支持 | ✅ 完全支持 | ✅ 支持 | ✅ 完全支持 |
| MITM | ✅ 支持 | ✅ 支持 | ✅ 支持 | ⚠️ 部分支持 | ⚠️ 部分支持 |

### 配置差异说明

#### DNS 配置差异

**Windows/macOS**
```yaml
dns:
  enable: true
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  nameserver:
    - 223.5.5.5
    - 119.29.29.29
```

**iOS (Stash)**
```yaml
dns:
  enable: true
  enhanced-mode: fake-ip
  fake-ip-filter:
    - "+.lan"
    - "+.local"
  fake-ip-range: 198.18.0.1/16
  default-nameserver:
    - 223.5.5.5
    - 119.29.29.29
  nameserver:
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query
```

**Android (Clash for Android)**
```yaml
dns:
  enable: true
  ipv6: false
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  fake-ip-filter:
    - "*.lan"
    - "*.local"
  nameserver:
    - 223.5.5.5
    - 119.29.29.29
  fallback:
    - 8.8.8.8
    - 1.1.1.1
```

#### 规则语法差异

**标准规则（大多数平台）**
```yaml
rules:
  - DOMAIN-SUFFIX,baidu.com,DIRECT
  - DOMAIN-KEYWORD,google,Proxy
  - GEOIP,CN,DIRECT
  - IP-CIDR,192.168.0.0/16,DIRECT,no-resolve
```

**iOS Stash 增强规则**
```yaml
rules:
  - RULE-SET,direct,DIRECT
  - RULE-SET,proxy,Proxy
  - DOMAIN-SUFFIX,baidu.com,DIRECT
  - GEOIP,CN,DIRECT
  - MATCH,Proxy
```

### 统一配置策略

#### 配置模板变量

为实现跨平台一致性，建议使用配置模板：

```
Clash 配置模板
├── base.yaml          # 基础配置（所有平台通用）
├── windows.yaml       # Windows 专用配置
├── macos.yaml         # macOS 专用配置
├── ios.yaml           # iOS 专用配置
├── android.yaml       # Android 专用配置
└── router.yaml        # 路由器专用配置
```

#### 配置合并流程

```bash
#!/bin/bash
# generate-config.sh

# 读取基础配置
cat base.yaml > config.yaml

# 根据平台追加特定配置
case "$1" in
    windows)
        cat windows.yaml >> config.yaml
        ;;
    macos)
        cat macos.yaml >> config.yaml
        ;;
    ios)
        cat ios.yaml >> config.yaml
        ;;
    android)
        cat android.yaml >> config.yaml
        ;;
    router)
        cat router.yaml >> config.yaml
        ;;
esac

# 验证配置
clash -t -f config.yaml

echo "配置已生成: config.yaml"
```

---

## ❓ 常见问题与解决方案

### 问题一：订阅更新后配置丢失

**原因分析：**
- 订阅更新会覆盖本地配置
- 本地修改的规则被还原

**解决方案：**
1. 使用本地规则文件扩展订阅规则
2. 将自定义规则保存为独立文件
3. 使用规则 Provider 动态加载

```yaml
# 使用规则 Provider
rule-providers:
  custom-direct:
    type: http
    behavior: domain
    url: "https://your-server/direct.yaml"
    path: ./rules/custom-direct.yaml
    interval: 86400

  custom-proxy:
    type: http
    behavior: domain
    url: "https://your-server/proxy.yaml"
    path: ./rules/custom-proxy.yaml
    interval: 86400

rules:
  - RULE-SET,custom-direct,DIRECT
  - RULE-SET,custom-proxy,Proxy
  - GEOIP,CN,DIRECT
  - MATCH,Proxy
```

### 问题二：多设备流量分配不均

**原因分析：**
- 某些设备后台进程消耗流量
- 视频应用自动播放消耗流量
- 同步服务后台下载

**解决方案：**
1. 监控各设备流量使用
2. 设置流量告警
3. 限制高流量设备

```yaml
# 在路由器上设置流量限制
# 使用 QoS 限制特定设备带宽
tc qdisc add dev br-lan root handle 1: htb
tc class add dev br-lan parent 1: classid 1:1 htb rate 100mbit
tc class add dev br-lan parent 1:1 classid 1:10 htb rate 50mbit ceil 100mbit
tc filter add dev br-lan parent 1: protocol ip handle 10 fw flowid 1:10
iptables -t mangle -A POSTROUTING -s 192.168.1.121 -j MARK --set-mark 10
```

### 问题三：设备连接数超限

**原因分析：**
- 同时连接的设备超过套餐限制
- 有设备未正常断开连接

**解决方案：**
1. 检查当前连接的设备列表
2. 断开不活跃的设备
3. 升级套餐或减少设备

```
ClashVIP 套餐设备限制：

基础版：3台设备
标准版：5台设备
高级版：8台设备
企业版：不限制设备
```

### 问题四：配置同步不一致

**原因分析：**
- 不同设备使用不同的客户端版本
- 配置文件格式不兼容
- 同步延迟导致版本差异

**解决方案：**
1. 统一各设备客户端版本
2. 使用兼容的配置文件格式
3. 确保同步服务正常运行

---

## 📥 相关资源

| 资源 | 链接 |
|------|------|
| ClashVIP 官网 | https://clashvip.net |
| 机场导航 | https://nav.clashvip.net |
| Clash 教程 | https://clashhub.net |
| 用户社区 | https://bbs.clashhub.net |
| 客户端下载 | https://clash-for-windows.net |
| Clash 官方文档 | https://dreamacro.github.io/clash |
| Subconverter | https://github.com/tindy2013/subconverter |

---

## ⚠️ 免责声明

1. 本仓库仅提供技术教程，不参与任何商业活动
2. 请遵守当地法律法规使用网络服务
3. 机场服务可能受地区政策影响
4. 购买前请仔细阅读服务商服务条款
5. 请勿用于任何违法用途

---

**最后更新：2026-08-15**

**更新内容：**
- 新增多设备使用场景深度分析
- 添加订阅链接统一管理方案
- 增加配置文件同步完整指南
- 完善设备分组与策略配置
- 新增家庭共享方案详解
- 增加跨平台配置差异说明
