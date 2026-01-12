```yaml
number: 2701
title: "Handle functions that never return in RET503 (#2602)"
type: pull_request
state: merged
author: ppentchev
labels: []
assignees: []
merged: true
base: main
head: roam-implicit-return
created_at: 2023-02-10T01:31:20Z
updated_at: 2023-02-10T16:31:23Z
url: https://github.com/astral-sh/ruff/pull/2701
synced_at: 2026-01-12T15:55:10Z
```

# Handle functions that never return in RET503 (#2602)

---

_@ppentchev_

Hi,

Thanks for your relentless work on Ruff!

What do you think about this first shot, extremely rough, extremely naive and incomplete attempt to handle https://github.com/charliermarsh/ruff/issues/2602 by trying to figure out whether the last statement is a call to one of the known no-return functions? This is my first timid look into the Ruff source code, and my first ever look into rustpython-parser, too, so there will certainly be several "this is not the way" objections... assuming this is even on the right path.

And to foresee at least one of these objections: as noted in the code comments, this is naive and incomplete in that it compares the name under which the module ("os", "pytest", etc) is referenced, and not the actual name of the imported module. I would guess that Ruff already records imported modules somewhere so that there is a way for this code to obtain the actual (possibly fully qualified) name of the module that the expression refers to; if you could point me to a place in the Ruff code where something like that is done, I'd be happy to use it here. Also the same applies to unqualified names in function calls: when we see a call to `abort()` or `xfail(...)` as the last statement, it would be nice to be able to notice that this is, in this particular Python source file, actually `from pytest import xfail` and not complain about it.

Of course, as noted in #2602, a real solution would not only resolve called functions and methods, but also check whether they are type-annotated as NoReturn, and base the decision on that. If you say that my temporary band-aid is not worth merging since a type-checking solution may be the way to go, that would be fine, too.

Thanks again, and keep up the great work!

G'luck,
Peter


---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_return/rules.rs`:181 on 2023-02-10 01:41_

So we do have a system to support these -- grep around for `checker.resolve_call_path`. That will abstract away all the concerns around `import`, `import from`, aliasing, etc., and do the "right thing" in all cases to ensure that `func` is in fact a reference to the thing we care about here.

---

_@charliermarsh reviewed on 2023-02-10 01:41_

---

_@charliermarsh reviewed on 2023-02-10 01:42_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_return/rules.rs`:181 on 2023-02-10 01:42_

`crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs` would be one concrete example of the syntax and usage.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_return/rules.rs`:232 on 2023-02-10 01:44_

This is totally unrelated but I wonder why we don't raise on `While` and `Try`.

---

_@charliermarsh reviewed on 2023-02-10 01:44_

---

_@ppentchev reviewed on 2023-02-10 09:54_

---

_Review comment by @ppentchev on `crates/ruff/src/rules/flake8_return/rules.rs`:181 on 2023-02-10 09:54_

Wow. That made things... a lot simpler. But then, yes, that's what happens when a project already has the infrastructure in place to build upon. Thanks!

---

_Comment by @ppentchev on 2023-02-10 09:56_

Right, so after @charliermarsh pointed me in the right direction on resolving function calls, things are a bit simpler now in https://github.com/charliermarsh/ruff/pull/2701/commits/41ee9a827b1c25e34f2ef2391224d3fecbca565e
Of course, the question remains of whether an allowlist that Somebody(tm) needs to maintain is the right path to take, but it might help things along for the present.

---

_Merged by @charliermarsh on 2023-02-10 14:28_

---

_Closed by @charliermarsh on 2023-02-10 14:28_

---

_Comment by @charliermarsh on 2023-02-10 14:28_

This is great, thanks! I'm generally a fan of pragmatic improvements like this.

---

_Comment by @ngnpope on 2023-02-10 14:53_

Thanks for this @ppentchev! Without typing support this is the next best thing.

Also nice picking up on stuff in the standard library.

---

_Branch deleted on 2023-02-10 16:31_

---
