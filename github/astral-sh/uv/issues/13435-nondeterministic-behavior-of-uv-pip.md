---
number: 13435
title: "Nondeterministic behavior of `uv pip`"
type: issue
state: open
author: yjsongamz
labels:
  - bug
  - great writeup
assignees: []
created_at: 2025-05-13T19:11:45Z
updated_at: 2025-09-16T22:09:21Z
url: https://github.com/astral-sh/uv/issues/13435
synced_at: 2026-01-07T13:12:18-06:00
---

# Nondeterministic behavior of `uv pip`

---

_Issue opened by @yjsongamz on 2025-05-13 19:11_

### Summary

Minimal reproducible example: https://github.com/yjsongamz/uv-mre
Expected behavior: when I run `run.sh`, I expect it to always succeed (or at least be deterministic), but it sometimes fail randomly.
When I run `diff -r` on the `.venv` of the successful run and unsuccessful run, I found this diff:
```
> from .z3 import *
>
> from . import z3num
> from . import z3poly
> from . import z3printer
> from . import z3rcf
> from . import z3types
> from . import z3util
>
> # generated files
> from . import z3core
> from . import z3consts
```
(successful run has 9 more SLOC in `.venv`)


OS version:
```
~/mre$ sw_vers
ProductName:		macOS
ProductVersion:		15.3.1
BuildVersion:		24D70
~/mre$ uname -orsm
Darwin 24.3.0 arm64
```
I tested it also on linux virtual machine and was able to reproduce there.

UV version:
```
~/mre$ uv self version
uv 0.7.3 (Homebrew 2025-05-07)
```

### Platform

macOS 15.3.1 arm64

### Version

uv 0.7.3 (Homebrew 2025-05-07)

### Python version

Python 3.8.20

---

_Label `bug` added by @yjsongamz on 2025-05-13 19:11_

---

_Label `great writeup` added by @konstin on 2025-05-13 19:13_

---

_Comment by @charliermarsh on 2025-05-13 19:16_

Are there two packages in the environment that are overwriting each other?

---

_Comment by @konstin on 2025-05-13 19:18_

Thank you for the MRE!

Both z3 and z3-solver contain the the `z3` module, which means that they write an overlapping set of files. As uv installs in parallel, you may get either installation.

---

_Comment by @yjsongamz on 2025-05-13 19:30_

@charliermarsh @konstin Aha, thank you for the clarification!
I believe I can fix this specific issue (e.g., by splitting the installation into two phases or by just removing `z3` in `pyprojects.toml` - `z3-solver` already installs `z3`) since I know which specific packages are conflicting.
That said, would there be a systematic way to detect and avoid this type of issue? (to rule out this kind of issue in the future)

---

_Comment by @konstin on 2025-05-13 19:37_

A proper solution could be https://discuss.python.org/t/pre-pep-import-name-metadata/90506

---

_Referenced in [astral-sh/uv#13437](../../astral-sh/uv/pulls/13437.md) on 2025-05-13 20:55_

---

_Referenced in [astral-sh/uv#15357](../../astral-sh/uv/issues/15357.md) on 2025-08-18 16:36_

---

_Comment by @bitbier on 2025-09-16 21:13_

Is there a way to define the ordering of such packages. There is another example of `jmespath` and `jmespath_community`. The first one is used by `boto` and other tools but we use the community version because it has some extra features that we use in our code. We recently switched to `uv pip` instead of `pip` but in this specific instances. We started seeing nondeterministic build failures because of the issue above with `z3`.

We unfortunately don't control `jmespath_community` implementation and we can ask for a package the has a separate namespace, but short of forking and doing our own is there another solution?

I was looking for a way to turn off parallelization for now until we can have more time to fix the issue, but I don't see this option anywhere.

We are looking for some way to make the order deterministic.

---

_Comment by @zanieb on 2025-09-16 21:33_

You can use #4422 to drop `jmespath` entirely, or you could turn off [build isolation](https://docs.astral.sh/uv/concepts/projects/config/#disabling-build-isolation) for `jmsepath_community` which would cause it to be installed in a second stage (which is really a hack, but might unblock you).

---

_Comment by @bitbier on 2025-09-16 21:41_

I think to unblock ourselves, we are just using a one liner to re-install `jmespath-community` after the initial install of our requirements. We can also drop `jmespath` but we use `uv pip compile` to generate the requirements.txt file. It will always get pulled in because `boto` and other libraries just depends on `jmespath` is there a way to prevent `uv pip compile` from including that and include the other one instead?

Just trying to figure out which end the hack needs to go so that everything works again until we can reassess and do the right thing.

---

_Comment by @zanieb on 2025-09-16 21:55_

The override at https://github.com/astral-sh/uv/issues/4422#issuecomment-2254182800 will drop jmsepath from your transitive dependencies as well.

---

_Comment by @bitbier on 2025-09-16 22:09_

Thank you for being responsive. This helps us greatly.

---
