```yaml
number: 11615
title: "Add Vim and Kate setup guide for `ruff server`"
type: pull_request
state: merged
author: JaRoSchm
labels:
  - documentation
assignees: []
merged: true
base: main
head: server-vim-kate-setup
created_at: 2024-05-30T08:32:46Z
updated_at: 2024-05-31T19:10:16Z
url: https://github.com/astral-sh/ruff/pull/11615
synced_at: 2026-01-10T21:56:00Z
```

# Add Vim and Kate setup guide for `ruff server`

---

_Pull request opened by @JaRoSchm on 2024-05-30 08:32_

## Summary

In the [roadmap for `ruff server`](https://github.com/astral-sh/ruff/discussions/10581) support for vim and kate is listed. Therefore I added setup guides for them based on the neovim guide. As I don't use pyright I wasn't able to translate the corresponding part from the neovim guide.

## Test Plan

Doesn't apply.


---

_Review comment by @dhruvmanila on `crates/ruff_server/docs/setup/VIM.md`:42 on 2024-05-31 07:08_

I think this requires an example configuration? Do you have that? If not, we can remove this part.

---

_@dhruvmanila approved on 2024-05-31 07:09_

I pushed some minor nits, and apart from that one question this is good to go. Thank you for working on this!

---

_Label `documentation` added by @dhruvmanila on 2024-05-31 07:09_

---

_Renamed from "DOCS: Add vim and kate setup to `ruff server`" to "Add Vim and Kate setup guide for `ruff server`" by @dhruvmanila on 2024-05-31 07:09_

---

_@JaRoSchm reviewed on 2024-05-31 07:23_

---

_Review comment by @JaRoSchm on `crates/ruff_server/docs/setup/VIM.md`:42 on 2024-05-31 07:23_

It took that from the Neovim setup guide. However, as noted in my post above, I don't use pyright personally, so I am not able to translate this part.

I am using `ruff server` in combination with pylsp. If this is helpful for someone, I could add the configuration necessary for this combination.

---

_@charliermarsh approved on 2024-05-31 19:03_

Thanks!

---

_Merged by @charliermarsh on 2024-05-31 19:06_

---

_Closed by @charliermarsh on 2024-05-31 19:06_

---

_Branch deleted on 2024-05-31 19:10_

---
