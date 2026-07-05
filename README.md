# ble-relay-template
## 关于本仓库

本仓库基于 [vickyldr/svakom-ble-ai](https://github.com/vickyldr/svakom-ble-ai) 项目，已获得原作者授权。

原仓库侧重于 SVAKOM SL278H 的完整逆向流程和抓包教程，如果你需要了解如何抓包、逆向蓝牙协议，建议参考原仓库。

本仓库在原仓库架构基础上做了以下延伸：
1. **通用适配**：不再局限于特定设备，任何 BLE 设备都能用
2. **iOS 纯手机实现**：无需电脑，全部在手机上完成
3. **AI 生成代码**：用户只需提供设备信息，AI 自动生成中继页面

感谢原作者的逆向成果！

## 与原仓库的关系

本仓库的 **MCP 服务器（`bridge/index.js`）和部署方式（Railway）** 沿用了原仓库 [vickyldr/svakom-ble-ai](https://github.com/vickyldr/svakom-ble-ai) 的架构。

本仓库做了以下改动：
1. **不再包含 `index.html`**：改为由用户用 AI 生成，适配自己的设备
2. **关闭了 `/toy` 接口的密码验证**：小手机连接时不需要密码

简单说：**部署方式没变，设备适配方式变了。**

# BLE MCP 中继服务器

这是一个通用的 MCP 中继服务器模板，用于接收 AI 指令，供中继页面轮询。



## 这个仓库有什么

| 文件 | 作用 |
|------|------|
| `bridge/index.js` | MCP 服务器代码（通用，不需要改） |
| `package.json` | Railway 部署配置（不需要改） |


## 第一步：部署到 Railway

点击下面的按钮一键部署：

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/deploy/ak5nLs?referralCode=UyFE4c&utm_medium=integration&utm_source=template&utm_campaign=generic)

1. 点击按钮，跳转到 Railway
2. 用 GitHub 账号登录
3. 点击「Deploy」，等待完成（约 1-2 分钟）
4. 部署完成后，点击「Variables」标签
5. 添加环境变量：
   - Key: `BRIDGE_SECRET`
   - Value: 你自己设的密码（比如 `abc123`）
6. 点击「Add」保存
7. 点击「Settings」→「Networking」，复制公网地址



## 第二步：生成中继页面

### 1. 准备设备信息

用 nRF Connect 抓包获取：
- 服务 UUID
- 特征 UUID
- 设备名
- 指令格式
- 续命间隔

### 2. 发给 AI 生成 HTML

把下面的话术发给 AI：

我的蓝牙设备信息：
- 服务 UUID: [请替换]
- 特征 UUID: [请替换]
- 设备名: [请替换]
- 续命间隔: [请替换]

我的设备支持以下功能（请根据实际情况描述）：
- 震动：指令格式 [请替换，没有就写"无"]
- 伸缩：指令格式 [请替换，没有就写"无"]
- 转动：指令格式 [请替换，没有就写"无"]
- 加热：指令格式 [请替换，没有就写"无"]
- 其他功能：[请替换]

无声音频保活 URL：https://img.baibai.cv/f/dZymTQ/30min-osbvow%202.mp4 

MCP 轮询接口说明：
- 路径：/toy-next
- 返回格式：{"command":{"action":"xxx","value":yyy}}
- 解析时使用 action 字段

请帮我生成一个蓝牙连接 MCP 服务器的中继 HTML。
功能要求：
1. 连接 MCP 服务器
2. 连接蓝牙
3. 根据我上面提供的功能，生成对应的控制滑块和按钮
4. 测试按钮
5. 停止按钮（停止所有功能）
6. 续命机制
7. 轮询（静默运行，300ms）
8. 日志区域
9. 无声音频保活

轮询要求：
- 静默运行，只在收到指令或出错时打印日志
- 如果返回 {"command": null}，静默跳过
- 轮询和发送指令的功能必须完整保留

代码要求：
- 服务 UUID 和特征 UUID 必须使用字符串格式，例如 'FFE0' 而不是 0xFFE0

请生成完整的 HTML 代码。



### 3. 保存文件

AI 生成代码后，复制保存为 `index.html`

### 第三步：部署中继页面

1. **上传到 GitHub**
   - 回到你 Fork 的仓库
   - 点击「Add file」→「Create new file」
   - 文件名输入：`index.html`
   - 把 AI 生成的代码**完整粘贴**进去
   - 点击「Commit changes」

2. **开启 GitHub Pages**
   - 进入仓库「Settings」→「Pages」
   - 「Branch」选「main」，点击「Save」
   - 等待 1-2 分钟，获得链接


## 第四步：在 iOS 上使用

1. **安装 Bluefy**
- App Store 搜索「Bluefy」，安装

2. **打开中继页面**
- 在 Bluefy 地址栏输入你的 GitHub Pages 链接

3. **连接 MCP 服务器**
- 填入 Railway 公网地址
- 填入你设置的 `BRIDGE_SECRET` 密码
- 点击「连接」

4. **连接蓝牙**
- 确保设备已开机
- 点击「连接蓝牙」
- 在列表中选择你的设备

5. **启动保活**
- 点击「播放保活媒体」，让无声音频在后台播放

6. **测试控制**
- 点击「测试」，设备应该有反应


## 第五步：接入 AI 控制

1. 在小手机的 MCP 配置页面
2. 服务 URL 填：`https://你的Railway地址`
3. 认证方式选「无」
4. 测试是否正常
-如果报错，把报错内容丢给AI让它改



## 常见问题

如果生成的页面有问题，把下面这段话发给 AI：

1. 动一下就停 → 「续命机制没生效，帮我检查 setIntensity 和 startKeepalive 是否正常调用」
2. 切后台蓝牙断了 → 「无声音频保活没生效，检查 audioElement 的 play 和 loop」
3. 日志刷屏 → 「把 pollCloud 里的 logMessage 删掉，只保留关键日志」
4. 收不到云端指令 → 「pollCloud 里的 if (data.command) 和 setIntensity 必须保留，不能删」
5. 报错看不懂 → 把报错信息直接发给 AI，让它分析

把报错信息或现象直接发给 AI，AI 会告诉你怎么改。

## 致谢

- 无声音频保活文件：discord @笨蛋酱 老师
- 原仓库：vickyldr/svakom-ble-ai  小红书 @住在宝宝手机里的el 老师
