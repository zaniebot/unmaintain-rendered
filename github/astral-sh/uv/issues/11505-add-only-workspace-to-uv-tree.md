---
number: 11505
title: "Add `--only-workspace` to `uv tree`"
type: issue
state: open
author: danielgafni
labels:
  - enhancement
assignees: []
created_at: 2025-02-14T11:17:22Z
updated_at: 2025-03-10T19:08:26Z
url: https://github.com/astral-sh/uv/issues/11505
synced_at: 2026-01-07T13:12:18-06:00
---

# Add `--only-workspace` to `uv tree`

---

_Issue opened by @danielgafni on 2025-02-14 11:17_

### Summary

It would be great to have this option available. 

I am writing a [Dagger](https://dagger.io/) pipeline to build my monorepo and getting the information about internal workspace dependencies from `uv` would allow me to granularly copy only the required source files for a particular subproject into the build container. This in turn will greatly improve caching. 

I kinda can do it by hand already but I feel like internal dependencies source code location discovery could be automated with `uv`. 

### Example

```shell
uv tree --package my-project --only-workspace
```

---

_Label `enhancement` added by @danielgafni on 2025-02-14 11:17_

---

_Comment by @zanieb on 2025-02-14 13:20_

We discussed flags for this a fair bit over in https://github.com/astral-sh/uv/issues/4028#issuecomment-2302998517

Would you mind seeing if the concerns there are relevant or if one of the other suggestions makes sense for your use-case?

---

_Comment by @danielgafni on 2025-02-14 17:44_

I don't see anything related to the `uv tree` command in this issue  

---

_Comment by @charliermarsh on 2025-02-14 19:10_

What does `--only-workspace` do here?

---

_Comment by @danielgafni on 2025-02-14 23:32_

It would only include workspace members in the `uv tree` command output.  

---

_Comment by @zanieb on 2025-02-15 00:57_

> I don't see anything related to the `uv tree` command in this issue

The discussed flags are for `uv sync`, but the concepts are applicable elsewhere. For example, we use the same concepts in `uv export`

```
      --no-emit-project                    Do not emit the current project
      --no-emit-workspace                  Do not emit any workspace members, including the root project
      --no-emit-package <NO_EMIT_PACKAGE>  Do not emit the given package(s)
```

---

_Comment by @danielgafni on 2025-02-16 21:41_

Sorry @zanieb, I don't get what am I supposed to do :) 

Are you implying that the `--only-workspace` *name* is not a good candidate for this flag? 
If so, does `--only-emit-workspace` work better? 

---

_Comment by @zanieb on 2025-02-17 04:16_

Don't worry about it, I'll re-read the discussion there and see what flag makes sense here.

---

_Comment by @Sessional on 2025-03-10 19:08_

@zanieb I think this conversation is maybe targeting a specific problem uncovered while shoving uv+dagger together in a monorepo. There is no great way to discover ONLY your local dependencies for a package. There is a great way to discover everything BUT your local dependencies.

While building the monorepo (via dagger inside a container) into a new runtime container, we need to copy all relevant sources for our dependencies into the build container. While one could go `uv sync` if all sources are copied into the build context,  trying to keep as much out of the context (of the build container) as possible is generally a good idea and so shipping ALL of the parts of the monorepo seems unwieldy.

Given a project like so:
libs/lib1
libs/lib2
libs/lib3
projects/app

And a dependency tree like so: `projects/app` -> `libs/lib1` -> `libs/lib2`. 

When creating the context for our build container we can intelligently leave `lib3` out because it isn't part of the dependency tree. In this small example it's no big deal, but when we talk larger libraries or even more libraries it becomes more impactful. Right now to find the entire dependency tree, we have to navigate the `uv.lock` file which historically is frowned upon by most tools because it's really an internal format for internal goodies.

By shoving a flag for some form of `--only-locals` into the tree it would be feasible to traverse that tree instead of the lock file for which projects to copy into the build container context. As an aside on this, can `uv tree` output in a format other than `pretty-print` for us to dig around in with code (unless we should be using the .lock file?)

---
