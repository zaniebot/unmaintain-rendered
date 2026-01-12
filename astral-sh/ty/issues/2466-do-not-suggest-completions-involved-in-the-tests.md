```yaml
number: 2466
title: Do not suggest completions involved in the tests for non-first party code
type: issue
state: open
author: BurntSushi
labels:
  - completions
assignees: []
created_at: 2026-01-12T14:09:25Z
updated_at: 2026-01-12T14:09:58Z
url: https://github.com/astral-sh/ty/issues/2466
synced_at: 2026-01-12T15:54:26Z
```

# Do not suggest completions involved in the tests for non-first party code

---

_@BurntSushi_

I have a project with these dependencies:

```toml
[project]
name = "play"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.13"
dependencies = [
    "coverage>=7.13.1",
    "matplotlib>=3.10.8",
    "numpy>=2.4.0",
    "pandas>=2.3.3",
    "psutil>=7.2.1",
    "pygame>=2.6.1",
    "pyqt5>=5.15.11",
    "regex>=2025.11.3",
    "requests>=2.32.5",
    "scikit-learn>=1.8.0",
    "scipy>=1.16.3",
    "whenever>=0.9.4",
]
```

It is not uncommon for me to type something and see a flood of test functions from one of the dependencies. (I'll show a demo in a subsequent comment.) Unless we are in a context where we are writing tests, I don't think we ever want to see these.

The main challenge here is just figuring out what it means for:

1. A symbol to be part of tests.
2. To be writing tests.

We could look at how other completion systems work. And also look at what pytest does for test detection.

---

_Added to milestone `Pre-stable 1` by @BurntSushi on 2026-01-12 14:09_

---

_Label `completions` added by @BurntSushi on 2026-01-12 14:09_

---

_Comment by @BurntSushi on 2026-01-12 14:09_

https://github.com/user-attachments/assets/71189de7-06d7-46f4-b514-c649d856b633

---
