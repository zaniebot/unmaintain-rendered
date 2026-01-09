---
number: 1661
title: "`uv pip install` fails for dependencies on already-installed local packages"
type: issue
state: closed
author: charlesnicholson
labels:
  - bug
assignees: []
created_at: 2024-02-18T18:00:03Z
updated_at: 2024-04-01T22:10:47Z
url: https://github.com/astral-sh/uv/issues/1661
synced_at: 2026-01-07T13:12:16-06:00
---

# `uv pip install` fails for dependencies on already-installed local packages

---

_Issue opened by @charlesnicholson on 2024-02-18 18:00_

When a package depends on an editable-installed namespaced package, `uv pip install` fails saying that the already-installed package can't be found. Possibly related to string hygiene and converting `ns1.package1` to `ns1-package1`?

Full cloneable repro case here: https://github.com/charlesnicholson/uv-pep420-bug/tree/main 
(Runs at least on macOS and maybe Linux, let me know if it's insufficient and I'll fix it up)

<img width="724" alt="Screenshot 2024-02-18 at 12 58 16 PM" src="https://github.com/astral-sh/uv/assets/3010295/50b162c4-03cd-4b17-9091-058daca3ef4e">


---

_Comment by @charlesnicholson on 2024-02-18 18:12_

Ah, sorry:
```
❯ ./uv --version
uv 0.1.3
```

---

_Comment by @charlesnicholson on 2024-02-18 18:44_

Confirmed still happens on `0.1.4` (albeit with a much more soothing color scheme!):

<img width="905" alt="Screenshot 2024-02-18 at 1 43 43 PM" src="https://github.com/astral-sh/uv/assets/3010295/76308b51-b871-45c3-9cfd-ee70e28369d0">


---

_Label `bug` added by @zanieb on 2024-02-18 21:04_

---

_Comment by @charliermarsh on 2024-02-20 04:41_

So, I think `cargo run pip install -e ns2.package2 -e ns1.package1` works, but installing the packages one-by-one does not. I need to think about what the right solution is here. When we go to install the second package (`-e ns2.package2`), we start by performing a resolution, to figure out the required dependencies. We don't consider the "current virtual environment" to be a valid package source, so we have no way of finding `ns1-package1`.

---

_Comment by @charlesnicholson on 2024-02-20 13:41_

Ah, interesting, thanks for taking a look and sharing your thoughts!

For context, if it's useful, the reason we incrementally `pip install -e` our various packages in order is because they're part of a larger build system that understands the dependencies between our packages and other tools. Each package has an "editable install" target, and for our build system to satisfy our version of `ns2.package2`'s dependencies, it needs to bring the "editable-install `ns1.package1`" target up-to-date first, which it does via a separate invocation of `pip install -e`.

---

_Comment by @charlesnicholson on 2024-02-20 13:50_

OTOH maybe since `uv` is so hilariously fast, there isn't really a build-time performance penalty for just always re-editable-installing our dependent packages along with the new package :-D (Transitive package dependencies make this complicated though)

Doing the same work with pip definitely incurs a performance penalty.

---

_Comment by @charlesnicholson on 2024-02-20 14:25_

I am curious, though- the venv seems like a pretty reasonable source of packages given that it holds strongly-versioned already-installed dependencies, and that's more or less its reason to exist. To my mind, it seems reasonable that `uv` would behave identically with both a unified and multiple separate invocations. I am a brand new user, though, and not the author, so I certainly don't have the context that you do.

---

_Referenced in [astral-sh/uv#1476](../../astral-sh/uv/issues/1476.md) on 2024-02-22 03:14_

---

_Referenced in [astral-sh/uv#1856](../../astral-sh/uv/issues/1856.md) on 2024-02-22 05:27_

---

_Comment by @charliermarsh on 2024-02-22 05:31_

I think it's probably correct for us to fix this. It just changes a lot of assumptions -- right now, we perform a resolution that's independent of the virtual environment.

---

_Renamed from "`uv pip install` fails with PEP420 namespace package dependencies" to "`uv pip install` fails when packages rely on installed, locally-derived packages" by @charliermarsh on 2024-02-22 05:32_

---

_Renamed from "`uv pip install` fails when packages rely on installed, locally-derived packages" to "`uv pip install` fails for dependencies on already-installed, locally-derived packages" by @charliermarsh on 2024-02-22 05:32_

---

_Renamed from "`uv pip install` fails for dependencies on already-installed, locally-derived packages" to "`uv pip install` fails for dependencies on already-installed local packages" by @charliermarsh on 2024-02-22 05:32_

---

_Comment by @kkpattern on 2024-02-22 05:33_

Installing all the editable packages at once works for us.

---

_Comment by @charliermarsh on 2024-02-22 05:35_

Yeah, the problem is that the second install doesn't "know" where to find the existing editable. We need to make the resolver aware of packages that already exist in the virtual environment.

---

_Comment by @tylerw on 2024-02-25 21:37_

Possibly related (if not, I can file a separate issue—or none at all if this is a misunderstanding on my part), when I `uv pip compile` a `requirements.in` or `uv pip install` a `requirements.txt` that has local, editable-installed modules (e.g, specified with `-e path/to/module`) they are all rebuilt and I assume cached, even if they haven't changed and don't need it. Is this avoidable?

For the record, this is a large monorepo with dozens of modules. The commands complete really fast and therefore it's not a huge problem, just wondering if there is a way (or possibly a different work flow) to prevent this.

---

_Comment by @charliermarsh on 2024-02-25 21:39_

@tylerw -- I _think_ we try to avoid rebuilding with `uv pip compile`, but we always rebuild right now in `uv pip install`. Building editables is typically pretty cheap, but I'm looking into removing that step.

---

_Referenced in [astral-sh/uv#2093](../../astral-sh/uv/issues/2093.md) on 2024-02-29 18:59_

---

_Referenced in [astral-sh/uv#2282](../../astral-sh/uv/issues/2282.md) on 2024-03-07 15:56_

---

_Comment by @charliermarsh on 2024-03-07 16:44_

(I intend to fix this.)

---

_Referenced in [astral-sh/uv#2383](../../astral-sh/uv/issues/2383.md) on 2024-03-12 14:45_

---

_Referenced in [astral-sh/uv#2560](../../astral-sh/uv/issues/2560.md) on 2024-03-20 13:57_

---

_Assigned to @zanieb by @zanieb on 2024-03-21 17:41_

---

_Referenced in [astral-sh/uv#2596](../../astral-sh/uv/pulls/2596.md) on 2024-03-21 21:21_

---

_Referenced in [astral-sh/rye#813](../../astral-sh/rye/issues/813.md) on 2024-03-24 05:23_

---

_Closed by @zanieb on 2024-03-28 18:49_

---

_Referenced in [astral-sh/uv#2726](../../astral-sh/uv/issues/2726.md) on 2024-03-29 13:36_

---

_Comment by @zanieb on 2024-04-01 21:38_

Hi! Charlie tricked me into taking on this hard task instead :)

It should be addressed in the latest release ([0.1.27](https://github.com/astral-sh/uv/releases/tag/0.1.27)) via #2596.

Let us know if you have any problems.

---

_Comment by @charlesnicholson on 2024-04-01 22:10_

So far it's great, thanks! (I'm integrating it as I type this :) )

---
