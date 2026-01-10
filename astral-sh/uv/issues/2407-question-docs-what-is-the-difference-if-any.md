---
number: 2407
title: "Question/ docs: What is the difference, if any, between `pip install --pre` and `uv pip install --prerelease=allow`?"
type: issue
state: closed
author: ivirshup
labels:
  - question
assignees: []
created_at: 2024-03-13T10:46:56Z
updated_at: 2024-03-13T14:29:17Z
url: https://github.com/astral-sh/uv/issues/2407
synced_at: 2026-01-10T01:23:17Z
---

# Question/ docs: What is the difference, if any, between `pip install --pre` and `uv pip install --prerelease=allow`?

---

_Issue opened by @ivirshup on 2024-03-13 10:46_

As title, is `uv pip install --prerelease=allow` equivalent to `pip install --pre`? Are there differences I should be aware of?

I'm looking at switching some CI processes over to `uv`, but I'm having a little trouble understanding if I can assume `--prerelease=allow` is equivalent to `--pre`. This is more an issue of not really understanding what `pip`'s `--pre` argument does, since the documentation for it is less specific than the docs here.

Thanks for any guidance!

---

_Comment by @charliermarsh on 2024-03-13 14:20_

Good question: there's no difference at all. `--pre` is just an alias for `--prerelease=allow`, to match pip's interface.

---

_Comment by @charliermarsh on 2024-03-13 14:20_

They both mean the same thing: allow prereleases for any dependency.

---

_Label `question` added by @charliermarsh on 2024-03-13 14:20_

---

_Closed by @charliermarsh on 2024-03-13 14:29_

---
