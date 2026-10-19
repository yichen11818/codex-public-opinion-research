# 舆情深研 · Public Opinion Research for Codex

一个可直接用于 **OpenAI Codex 的中文舆情研究 Skill**：寻找持续发酵的社会热点、复盘品牌危机、核验争议事件，并生成带来源的 HTML 和 Markdown 报告。

**热搜榜不是社会共识，模型之间意见一致也不是事实证明。** 这个 skill 把重点放在原始证据、持续传播依据、群体诉求和可追溯交付上。

English: A Chinese-first Codex skill for evidence-based public opinion research, controversy analysis, fact-checking, and traceable HTML/Markdown reports. Single-agent workflow; no Docker or dedicated database required.

## 能做什么

- **找热点**：筛选多日发酵、跨群体参与的候选事件，解释为什么值得研究，而非照抄榜单。
- **查事实**：区分已核实事实、当事方主张、未知争议和研究推断，追踪转载的原始来源。
- **做复盘**：按节检索、核验、暂结、补搜，记录反证和改变判断的新进展。
- **分析观点**：区分立场、情绪和诉求；保留少数意见，说明评论样本与覆盖局限。
- **交付报告**：深研默认实际生成 HTML、Markdown 和来源清单，附文件链接。

适用于社会事件、消费者争议、品牌危机与汽车安全事件。不是全平台爬虫，也不会凭空提供全网热度或网民情绪比例。

## 安装

需要能加载本地 skills 的 Codex，以及研究时可用的联网搜索或网页读取工具。Skill 本身不需要 Docker、数据库、Python 包或专用 API Key；具体联网工具使用你当前环境的配置。

让 Codex 使用内置 skill-installer 安装本仓库根目录：

```text
使用 $skill-installer 安装 https://github.com/yichen11818/codex-public-opinion-research 的根目录 skill，安装名为 public-opinion-research。
```

也可手动克隆到技能目录，未设置 CODEX_HOME 时默认如下：

```bash
git clone https://github.com/yichen11818/codex-public-opinion-research.git ~/.codex/skills/public-opinion-research
```

Windows PowerShell：

```powershell
git clone https://github.com/yichen11818/codex-public-opinion-research.git "$env:USERPROFILE\.codex\skills\public-opinion-research"
```

如果设置了 `CODEX_HOME`，请改用其 `skills/public-opinion-research` 目录。已有同名 skill 时先检查本地修改，不要直接覆盖。安装后新开一个 Codex 对话；若尚未发现技能，再重启客户端。

## 使用示例

### 找持续发酵的热点

```text
$public-opinion-research 找几个今年持续发酵多日的社会舆情事件。
不要照抄热搜榜，给出传播窗口、不同群体参与证据，以及为什么值得研究。
```

### 深入研究争议

```text
$public-opinion-research 研究这个事件：[链接或事件名称]。
先辨认事件，核对最新进展，再分析事实、持续热度与公众诉求。
生成可直接阅读的 HTML 报告、Markdown 报告和来源清单。
```

### 汽车制动争议

```text
$public-opinion-research 研究今年华为相关车型的刹车争议。
先区分具体事件、车型和供应关系，不要把 AEB 未触发与基础制动失效混为一谈。
```

“今年”按执行时日期解析，不固定指某一年。事件名称是待检索线索，不是预设事实。没有明确个案时先查找并区分；如果允许自行选题，就选择证据较完整的事件并说明理由。

## 输出是什么

普通选题请求提供简洁的候选与来源；“研究一下 / 复盘”等深研请求默认在当前工作区生成：

```text
research/<事件简称>-<日期>/
├── report.html          可直接阅读的网页报告
├── report.md            便于编辑、分享的文字报告
└── sources.jsonl        来源与证据清单，也可使用 sources.json
```

根据任务需要增加时间线、研究状态、结构化内容底稿或真实评论数据。最终回答应直接提供文件链接；用户明确只要聊天回答时不强制落盘。PDF 在用户需要时另行导出。

报告通常包含：核心结论、事件身份与研究截止日期、时间线、争议与证据对照、持续性判断、公开诉求、未知问题和来源。没有可比数据时不会为了好看生成热度曲线或情绪饼图。

本仓库不附具体事故的事实报告；使用示例不是经过核验的事件断言。

## 工作方式

1. 确定时间范围与事件身份，防止把旧闻或同名事件混在一起。
2. 分渠道收集原始文件、当事方陈述、独立报道、图像视频及公开讨论。
3. 按问题建立提纲，每节执行“检索 → 核验 → 暂结 → 针对缺口补搜”。
4. 在研究中途检查矛盾、反证和结论变化，回到原始材料验证。
5. 先完成内容与证据底稿，再生成报告；排版变化无需重跑研究。

全程采用**单 agent**，不创建子 agent，不把角色扮演当作独立验证。评论聚类和情绪编码是按数据与工具条件选用的方法，不是内置模型服务。

## 能力边界

- 实际覆盖取决于当前可访问的网页、搜索工具和用户提供的数据；不会因安装 skill 就自动拥有社媒数据库。
- 未看原视频、未取得原始报告或评论时会标明；搜索摘要不替代正文核验。
- 转载数量不等于独立证据数量，报道跨度不等于持续高热，便利评论样本不代表全民比例。
- HTML 排版正确不代表事实正确；应检查关键来源、研究窗口和缺失证据。
- 默认研究与本地文件交付，不自动发帖、联系当事人或设置长期监控。
- 浏览器操作遵守使用者环境的工具与授权规则；不要求其他使用者安装本机专用桥接工具。

## 文件结构

| 文件 | 用途 |
|---|---|
| [SKILL.md](SKILL.md) | Codex 入口、研究步骤和交付要求 |
| [agents/openai.yaml](agents/openai.yaml) | 技能展示名称与默认提示 |
| [research-artifacts.md](references/research-artifacts.md) | 来源记录、去重与报告组织 |
| [research-loop-and-output.md](references/research-loop-and-output.md) | 研究闭环、状态与多格式输出 |
| [vehicle-incidents.md](references/vehicle-incidents.md) | 制动、AEB 与辅助驾驶争议核验 |

## 致谢与参考

感谢 [666ghj/BettaFish（微舆）](https://github.com/666ghj/BettaFish) 的开源工作。本 skill 的设计参考了该项目以下实现思路：

| BettaFish 的机制 | 本 skill 的借鉴方式 |
|---|---|
| Query / Media / Insight 分工 | 分别保留网页事实、媒体语境与公众观点渠道，再按问题整合 |
| 分节搜索与反思补搜 | 每节围绕证据缺口迭代，避免一次搜索后直接成文 |
| Forum 主持人读取阶段总结并反馈 | 转换为单 agent 的中途综合检查、矛盾记录和结论修订 |
| 语义聚类与组内抽样 | 材料过多时按议题保留典型材料和反例，避免只看热门 |
| Document IR 与报告渲染分离 | 先保存可追溯内容底稿，再交付 Markdown / HTML，支持后续改版 |

参考源码版本：[`b67928f`](https://github.com/666ghj/BettaFish/tree/b67928ff1ce515dd7672408221fb1e9500d0972d)。

这是独立编写的 Codex 工作流，非 BettaFish 官方版本、插件或完整移植；不打包其代码、模型、爬虫或报告模板，也不继承其平台覆盖、采集规模或效果声明。上述致谢不表示原项目作者参与或认可本项目。

## 许可与反馈

本仓库独立编写的 skill 与文档使用 [MIT License](LICENSE)。BettaFish 及其他外部项目保持各自许可。

欢迎通过 Issues 提供可复现的研究失败案例，例如错误合并事件、漏掉关键反证或未生成报告；请勿提交个人隐私、登录凭证或未公开敏感材料。
