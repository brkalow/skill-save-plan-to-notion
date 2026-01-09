---
name: save-plan-to-notion
description: Save a generated plan to Notion. Use this to persist implementation plans, project plans, or any structured plan document to the Plans page in Notion. Triggers on "save plan to notion", "store plan", "persist plan to notion".
---

# Save Plan to Notion

This skill saves plans to Notion as sub-pages under the Plans page.

## Target Location

Parent page ID: `YOUR_NOTION_PAGE_ID`
Parent page URL: https://www.notion.so/YOUR_WORKSPACE/Plans-YOUR_NOTION_PAGE_ID

> **Setup**: Replace `YOUR_NOTION_PAGE_ID` above with the ID of your Notion page where plans should be saved. You can find the page ID in the URL of your Notion page (the 32-character string after the page title).

## Required Page Format

Each plan page follows this structure:

1. **Title**: The plan name (extracted from the plan or provided explicitly)
2. **Formatted Plan**: Heading `# Formatted plan` followed by the plan rendered with proper Notion markdown
3. **Divider**: Horizontal rule `---`
4. **Raw Plan**: The COMPLETE original plan markdown in a collapsible toggle block with escaped code blocks

## CRITICAL Requirements

### 1. Use Markdown Tables (NOT HTML)

Notion does NOT render HTML `<table>` tags properly. Always use markdown tables:

```markdown
| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Value 1  | Value 2  | Value 3  |
```

NEVER use `<table>`, `<tr>`, `<td>` HTML tags.

### 2. Escape Code Blocks in Raw Plan Section

**The raw plan section MUST escape ALL triple backtick delimiters.**

Before inserting the raw plan into the code block:
1. Replace all opening ``` (with or without language) with: `[CODEBLOCK:lang]` where lang is the language (or empty)
2. Replace all closing ``` with: `[/CODEBLOCK]`

Example transformation:
- Input: ` ```javascript\ncode here\n``` `
- Output: `[CODEBLOCK:javascript]\ncode here\n[/CODEBLOCK]`

### 3. Include the COMPLETE Raw Plan

The raw plan section must contain the ENTIRE original plan content, not a summary or placeholder. This is critical for reference.

## Instructions

When saving a plan:

1. **Read the plan file** to get the full content

2. **Extract the title** from the plan content (usually the first `#` heading)

3. **Prepare the raw plan**:
   - Take the COMPLETE plan content
   - Replace all ` ``` ` with `[CODEBLOCK:lang]` and `[/CODEBLOCK]`

4. **Use the `mcp__notion__notion-create-pages` tool** with:
   - `parent`: `{"page_id": "YOUR_NOTION_PAGE_ID"}`
   - `pages`: Array with one page object containing `properties` and `content`

5. **Format the content** using this structure:

```
# Formatted plan

{the plan content with:
 - Proper markdown headings
 - Markdown tables (NOT HTML)
 - Code blocks with language tags
 - Lists and checkboxes}

---

▶ Raw Plan Markdown
	```text
	{COMPLETE raw plan with ALL code blocks escaped using [CODEBLOCK:lang] and [/CODEBLOCK]}
	```
```

## Example

Given this plan:
```markdown
## Add User Authentication

### Overview
Implement OAuth2 authentication.

| Feature | Status |
|---------|--------|
| Login   | Done   |

### Code Example
```typescript
const auth = new Auth();
```
```

Create a page with content:

```
# Formatted plan

## Overview
Implement OAuth2 authentication.

| Feature | Status |
|---------|--------|
| Login   | Done   |

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

	| Feature | Status |
	|---------|--------|
	| Login   | Done   |

	### Code Example
	[CODEBLOCK:typescript]
	const auth = new Auth();
	[/CODEBLOCK]
	```
```

## After Creating

Return the Notion page URL to confirm the plan was saved successfully.
