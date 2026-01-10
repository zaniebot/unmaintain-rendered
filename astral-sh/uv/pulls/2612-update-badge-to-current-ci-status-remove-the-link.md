```yaml
number: 2612
title: "Update badge to current CI status. Remove the link to `/actions`"
type: pull_request
state: merged
author: AucaCoyan
labels: []
assignees: []
merged: true
base: main
head: update-badge
created_at: 2024-03-22T12:30:49Z
updated_at: 2024-03-22T15:41:34Z
url: https://github.com/astral-sh/uv/pull/2612
synced_at: 2026-01-10T14:49:08Z
```

# Update badge to current CI status. Remove the link to `/actions`

---

_Pull request opened by @AucaCoyan on 2024-03-22 12:30_

## Summary

Hi! I noticed the badge in the `README.md` says CI it's failing

[![Actions status](https://github.com/astral-sh/uv/workflows/CI/badge.svg)](https://github.com/astral-sh/uv/actions)

But current main CI is okay. The problem is that badge is bringing the latest actions ran in the repo. Even if it's from a PR
So I changed to this link:

![Action Status](https://github.com/astral-sh/uv/actions/workflows/ci.yml/badge.svg)

It shows the current main branch CI status, but _I couldn't get the link to work (go into `uv/actions`)_. Is it acceptable to lose some functionality in order to display the correct information?

I investigated with google but only found a sample that github is not rendering correctly the badge with the link
[medium post](https://nklya.medium.com/useful-status-badges-for-github-actions-16e658d52ba2), [similar repo with the same badge](https://github.com/Nklya/test-actions)

## Test Plan

manually


---

_@konstin approved on 2024-03-22 12:36_

Thanks!

(See https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows/adding-a-workflow-status-badge#using-the-workflow-file-name for the matching recommendation on github's side)

---

_Merged by @konstin on 2024-03-22 12:38_

---

_Closed by @konstin on 2024-03-22 12:38_

---

_Comment by @AucaCoyan on 2024-03-22 12:44_

Yeah, I followed that recommendation 😄 

---

_Branch deleted on 2024-03-22 12:44_

---

_@T-256 reviewed on 2024-03-22 13:26_

---

_Review comment by @T-256 on `README.md`:7 on 2024-03-22 13:26_

Currently, badge broken to show. I think you missed `.yml`:
```suggestion
![Actions status](https://github.com/astral-sh/uv/actions/workflows/ci.yml/badge.svg)
```

---

_Review comment by @konstin on `README.md`:7 on 2024-03-22 13:49_

Checked the wrong preview, thanks

---

_@konstin reviewed on 2024-03-22 13:49_

---

_Comment by @AucaCoyan on 2024-03-22 14:50_

This badge works:
```markdown
[![Actions status](https://github.com/astral-sh/uv/actions/workflows/ci.yml/badge.svg)](https://github.com/astral-sh/uv/actions)
```
[![Actions status](https://github.com/astral-sh/uv/actions/workflows/ci.yml/badge.svg)](https://github.com/astral-sh/uv/actions)

Thanks! Will do another PR

---

_Comment by @konstin on 2024-03-22 14:50_

Thanks, but i've already done https://github.com/astral-sh/uv/pull/2613

---

_Comment by @AucaCoyan on 2024-03-22 14:56_

Yes, but I meant the link to `uv/actions`. The badge [here](https://github.com/astral-sh/uv/pull/2612#issuecomment-2015269026) is a link to that actions tab

---

_Comment by @konstin on 2024-03-22 15:41_

Sure, feel free to make a PR :)

---
