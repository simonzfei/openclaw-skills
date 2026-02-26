# Daily AI News Skill

面向“每日 AI 资讯”场景的聚合与解读技能。

## 能力概览

- 多源抓取：覆盖全球科技、中国科技、开源社区与市场资讯。
- AI 聚焦：自动对关键词进行 AI 语义扩展，优先筛选 LLM/Agent/生成式 AI 相关动态。
- 智能补全：严格时间窗不足时，自动补充高热度信息并清晰标注。
- 深度解读：每条新闻给出“为什么重要”的可执行洞察。
- 日报输出：自动保存为 `reports/ai_daily_YYYYMMDD_HHMM.md`。

## 典型使用场景

- 每日站会前获取 AI 早报
- 产品/技术雷达扫描
- 投融资与政策跟踪
- 开源 AI 项目趋势观察

## 如何将这个 Skill 迁移到 OpenClaw

下面给你一套最稳妥的迁移流程（按推荐顺序）：

1. **定位 OpenClaw 的 skills 目录**  
   通常是 `~/.openclaw/skills/`、`$OPENCLAW_HOME/skills/`，或项目内约定的 `skills/` 目录。
2. **复制整个技能文件夹**  
   将当前目录下的 `daily-ai-news-skill/` 原样复制到 OpenClaw 的 skills 根目录。
3. **检查必需文件是否完整**  
   至少包含：
   - `SKILL.md`（技能元信息 + 执行规则）
   - `templates.md`（菜单模板）
   - `README.md`（说明文档，可选但推荐）
4. **确认 Frontmatter 可被 OpenClaw 识别**  
   `SKILL.md` 顶部 `name` 与 `description` 要保留，`name` 建议保持唯一（当前为 `daily-ai-news-skill`）。
5. **创建输出目录并校验写权限**  
   该 skill 会写入 `reports/`，请确保 OpenClaw 运行用户对该目录可写。
6. **在 OpenClaw 中重载/重启技能**  
   根据你的 OpenClaw 启动方式执行 reload（或重启服务）让新 skill 生效。
7. **做一次烟雾验证**  
   触发一条指令（如“给我今天 AI 日报”），确认：
   - 能命中该 skill
   - 能生成并保存 `reports/ai_daily_YYYYMMDD_HHMM.md`
   - 中文分区结构符合 `SKILL.md` 规范

### 最小迁移命令示例

```bash
# 1) 复制 skill（把路径替换成你的 OpenClaw skills 根目录）
cp -R ./daily-ai-news-skill "$OPENCLAW_HOME/skills/"

# 2) 确认文件存在
ls -la "$OPENCLAW_HOME/skills/daily-ai-news-skill"

# 3) 预创建 reports 目录（可选）
mkdir -p "$OPENCLAW_HOME/skills/daily-ai-news-skill/reports"
```

### 常见迁移问题

- **问题：skill 能被发现，但不触发**  
  排查 `SKILL.md` 头部 Frontmatter 是否损坏，`description` 是否过于模糊。
- **问题：报告不落盘**  
  检查 `reports/` 写权限以及 OpenClaw 进程用户。
- **问题：菜单命令不显示**  
  检查 `templates.md` 文件是否一并复制，且触发词是否与 `SKILL.md` 约定一致。

## 文件说明

- `SKILL.md`：技能主说明与执行规范。
- `templates.md`：交互菜单模板（`如意如意`触发）。
