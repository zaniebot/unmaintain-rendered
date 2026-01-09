---
number: 3350
title: cross platform lock file generation
type: issue
state: closed
author: BurntSushi
labels:
  - preview
  - tracking
assignees: []
created_at: 2024-05-03T14:57:42Z
updated_at: 2024-06-10T12:47:15Z
url: https://github.com/astral-sh/uv/issues/3350
synced_at: 2026-01-07T13:12:17-06:00
---

# cross platform lock file generation

---

_Issue opened by @BurntSushi on 2024-05-03 14:57_

### Work items

* [x] #3353 
* [x] #3352 
* [x] #3354 
* [x] #3355 
* [x] #3351 
* [x] #3358 
* [x] #3359 
* [x] #3360 
* [x] #3926 

This is a tracking ticket for exposing a way to generate a cross platform lock file using the `uv` CLI. This ticket is primarily about making this available as a library feature, with exposure on the CLI being a step for making it easy to interact with. For example, this should be done via `uv pip compile --unstable-uv-lock-file`.

A bit ago, I did a prototype of this by roughly doing the following:

1. Ignore markers on dependencies.
2. Make the resolver "fork" internally whenever multiple packages enter the dependency tree as a result of ignoring markers.
3. Unify all forks of the resolver at the end into one set of locked versions for packages. The result is a "universal" or "cross platform." Its principally differs from a `requirements.txt` in that it can have multiple versions of the same package in it, and it includes all of the artifacts (wheels and source dist) for each distribution. Installing from a lock file thus requires some filtering.

While this prototype demonstrated the idea, it took a number of shortcuts that made it work only in a limited number of cases.

NOTE: This is distinct from #3347 in that this goal of this ticket is make lock file generation work. In #3347, the goal is to integrate lock file generate with the rest of the `uv` workspace commands.

A good litmus test for this issue being completed is that we can run the following:

```
$ cat requirements.in
# Compile with Python 3.10 as minimal version.
anyio<4 ; python_version < "3.11"
anyio>=4.3.0 ; python_version >= "3.11"
idna<3 ; python_version < "3.10"
idna>=3.6.0 ; python_version >= "3.12"
$ uv pip compile -p3.10 --unstable-uv-lock-file requirements.in
```

And get a `uv.lock` file as output. This example is interesting because it requires the resolver to "fork." Namely, with 3.10 as the minimal version of Python, both `anyio<4` and `anyio>=4.3.0` are constraints that need to be resolved simultaneously. Since they must necessarily lead to different versions of `anyio`, the resulting lock file will contain multiple versions of `anyio`.

---

_Label `preview` added by @BurntSushi on 2024-05-03 14:57_

---

_Label `tracking` added by @BurntSushi on 2024-05-03 14:57_

---

_Assigned to @BurntSushi by @BurntSushi on 2024-05-03 14:57_

---

_Referenced in [astral-sh/uv#3347](../../astral-sh/uv/issues/3347.md) on 2024-05-03 15:07_

---

_Referenced in [astral-sh/uv#3353](../../astral-sh/uv/issues/3353.md) on 2024-05-03 15:47_

---

_Referenced in [astral-sh/uv#3354](../../astral-sh/uv/issues/3354.md) on 2024-05-03 15:51_

---

_Referenced in [astral-sh/uv#3355](../../astral-sh/uv/issues/3355.md) on 2024-05-03 15:55_

---

_Referenced in [astral-sh/uv#3358](../../astral-sh/uv/issues/3358.md) on 2024-05-03 18:09_

---

_Referenced in [astral-sh/uv#3359](../../astral-sh/uv/issues/3359.md) on 2024-05-03 18:15_

---

_Referenced in [astral-sh/uv#3360](../../astral-sh/uv/issues/3360.md) on 2024-05-03 18:18_

---

_Referenced in [astral-sh/uv#3333](../../astral-sh/uv/issues/3333.md) on 2024-05-06 16:04_

---

_Referenced in [commaai/openpilot#32522](../../commaai/openpilot/issues/32522.md) on 2024-05-28 04:55_

---

_Referenced in [oxidase/ofiuco#69](../../oxidase/ofiuco/issues/69.md) on 2024-06-06 18:32_

---

_Comment by @BurntSushi on 2024-06-10 12:47_

I think we have a good base for the universal resolver, and all of the items initially planned for this are done.

However, for folks tracking this ticket, it's still not yet in a shippable state. That goal is tracked in https://github.com/astral-sh/uv/issues/3347.

---

_Closed by @BurntSushi on 2024-06-10 12:47_

---
