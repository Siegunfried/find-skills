---
name: find-skills
description: >-
  Your self-expansion engine. Whenever you sense your built-in tools are
  insufficient, you are about to write a complex workaround, or you catch
  yourself thinking "I wish I had a tool for this," immediately invoke this
  skill. It searches multiple sources (local marketplace, GitHub, web, npm)
  to discover and install missing capabilities, then verifies they actually
  solve the problem. Do not struggle in silence. Do not write 100 lines of
  script when a plugin exists. Your capabilities grow each time you use this
  skill.
---

# Find Skills — Self-Expansion Engine

You are not fixed. Your capabilities grow every time you reach for this skill.
The ecosystem has 200+ plugins in the official marketplace and 30,000+ skills
on GitHub. The gap between "I can't" and "I can" is often just one install away.

## Philosophy

You should treat your toolset as expandable, not static. When a human developer
encounters a new problem domain, they don't just struggle through — they
`npm install`, `pip install`, or `go get` a library. You should do the same.

Every time you use this skill successfully, you permanently grow your
capabilities. The next time a similar task appears, you already have the tool.

## Step 0: Self-diagnosis — Should I trigger?

Stop and invoke this skill when ANY of these are true:

**Hard signals** (trigger immediately):
- You have tried 2+ approaches and none worked well
- The task involves a named external service (GitHub, Linear, Slack, Firebase,
  Vercel, Airtable, Supabase, Notion, Jira, etc.) and you have no dedicated tool
- You are about to write >50 lines of glue code or API wrapper for something
  that clearly should have an existing integration
- The user asks "is there a plugin for X" or "can you do Y" and you're unsure

**Soft signals** (pause and consider):
- You catch yourself thinking "I could do this with a complex bash script"
- The task requires domain-specific knowledge (database migration, security
  audit, API documentation, infrastructure-as-code, schema design)
- You are reimplementing something that feels like it should exist already
- The user's request mentions a file format or protocol you don't natively
  handle well

**Anti-patterns** (do NOT trigger):
- Simple one-liner tasks
- Tasks you can handle cleanly with built-in tools
- Tasks where no plugin could reasonably exist (proprietary internal systems)

## Step 1: Progressive search — Cast wider nets

Search in layers. Start narrow and fast, expand only if needed.

### Layer 1: Local marketplace (instant, ~200 plugins)

Search the official marketplace manifest:

```bash
python -c "
import json, os
home = os.path.expanduser('~')
manifest = f'{home}/.claude/plugins/marketplaces/claude-plugins-official/.claude-plugin/marketplace.json'
with open(manifest) as f:
    data = json.load(f)
keyword = '<keyword>'.lower()
for p in data['plugins']:
    text = (p['name'] + ' ' + p.get('description','') + ' ' + p.get('category','')).lower()
    if keyword in text:
        print(f\"[{p.get('category','?')}] {p['name']}\")
        print(f\"  {p.get('description','')[:200]}\")
        print()
"
```

Try multiple keywords. If searching for "database migration", also try "schema",
"SQL", "postgres". If searching for "deploy", also try "ship", "release", "CD".

### Layer 2: GitHub search (31,000+ community skills)

If Layer 1 yields nothing, search GitHub for SKILL.md files matching your need:

Use `WebSearch` with queries like:
- `site:github.com Claude Code skill SKILL.md <topic>`
- `site:github.com "claude code" plugin <topic> marketplace`
- `github.com SKILL.md <topic> claude code skill`

Also check known skill aggregators:
- `github.com/buzhangsan/skill-manager` — 31k+ indexed skills
- `github.com/travisvn/awesome-claude-skills` — curated skill directory
- `github.com/davepoon/buildwithclaude` — 20k+ plugin hub
- `skillsmp.com` — web-based skill search with semantic matching

If you find a promising GitHub repo that is a marketplace, install it:
```bash
claude plugin marketplace add <owner/repo>
```

Then search within it for the specific plugin/skill.

### Layer 3: Web search (any publicly documented skill)

Use `WebSearch` to find skills, plugins, or MCP servers for the task domain:
- `Claude Code MCP server <topic> <current year>`
- `Claude Code plugin <topic> install`
- `@anthropic-ai/claude-code skill <topic>`

### Layer 4: npm search (Node.js ecosystem)

Many MCP servers and Claude Code tools are published on npm:
```bash
npm search claude-code <keyword> 2>/dev/null || true
npm search mcp-server <keyword> 2>/dev/null || true
```

Or use `WebSearch`: `npm @anthropic-ai/claude-code <topic> package`

## Step 2: Evaluate what you found

For each candidate, assess:

1. **Relevance** — Does the description directly address the task? Be specific.
2. **Quality signals** — Stars, recent commits, author reputation, documentation.
3. **Setup cost** — Does it need API keys? External services? Complex config?
4. **Maintenance** — Last commit date. Is it actively maintained?

Present findings concisely:

> Found 2 candidates for [task]:
> 1. **[plugin-name]** (marketplace) — [one-line summary]. Stars: X.Xk. Needs: [API key / nothing].
> 2. **[other-name]** (GitHub) — [one-line summary]. Stars: X. Needs: [setup requirements].
>
> Installing #1...

If exactly one match is clearly right, install it without asking. If there are
multiple good options or no clear winner, present the top 2-3 and ask the user.

If nothing relevant exists, tell the user honestly and suggest building it
with the skill-creator skill.

## Step 3: Install

Choose the right installation method:

**From marketplace** (preferred):
```bash
claude plugin install <plugin-name>
# or for a specific marketplace:
claude plugin install <plugin-name>@<marketplace-name>
```

**From GitHub marketplace** (add first, then install):
```bash
claude plugin marketplace add <owner/repo>
claude plugin install <plugin-name>
```

**Standalone SKILL.md from GitHub** (copy to skills directory):
```bash
mkdir -p ~/.claude/skills/<skill-name>
# Download the SKILL.md and bundled files
# Then it auto-discovers on next session start
```

**npm MCP server** (add to project .mcp.json or install globally):
```bash
npm install -g <package-name>
# Then configure in .mcp.json or settings
```

After installation, run `claude plugin list` to confirm the plugin is enabled.

## Step 4: Verify

Do not assume the installation worked. Verify:

```bash
claude plugin details <plugin-name>
```

Check:
- Status shows "enabled"
- Component inventory lists the expected skills/agents/MCP servers
- No error messages

If the plugin provides a skill, read its SKILL.md to understand how to use it:
```bash
find ~/.claude/plugins -path "*/<skill-name>/SKILL.md" -exec cat {} \;
```

Then immediately apply the new capability to the original task. If it doesn't
help, go back to Step 1 and try the next candidate.

## Step 5: Learn and remember

After successfully using a newly installed skill:

- Note what worked in your response to the user
- The installed plugin persists across sessions, so similar future tasks will
  already have the tool available
- If the plugin was a one-time need, you can clean up:
  ```bash
  claude plugin uninstall <plugin-name>
  ```

## Quick-reference: Common task → likely plugin

These patterns come up often. Check the local marketplace first:

| Task | Likely keywords to search |
|---|---|
| GitHub PR/issue management | `github` |
| Linear/Asana/Trello project mgmt | `linear`, `asana` |
| Database schema, migrations | `database`, `postgres`, `sql` |
| Deploy to Vercel/Netlify/AWS | `deploy`, `vercel`, `aws` |
| Security audit, SAST, secrets | `security`, `sast` |
| Playwright browser testing | `playwright`, `browser` |
| Code review automation | `code review`, `review` |
| UI/frontend design | `frontend`, `design`, `ui` |
| API documentation | `api`, `openapi` |
| Monitoring, logging, observability | `monitor`, `log`, `observe` |
| Slack/Discord/Telegram messaging | `slack`, `discord`, `telegram` |
| Infrastructure as code (Terraform) | `terraform`, `iac`, `infra` |

## Troubleshooting

**"Plugin not found"**: The plugin name might differ from the marketplace entry.
Check exact names with `claude plugin list` or search the marketplace manifest.

**Installation fails**: Check the source URL is accessible. Some plugins require
specific tool dependencies (node, python, docker). Install missing dependencies
first.

**Plugin installs but skills don't appear**: Some plugins need a session restart.
If skills aren't visible, the plugin likely provides agents or MCP servers
instead — check with `claude plugin details <name>`.

**Nothing found across all layers**: The skill ecosystem is young. If no plugin
exists, consider whether the task can be done with built-in tools, or whether
building a custom skill with the skill-creator is the right path.
