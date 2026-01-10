---
number: 1655
title: "Add `pip cache info` and `pip cache list`"
type: issue
state: open
author: hugovk
labels:
  - enhancement
  - cache
assignees: []
created_at: 2024-02-18T15:45:44Z
updated_at: 2024-09-27T04:13:36Z
url: https://github.com/astral-sh/uv/issues/1655
synced_at: 2026-01-10T01:23:08Z
---

# Add `pip cache info` and `pip cache list`

---

_Issue opened by @hugovk on 2024-02-18 15:45_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Follow on from https://github.com/astral-sh/uv/issues/1646#issuecomment-1951356710.

It would be useful to add something like  `pip cache info`, because we will end up with large pip and uv caches on some systems:

```console
❯ pip cache info
Package index page cache location (pip v23.3+): /Users/hugo/Library/Caches/pip/http-v2
Package index page cache location (older pips): /Users/hugo/Library/Caches/pip/http
Package index page cache size: 187.6 MB
Number of HTTP files: 870
Locally built wheels location: /Users/hugo/Library/Caches/pip/wheels
Locally built wheels size: 5.9 MB
Number of locally built wheels: 10
```

Something like `pip cache list` could be useful too (it's normally _much_ bigger than this, but I recently did `pip cache clear`!):

```console
❯ pip cache list
Cache contents:

 - MarkupSafe-2.1.5-cp313-cp313d-macosx_14_0_arm64.whl (15 kB)
 - cffi-1.16.0-cp38-cp38-macosx_11_0_universal2.whl (256 kB)
 - coverage-7.4.1-cp313-cp313d-macosx_14_0_arm64.whl (208 kB)
 - pillow-10.3.0.dev0-cp312-cp312-macosx_10_9_universal2.whl (888 kB)
 - pip-24.1.dev0-py3-none-any.whl (2.1 MB)
 - pip-24.1.dev0-py3-none-any.whl (2.1 MB)
 - scripttest-1.3-py3-none-any.whl (7.8 kB)
 - simple-1.0-py3-none-any.whl (1.1 kB)
 - simple2-3.0-py3-none-any.whl (1.1 kB)
 - tinytext-3.6.1.dev64-py3-none-any.whl (4.4 kB)
```


---

_Referenced in [astral-sh/uv#1646](../../astral-sh/uv/issues/1646.md) on 2024-02-18 15:46_

---

_Label `enhancement` added by @zanieb on 2024-02-18 17:29_

---

_Label `cache` added by @zanieb on 2024-02-18 17:29_

---

_Comment by @vbmade2000 on 2024-09-26 06:22_

@zanieb  I want to take a chance at this issue.  

---

_Comment by @zanieb on 2024-09-26 12:20_

@vbmade2000 I'm not sure this is a good first issue as there's a fair amount of design work that needs to be done. You could start looking into it if you want and put forward a proposal, but just note that adding new commands is a big task with a lot of user-experience concerns.

---

_Comment by @vbmade2000 on 2024-09-27 04:13_

I see. I'll pick up later probably. Thanks.

---
