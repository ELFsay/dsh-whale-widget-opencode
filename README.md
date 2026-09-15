# DSH 小鲸鱼挂件 · OpenCode Go 版

![DSH 小鲸鱼余额挂件](assets/DSH2.png)

DeepSeek Harness（DSH）Web 界面右下角的常驻小鲸鱼挂件：余额 / 今日已用 / 峰谷定价 / 自定义泡泡 / 音效 / 每轮消耗，**外加 OpenCode Go 订阅额度（5h / 周 / 月三窗口）显示**。随界面自动启用，标准 DSH 插件包，支持 `dsh plugin` 安装卸载。

> 🐋 **原插件出处**：本项目基于 [MeteorNOX/DeepSeek-Balance-Whale-Widget](https://github.com/MeteorNOX/DeepSeek-Balance-Whale-Widget)（dsh-whale-widget）**v0.3.0** 二次开发，原作者 [MeteorNOX](https://github.com/MeteorNOX)，遵循 MIT 许可，LICENSE 保留原作者版权声明。
>
> 本仓库相对上游的新增：**OpenCode Go 订阅额度厂商模板**（上游 33 个模板 → 本仓库 34 个），以及订阅额度解析中的**多窗口支持**（同一接口返回多个用量窗口时逐窗口展示）。其余功能与上游一致，**完整文档请看[上游 README](https://github.com/MeteorNOX/DeepSeek-Balance-Whale-Widget#readme)**。

## 本仓库新增：OpenCode Go 额度显示

在**菜单 → 小鲸鱼记账 →（添加模型）**里选择厂商模板 **`OpenCode Go（订阅）`**，凭据名保持模板自动填好的 `OPENCODE_GO_API_KEY`，保存即可：

```
厂商额度 5h 1% · 4h39m后重置 | 周 8% · 5天22h后重置 | 月 59% · 5天8h后重置
```

- 接口：`https://opencode.ai/zen/go/v1/usage`（官方用量接口，鉴权 `Authorization: Bearer <key>`）
- 返回 `usage.rolling / weekly / monthly` 三个窗口，含 `percent`（已用百分比）与 `resetsAt`（重置时间）
- 密钥**无需手动输入**：只填凭据名，密钥由 DSH 凭据服务提供（详见下节）
- 想让它出现在泡泡里，用该模型的「额度·<模型名>」模块

## 密钥怎么给（三种方式，任选其一，都不用在插件里手输）

插件配置里只存**凭据名**（如 `OPENCODE_GO_API_KEY`），密钥由 DSH 凭据服务按以下优先级解析：

```text
启动环境变量（只读，最高优先）
> $DSH_HOME/.credentials.yaml（可写；插件/设置页写入的是这层）
> <cwd>/.env（只读兜底）
> $DSH_HOME/.env（只读兜底）
```

1. **已配置过**：凭据里已有 `OPENCODE_GO_API_KEY` → 什么都不用做，添加模型时密钥框留空
2. **环境变量**：启动前导出（DSH Desktop 请设用户级环境变量后重启应用）
   ```powershell
   $env:OPENCODE_GO_API_KEY = "sk-…"; dsh web
   ```
   ⚠️ 来自启动环境的引用是只读的，此时 UI/插件写入该 ref 会被拒绝
3. **写文件**：`$DSH_HOME/.env` 或 `$DSH_HOME/.credentials.yaml`

## 安装

```powershell
dsh plugin --profile web add github:ELFsay/dsh-whale-widget-opencode
```

装完重启 `dsh web`，再 F5 刷新浏览器。本地开发可用 `dsh plugin --profile web add link:<本目录绝对路径>`。

## 与上游的关系 / 同步上游

- 本仓库 = 上游 **v0.3.0** + OpenCode Go 模板 + 多窗口额度支持
- 同步上游新版本时：覆盖上游内容后，重新应用两处新增 —— `lib/index.js` 的 `opencode_go` 模板与 `fetchModelQuota` 里的 `windows` 分支、`assets/whale-widget.js` 的 `apiPlanSummary` 多窗口渲染
- 为避免与原版插件同时挂载，本包 bundle id / 插件名 / 客户端守卫都带 `-opencode` 后缀（两个都装时只激活一个，并在控制台给出提示）

## 卸载

```powershell
dsh plugin --profile web remove dsh-whale-widget-opencode
```

## 许可证

MIT License，详见 [LICENSE](LICENSE)（版权归原作者 MeteorNOX）。
