---
number: 16851
title: Allow configuring a minimum release age for package resolution
type: issue
state: closed
author: majutsushi
labels:
  - enhancement
assignees: []
created_at: 2025-11-25T21:01:49Z
updated_at: 2025-11-25T22:09:45Z
url: https://github.com/astral-sh/uv/issues/16851
synced_at: 2026-01-07T13:12:19-06:00
---

# Allow configuring a minimum release age for package resolution

---

_Issue opened by @majutsushi on 2025-11-25 21:01_

### Summary

With supply chain attacks becoming more common and serious ([recent example from NPM](https://helixguard.ai/blog/malicious-sha1hulud-2025-11-24)), it's important to put some protections in place where possible.

Since these kinds of compromises are usually detected pretty quickly, one simple protection is to not resolve packages that are very new, before package maintainers had a chance to clean up compromised packages. Some JavaScript package managers therefore include a setting that lets you specify a timespan during which new packages should not be considered for resolves, i.e. a minimum release age:
https://pnpm.io/settings#minimumreleaseage
https://bun.com/docs/pm/cli/install#minimum-release-age

Since these kinds of compromises are possible with PyPI as well it would be really useful if uv could implement a similar kind of protection.

Note that this is different from the existing `exclude-newer` setting since that is about configuring a cut-off at a specific point in time, rather than a minimum release age.

### Example

_No response_

---

_Label `enhancement` added by @majutsushi on 2025-11-25 21:01_

---

_Comment by @charliermarsh on 2025-11-25 21:04_

I believe this is covered in https://github.com/astral-sh/uv/issues/14992.

---

_Comment by @majutsushi on 2025-11-25 22:09_

You're right, I missed that in my search of previous issues somehow.

---

_Closed by @majutsushi on 2025-11-25 22:09_

---
