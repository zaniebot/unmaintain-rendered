```yaml
number: 11319
title: Indicate the version of uv that was used to generate/update the uv.lock file in the file itself
type: issue
state: closed
author: sebn
labels:
  - enhancement
assignees: []
created_at: 2025-02-07T15:36:18Z
updated_at: 2025-02-15T08:48:23Z
url: https://github.com/astral-sh/uv/issues/11319
synced_at: 2026-01-10T03:50:31Z
```

# Indicate the version of uv that was used to generate/update the uv.lock file in the file itself

---

_Issue opened by @sebn on 2025-02-07 15:36_

### Summary

When starting a project with `uv` version 0.5.28, the `uv.lock` file would include something like this:

```
[[uv]]
version = 0.5.28
```

The value would be updated everytime a different uv binary version is used to change the lockfile, be it a newer version:

```
[[uv]]
version = 0.5.29
```

Or an older one:

```
[[uv]]
version = 0.5.27
```


### Example

This would make it possible or at least easier to:

- Understand both for uv maintainers & uv users that some unexpected changes in the `uv.lock` file can be related to the version of the uv binary that was used & decide whether the change is legit or not.
- Display a warning in the console about the current version being used not matching the one in the lock file (separate feature, built on top of this one)
- Write a simple shell script in one's git repo that automatically install the uv version matching the one in their `uv.lock` file

---

_Label `enhancement` added by @sebn on 2025-02-07 15:36_

---

_Renamed from "Indicate the version of uv that was used to lock the dependencies in the lockfile itself" to "Indicate the version of uv that was used to generate/update the uv.lock file in the file itself" by @sebn on 2025-02-07 15:38_

---

_Assigned to @charliermarsh by @zanieb on 2025-02-07 15:38_

---

_Comment by @charliermarsh on 2025-02-09 20:07_

Hmm. I think this would be slightly problematic in that it would unnecessarily couple the lockfile to the uv version. For example, the lockfile would now be invalidated every time you update the uv version, despite the fact that the semantic contents may not change, and the lockfile itself is already versioned (and intended to be compatible with a wide range of uv versions).

I would suggest using [`required-version`](https://docs.astral.sh/uv/reference/settings/#required-version) for this? It's intended for projects that want to enforce a specific uv version.

---

_Comment by @sebn on 2025-02-13 17:23_

Thanks you very much for taking the time to answer 🙂 

I was assuming that the lockfile would only be invalidated when some semantic contents other than this version number would actually need to be changed & that the version number would only be updated together with those actual changes. But I know basically nothing of uv's internal, so I'm possibly missing something, maybe this behavior would be inherently incompatible with the way it works?

By the way, [required-version](https://docs.astral.sh/uv/reference/settings/#required-version) could definitely avoid some issues, thanks for pointing this out! (although the idea was more about knowing which version triggered a lockfile change, than enforcing some specific version)

---

_Comment by @charliermarsh on 2025-02-14 20:01_

@sebn -- That makes sense and is possible in theory. I just think it's going to cause too much churn for teams. It's not a strict requirement that users within a given project are all using the same uv version; and if we start tracking the version in the lockfile, it'll oscillate back and forth whenever folks change the project dependencies.

For now, I'd recommend `required-version` for teams that want to track and enforce this. I'm sorry that I wasn't able to deliver on the exact ask here.

---

_Closed by @charliermarsh on 2025-02-14 20:01_

---

_Comment by @sebn on 2025-02-15 08:40_

> I'm sorry that I wasn't able to deliver on the exact ask here.

No problem, I'm completely fine with the feature request being closed, the `required-version` solution makes sense. 👍 

Thank you for all the hard work btw, uv is really a game changer on a daily basis 😄 

---
