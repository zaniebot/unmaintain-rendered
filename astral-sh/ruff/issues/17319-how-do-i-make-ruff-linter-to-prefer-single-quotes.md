```yaml
number: 17319
title: How do I make ruff linter to prefer single quotes?
type: issue
state: closed
author: Ignalion
labels:
  - question
assignees: []
created_at: 2025-04-09T15:47:26Z
updated_at: 2025-04-10T19:53:12Z
url: https://github.com/astral-sh/ruff/issues/17319
synced_at: 2026-01-10T11:09:58Z
```

# How do I make ruff linter to prefer single quotes?

---

_Issue opened by @Ignalion on 2025-04-09 15:47_

### Question

I've read all the issues here about that, but nothing of the suggested ways work.

I've tried

```toml
[tool.ruff.flake8-quotes]
inline-quotes = "single"

[tool.ruff.lint.flake8-quotes]
inline-quotes = "single"
```

I've tried the same in ruff.toml, nothing seems to work.
When I run `ruff check test_file.py` it still says that double quotes are preferred.

It is somewhat confusing to me overall since flake8-quotes defaults to 'single', so it should be specifically overriden by ruff somewhat?

### Version

_No response_

---

_Label `question` added by @Ignalion on 2025-04-09 15:47_

---

_Comment by @MichaReiser on 2025-04-09 16:40_

Either of the two should work. Have you explicitly selected the flake8-quotes rules?

```toml
extend-select = ["Q"]
```

The flake8-quotes rules aren't enabled by default

---

_Comment by @Ignalion on 2025-04-10 19:53_

No, I don't include it in `expand-select`, but it's included in `select` along with many others.
I've finally found an option that makes it work.
```toml
[lint.flake8-quotes]
inline-quotes = "single"
```

specifically in ruff.toml works. It's unclear why it doesn't work in pyproject.toml but I'll take that as well

---

_Closed by @Ignalion on 2025-04-10 19:53_

---
