# DSH 小鲸鱼挂件 · OpenCode Go 版

![DSH 小鲸鱼余额挂件](assets/DSH2.png)

DeepSeek Harness（DSH）Web 界面右下角的常驻小鲸鱼挂件：余额气泡 + 今日已用 + 每轮对话消耗统计，并新增 **OpenCode Go 套餐三档用量显示**。随界面自动启用，标准 DSH 插件包，支持 `dsh plugin` 安装/卸载。

> 🐋 **原插件出处**：本项目是 [MeteorNOX/DeepSeek-Balance-Whale-Widget](https://github.com/MeteorNOX/DeepSeek-Balance-Whale-Widget)（dsh-whale-widget）的二次开发版，原作者 [MeteorNOX](https://github.com/MeteorNOX)，遵循 MIT 许可。本仓库在其基础上新增 OpenCode Go 套餐用量显示，LICENSE 保留原作者版权声明。

## 特性

- 🐋 常驻自启，随 DSH Web 界面自动出现；拖拽 + 四边四分之一吸附，左吸附自动水平镜像
- 💰 余额显示：60 秒自动刷新 + 点击手动刷新，余额变化滚动动画
- 📊 今日已用：**小鲸鱼记账**（默认，免令牌，余额差值本地记账）/ **实时·令牌**（可选，平台接口按峰谷定价实时换算）两种模式
- 💬 每轮对话消耗统计：按真实 usage 结算，可开关、可设自动关闭时间
- 🎚️ 汉堡菜单：大小（0.6–2.5 倍）、音效、音量、用量模式、气泡开关
- 🔊 按压 Q 弹 + 音效（mp3 可选，缺失时静默降级）
- 📊 **新增**：OpenCode Go 套餐用量（三档）显示

## 安装

```powershell
dsh plugin --profile web add github:ELFsay/dsh-whale-widget-opencode
```

装完后重启 `dsh web`，再 F5 刷新浏览器。本地开发调试可用 `dsh plugin --profile web add link:.<仓库绝对路径>`。

## 配置（令牌）

| 凭据 | 必需 | 用途 |
| --- | --- | --- |
| `DEEPSEEK_API_KEY` | ✅ | 拉取 DeepSeek API 余额 |
| `DEEPSEEK_PLATFORM_TOKEN` | ❌ | 「实时·令牌」模式的平台会话令牌（登录 platform.deepseek.com 后从 Network 请求头复制，可选） |

不配置 `DEEPSEEK_PLATFORM_TOKEN` 时，「今日已用」自动使用默认的小鲸鱼记账模式，开箱即用。

## 验证

```powershell
curl http://127.0.0.1:3080/dsh-whale/balance.json
curl http://127.0.0.1:3080/dsh-whale/image.png
```

浏览器 F5 后右下角出现挂件即安装成功。

## 卸载

```powershell
dsh plugin --profile web remove dsh-whale-widget-opencode
```

## 常见问题

- **挂件不出现**：确认 `dsh plugin add` 成功、`--dump-config` 能看到 `dsh-whale-widget-opencode`，重启 `dsh web` 后 F5
- **余额报「未配置 DEEPSEEK_API_KEY」**：在 DSH 凭据服务中配置
- **今日已用显示 --**：记账模式需先完成一次余额观测（60 秒内自动完成）
- **没有声音**：确认 `assets/*.mp3` 在包内，缺失则静默降级为无声音

## 许可证

MIT License，详见 [LICENSE](LICENSE)（版权归原作者 MeteorNOX）。