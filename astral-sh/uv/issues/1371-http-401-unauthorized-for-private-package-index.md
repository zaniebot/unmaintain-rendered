```yaml
number: 1371
title: HTTP 401 Unauthorized for private package index
type: issue
state: closed
author: exs-avianello
labels:
  - bug
  - registry
assignees: []
created_at: 2024-02-15T22:14:10Z
updated_at: 2024-02-23T07:17:25Z
url: https://github.com/astral-sh/uv/issues/1371
synced_at: 2026-01-12T15:58:26Z
```

# HTTP 401 Unauthorized for private package index

---

_@exs-avianello_

Hello! Very excited for this project ❤️ 

Installing a package from a private azure package index (Azure Artifacts) seems to be failing with a `HTTP status client error (405 Method Not Allowed)`:

```
uv pip install --extra-index-url "https://<FEED_NAME>:<PERSONAL_ACCESS_TOKEN>@pkgs.dev.azure.com/<ORGANIZATION_NAME>/_packaging/<FEED_NAME>/pypi/simple/" package-name                                          
error: Failed to download: package-name==X.Y.Z
  Caused by: HTTP status client error (405 Method Not Allowed) for url (pkgs.dev.azure.com/<ORGANIZATION_NAME>/_packaging/.../pypi/download/package-name/X.Y.Z/package_name-X.Y.Z-py3-none-any.whl#[sha256=])
````

The equivalent pip-native command works as expected
```
python -m pip install --extra-index-url "https://<FEED_NAME>:<PERSONAL_ACCESS_TOKEN>@pkgs.dev.azure.com/<ORGANIZATION_NAME>/_packaging/<FEED_NAME>/pypi/simple/" package-name
```

---

_Label `bug` added by @charliermarsh on 2024-02-15 22:15_

---

_Comment by @charliermarsh on 2024-02-15 22:15_

Thank you! Will take a look.

---

_Label `index` added by @zanieb on 2024-02-15 23:47_

---

_Comment by @yogevyuval on 2024-02-18 14:54_

Same here - Really want to test (and replace pip with) uv but waiting for Azure artifacts support :( !

---

_Comment by @charliermarsh on 2024-02-18 15:03_

Thanks and sorry about that -- we'll get this setup internally and see if we can reproduce.

---

_Comment by @zanieb on 2024-02-18 17:19_

Yep this is a priority for me next week!

---

_Comment by @MarcSkovMadsen on 2024-02-19 11:09_

+1. Same issue for me.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-19 14:42_

---

_Comment by @charliermarsh on 2024-02-19 14:43_

I believe this is the same as https://github.com/astral-sh/uv/issues/1458 (lack of support for HEAD requests).

---

_Comment by @charliermarsh on 2024-02-19 20:49_

Okay, @olivierlefloch fixed HEAD requests, but there's now an auth problem. @zanieb, do you want to take from here?

---

_Comment by @exs-avianello on 2024-02-21 13:38_

Thank you all! 

I can confirm that on `uv 0.1.6` this is no longer giving `405 Method Not Allowed` but a `401 Unauthorized`

```
uv pip install --extra-index-url "https://<FEED_NAME>:<PERSONAL_ACCESS_TOKEN>@pkgs.dev.azure.com/<ORGANIZATION_NAME>/_packaging/<FEED_NAME>/pypi/simple/" package-name                                          
error: Failed to download: package-name==X.Y.Z
  Caused by: HTTP status client error (401 Unauthorized) for url (pkgs.dev.azure.com/<ORGANIZATION_NAME>/_packaging/.../pypi/download/package-name/X.Y.Z/package_name-X.Y.Z-py3-none-any.whl#[sha256=])
```

---

_Comment by @gwdekker on 2024-02-22 09:07_

Same for gitlab hosted private package index: getting a 401 Unauthorized.  Interestingly, opening the URL in the error message in the browser downloads the package for me without a problem. the url looks like this: 

`https://gitlab.com/api/v4/groups/<GROUP_NR>/-/packages/pypi/files/<SOME_LONG_HASH>/<PKG_NAME>-py3-none-any.whl#sha256=<PKG_SHA>
`

---

_Assigned to @zanieb by @charliermarsh on 2024-02-22 19:18_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-02-22 19:18_

---

_Comment by @zanieb on 2024-02-22 20:31_

Hi! We just merged a fix with #1874 that's out in v0.1.8 — let me know if that helps. I'll continue testing against various private repositories.

---

_Comment by @exs-avianello on 2024-02-22 20:40_

Thank you @zanieb! At least for Azure Artifacts, I am still seeing the same error on `0.1.8` (`401 Unauthorized`)

---

_Comment by @yogevyuval on 2024-02-22 21:05_

Same here, `401 Unauthorized` for Azure Artifacts with uv `0.1.8`

---

_Renamed from "uv pip install HTTP status client error (405 Method Not Allowed) for private azure package index" to "HTTP 401 Unauthorized for private package index" by @zanieb on 2024-02-22 21:23_

---

_Comment by @zanieb on 2024-02-22 21:27_

For my own sanity, note this is also being tracked in https://github.com/astral-sh/uv/issues/1709

---

_Comment by @zanieb on 2024-02-22 22:31_

A fix is up at https://github.com/astral-sh/uv/pull/1886 if anyone wants to give it a try against a private index.

---

_Comment by @inigohidalgo on 2024-02-22 22:54_

Verified working on azure artifacts feed using token authentication!!

Incredible work @zanieb, @olivierlefloch and @charliermarsh 🚀

---

_Closed by @zanieb on 2024-02-23 00:10_

---

_Comment by @exs-avianello on 2024-02-23 07:17_

I can confirm as well ❤️ Thank you so much!

---
