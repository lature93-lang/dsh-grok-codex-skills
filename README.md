# DSH · Grok · Codex 协作技能

两份可迁移的 skill，版本 `v0.1.0`。DSH 主调度，Grok 设计及独立质检，Codex 实现。

## 流程

用户需求 → DSH 调 Codex 做最小 MVP/POC → 用户确认 → DSH 汇总实测技术边界 → Grok 设计 → DSH 派 Codex 开发自检 → Grok 独立质检 → 必要时增量返修 → 用户最终验收。

正常状态禁止 Grok 直接或经内部 agent 调 Codex。只有确认 DSH 故障、核查在途任务、登记范围及到期时间后才可临时接管；DSH 恢复后交回。

## 在另一台 Windows 电脑部署

1. 安装并登录 DSH、Grok Bot、Codex；各应用的账号与桥接能力需在新电脑单独配置。本仓库只提供技能和模板，不包含密钥、桥接程序、监控程序或软件安装包。
2. 克隆本仓库或下载 ZIP 解压。下列命令在仓库根目录执行，将 DSH 整个技能目录复制到当前用户目录。若目标已存在，先备份、比对，命令会拒绝覆盖。

```powershell
$skillTarget = Join-Path $env:USERPROFILE '.dsh\skills\dsh-delivery-dispatch'
if (Test-Path -LiteralPath $skillTarget) { throw '目标已存在，请先备份并比对版本' }
New-Item -ItemType Directory -Force -Path (Split-Path $skillTarget) | Out-Null
Copy-Item -LiteralPath '.\skills\dsh-delivery-dispatch' -Destination $skillTarget -Recurse
```

3. 在 DSH 新会话中，明确要求按名称加载 `dsh-delivery-dispatch`，确认能够读到 `references/environment-boundary.md`。本机曾验证目录为 `%USERPROFILE%\.dsh\skills`；其他版本若不发现技能，以该应用实际支持的目录为准。
4. 将 `skills/grok-delivery-review/SKILL.md` 的完整内容交给 Grok Bot，要求用它的原生技能创建/更新能力保存为 `grok-delivery-review`，再按名称重新加载并读回比对。已验证过原生能力，但不同版本界面可能不同；不要把旧机器的内部 `/home/box/...` 路径当作新电脑安装路径。无法使用原生技能时，可要求当前会话直接读取完整文件，明确这是会话指令。
5. 每次新任务交接明确要求加载对应 skill；不要假定会话间或内部 agent 自动继承。DSH 给 Grok 内部 agent 相同的边界包与调用限制。
6. 在新电脑重新核查软件路径、版本、权限、网络和桥接归属。使用技术边界模板，不复制旧机器的环境结论。独立 `codex exec` 不继承当前聊天上下文：提示词需完整列明工作目录、目标、基线和验收方法。

## 最小验收

- DSH 和 Grok 分别按名加载、读回，确认正文及模板完整。
- DSH 向 Codex 派一个无付费、独立目录的小任务；核对真实提示词、回复、产物和运行证据。
- POC 未获用户确认时停止正式设计；用户确认后，DSH 才向 Grok 交接技术边界。
- Grok 返回设计及质检意见给 DSH，不直接调用 Codex。质检通过与用户最终验收分开记录。
- GUI 桥接先确认会话、焦点和发送结果；多个项目共用 Grok 窗口需串行，发送不明时先核查，避免重复发单。

## 版本与限制

技能正文沿用本次部署版本，SHA-256 见 `CHECKSUMS.json`。两端加载已在原机器验证；试点正式开发及独立复验通过，最后一次本聊天确认时 Grok 质检结果仍待核实，不宣称完整流程已通过。

Skill 是协作规则，不是身份认证或不可绕过的权限隔离。原项目的实验 guard 尚未接入真实桥接，故未随这次两份技能迁移；新电脑也不能据此宣称已实现系统级限制。
