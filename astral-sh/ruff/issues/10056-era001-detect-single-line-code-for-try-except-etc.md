```yaml
number: 10056
title: "ERA001: detect single-line code for try:, except:, etc."
type: issue
state: closed
author: ottaviohartman
labels:
  - rule
assignees: []
created_at: 2024-02-20T03:47:47Z
updated_at: 2024-02-20T17:40:20Z
url: https://github.com/astral-sh/ruff/issues/10056
synced_at: 2026-01-12T15:54:49Z
```

# ERA001: detect single-line code for try:, except:, etc.

---

_@ottaviohartman_

```
❯ ruff --version
ruff 0.2.2
```

As discussed in #10055 , detect single-line code for the eradicate rule:

Example [playground](https://play.ruff.rs/6d31dca7-7abc-4a8f-8f29-3c4547e29f73)
```python
# try:
# ^ works

# try:  # doesn't work

# try: print()
# ^ doesn't work

# same with except, which also doesn't correctly detect plain `expect:`

# except:
# ^ doesn't work

# except Foo:
# ^ works

# except Exception as e:
# ^ works

# except Exception as e: print()
# ^ doesn't work
```

---

_Label `rule` added by @charliermarsh on 2024-02-20 16:26_

---

_Closed by @MichaReiser on 2024-02-20 17:40_

---
