---
number: 10185
title: "[website] monospace text should include `monospace` as final fallback"
type: issue
state: closed
author: WhyNotHugo
labels:
  - documentation
assignees: []
created_at: 2024-03-01T14:31:53Z
updated_at: 2024-03-03T18:17:53Z
url: https://github.com/astral-sh/ruff/issues/10185
synced_at: 2026-01-07T13:12:15-06:00
---

# [website] monospace text should include `monospace` as final fallback

---

_Issue opened by @WhyNotHugo on 2024-03-01 14:31_

This issue is about astral.sh, not ruff itself. I didn't find the repository for the website, so I hope reporting here is okay (is the website still under somebody's personal account?).

The rule for `code, kbd, pre, samp` includes:

    font-family: Roboto Mono;

This will use the default serif font as fallback on situations where this font is unavailable. Example:

![screenshot showing a code block rendered with sans serif font](https://github.com/astral-sh/ruff/assets/730811/43a07e0b-eddb-4ee7-ad8e-6fff81bc7d24)

The rule should include `monospace` as a final fallback. E.g.:

    font-family: Roboto Mono, monospace;



---

_Comment by @charliermarsh on 2024-03-01 14:51_

Thanks! Totally fine to report here in absence of better options. (The repo is in our GitHub Org, but it's private since we often draft announcements there etc.)

---

_Label `bug` added by @charliermarsh on 2024-03-01 14:51_

---

_Referenced in [astral-sh/astral-sh#64](../../astral-sh/astral-sh/pulls/64.md) on 2024-03-01 16:38_

---

_Label `documentation` added by @MichaReiser on 2024-03-01 16:39_

---

_Label `bug` removed by @MichaReiser on 2024-03-01 16:39_

---

_Comment by @MichaReiser on 2024-03-01 16:39_

Thanks. I added `Courier New` and `monospace` as fallback fonts.

---

_Closed by @MichaReiser on 2024-03-01 16:39_

---

_Comment by @WhyNotHugo on 2024-03-03 18:17_

Thanks!

---
