---
number: 9449
title: "Improve documentation on how to do `uv lock --upgrade-package <package>` for optional dependency package"
type: issue
state: closed
author: DeflateAwning
labels:
  - question
assignees: []
created_at: 2024-11-26T19:53:52Z
updated_at: 2024-12-22T13:58:29Z
url: https://github.com/astral-sh/uv/issues/9449
synced_at: 2026-01-07T13:12:18-06:00
---

# Improve documentation on how to do `uv lock --upgrade-package <package>` for optional dependency package

---

_Issue opened by @DeflateAwning on 2024-11-26 19:53_

It is not clear from the documentation and `uv lock -h` docs how to upgrade an optional dependency with the `lock` subcommand.

For example, I have pyright unpinned in my `pyproject.toml` file, in the optional-dependencies / dev section. I want to upgrade it. Is there a feasible way to do that with the `uv lock` command? 

---

_Comment by @charliermarsh on 2024-11-26 19:54_

Does `uv lock --upgrade-package pyright` not work for you?

---

_Label `question` added by @charliermarsh on 2024-11-27 13:40_

---

_Comment by @DeflateAwning on 2024-12-21 23:26_

I'm not sure what I was doing wrong earlier (could have sworn I tried that exact command), but it seems that it does now! Wish I mentioned what version of `uv` I was on back then. 

In the docs, I still think it'd be good to explicitly explain that optional dependencies are locked/upgraded the same as non-optional ones. Thanks for the clarification here! 

---

_Comment by @charliermarsh on 2024-12-22 13:58_

No worries! I re-read the docs, and personally I think the line on upgrades is okay as-is. If we get another issue like this, I'll definitely reconsider.

---

_Closed by @charliermarsh on 2024-12-22 13:58_

---

_Referenced in [MountainGod2/chaturbate_poller#614](../../MountainGod2/chaturbate_poller/pulls/614.md) on 2025-08-18 23:38_

---
