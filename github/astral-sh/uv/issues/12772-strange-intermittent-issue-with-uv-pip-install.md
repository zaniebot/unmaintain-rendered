---
number: 12772
title: Strange intermittent issue with uv pip install failing on multiple versions
type: issue
state: open
author: kimdwkimdw
labels:
  - question
assignees: []
created_at: 2025-04-09T05:54:17Z
updated_at: 2025-04-09T05:56:55Z
url: https://github.com/astral-sh/uv/issues/12772
synced_at: 2026-01-07T13:12:18-06:00
---

# Strange intermittent issue with uv pip install failing on multiple versions

---

_Issue opened by @kimdwkimdw on 2025-04-09 05:54_

### Question

Hi, first of all—thank you for the great work on uv. I’ve been using it happily, but I encountered a very strange issue yesterday and I’m hoping to understand if it might be related to uv.

I’m using uv with multiple index URLs configured in my pyproject.toml like this:

```toml
[tool.uv.pip]
extra-index-url = ["https://pypi.org/simple", "https://pypi.private.domain/"]
```

Normally everything works fine, but yesterday(specifically from PST 1:00 ~ 7:00), both of the following commands suddenly started failing:

```sh
uv pip compile
uv pip install
```

However, running pip install directly (outside of uv) worked without any issues.

At first, I thought this might be a bug in a specific version of uv==0.6.x, but I found that no version of uv worked, until I downgraded all the way to uv==0.5.31, which seemed to resolve the issue.

The most confusing part is that I had been using uv==0.6.5 just fine before, but after the issue started, even rolling back to that version didn’t help. Only switching to 0.5.31 fixed things.

Even more mysteriously, I re-ran a GitHub Action workflow just now that had been failing yesterday—and now it passes without any changes on my end.

Since pip install always worked while uv pip install failed, I’m quite curious what could have caused this, and whether uv’s index handling might be involved.

Thanks in advance for any insights!

### Platform

linux

### Version

0.6.x

---

_Label `question` added by @kimdwkimdw on 2025-04-09 05:54_

---
