# Meta 客服回复与社媒发布流程 / Meta Reply and Social Publishing Workflow

**简体中文** | [English](README.en.md)

整理 Meta 客服/KOL 回复规则，并附带本地社媒发布原型源码。

[下载完整项目包](https://github.com/hezonglinhzl-creator/varla-electric-mobility-skills-and-tools/raw/main/packages/skill-meta-auto-reply-skill.zip) · [全部项目](https://github.com/hezonglinhzl-creator/varla-electric-mobility-skills-and-tools)

## 功能特性

- 客服、售后和 KOL 路由规则
- 人工接管与回复草稿
- 本地排期与 Mock 发布原型

## 这个包里有什么

| 内容 | 位置 |
| --- | --- |
| 社媒排期与模拟发布原型 | [varla-social-publisher](apps/varla-social-publisher/README.md) |
| Skill 指令 | [SKILL.md](skill/meta-auto-reply-skill/SKILL.md) |

## 下载和解压

1. 在本仓库点绿色 **Code → Download ZIP**，或者点上方“下载完整项目包”。
2. 下载完成后右键 ZIP，选择**全部解压**，打开能看到此 README 的文件夹。
3. 保留整个目录；先按下一节准备程序，再安装和使用对应 Skill。

## 1. 先准备程序

### 社媒排期与模拟发布原型 (`varla-social-publisher`)

Node.js 22、npm、Redis；Docker 可提供 Redis/MinIO，FFmpeg 用于媒体。

先进入 `apps/varla-social-publisher`。本地仪表板、任务验证、BullMQ Worker 和 Mock adapter。

1. 在本目录打开 PowerShell，运行 `Copy-Item .env.example .env`，自行填写本地密钥，保持 `MOCK_PUBLISHING=true`。
2. 运行 `npm install`，随后 `npm run db:generate` 和 `npm run db:push`。
3. 启动自己的 Redis，确认 `.env` 中 REDIS_URL 正确，然后运行 `npm run dev`。
4. 第二个终端运行 `$env:RUN_WORKER='true'; $env:MOCK_PUBLISHING='true'; $env:REDIS_URL='redis://127.0.0.1:6379'; npm run worker`。
5. 打开仪表板，只做模拟任务。健康检查显示依赖未就绪时先检查数据库、Redis、存储和 Worker。

本地地址：`http://127.0.0.1:3000`

[查看完整程序与浏览器插件安装说明](apps/varla-social-publisher/README.md)

源码明确拒绝未配置的真实平台 adapter。Meta/TikTok 发布、OAuth、审核和外部媒体 URL 尚需集成，不能把 Mock 成功当成真实发布。

## 2. 再安装 Skill

1. 回到解压后的项目根目录，双击 `安装Skill.cmd`。
2. 安装器把 `skill/meta-auto-reply-skill` 复制到 `%USERPROFILE%\.codex\skills\meta-auto-reply-skill`。遇到同名 Skill 会跳过，留意窗口结果。
3. 重新打开 Codex，新建对话，使用下面的 Skill 名称和示例。
4. 公司策略禁止脚本时，可先阅读 install.ps1，或手动把整个 Skill 目录复制到同一目标位置。

## 3. 第一次怎么用

| 准备项 | 内容 |
| --- | --- |
| 输入 | 消息内容、当前产品知识、平台与已批准回复规则。 |
| 输出 | 待审核回复、路由建议或本地模拟发布任务。 |

```text
使用 meta-auto-reply-skill，给这条私信分类并生成待审核回复；不要发送，也不要安排真实发布。
```


## 当前限制与常见问题

附带发布程序当前真实平台适配器未配置；启动网页或 Mock 任务成功不代表内容已发布。

| 问题 | 怎么处理 |
| --- | --- |
| 找不到 Skill | 检查安装窗口结果和目标 SKILL.md，重新打开 Codex 并新建对话。 |
| 程序启动失败 | 在程序目录打开 PowerShell 运行本文命令，让错误保留在窗口，再对照对应程序 README。 |
| 网页看不到插件 | 确认选中直接包含 manifest.json 的目录、网站受支持，并刷新网页。 |
| 外部服务不可用 | 配置自己的授权并检查资源权限；保留本地结果，不把失败说成同步/发送成功。 |

## 隐私与验证范围

这是程序源码与 Skill 分发包，不附带真实账号、OAuth 令牌、客户/KOL 台账、合同、订单、邮箱导出或业务媒体。服务需使用自己的配置。已检查文档和包内入口；此次文档核对没有执行真实下单、发邮件或发布内容。
