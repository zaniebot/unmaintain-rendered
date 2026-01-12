```yaml
number: 8536
title: "docs: Apply consistent md code block indentation"
type: pull_request
state: closed
author: doolio
labels: []
assignees: []
base: main
head: consistent-md-code-blk-indentation
created_at: 2023-11-07T09:14:10Z
updated_at: 2023-11-07T18:17:49Z
url: https://github.com/astral-sh/ruff/pull/8536
synced_at: 2026-01-10T23:40:55Z
```

# docs: Apply consistent md code block indentation

---

_Pull request opened by @doolio on 2023-11-07 09:14_

Note, I did not adjust the auto-generated text code blocks in configuration.md as I presumed any changes would be overwritten. To apply the same style for those blocks I presume it would have to be applied from where those blocks are generated. But I was unsure where that was.


---

_Comment by @zanieb on 2023-11-07 14:57_

Is this enforceable by one of our pre-commit tools for linting markdown? I'm not sure I really care either way but if it's not checked by a tool it's probably not going to remain consistent.

---

_Comment by @doolio on 2023-11-07 15:24_

This PR relates to the inconsistency I highlighted [here](https://github.com/astral-sh/ruff/issues/8505#issuecomment-1794505040) and it was explained [later](https://github.com/astral-sh/ruff/issues/8505#issuecomment-1795850229) that it is required or did I misinterpret that explanation. @charliermarsh also added indentation to code blocks (commit https://github.com/astral-sh/ruff/pull/8512/commits/231aff02c9bb3b0d754a15890f2fd20008a7da17) in another PR I submitted so I presumed that was the preferred style.

But in reality my understanding is indentation is **not** necessary if the code block is fenced which they all seemed to be. So if anything the indentation should be removed from all code blocks, no?

Edit: 
> Is this enforceable by one of our pre-commit tools for linting markdown?

I don't know but if so then it doesn't seem to enforce it consistently whichever style is preferred.

---

_Comment by @trag1c on 2023-11-07 17:59_

> did I misinterpret that explanation

yep, I specifically said that
> [...] indented code blocks are only used for tabs because that's the syntax they require [...]

Charlie's commit specifically corrects that

I think this PR can be closed 👍

---

_Comment by @doolio on 2023-11-07 18:06_

Ah I see. Thanks for the clarification.

---

_Closed by @doolio on 2023-11-07 18:06_

---

_Branch deleted on 2023-11-07 18:17_

---
