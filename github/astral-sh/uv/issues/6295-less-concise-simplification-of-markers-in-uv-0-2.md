---
number: 6295
title: Less concise simplification of markers in uv 0.2.35
type: issue
state: closed
author: A5rocks
labels:
  - duplicate
  - wish
assignees: []
created_at: 2024-08-21T02:27:14Z
updated_at: 2024-08-21T13:50:46Z
url: https://github.com/astral-sh/uv/issues/6295
synced_at: 2026-01-07T13:12:17-06:00
---

# Less concise simplification of markers in uv 0.2.35

---

_Issue opened by @A5rocks on 2024-08-21 02:27_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Noticed in https://github.com/python-trio/trio/pull/3068, I haven't tried to get a smaller case. The command is `uv pip compile --universal --python-version=3.8 test-requirements.in -o test-requirements.txt`. Platform is Ubuntu, `uv` is version 0.3.0 but we didn't run `uv --version`.

uv 0.3.0 tries to make this change:
```diff
-colorama==0.4.6 ; sys_platform == 'win32' or (implementation_name == 'cpython' and platform_system == 'Windows')
+colorama==0.4.6 ; (implementation_name != 'cpython' and sys_platform == 'win32') or (platform_system != 'Windows' and sys_platform == 'win32') or (implementation_name == 'cpython' and platform_system == 'Windows')
```

This might mean https://github.com/astral-sh/uv/pull/5898 doesn't simplify in this case as well. Maybe it's missing some necessary case?

Why is `(implementation_name != 'cpython' and sys_platform == 'win32') or (platform_system != 'Windows' and sys_platform == 'win32') or (implementation_name == 'cpython' and platform_system == 'Windows')` equivalent? (just to show the previous result wasn't just a bug and since it was a bit confusing to me)

| markers | why is this equivalent |
|--------|--------|
| `(implementation_name != 'cpython' and sys_platform == 'win32') or (platform_system != 'Windows' and sys_platform == 'win32') or (implementation_name == 'cpython' and platform_system == 'Windows')` | its what we start with |
| `(sys_platform == 'win32' and (implementation_name != 'cpython' or platform_system != 'Windows')) or (implementation_name == 'cpython' and platform_system == 'Windows')` | distribution |
| `((implementation_name == 'cpython' and platform_system == 'Windows') or (sys_platform == 'win32')) and ((implementation_name == 'cpython' and platform_system == 'Windows') or (implementation_name != 'cpython' or platform_system != 'Windows'))` | distribution |
| `((implementation_name == 'cpython' and platform_system == 'Windows') or (sys_platform == 'win32')) and ((implementation_name == 'cpython' and platform_system == 'Windows') or NOT (implementation_name == 'cpython' and platform_system == 'Windows'))` | demorgan's law |
| `((implementation_name == 'cpython' and platform_system == 'Windows') or (sys_platform == 'win32')) and TRUE` | negation | 
| `(implementation_name == 'cpython' and platform_system == 'Windows') or (sys_platform == 'win32')` | identity |

---

_Comment by @A5rocks on 2024-08-21 02:37_

Minimal reproduction:

`x.in`:

```
pytest >= 5.0
black; implementation_name == "cpython"
```

Then run `uv pip compile x.in --no-strip-markers`. I can reproduce this in 0.2.35 and not in 0.2.34. `uv --version`: `uv 0.2.35 (e097f948c 2024-08-09)`, reproducing on Windows.

---

_Renamed from "Less concise simplification in uv 0.3.0's pip-compile universal mode" to "Less concise simplification of markers in uv 0.2.35" by @A5rocks on 2024-08-21 02:38_

---

_Comment by @charliermarsh on 2024-08-21 02:39_

I believe this is because we simplify to DNF as a normalized representation. In some cases, though, CNF would be more concise. I think this can be merged into https://github.com/astral-sh/uv/issues/5992, but let me know if you disagree.

---

_Comment by @A5rocks on 2024-08-21 02:44_

I don't think so, because the simpler version only has `or` at the top level. But maybe that's preventing one of the simplification steps?

---

_Label `wish` added by @charliermarsh on 2024-08-21 02:45_

---

_Comment by @BurntSushi on 2024-08-21 13:46_

Here's a comment from our code for DNF simplification:

```
/// Simplifies a DNF expression.
///
/// A decision diagram is canonical, but only for a given variable order. Depending on the
/// pre-defined order, the DNF expression produced by a decision tree can still be further
/// simplified.
///
/// For example, the decision diagram for the expression `A or B` will be represented as
/// `A or (not A and B)` or `B or (not B and A)`, depending on the variable order. In both
/// cases, the negation in the second clause is redundant.
///
/// Completely simplifying a DNF expression is NP-hard and amounts to the set cover problem.
/// Additionally, marker expressions can contain complex expressions involving version ranges
/// that are not trivial to simplify. Instead, we choose to simplify at the boolean variable
/// level without any truth table expansion. Combined with the normalization applied by decision
/// trees, this seems to be sufficient in practice.
///
/// Note: This function has quadratic time complexity. However, it is not applied on every marker
/// operation, only to user facing output, which are typically very simple.
```

The main issue here is that DNF on its own isn't actually unique. And full simplification might not be feasible. But I think this might require more investigation. But I think this can be safely folded into #5992, which is, I think, the same problem that is reported here: the marker is correct but not as simple as it could be.

It is really important though that we have something approach a canonical representation for markers, otherwise the lock file might not be stable. This may come at the cost of concise markers.

---

_Label `duplicate` added by @BurntSushi on 2024-08-21 13:50_

---

_Closed by @BurntSushi on 2024-08-21 13:50_

---
