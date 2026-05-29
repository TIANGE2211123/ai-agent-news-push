# ai-agent-news-push

低代码、配置驱动的**企业级 AI / Agent 资讯推送工具**，也是一个 [Claude Code Skill](https://docs.claude.com/en/docs/claude-code/skills)。

抓取 RSS + Hacker News → 关键词过滤 → 本地去重 → LLM 富集（去重合并 / 分类 / 价值打分 / 中文摘要）→ 多渠道推送到**飞书 / 企业微信 / 钉钉 / 邮件**。

> 你只需编辑一个 `config.yaml`，运行一条命令。新增数据源或推送渠道无需改核心逻辑。

## 特性

- **低代码**：所有行为由 `config.yaml` 控制（数据源、关键词、渠道、推送条数、质量门槛）。
- **多渠道**：飞书自定义机器人、企业微信群机器人、钉钉机器人（支持加签）、SMTP 邮件。
- **AI 富集**：通过 OpenAI 兼容接口，对资讯做跨源去重合并、5 类分类（Tool Mastery / Workflow Mastery / Thinking / Funding / Research）、1-10 价值打分、生成中文标题与摘要。
- **优雅降级**：LLM 不可用时自动回退为原始条目，工具不中断。
- **去重存档**：基于 URL 哈希的本地 JSON 存档，避免重复推送。

## 目录结构

```
SKILL.md                      # Claude Code 技能入口（被 Claude 自动调用）
assets/config.example.yaml    # 配置模板（复制为 config.yaml 后编辑）
scripts/
  news_push.py                # CLI 入口：编排五阶段流水线
  sources.py                  # 采集层：RSS + Hacker News
  processor.py                # AI 处理层：批量 LLM 富集
  store.py                    # 去重存档
  pushers.py                  # 推送层：飞书/企业微信/钉钉/邮件
  requirements.txt
references/
  setup-webhooks.md           # 各渠道 Webhook 获取与签名说明
  sources-catalog.md          # 可选数据源清单
evals/evals.json              # 技能评测用例
```

## 快速开始

```bash
# 1. 安装依赖
pip install -r scripts/requirements.txt

# 2. 准备配置
cp assets/config.example.yaml config.yaml
#    编辑 config.yaml：填入渠道 webhook，按需增删数据源 / 关键词

# 3. 预览（不真正发送）
python scripts/news_push.py --config config.yaml --dry-run

# 4. 正式推送
python scripts/news_push.py --config config.yaml
```

## 配置要点（config.yaml）

```yaml
llm:                      # OpenAI 兼容接口；默认读取环境变量里的网关
  base_url_env: AI_GATEWAY_BASE_URL
  api_key_env: AI_GATEWAY_API_KEY
  model: anthropic/claude-haiku-4.5

keywords_include: [LLM, Agent, Agentic, MCP, 融资, ...]
keywords_exclude: [...]

sources:
  rss:
    - { name: OpenAI Blog, url: https://openai.com/blog/rss.xml }
  hackernews: { enabled: true, min_score: 100 }

push:
  feishu:      { enabled: true,  webhook: "https://open.feishu.cn/.../hook/xxxx", secret: "" }
  wechat_work: { enabled: true,  webhook: "https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=xxxx" }
  dingtalk:    { enabled: false, webhook: "...", secret: "" }
  email:       { enabled: false, smtp_host: "", username: "", password: "", to_addrs: [] }

delivery:
  max_items: 8
  min_value_score: 6
  dedup_store: pushed.json
```

各渠道 Webhook 获取方式与签名细节见 [`references/setup-webhooks.md`](references/setup-webhooks.md)。

## 定时推送（cron）

```cron
# 每天 09:00 推送
0 9 * * * cd /path/to/tool && python scripts/news_push.py --config config.yaml >> push.log 2>&1
```

## 企业微信接入说明

企业微信群机器人使用 markdown 消息：`POST {webhook} {"msgtype":"markdown","markdown":{"content": ...}}`。
在群设置 → 群机器人 → 添加机器人，复制 Webhook 地址填入 `push.wechat_work.webhook` 并设 `enabled: true` 即可。
注意：单条消息上限约 4096 字节、每分钟 20 条，工具已自动截断超长内容。

## License

MIT
