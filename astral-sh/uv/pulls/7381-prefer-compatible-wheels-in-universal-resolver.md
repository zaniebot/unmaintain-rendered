```yaml
number: 7381
title: Prefer compatible wheels in universal resolver
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/tag-preference
created_at: 2024-09-14T00:17:55Z
updated_at: 2025-02-11T02:36:17Z
url: https://github.com/astral-sh/uv/pull/7381
synced_at: 2026-01-12T16:07:48Z
```

# Prefer compatible wheels in universal resolver

---

_@charliermarsh_

## Summary

I need input on whether this is a good change. In the universal resolver, we currently choose an arbitrary wheel when extracting package metadata. A downside here is that if we need to download the entire wheel, we might end up downloading a _second_ (compatible) wheel when we go to install. So, e.g., you could end up downloading two PyTorch wheels, one during resolution, and one during installation (if your registry doesn't support range requests -- otherwise, we don't download during resolution anyway).

The idea here is to respect platform tags when deciding which wheel to use (but _not_ error if we can't find a compatible wheel).

The major downside here is that resolution could actually vary depending on the platform you're on, if a package has inconsistent metadata across wheels, since we'll look at different wheels for metadata on Windows vs. on Linux, for example.

I'm inclined _not_ to do this, since it breaks the nice property that resolution is entirely machine-independent. But it's weird to download two wheels like that.

Closes https://github.com/astral-sh/uv/issues/5921.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-09-14 00:18_

---

_Review requested from @konstin by @charliermarsh on 2024-09-14 00:18_

---

_Comment by @charliermarsh on 2024-09-14 00:18_

(Ignore the code for now. Mostly want input on whether we should even do this.)

---

_Comment by @zanieb on 2024-09-14 03:11_

I'm roughly in favor of this, since we already strongly encourage metadata to be consistent across wheels. Downloading two different wheels seems problematic. 

What happens if you have varied wheel metadata and lock on one platform then another? Will the lockfile change? Can we mitigate the bad experience there somehow?

---

_Comment by @charliermarsh on 2024-09-14 03:20_

I think we would let you keep the current lockfile (i.e., we'd consider it "up-to-date"), since we assume that registry metadata is immutable anyway in that check.

---

_Comment by @zanieb on 2024-09-14 03:47_

But if you ran `lock --upgrade` it would do a new resolution and choose the new wheel for this package and more things would change? Or would preferences save us there? Just trying to understand the "failure" mode.

---

_Comment by @charliermarsh on 2024-09-14 12:33_

> But if you ran lock --upgrade it would do a new resolution and choose the new wheel for this package and more things would change? Or would preferences save us there? Just trying to understand the "failure" mode.

Yeah, if the lock was generated on macOS, and you're on Windows, then I _think_ running `lock --upgrade` could lead to changes in metadata even if the versions didn't change (e.g., maybe you get a different TensorFlow wheel due to your platform).

So that wouldn't be great. It's possible we could fix that though by using the lockfile to populate the cache.


---

_Comment by @konstin on 2024-09-16 09:39_

> What happens if you have varied wheel metadata and lock on one platform then another? Will the lockfile change? Can we mitigate the bad experience there somehow?

Currently, we silently do the wrong thing. We could mitigate this by always showing diagnostics (`SitePackages::diagnostics`) after sync. For example, if I delete asgiref from a django lockfile I get:

```
Resolved 4 packages in 4ms
Prepared 2 packages in 0.28ms
Installed 2 packages in 35ms
 + django==5.1.1
 + sqlparse==0.5.1
warning: The package `django` requires `asgiref>=3.8.1,<4`, but it's not installed
```

In transformers, this already happens on main:

```
warning: The package `torch` requires `setuptools`, but it's not installed
```

We should consider checking at install time that the metadata is consistent. On a quick test, adding this adds 2ms to the existing 88ms on transformers, but we could e.g. reuse the METADATA reading in the installer threadpool instead of doing duplicate work after installation. To me, handling inconsistent metadata correctly means that we can use any wheel picking strategy without risk.

---

_Comment by @BurntSushi on 2024-09-16 14:31_

I think if this only leads to problems where metadata varies across wheels, then I'd be in favor of it personally. Like, we already make the assumption that the metadata is consistent across wheels in various places right? If it doesn't hold, then won't more things break?

---

_Closed by @charliermarsh on 2025-02-11 02:35_

---

_Comment by @charliermarsh on 2025-02-11 02:36_

I worry we'll end up regretting this change, because suddenly issues etc. will become harder to reproduce in that we can no longer rely on resolution being consistent across platforms...

---
