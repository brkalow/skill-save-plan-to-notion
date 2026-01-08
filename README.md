# Save Plan to Notion - Claude Code Skill

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that saves implementation plans directly to Notion.

## What it does

When triggered, this skill creates a new Notion page under your designated Plans page with:
- A formatted version of your plan with proper Notion markdown
- The raw plan markdown preserved in a collapsible toggle

## Prerequisites

- Claude Code with the [Notion MCP server](https://github.com/makenotion/notion-mcp-server) configured
- A Notion page where you want to save plans

## Installation

1. **Clone this repository to your Claude Code skills directory:**

   ```bash
   git clone https://github.com/brkalow/skill-save-plan-to-notion.git ~/.claude/skills/save-plan-to-notion
   ```

2. **Configure your Notion page ID:**

   Edit `~/.claude/skills/save-plan-to-notion/SKILL.md` and replace `YOUR_NOTION_PAGE_ID` with your actual Notion page ID.

   To find your page ID:
   - Open your Plans page in Notion
   - Copy the URL (e.g., `https://www.notion.so/workspace/Plans-2e22b9ab44fe805ba74ef50466694ddb`)
   - The page ID is the 32-character string at the end (e.g., `2e22b9ab44fe805ba74ef50466694ddb`)

## Usage

In Claude Code, use any of these trigger phrases:
- "save plan to notion"
- "store plan"
- "persist plan to notion"

Claude will extract the plan from the conversation and save it to your configured Notion page.

## License

MIT
