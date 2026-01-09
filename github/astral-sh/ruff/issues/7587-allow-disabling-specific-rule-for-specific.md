---
number: 7587
title: ✨ Allow disabling specific rule for specific directory or file
type: issue
state: closed
author: jd-solanki
labels: []
assignees: []
created_at: 2023-09-22T03:31:08Z
updated_at: 2023-09-22T06:17:49Z
url: https://github.com/astral-sh/ruff/issues/7587
synced_at: 2026-01-07T13:12:15-06:00
---

# ✨ Allow disabling specific rule for specific directory or file

---

_Issue opened by @jd-solanki on 2023-09-22 03:31_

Hi 👋🏻 

I'm loving Ruff so far. Today I was writing some tests and found that I'm getting an error on the `assert` statement due to [this](https://beta.ruff.rs/docs/rules/assert) rule in my tests file. I want this rule for my codebase but not for my tests. Additionally, there are some other rules that I don't want to enable in tests like [magic value comparison](https://beta.ruff.rs/docs/rules/magic-value-comparison).

For this kind of scenario, I would like to disable the rule for the specific directories.

Thanks for such a great library ❤️ 

> P.S. Thanks to the ruff I was able to identify one minor type improvement in FastAPI docs and made my first contribution 😇 

---

_Comment by @charliermarsh on 2023-09-22 03:34_

Thanks for the kind words, it made my day.

Can you accomplish what you need with [per-file-ignores](https://docs.astral.sh/ruff/settings/#per-file-ignores)? In your `pyproject.toml`, you'd typically do something like:

```toml
[tool.ruff.per-file-ignores]
"tests/**" = ["S101", "PLR2004"]
```

---

_Closed by @jd-solanki on 2023-09-22 06:17_

---
