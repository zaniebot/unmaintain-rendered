```yaml
number: 18719
title: "[ty] ecosystem-analyzer workflow"
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/ecosystem-analyzer
created_at: 2025-06-17T10:40:12Z
updated_at: 2025-06-20T13:23:24Z
url: https://github.com/astral-sh/ruff/pull/18719
synced_at: 2026-01-10T18:39:08Z
```

# [ty] ecosystem-analyzer workflow

---

_Pull request opened by @sharkdp on 2025-06-17 10:40_

## Summary

Adds a new ecosystem-analyzer workflow with a similar purpose to the mypy-primer workflow. It creates a richer ecosystem diff report using [ecosystem-analyzer](https://github.com/astral-sh/ecosystem-analyzer/) ([example report](https://shark.fish/diff-attr-subscript-narrowing.html)). This is still experimental and also quite a bit slower than mypy_primer, so I chose to make this opt-in for now via a `ecosystem-analyzer` label. This would give us a way to play with this while still evaluating if we should further invest in this or not.

Advantages over the mypy_primer diff output:
- Interactive filtering of diagnostics
- Statistics overview which breaks down added/removed/changed diagnostics across lint rules
- Has the concept of "changed" diagnostics, which makes it easier to review changes where diagnostic messages have changed (along with other changes).
- Compute diff based on old and new project-lists (`good.txt`). This allows us to diff changes to the project list itself. This has caused confusion in the past where we tried to add new projects to `good.txt`, but then ran the `main`-branch version of ty on that new list (where the bug was not yet fixed)

Disadvantages:
- The report currently needs to be downloaded from the workflow run, as I don't know if we have a way of deploying HTML files like this temporarily to some hosted infrastructure.

---

_Label `ci` added by @sharkdp on 2025-06-17 10:40_

---

_Label `ty` added by @sharkdp on 2025-06-17 10:40_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-06-20 09:06_

---

_Marked ready for review by @sharkdp on 2025-06-20 09:17_

---

_Comment by @AlexWaygood on 2025-06-20 13:02_

> This would give us a way to play with this while still evaluating if we should further invest in this or not.

I honestly think it's "useful enough" just for the top-level summary it gives us, which is a fantastic improvement for tackling huge primer reports that sprawl for many, many lines!

> Disadvantages:
> 
>     * The report currently needs to be downloaded from the workflow run, as I don't know if we have a way of deploying HTML files like this temporarily to some hosted infrastructure.

Could we have a bot post a comment with a link that you could click on to download the `diff.html` file...? It's currently quite a few clicks to get to the link to download the HTML file, and you don't get a notification when the workflow has successfully completed either

---

_Comment by @sharkdp on 2025-06-20 13:23_

> Could we have a bot post a comment with a link that you could click on to download the `diff.html` file...? It's currently quite a few clicks to get to the link to download the HTML file, and you don't get a notification when the workflow has successfully completed either

I thought about it, but it's still a horrible experience because it's a zip file. So if we want to move forward with this, I'd really love to have a way to host these reports for a limited period of time (e.g. two months).

---

_Comment by @sharkdp on 2025-06-20 13:23_

Merging this for now, so we can play with it.

---

_Merged by @sharkdp on 2025-06-20 13:23_

---

_Closed by @sharkdp on 2025-06-20 13:23_

---

_Branch deleted on 2025-06-20 13:23_

---
