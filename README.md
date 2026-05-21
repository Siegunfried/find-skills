# find-skills — AI 自我扩容引擎 · Self-Expansion Engine

[English](#english) | [中文](#中文)

---

## 中文

### 一句话说清楚

**find-skills 是 Claude Code 的一个被动触发技能——当 AI 遇到自己搞不定的任务时，自动搜索、安装并使用更合适的插件来解决问题。**

现有的 skill 搜索工具（skill-issue、skill-manager）都需要用户手动调用。find-skills 的核心理念不同：**AI 应该自己知道什么时候需要帮助**。

### 和现有工具的区别

| | find-skills | skill-issue | skill-manager |
|---|---|---|---|
| 触发方式 | **自动（自诊断）** | 手动指令 | 手动请求 |
| 数据来源 | 实时搜索 | 内置 JSON | 内置 30MB DB |
| 外部依赖 | 无 | Python | Node.js + SVN |
| 验证闭环 | 有 | 无 | 无 |
| 安装后自动使用 | 有 | 无 | 无 |

### 工作流程

```
AI 卡住 → 自诊断 → 触发 find-skills
  ├── Layer 1: 搜索本地 marketplace（~200 插件，即时）
  ├── Layer 2: 搜索 GitHub（31k+ skills）
  ├── Layer 3: Web 搜索
  └── Layer 4: npm 搜索
  → 评估候选 → 安装 → 验证 → 完成原任务
```

### 触发条件

**硬信号**（立刻触发）:
- 已尝试 2+ 种方案都不行
- 涉及命名外部服务（GitHub/Linear/Slack/Firebase...）但无专用工具
- 即将写 >50 行胶水代码代替已有集成

**软信号**（停下来想一想）:
- 脑子里冒出"我可以写个复杂脚本搞定"
- 涉及特定领域知识（数据库迁移/安全审计/API 文档...）
- 在重新发明轮子

### 安装

```bash
# 一键安装
claude plugin marketplace add Siegunfried/find-skills
claude plugin install find-skills

# 验证
claude plugin details find-skills
```

### 开发计划

- [ ] 对接 skill-manager 的 31k 索引作为搜索后端
- [ ] 记忆机制：安装过的 skill 以后优先使用
- [ ] 安装失败自动回退到下一候选
- [ ] skill 使用效果评分，指导未来选择

---

## English

### What is this?

**find-skills is a passively-triggered Claude Code skill. When the AI encounters a task it cannot complete with built-in tools, it automatically searches for, installs, and uses better-suited plugins.**

Existing skill discovery tools (skill-issue, skill-manager) all require manual invocation. find-skills is different: **the AI should know when it needs help, without being told**.

### Comparison

| | find-skills | skill-issue | skill-manager |
|---|---|---|---|
| Trigger | **Automatic (self-diagnosis)** | Manual command | Manual request |
| Data source | Live search | Bundled JSON | Bundled 30MB DB |
| Dependencies | None | Python | Node.js + SVN |
| Verification loop | Yes | No | No |
| Auto-use after install | Yes | No | No |

### How it works

```
AI gets stuck → self-diagnosis → triggers find-skills
  ├── Layer 1: Search local marketplace (~200 plugins, instant)
  ├── Layer 2: Search GitHub (31k+ skills)
  ├── Layer 3: Web search
  └── Layer 4: npm search
  → Evaluate → Install → Verify → Complete original task
```

### Trigger conditions

**Hard signals** (trigger immediately):
- 2+ approaches have failed
- Task involves a named external service with no dedicated tool available
- About to write >50 lines of glue code for an integration that should exist

**Soft signals** (pause and consider):
- Thinking "I could script this" for something a plugin likely handles
- Domain-specific knowledge required (DB migration, security audit, etc.)
- Reinventing the wheel

### Install

```bash
claude plugin marketplace add Siegunfried/find-skills
claude plugin install find-skills
claude plugin details find-skills
```

### Roadmap

- [ ] Integrate skill-manager's 31k index as search backend
- [ ] Memory: prefer previously-used skills for similar tasks
- [ ] Auto-fallback: install next candidate on failure
- [ ] Skill effectiveness scoring

---

**License**: MIT
