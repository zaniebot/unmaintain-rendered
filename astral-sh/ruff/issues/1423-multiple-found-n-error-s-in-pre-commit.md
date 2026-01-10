---
number: 1423
title: "Multiple \"Found n error(s)\" in pre-commit"
type: issue
state: closed
author: jad-haddad
labels: []
assignees: []
created_at: 2022-12-28T13:30:29Z
updated_at: 2022-12-28T14:36:32Z
url: https://github.com/astral-sh/ruff/issues/1423
synced_at: 2026-01-10T01:22:39Z
---

# Multiple "Found n error(s)" in pre-commit

---

_Issue opened by @jad-haddad on 2022-12-28 13:30_

I ran the pre-commit on all files and I noticed that the result is reported in a fractionated manner as shown in the image below
![SCR-20221228-j6g](https://user-images.githubusercontent.com/27054843/209815644-7edbc0eb-ab09-42d1-b9ed-72ee0aa690f0.png)
I was expecting an overall result at the end with the total number of errors found.

Is this an expected behaviour? I can't seem to get the pattern of when the result is displayed, it's not per module, nor per rule 🤷‍♂️ 

ruff version: v0.0.198

---

_Comment by @charliermarsh on 2022-12-28 14:25_

Ah, I think we need to set `require_serial: true` in the hook. I added that, but the mirror-maker seems to have ignored or reverted it. Will find a way to fix.

---

_Comment by @charliermarsh on 2022-12-28 14:26_

(To be clear, user error on my part, not the mirror-maker's fault.)

---

_Comment by @charliermarsh on 2022-12-28 14:36_

Ok, this should be fixed in the next release.

---

_Closed by @charliermarsh on 2022-12-28 14:36_

---

_Referenced in [astral-sh/ruff-pre-commit#15](../../astral-sh/ruff-pre-commit/pulls/15.md) on 2022-12-28 14:36_

---
