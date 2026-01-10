---
number: 6804
title: "Confusing documentation for `uvx`"
type: issue
state: open
author: weihenglim
labels:
  - documentation
assignees: []
created_at: 2024-08-29T10:57:51Z
updated_at: 2024-09-03T13:52:41Z
url: https://github.com/astral-sh/uv/issues/6804
synced_at: 2026-01-10T01:24:06Z
---

# Confusing documentation for `uvx`

---

_Issue opened by @weihenglim on 2024-08-29 10:57_

Sorry if this is something obvious, but I am very confused on the difference between a `uvx <package>` invocation and `uvx <package>@latest`. Through my own testing, `uvx <package>` seems to always install the latest version even if a cached version is available.
```cmd
>uvx ruff@0.6.0 -V
Installed 1 package in 293ms
ruff 0.6.0

>uvx ruff -V
Installed 1 package in 290ms
ruff 0.6.2
```

Reading the [documentation](https://docs.astral.sh/uv/concepts/tools/#tool-versions) had me even more confused as it seems to be contradicting itself. The linked section states that:

> uvx will use the latest available version of the requested tool on the first invocation. After that, uvx will **_use the cached version_** of the tool unless a different version is requested, the cache is pruned, or the cache is refreshed

But then immediately proceeds to contradict the statement by saying:

> A subsequent invocation of uvx will use the **_latest, not the cached, version_**.

And then goes back to saying:

> But, if a new version of Ruff was released, **_it would not be used_** unless the cache was refreshed.

Would appreciate it if someone could help clarify this, and again, I apologize if I am just missing something obvious.






---

_Comment by @zanieb on 2024-08-29 13:50_

Oops, yeah sorry this is really confusing. I think it's roughly like this:

- If you request a specific version, we'll respect the requested version and we'll cache it, e.g. as `foo@0.1.0`
- Requests without a version will fetch the latest version and cache it, e.g. as `foo`
- Subsequent requests without a version will use the `foo` cached version
    - If the library releases a new version, we won't fetch it
- If you use `foo@latest`, we'll refresh the cache and fetch the latest version again and cache it, e.g. as `foo`
 
This is, at least, the intent of what I wrote. I'm actually not entirely sure it's correct, and I think it needs to be rephrased.

@charliermarsh would you be willing to take a look since you implemented the caching?

---

_Label `question` added by @zanieb on 2024-08-29 13:50_

---

_Label `documentation` added by @zanieb on 2024-08-29 13:50_

---

_Label `question` removed by @zanieb on 2024-08-29 13:50_

---

_Comment by @weihenglim on 2024-08-30 02:44_

> If the library releases a new version, we won't fetch it

I think this is the part that got me confused because in my experience that wasn't the case. Case in point, `uvx ruff` actually fetched the latest release of `ruff` for me this morning even without the `@latest` flag.
```cmd
>uvx ruff -V
Installed 1 package in 164ms
ruff 0.6.3
```

---

_Assigned to @charliermarsh by @zanieb on 2024-09-03 13:52_

---
