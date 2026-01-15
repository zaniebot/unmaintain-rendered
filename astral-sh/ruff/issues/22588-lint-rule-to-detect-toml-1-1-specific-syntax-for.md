```yaml
number: 22588
title: "Lint rule to detect TOML 1.1 specific syntax for `pyproject.toml`, `pylock.toml`, or any other Python packaging TOML file"
type: issue
state: open
author: notatallshaw
labels: []
assignees: []
created_at: 2026-01-14T23:51:12Z
updated_at: 2026-01-14T23:51:12Z
url: https://github.com/astral-sh/ruff/issues/22588
synced_at: 2026-01-15T00:42:02Z
```

# Lint rule to detect TOML 1.1 specific syntax for `pyproject.toml`, `pylock.toml`, or any other Python packaging TOML file

---

_@notatallshaw_

### Summary

There is an extensive discussion on DPO packaging about tools adopting TOML 1.1: https://discuss.python.org/t/adopting-toml-1-1/105624

A concern by some is that as some tools become permissive about TOML 1.1 specific syntax that users will write TOML 1.1 specific syntax intentionally, or otherwise, and older tools or tools that choose not to be permissive about TOML 1.1 specific syntax will throw an error.

If enacted I also propose this rule would essentially be deprecated once Python 3.15 has reached end of life (around October 2031).

---
