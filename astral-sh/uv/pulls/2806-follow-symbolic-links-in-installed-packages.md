```yaml
number: 2806
title: Follow symbolic links in installed packages
type: pull_request
state: closed
author: zanieb
labels:
  - bug
assignees: []
draft: true
base: main
head: zb/site-packages-link
created_at: 2024-04-03T14:31:42Z
updated_at: 2024-04-12T13:52:50Z
url: https://github.com/astral-sh/uv/pull/2806
synced_at: 2026-01-10T14:43:31Z
```

# Follow symbolic links in installed packages

---

_Pull request opened by @zanieb on 2024-04-03 14:31_

Closes https://github.com/astral-sh/uv/issues/2798

---

_Label `bug` added by @zanieb on 2024-04-03 14:31_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/site_packages.rs`:56 on 2024-04-03 14:50_

What about deduplicating in `venv.site_packages()`?

---

_@charliermarsh reviewed on 2024-04-03 14:50_

---

_@zanieb reviewed on 2024-04-03 14:57_

---

_Review comment by @zanieb on `crates/uv-installer/src/site_packages.rs`:56 on 2024-04-03 14:57_

Seems like it would be faster, yeah. I guess both might make sense. Do you see a downside to this approach?

---

_@charliermarsh reviewed on 2024-04-03 15:26_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/site_packages.rs`:56 on 2024-04-03 15:26_

I can think of contrived cases where this would be wrong. Imagine, e.g., that you symlink `anyio-4.0.0.dist-info` to `anyio-3.7.0.dist-info`. I don't know why this would happen. But then we'd resolve them to the same package, which would be wrong. Or imagine you have a symlink to a dist-info directory, like `foo` symlinks to `anyio-4.0.0.dist-info`. Then it would appear to be installed twice, when it's not. I don't know, these are weird, but following symlinks on the site-packages directory seems a bit more true to the spirit of what's happening. (I don't really understand why they're symlinked in the first place though.)


---

_Comment by @Daverball on 2024-04-03 15:42_

@zanieb Unfortunately that doesn't appear to have fixed the problem:
```
> ~/.cargo/bin/uv --version
uv 0.1.28 (192b214dc 2024-04-03)
> ~/.cargo/bin/uv pip install pip
Resolved 1 package in 1ms
warning: Failed to uninstall package at venv/lib/python3.10/site-packages/pip-23.0.1.dist-info due to missing RECORD file. Installation may result in an incomplete environment.
Installed 1 package in 15ms
 - pip==23.0.1
 - pip==23.0.1
 + pip==23.0.1
```

---

_Comment by @zanieb on 2024-04-03 16:02_

Gahh... that's why we write tests. Thanks for trying it! Will investigate further.

---

_@zanieb reviewed on 2024-04-03 18:37_

---

_Review comment by @zanieb on `crates/uv-installer/src/site_packages.rs`:56 on 2024-04-03 18:37_

> Then it would appear to be installed twice, when it's not

It wouldn't appear twice in this cause because they'd resolve to the same directory and be de-duplicated?

> you symlink anyio-4.0.0.dist-info to anyio-3.7.0.dist-info... then we'd resolve them to the same package

True, but insane haha. I'm not sure if we should account for that.

I'm okay with a more limited implementation, but I'm worried not resolving links here could cause problems in the future too.

---

_Comment by @charliermarsh on 2024-04-12 13:52_

Superseded by https://github.com/astral-sh/uv/pull/3002.

---

_Closed by @charliermarsh on 2024-04-12 13:52_

---
