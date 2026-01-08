---
name: save-plan-to-notion
description: Save a generated plan to Notion. Use this to persist implementation plans, project plans, or any structured plan document to a Plans page in Notion. Triggers on "save plan to notion", "store plan", "persist plan to notion".
---

# Save Plan to Notion

This skill saves plans to Notion as sub-pages under a designated Plans page.

## Target Location

Parent page ID: `YOUR_NOTION_PAGE_ID`
Parent page URL: https://www.notion.so/YOUR_WORKSPACE/Plans-YOUR_NOTION_PAGE_ID

> **Setup**: Replace `YOUR_NOTION_PAGE_ID` above with the ID of your Notion page where plans should be saved. You can find the page ID in the URL of your Notion page (the 32-character string after the page title).

## Required Page Format

Each plan page follows this structure:

1. **Title**: The plan name (extracted from the plan or provided explicitly)
2. **Formatted Plan**: Heading `# Formatted plan` followed by the plan rendered with proper Notion markdown
3. **Divider**: Horizontal rule `---`
4. **Raw Plan**: The original plan markdown in a collapsible toggle block

## Instructions

When saving a plan:

1. **Extract the title** from the plan content (usually the first heading) or use a provided title

2. **Use the `mcp__notion__notion-create-pages` tool** with these parameters:
   - `parent`: `{"page_id": "YOUR_NOTION_PAGE_ID"}`
   - `pages`: Array with one page object containing `properties` and `content`

3. **Format the content** using this structure:

```
# Formatted plan
{the plan content rendered with proper Notion markdown - headings, lists, checkboxes, code blocks, tables, etc.}

---

▶ Raw Plan Markdown
	```text
	{raw plan with all triple backticks replaced with CODEBLOCK_START and CODEBLOCK_END markers}
	```
```

## CRITICAL: Handling Code Blocks in Raw Plan

**The raw plan section MUST escape code block delimiters to prevent parsing conflicts.**

Before inserting the raw plan into the code block:
1. Replace all opening ``` (with or without language) with: `[CODEBLOCK:lang]` where lang is the language (or empty)
2. Replace all closing ``` with: `[/CODEBLOCK]`

Example transformation:
- Input: ` ```javascript\ncode here\n``` `
- Output: `[CODEBLOCK:javascript]\ncode here\n[/CODEBLOCK]`

This prevents nested code block delimiter conflicts in Notion.

## Example

Given this plan:
```markdown
## Add User Authentication

### Overview
Implement OAuth2 authentication.

### Code Example
```typescript
const auth = new Auth();
```
```

Create a page with:
- **Title**: "Add User Authentication"
- **Content**:

```
# Formatted plan
## Overview
Implement OAuth2 authentication.

### Code Example
```typescript
const auth = new Auth();
```

---

▶ Raw Plan Markdown
	```text
	## Add User Authentication

	### Overview
	Implement OAuth2 authentication.

	### Code Example
	[CODEBLOCK:typescript]
	const auth = new Auth();
	[/CODEBLOCK]
	```
```

## After Creating

Return the Notion page URL to confirm the plan was saved successfully.
