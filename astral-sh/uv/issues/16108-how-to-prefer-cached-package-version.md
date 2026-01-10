```yaml
number: 16108
title: How to prefer cached package version?
type: issue
state: open
author: pycaw
labels:
  - question
assignees: []
created_at: 2025-10-02T21:32:25Z
updated_at: 2025-10-07T11:03:36Z
url: https://github.com/astral-sh/uv/issues/16108
synced_at: 2026-01-10T03:23:54Z
```

# How to prefer cached package version?

---

_Issue opened by @pycaw on 2025-10-02 21:32_

### Question

I essentially want to do this, `uv run --with rich example.py`, but in a way so that the version chosen is the latest stable cached one (if there is such, otherwise fall back to default behavior: latest stable -- I think). What do you recommend?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @pycaw on 2025-10-02 21:32_

---

_Comment by @zanieb on 2025-10-02 21:59_

I think you want #10380 or similar

---

_Comment by @pycaw on 2025-10-03 08:36_

> I think you want [#10380](https://github.com/astral-sh/uv/issues/10380) or similar

Sort of, but not quite. OP there writes,

> **Online with Fallback**: Act like the default however, if a network request fails, fall back to --offline type behavior for that dependency and check if it exists in the local cache. If it doesn't, then fail just like how uvx --offline <package not in cache> would fail. e.g. uv --offline-fallback ruff format.

Whereas I'd like to see **Offline with online fallback**. Point here would be to keep complexity of various scripts using `uvx`/`uv run` to the minimum (so no or minimal version specification -- unless it's warranted) and avoid unnecessary network requests to further speed things up.

But, this is probably backwards thinking so for now I think I will settle with using a wrapper command that tries `uv` with `--offline` flag and then depending on that proceeds one way or another. Though... **On second thought, it should be considered legitimate grounds to avoid minor/patch upgrades when running scripts AND to avoid the need to uniformly specify dependencies within them.**

*Question*: Would you happen to know a method to reliably print the packages that are in uv's cache? It being a kind of a black box is a bit disheartening -- even if expected.

<details>
<summary>On another note, I was a bit suprised to discover this sort of redundancy:</summary>

```bash
for f in $(find ~/.cache/uv -name '*.dist-info' | grep rich); do tree --noreport -L1 $(dirname $f); done
```

```
~/.cache/uv/archive-v0/neb81XO1tDHvbSt4V1ZcF
├── rich
└── rich-14.0.0.dist-info
~/.cache/uv/archive-v0/De0fb_wCOY0uJymcu25zD
├── rich
└── rich-14.0.0.dist-info
~/.cache/uv/archive-v0/Ke2lp_lnQsgHE-N9xa6s4
├── rich
└── rich-14.1.0.dist-info
~/.cache/uv/archive-v0/R1zJCwQTde-8guPw6x2CL
├── rich
└── rich-14.1.0.dist-info
~/.cache/uv/archive-v0/2wMhbc3nUzWURYxv4AKDB
├── rich
└── rich-14.1.0.dist-info
~/.cache/uv/archive-v0/o2bfR_w3aXNkRwq1yOElB
├── rich
└── rich-14.1.0.dist-info
~/.cache/uv/archive-v0/p4ROBqHRgZk2csnJ4y7wb
├── rich-14.1.0.dist-info
└── rich.pth
```

And after prune:
```
~/.cache/uv/archive-v0/neb81XO1tDHvbSt4V1ZcF
├── rich
└── rich-14.0.0.dist-info
~/.cache/uv/archive-v0/De0fb_wCOY0uJymcu25zD
├── rich
└── rich-14.0.0.dist-info
~/.cache/uv/archive-v0/R1zJCwQTde-8guPw6x2CL
├── rich
└── rich-14.1.0.dist-info
~/.cache/uv/archive-v0/o2bfR_w3aXNkRwq1yOElB
├── rich
└── rich-14.1.0.dist-info
~/.cache/uv/archive-v0/p4ROBqHRgZk2csnJ4y7wb
├── rich-14.1.0.dist-info
└── rich.pth
```

</details>

---
