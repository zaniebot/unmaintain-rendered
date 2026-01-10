---
number: 5357
title: Add highlight lines to documentation examples
type: issue
state: open
author: zanieb
labels:
  - documentation
  - wish
assignees: []
created_at: 2024-07-23T17:50:34Z
updated_at: 2024-07-24T16:29:51Z
url: https://github.com/astral-sh/uv/issues/5357
synced_at: 2026-01-10T01:23:48Z
---

# Add highlight lines to documentation examples

---

_Issue opened by @zanieb on 2024-07-23 17:50_

Where relevant, code blocks should use `hl_lines` to highlight the interesting part.

Unfortunately, these can be hard to maintain because there's no indication in an editor if the right line is referenced.

---

_Label `documentation` added by @zanieb on 2024-07-23 17:50_

---

_Label `wish` added by @zanieb on 2024-07-23 17:50_

---

_Comment by @eth3lbert on 2024-07-24 05:40_

Perhaps you could try `linenums=1` to enable line numbers?

FYI: https://squidfunk.github.io/mkdocs-material/reference/code-blocks/#adding-line-numbers

---

_Comment by @zanieb on 2024-07-24 14:55_

I don't really want the line numbers though :D I guess it'd help if you render the docs alongside your editor... but then you should be able to see if the right line is highlighted anyway!

---

_Comment by @eth3lbert on 2024-07-24 16:29_

Is this primarily about the maintenance overhead?

If so, I tested it earlier and found that `linenums=0` hides line numbers while `linenums=1` shows them. I'm not sure if there's a way to set this value dynamically or based on environment variables.

Hopefully, this information is helpful.

---
