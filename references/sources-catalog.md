# 推荐数据源目录（企业级 AI / Agent 垂直领域）

当用户没指定来源时，从这里挑一组帮他填进 `config.yaml` 的 `sources.rss`。
RSS 地址会变动，接入前最好快速核实一下能否解析（`--dry-run` 看抓取条数即可）。

> 验证状态约定：✅=已实测可解析且有条目；⚠️=地址可能失效/需换源；🔁=本身不是 RSS，需 RSSHub 或自写 fetcher；🎙️=播客（RSS 只有音频，取标题+简介，全文需 ASR）。

## ⭐ 精选组合（Agent 前沿 + 中文实战，2026 实测）

面向两类人快速起步：

**开发者 / 架构师**（前沿技术与架构演进）：

| 来源 | 接入方式 | 状态 |
| :--- | :--- | :--- |
| Latent Space（AI 工程师播客/Newsletter） | `https://www.latent.space/feed` | ✅ Substack，含框架作者一线访谈 |
| Hugging Face smolagents | 无独立 feed；用 HF Blog feed + 版本追踪 `https://github.com/huggingface/smolagents/releases.atom` | ✅ releases.atom 可用 |
| Anthropic Research / Blog | 官网已无稳定 `rss.xml`（旧地址 404）；走 RSSHub：`https://rsshub.app/anthropic/news` | 🔁 官方无 RSS |
| The Cognitive Revolution（深度 Agent 拆解） | 播客，官网无 `/feed`；订阅播客 RSS（Apple/小宇宙）取标题简介，全文需 ASR | 🎙️ |

**产品 / 业务 / 创业者**（落地、商业化、行研）：

| 来源 | 接入方式 | 状态 |
| :--- | :--- | :--- |
| 量子位 | `https://www.qbitai.com/feed` | ✅ 网站 feed 可用（比公众号更易接入） |
| 机器之心 | `https://www.jiqizhixin.com/rss` | ⚠️ 偶尔抓 0 条，留意 |
| 极客公园 GeekPark | 官网 `/rss`、`/feed` 当前不可达；走 RSSHub：`https://rsshub.app/geekpark/breakingnews` | 🔁 |
| 「阿杰艾（AI）及之」公众号 | 微信公众号无官方 RSS；用 wechat2rss 自建 或 RSSHub 微信路由 | 🔁 见下「公众号接入」 |
| Way to AGI / Dify / Coze 社区 | 知识库 + Discord/论坛，非 feed；作为人工精读源，不入自动管线 | 🔁 无 feed |

### 公众号（微信）接入说明
微信公众号**没有官方 RSS**，三种办法：
1. **优先找网站等价源**：很多公众号同时有网站（如「量子位」→ `qbitai.com/feed`），直接用网站 feed 最稳。
2. **wechat2rss / werss 自建**：自托管服务把指定公众号转成 RSS，再把生成的 feed url 填进 `sources.rss`。
3. **RSSHub 微信路由**：部分公众号有社区维护路由，稳定性一般，需自建 RSSHub 实例。
没有网站等价源的纯公众号（如「阿杰艾」）只能走 2/3，属进阶。

## 大厂模型与产品发布（高价值，建议默认全开）

| 来源 | RSS（如失效请到官网找 feed） | 状态 |
| :--- | :--- | :--- |
| Anthropic News | 旧 `https://www.anthropic.com/rss.xml` 现 404；改用 RSSHub `https://rsshub.app/anthropic/news` | 🔁 官方无 RSS |
| OpenAI Blog | https://openai.com/blog/rss.xml | ✅ |
| Google AI / DeepMind | https://blog.google/technology/ai/rss/ | ✅ |
| Microsoft AI Blog | https://blogs.microsoft.com/ai/feed/ | ⚠️ |
| Meta AI | https://ai.meta.com/blog/rss/ | ⚠️ |
| Hugging Face Blog | https://huggingface.co/blog/feed.xml | ✅ |
| Mistral AI | https://mistral.ai/news/feed.xml | ⚠️ |

## Agent 框架与平台（PM/开发者关注）

| 来源 | 说明 |
| :--- | :--- |
| LangChain Blog | `https://blog.langchain.dev/rss/` 当前返回 HTML、解析 0 条；优先用版本追踪 `https://github.com/langchain-ai/langgraph/releases.atom` ⚠️ |
| LlamaIndex Blog | https://www.llamaindex.ai/blog/feed ⚠️ |
| GitHub Releases | 用 `https://github.com/<org>/<repo>/releases.atom`，如 crewAI / langgraph / autogen / smolagents ✅ 最稳 |

> 提示：GitHub 任意仓库的 release/commit/tag 都有 `.atom` feed，例如
> `https://github.com/langchain-ai/langgraph/releases.atom`。把要追的开源 Agent 项目逐个加进来即可，这是追踪「框架新版本」最省事的方式。

## 社区与学术

| 来源 | 接入方式 |
| :--- | :--- |
| Hacker News | 本工具内置 `sources.hackernews`，按分数+关键词过滤 |
| Reddit 子版块 | RSS：`https://www.reddit.com/r/AI_Agents/.rss`、`/r/LocalLLaMA/.rss`、`/r/artificial/.rss` |
| arXiv cs.AI / cs.CL | `http://export.arxiv.org/rss/cs.AI`、`http://export.arxiv.org/rss/cs.CL` |
| Papers with Code | https://paperswithcode.com/ （需另写 fetcher，暂用 arXiv 替代） |

## 中文资讯与播客

| 来源 | 说明 | 状态 |
| :--- | :--- | :--- |
| 机器之心 | https://www.jiqizhixin.com/rss | ⚠️ 偶尔 0 条 |
| 量子位 | `https://www.qbitai.com/feed`（网站 feed，比公众号好接） | ✅ |
| Latent Space | `https://www.latent.space/feed`（Substack，AI 工程师向） | ✅ |
| 播客（The Cognitive Revolution / OnBoard! / 42章经 / 既然如此 / 不合时宜 / Way to AGI 特辑） | 订阅播客 RSS 后本工具只取标题+简介；完整音频转写属进阶功能（见下「播客 ASR」） | 🎙️ |

## 进阶：超出 RSS 的来源

PRD 里提到的这些源**不是 RSS**，需要为 `sources.py` 各写一个 fetcher（属于「扩展数据源」工作量，按需做）：

- **Crunchbase 融资**：官方 API 需付费 key；或用 Apify 的 Crunchbase Scraper。写 `fetch_crunchbase(cfg)` 调其 API，把结果映射成标准 Item。
- **SEC EDGAR Form D**：早期私募融资，EDGAR 有免费全文检索 API（`https://efts.sec.gov/LATEST/search-index?q=...&forms=D`）。
- **播客 ASR 转写**：取 RSS 里的音频 enclosure URL → 下载 → Whisper/ASR 转文字 → 交给 processor 走「长文摘要」分支。这是 token 大户，建议只对少数精选播客开启。

接入新源时遵循 `sources.py` 的约定：返回标准 Item 列表，单源失败要被捕获（打日志 + 返回 `[]`），绝不让一个源拖垮整次运行。
