---
number: 14857
title: "Reference docs sidebar can't be scrolled"
type: issue
state: open
author: roy-work
labels:
  - needs-mre
assignees: []
created_at: 2025-07-23T20:36:51Z
updated_at: 2025-12-18T07:49:10Z
url: https://github.com/astral-sh/uv/issues/14857
synced_at: 2026-01-07T13:12:19-06:00
---

# Reference docs sidebar can't be scrolled

---

_Issue opened by @roy-work on 2025-07-23 20:36_

### Summary

For example, go here: https://docs.astral.sh/uv/reference/settings/

There's enough content on that page that the sidebar overflows the available vertical height on the page, and the sidebar cannot be scrolled. Everything past "no-build-isolation" on the left hand sidebar is inaccessible.

Firefox 141.0b3; 3840x2160 page viewing area as reported by `window.screen`.

<img width="2358" height="1834" alt="Image" src="https://github.com/user-attachments/assets/b1dd99e6-f608-4121-a348-c5e2ab35dba2" />

### Platform

macOS, arm64

### Version

N/A, the website

### Python version

N/A, the website

---

_Label `bug` added by @roy-work on 2025-07-23 20:36_

---

_Comment by @charliermarsh on 2025-07-23 20:38_

Thanks. It scrolls just fine for me in Chrome and Firefox on macOS. You may want to report this to https://github.com/squidfunk/mkdocs-material as we're just using standard Material for MkDocs?

---

_Comment by @zanieb on 2025-07-23 21:03_

It also works fine for me in Vivaldi (Chrome derivative)

---

_Label `bug` removed by @zanieb on 2025-07-24 21:20_

---

_Label `needs-mre` added by @zanieb on 2025-07-24 21:20_

---

_Comment by @frankier on 2025-12-18 07:49_

I've experienced this problem too. The reason is that Tridactyl sometimes breaks pages. It doesn't seem to happen every time -- sometimes refreshing fixes it.

---
