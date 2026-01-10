---
number: 15180
title: "uv python install --reinstall should not add a bin symlink where one doesn't exist"
type: issue
state: open
author: geofft
labels:
  - bug
assignees: []
created_at: 2025-08-08T23:15:56Z
updated_at: 2025-08-09T17:01:17Z
url: https://github.com/astral-sh/uv/issues/15180
synced_at: 2026-01-10T01:25:53Z
---

# uv python install --reinstall should not add a bin symlink where one doesn't exist

---

_Issue opened by @geofft on 2025-08-08 23:15_

### Summary

`uv python install --reinstall` is not quite like `uv python install`; it's used when you want to force reinstalling a particular version that you may have implicitly gotten from `uv run -p 3.x` etc. There isn't (to my knowledge) some other command to do this, especially not for all versions you have. So I think that it shouldn't default to adding ~/local/bin/python3.x symlinks where they didn't previously exist, because it's not the same kind of explicit request. (I think basically I'm asking for `--reinstall` to flip the default to `--no-bin`.)

Sort of in the same spirit as #15178; see also #15179.

### Platform

Linux

### Version

0.8.7

### Python version

_No response_

---

_Label `bug` added by @geofft on 2025-08-08 23:15_

---

_Comment by @zanieb on 2025-08-08 23:54_

>  it's used when you want to force reinstalling a particular version that you may have implicitly gotten from uv run -p 3.x etc

I don't think this is quite the right framing for this, because we _should_ add markers to identify implicitly installed versions and avoid adding bin symlinks for those too.

I the main question here is should `uv python install 3.13 --no-bin` then `uv python install 3.13 --reinstall` install bin links?

---

_Comment by @jtfmumm on 2025-08-09 17:01_

My inclination is to respect the original `--no-bin` request. But I don't think the default should be `--no-bin`. Basically, if there are already bin links, reinstall them; otherwise, don't install bin links.

---
