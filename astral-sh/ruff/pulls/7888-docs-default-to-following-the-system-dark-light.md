```yaml
number: 7888
title: "[docs] Default to following the system dark/light mode"
type: pull_request
state: merged
author: flying-sheep
labels: []
assignees: []
merged: true
base: main
head: fix-theme-toggle
created_at: 2023-10-10T07:36:00Z
updated_at: 2023-10-10T21:02:29Z
url: https://github.com/astral-sh/ruff/pull/7888
synced_at: 2026-01-12T02:32:41Z
```

# [docs] Default to following the system dark/light mode

---

_Pull request opened by @flying-sheep on 2023-10-10 07:36_

## Summary

This fixes the theme toggle so it has the most useful setting on by default: following the system dark/light mode

See https://squidfunk.github.io/mkdocs-material/setup/changing-the-colors/#automatic-light-dark-mode

## Test Plan

It follows the official docs exactly. If the docs get published for each PR branch, we can also check there if it works as intended.

---

_Renamed from "Fix theme toggle" to "[docs] Fix theme toggle" by @flying-sheep on 2023-10-10 07:56_

---

_Comment by @konstin on 2023-10-10 08:32_

That is a good idea, unfortunately it doesn't seem to work for me (firefox on ubuntu 22.04). Other tabs change theme, but the ruffs docs don't. I can see the "Linting the CPython codebase from scratch" figure switch themes though so i'm surprised the rest of the page doesn't.

---

_Renamed from "[docs] Fix theme toggle" to "[docs] Default to following the system dark/light mode" by @konstin on 2023-10-10 08:32_

---

_Comment by @flying-sheep on 2023-10-10 09:01_

It says the change is only in tag [insiders-4.18.0](https://squidfunk.github.io/mkdocs-material/insiders/changelog/#4.18.0), and the little heart says “sponsors only”.

![grafik](https://github.com/astral-sh/ruff/assets/291575/923a3fdb-ec11-446c-bb23-84abeddbe7a3)

I’m not sure how building ruff’s docs works. Does the presence of [mkdocs.insiders.yml](https://github.com/astral-sh/ruff/blob/main/mkdocs.insiders.yml) mean that the docs can be built with and without insiders “mode”?

If that assumption is correct, my change should probably live in the mkdocs.insiders.yml instead.

---

_Comment by @charliermarsh on 2023-10-10 11:58_

@konstin - Just confirming, are you building with insiders locally?

---

_Comment by @konstin on 2023-10-10 12:06_

Nope sorry i missed, i still have to check this with insiders (or someone else with access, i'm currently working on #7873 and would return to this PR later today)

---

_@konstin approved on 2023-10-10 14:55_

With the insiders version it works, thanks!

---

_Merged by @konstin on 2023-10-10 15:25_

---

_Closed by @konstin on 2023-10-10 15:25_

---

_Branch deleted on 2023-10-10 21:02_

---
