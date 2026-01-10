```yaml
number: 9949
title: "Normalize `platform_system` to `sys_platform`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/normalize
created_at: 2024-12-16T23:46:10Z
updated_at: 2024-12-18T15:29:37Z
url: https://github.com/astral-sh/uv/pull/9949
synced_at: 2026-01-10T12:00:01Z
```

# Normalize `platform_system` to `sys_platform`

---

_Pull request opened by @charliermarsh on 2024-12-16 23:46_

## Summary

A revival of an old idea (#9344) that I have slightly more confidence in now. I abandoned this idea because (1) it couldn't capture that, e.g., `platform_system == 'Windows' and sys_platform == 'foo'` (or some other unknown value) are disjoint, and (2) I thought that Android returned `"android"` for one of `sys_platform` or `platform_system`, which would've made this logic incorrect.

However, it looks like Android... doesn't do that? And the values here are almost always in a small, known set. So in the end, the tradeoffs here actually seem pretty good.

Vis-a-vis our current solution, this can (e.g.) _simplify out_ expressions like `sys_platform == 'win32' or platform_system == 'Windows'`.


---

_@zanieb reviewed on 2024-12-17 00:02_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:9097 on 2024-12-17 00:02_

nice!

---

_@zanieb approved on 2024-12-17 00:03_

I'm into this. Some churn for outputs though... I wonder if we should hold off on this until another minor version? (though these are less meaningful than some of the other resolution changes we've been making...)

---

_Comment by @charliermarsh on 2024-12-17 00:29_

I'm somewhat in favor of just shipping it, since it won't invalidate existing lockfiles.

---

_Comment by @samypr100 on 2024-12-17 02:27_

> I thought that Android returned "android"

Note, per [PEP-738](https://peps.python.org/pep-0738/#sys), it will return "android" starting in 3.13. Similar as "ios" in 3.13 for iPhones per [PEP-730](https://peps.python.org/pep-0730/#sys)

---

_Comment by @charliermarsh on 2024-12-17 02:34_

Thank you! Luckily it looks like it will also return `"Android"` for `platform.system()`, phew.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-12-17 02:40_

---

_Review requested from @konstin by @charliermarsh on 2024-12-17 02:40_

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/algebra.rs`:275 on 2024-12-17 11:33_

I would feel more confident if we could codify this upstream (packaging.python.org probably), so uv doesn't end up making assumptions that python redistributors don't make.

---

_@konstin approved on 2024-12-17 11:34_

---

_Review comment by @charliermarsh on `crates/uv-pep508/src/marker/algebra.rs`:275 on 2024-12-17 12:45_

Can you open a convo about it?

---

_@charliermarsh reviewed on 2024-12-17 12:45_

---

_Review comment by @BurntSushi on `crates/uv-pep508/src/marker/algebra.rs`:275 on 2024-12-17 15:38_

Also, do we have any context on _why_ there is `platform_system` and `sys_platform`?

---

_Review comment by @BurntSushi on `crates/uv-pep508/src/marker/algebra.rs`:275 on 2024-12-17 15:39_

And maybe it's obvious, but I think a comment saying why we normalize to `sys_platform` (instead of the other way around) would be good too.

---

_@BurntSushi reviewed on 2024-12-17 15:40_

Nice!

---

_@BurntSushi approved on 2024-12-17 15:40_

(Meant to approve.)

---

_Review comment by @zanieb on `crates/uv-pep508/src/marker/algebra.rs`:275 on 2024-12-17 15:41_

https://discuss.python.org/t/clarify-usage-of-platform-system/70900 from Konsti's prompt in the [PyPA Discord](https://discord.com/channels/803025117553754132/1259612340509343805/1318596370999021668) 

---

_@zanieb reviewed on 2024-12-17 15:41_

---

_Comment by @charliermarsh on 2024-12-17 17:17_

From the PyPA Discord, it sounds like this isn't really safe to assume in all cases:

> I gave the example of iPad, but looking at the docs, but some Unix OSes will have the major version number appended in sys.platform... in fact, that should be the case for FreeBSD, NetBSD, OpenBSD, and SunOS in your example

It might be fine for macOS, Windows, and Linux though...? The downside is that the fewer values we rewrite, the more we risk messing up when we see a marker like `platform_system == "Darwin" and platform_system == "FreeBSD"`, since we'd then rewrite that to `sys_platform == "darwin" and platform_system == "FreeBSD"`, and no longer realize they're disjoint.


---

_Comment by @charliermarsh on 2024-12-18 03:15_

Maybe we run with this for macOS, Linux, Windows, and any other platforms that _don't_ encode a version number.

Then, we can use our existing "incompatible markers" infrastructure to mark those other platforms as incompatible with these `sys_platform` values? Like, we can mark `platform_system == "FreeBSD"` as incompatible with all of these `sys_platform` values.

---

_Comment by @charliermarsh on 2024-12-18 15:03_

Ok, I did a second pass on this with more care around known semantics. I think it's good to go. Our strategy has two parts: (1) we normalize `platform_system` to `sys_platform` when we can; and (2) when we can't, we encode as many known incompatibilities as possible. (1) is preferable to (2), but (1) isn't always possible.

---

_Merged by @charliermarsh on 2024-12-18 15:29_

---

_Closed by @charliermarsh on 2024-12-18 15:29_

---

_Branch deleted on 2024-12-18 15:29_

---
