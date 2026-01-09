---
number: 19369
title: Renovate does not update 2 sha-pinned dependencies used in docs build
type: issue
state: open
author: corneliusroemer
labels:
  - documentation
assignees: []
created_at: 2025-07-15T18:15:58Z
updated_at: 2025-07-15T19:56:55Z
url: https://github.com/astral-sh/ruff/issues/19369
synced_at: 2026-01-07T13:12:16-06:00
---

# Renovate does not update 2 sha-pinned dependencies used in docs build

---

_Issue opened by @corneliusroemer on 2025-07-15 18:15_

@ntBre pointed out in #19362 that mkdocs-material was likely lagging behind the latest version. I tried to find out why this dependency wasn't automatically updated and found that 2 dependencies used by the docs build are sha-pinned which renovate doesn't seem to (know how to) update. As renovate seems to be used widely, there might be a false sense of "everything is always kept up to date" so I thought I make an issue to document the status quo which is likely not the ideal state.

These 2 dependencies aren't updated by renovate:

https://github.com/astral-sh/ruff/blob/c0d04f2d56a7de8c7a6ef3f7976a7a59925d5acc/docs/requirements-insiders.txt#L4

https://github.com/astral-sh/ruff/blob/c0d04f2d56a7de8c7a6ef3f7976a7a59925d5acc/docs/requirements-insiders.txt#L8

I've noticed that astral-sh's fork of `mkdocs-material-insiders` is not public/OSS ~(@ntBre is this on purpose? It makes it hard for outsiders to investigate docs related stuff but that might be a conscious decision)~ this seems to be due to sponsorship/licensing reasons as I learned in CONTRIBUTING.md:
https://github.com/astral-sh/ruff/blob/a0d4e1f85408942479d96a47443a7a9b7fc34ee3/CONTRIBUTING.md#L282-L287


I'm not sure why an ssh pin is used for the public dependency: https://github.com/PGijsbers/admonitions - there's no comment in the requirements.txt. It could be pip-installed: `python -m pip install mkdocs-github-admonitions-plugin` and hence added to requirements.txt AFAICT


---

_Label `documentation` added by @ntBre on 2025-07-15 18:32_

---

_Comment by @ntBre on 2025-07-15 18:44_

I actually meant that the statement on that issue from 2020 could be out of date, not the dependency 😅 

But nice sleuthing here! For the admonitions, I think the pin was added for a specific issue mentioned in https://github.com/astral-sh/ruff/pull/18163, which I think should be fixed now. We could probably remove that pin now at least.

---

_Referenced in [astral-sh/ruff#19373](../../astral-sh/ruff/pulls/19373.md) on 2025-07-15 19:12_

---

_Referenced in [astral-sh/ruff#19375](../../astral-sh/ruff/pulls/19375.md) on 2025-07-15 19:45_

---

_Comment by @corneliusroemer on 2025-07-15 19:56_

Ah great find! I've made a PR for the admonitions unpin in #19375 so that just leaves the insiders pin which I'm also addressing as well as it can be I think in the same PR by adding a comment. Not much more that can be done other than maybe setting up some sort of periodic reminder to update the remaining pinned dep as renovate doesn't handle that.

---
