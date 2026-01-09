---
number: 13901
title: Generate an llms-text file to improve the experience of working with AI agents and UV.
type: issue
state: closed
author: zerubeus
labels:
  - enhancement
assignees: []
created_at: 2025-06-08T19:48:09Z
updated_at: 2025-06-23T14:27:10Z
url: https://github.com/astral-sh/uv/issues/13901
synced_at: 2026-01-07T13:12:18-06:00
---

# Generate an llms-text file to improve the experience of working with AI agents and UV.

---

_Issue opened by @zerubeus on 2025-06-08 19:48_

### Summary

Many libraries—especially newer ones with frequent changes—are adopting https://llmstxt.org/. I propose we generate an llms.txt file for uv and other astral-sh projects to improve how LLMs understand and interact with these tools.

Currently, most LLMs struggle with commands like uv, ruff, and ty. Providing an llms.txt would significantly enhance the development experience.

### Example

Here's an example of a doc implementing llm-text https://gofastmcp.com/llms-full.txt

---

_Label `enhancement` added by @zerubeus on 2025-06-08 19:48_

---

_Comment by @zanieb on 2025-06-09 14:28_

There exists another request for this, but I can't find it. We had some discussion about it there. Overall, it'll be non-trivial to maintain unless we're somehow just consolidating the documentation into a single reduced file. (or, providing a brief landing page that links to the documentation)

---

_Referenced in [astral-sh/uv#13929](../../astral-sh/uv/pulls/13929.md) on 2025-06-10 11:46_

---

_Closed by @charliermarsh on 2025-06-10 11:46_

---

_Comment by @charliermarsh on 2025-06-23 14:27_

This now exists: http://docs.astral.sh/uv/llms.txt

---
