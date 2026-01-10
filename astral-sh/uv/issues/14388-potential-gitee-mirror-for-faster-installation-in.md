---
number: 14388
title: Potential Gitee Mirror for Faster Installation in China
type: issue
state: closed
author: machenme
labels:
  - enhancement
assignees: []
created_at: 2025-07-01T10:13:37Z
updated_at: 2025-07-01T11:27:48Z
url: https://github.com/astral-sh/uv/issues/14388
synced_at: 2026-01-10T01:25:44Z
---

# Potential Gitee Mirror for Faster Installation in China

---

_Issue opened by @machenme on 2025-07-01 10:13_

### Summary

We've noticed that GitHub access can be unreliable in mainland China, which significantly impacts the installation process for `uv`. Currently, using the standard PowerShell installation command:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

often results in extremely slow download speeds (sometimes taking 20+ minutes) or outright failures due to network restrictions.

Many popular developer tools (like Powerlevel10k) have addressed this by providing Gitee (a popular Chinese Git hosting service) mirrors to improve accessibility for users in China. 

Are there any plans to add a Gitee mirror for `uv`'s installation script or binaries to improve installation reliability/speed for Chinese users?

Regardless of the outcome, I want to emphasize that `uv` is hands-down the best Python package manager I've used - its performance and design are truly exceptional. We'll continue using it enthusiastically regardless of network conditions!

Thank you for your hard work on this amazing tool!

### Example

eg: power10k 
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git

git clone --depth=1 https://gitee.com/romkatv/powerlevel10k.git

---

_Label `enhancement` added by @machenme on 2025-07-01 10:13_

---

_Comment by @FishAlchemist on 2025-07-01 11:00_

Set ``UV_INSTALLER_GHE_BASE_URL`` or ``UV_INSTALLER_GITHUB_BASE_URL``, then just mirror it yourself. 
https://docs.astral.sh/uv/reference/environment/#uv_installer_ghe_base_url
This schema:
``$installer_base_url/astral-sh/uv/releases/download/0.7.17``
Maintaining an additional platform for non-global demands is, after all, not very realistic, in my opinion.

---

_Closed by @machenme on 2025-07-01 11:27_

---
