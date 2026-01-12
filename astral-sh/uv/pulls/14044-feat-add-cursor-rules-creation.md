```yaml
number: 14044
title: "Feat: add cursor rules creation"
type: pull_request
state: closed
author: IsaacGemal
labels: []
assignees: []
base: main
head: isaacgemal/add-cursor-rules-on-init
created_at: 2025-06-15T22:39:36Z
updated_at: 2025-06-30T22:37:11Z
url: https://github.com/astral-sh/uv/pull/14044
synced_at: 2026-01-12T16:10:59Z
```

# Feat: add cursor rules creation

---

_@IsaacGemal_

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Main points
  - Automatically creates `.cursor/rules/use-uv-instead-of-pip-poetry-conda.mdc` during `uv init` when Cursor is detected
  - Controlled by `--cursor-rules`/`--no-cursor-rules` flags (defaults to enabled)
  - Cross-platform Cursor detection (macOS, Windows, Linux)
  - Rules file guiding AI assistants to use uv over pip/poetry/conda

Why? 
   - The purpose of this change is to guide AI agents such as the ones that cursor uses immediate context and understanding as to how to use UV. Because of how new UV is, LLMs don't understand the syntax beyond a surface level. As these AIs write more and more code, we need to consider about how we can routinely and clearly provide them with context.
   - This was directly inspired by Jared Sumner who implemented this exact feature for Bun JS, see [here](https://bun.sh/blog/bun-v1.2.15#bun-init-cursor-rule) which provides the info from [this](https://github.com/oven-sh/bun/blob/main/src/init/rule.md) file. I really liked the idea, so I figured why not get the ball rolling myself. 

## Test Plan

<!-- How was it tested? -->

I created two regression tests for it, roughly in a similar format.

## Notes 

Side note - this is my first time ever contributing to a serious rust project, so I'm sure I missed some details. A few things off the top of my head - such as where to place the md file (are there performance considerations of where in the file tree the file is located?) Next, there's probably some refactoring / some code might belong in better areas, I'm not familiar with the entire codebase. Lastly, I think the content in the use uv markdown could use some adjustments. For instance, at the end of bun's md file it says
> For more information, read the Bun API docs in node_modules/bun-types/docs/**.md.

But I wasn't sure how to translate that for UV specifically.


---

_Comment by @konstin on 2025-06-16 09:38_

While I agree that we should add more guidance for LLMs, I think it's too early to add something tool-specific in the CLI. The space of LLMs tooling is still quickly evolving, so I'm hesitant to commit to adding something tool-specific in an interface where removal are breaking changes right now.

---

_Comment by @IsaacGemal on 2025-06-16 12:10_

What would you suggest would be a more practical solution then / tie into existing issues.

---

_Comment by @IsaacGemal on 2025-06-30 13:50_

Closed for now as this is quite a good compromise!
Thanks Charlie
https://x.com/charliermarsh/status/1937502689400881309

---

_Closed by @IsaacGemal on 2025-06-30 13:50_

---

_Comment by @konstin on 2025-06-30 13:53_

For future contest: Both codex and gemini cli now support `AGENTS.md`. If the ecosystem converges further, we can revisit those changes. And of course improve our llms.txt to be more useful in the meantime :)

---

_Branch deleted on 2025-06-30 22:37_

---
