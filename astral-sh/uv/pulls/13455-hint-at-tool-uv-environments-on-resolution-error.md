```yaml
number: 13455
title: "Hint at `tool.uv.environments` on resolution error"
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/hint-different-environments
created_at: 2025-05-14T16:13:52Z
updated_at: 2025-06-06T14:17:53Z
url: https://github.com/astral-sh/uv/pull/13455
synced_at: 2026-01-12T16:10:41Z
```

# Hint at `tool.uv.environments` on resolution error

---

_@konstin_

Users are not (yet) properly familiar with the concept of universal resolution and its implication that we need to resolve for all possible platforms and Python versions. Some projects only target a specific platform or Python version, and users experience resolution errors due to failures for other platforms. Indicated by the number of questions we get about it, `tool.uv.environments` for restricting environments is not well discoverable.

We add a special hint when resolution failed on a fork disjoint with the current environment, hinting the user to constrain `requires-python` and `tool.uv.environments` respectively.

The hint has false positives for cases where the resolution failed on a different platform, but equally fails on the current platform, in cases where the non-current fork was tried earlier. Given that conflicts can be based on `requires-python`, afaik we can't parse whether the current platform would also be affected from the derivation tree.

Two cases not covered by this are build errors as well as install errors that need `tool.uv.required-environments`.

---

_Label `error messages` added by @konstin on 2025-05-14 16:15_

---

_Marked ready for review by @konstin on 2025-05-16 09:59_

---

_@jtfmumm reviewed on 2025-05-23 18:52_

---

_Review comment by @jtfmumm on `crates/uv-resolver/src/error.rs`:381 on 2025-05-23 18:52_

Should this be `The resolution failed for a Python version range excluding the current ...`

---

_@konstin reviewed on 2025-05-23 19:03_

---

_Review comment by @konstin on `crates/uv-resolver/src/error.rs`:381 on 2025-05-23 19:03_

Weakly towards "outside" for being the simpler word than the mathematical range-excludes, but no strong opinion.

---

_@jtfmumm reviewed on 2025-05-23 19:06_

---

_Review comment by @jtfmumm on `crates/uv-resolver/src/error.rs`:381 on 2025-05-23 19:06_

It's mostly that you'd normally use "outside" the other way: "the current Python version falls outside the range"

---

_Review comment by @geofft on `crates/uv-resolver/src/error.rs`:381 on 2025-05-26 20:52_

Can we use even more words here and include the versions in question? What about something like

"hint: There is a resolution for your current Python version (3.9), but your project supports multiple Python versions, and your dependencies cannot be resolved on Python >=3.10. If your project does not need to support Python >=3.10, consider limiting the Python version range listed in `requires-python` in pyproject.toml."

Also, is it possible (as in, is it house style) for both of these errors to link to some page in the uv docs explaining why this might happen and what to do about it? A docs page would give us room to including (links to) the full syntax for `requires-python` and `tool.uv.environments` is, as well as some examples of how to write useful ones (e.g. how to do "exclude Windows" instead of "only Linux", or how to do "Python 3.13 and up" instead of "Python 3.13").

---

_@geofft reviewed on 2025-05-26 20:55_

---

_Review comment by @zanieb on `crates/uv-resolver/src/error.rs`:381 on 2025-05-27 14:40_

> Also, is it possible (as in, is it house style) for both of these errors to link to some page in the uv docs explaining why this might happen and what to do about it? 

We do not do this yet because we don't have the ability to permalink to documentation (i.e., it's not versioned)

---

_@zanieb reviewed on 2025-05-27 14:40_

---

_Assigned to @zanieb by @zanieb on 2025-05-27 14:40_

---

_@zanieb reviewed on 2025-05-27 14:46_

---

_Review comment by @zanieb on `crates/uv-resolver/src/error.rs`:381 on 2025-05-27 14:46_

I do think the suggestion to include concrete versions and environments seems important. I think having a concrete suggestion to the user e.g., about how to exclude the failing environment, would also be really helpful. I think suggesting a upper bound on `requires-python` seems feasible too? At least in the common / simple case?

---

_@konstin reviewed on 2025-05-27 14:55_

---

_Review comment by @konstin on `crates/uv-resolver/src/error.rs`:381 on 2025-05-27 14:55_

I added the current Python version. If we want the Python version of the split it gets more complex, as would have to traverse the ADD, but we already show the split markers on top.

---

_Review comment by @zanieb on `crates/uv-resolver/src/error.rs`:381 on 2025-05-27 14:55_

We also _could_ just call this an incremental improvement and open issues to track making the hint more helpful.

---

_@zanieb reviewed on 2025-05-27 14:55_

---

_@konstin reviewed on 2025-05-27 15:00_

---

_Review comment by @konstin on `crates/uv-resolver/src/error.rs`:381 on 2025-05-27 15:00_

The improvement was prompted by writing the advice that's now in the hints in issue reports repeatedly, so I would consider this an incremental improvement and refine if we still get questions from users.

---

_@zanieb reviewed on 2025-05-27 15:41_

---

_Review comment by @zanieb on `crates/uv-resolver/src/error.rs`:381 on 2025-05-27 15:41_

My concern is mostly that you have the context on this now and it'll be more work to pick it up later.

---

_@geofft reviewed on 2025-05-27 18:02_

---

_Review comment by @geofft on `crates/uv-resolver/src/error.rs`:381 on 2025-05-27 18:02_

Thanks, new version is helpful. I agree that given that the split marker is displayed in the full error, it's not necessary (but would be nice) to repeat it in the hint at the bottom.

I still think there's some room to improve the phrasing - "a Python version range excluding the current Python version" is mathematically accurate, but I can totally imagine people interpreting it as, "why is the current Python version excluded? I need to figure out how to include it." How about one of these?

```
Although the active Python version is {{current_python_version}},
the resolution failed for other Python versions supported by your
project. Consider limiting your project's supported Python versions
using `requires-python`.
```

```
Resolution succeeded for Python {{current_python_version}}, but not
for other Python versions supported by your project. Consider
limiting your project's supported Python versions using `requires-python`.
```

---

_@konstin reviewed on 2025-05-28 13:55_

---

_Review comment by @konstin on `crates/uv-resolver/src/error.rs`:381 on 2025-05-28 13:55_

The problem to giving better suggestions is the limited context we have when encountering this error. Consider the forks in transformers:

```toml
resolution-markers = [
    "python_full_version >= '3.13' and sys_platform == 'darwin'",
    "python_full_version == '3.12.*' and sys_platform == 'darwin'",
    "python_full_version >= '3.13' and platform_machine == 'aarch64' and sys_platform == 'linux'",
    "python_full_version == '3.12.*' and platform_machine == 'aarch64' and sys_platform == 'linux'",
    "(python_full_version >= '3.13' and platform_machine != 'aarch64' and sys_platform == 'linux') or (python_full_version >= '3.13' and sys_platform != 'darwin' and sys_platform != 'linux')",
    "(python_full_version == '3.12.*' and platform_machine != 'aarch64' and sys_platform == 'linux') or (python_full_version == '3.12.*' and sys_platform != 'darwin' and sys_platform != 'linux')",
    "python_full_version == '3.11.*' and sys_platform == 'darwin'",
    "python_full_version == '3.11.*' and platform_machine == 'aarch64' and sys_platform == 'linux'",
    "(python_full_version == '3.11.*' and platform_machine != 'aarch64' and sys_platform == 'linux') or (python_full_version == '3.11.*' and sys_platform != 'darwin' and sys_platform != 'linux')",
    "python_full_version == '3.10.*' and sys_platform == 'darwin'",
    "python_full_version == '3.10.*' and platform_machine == 'aarch64' and sys_platform == 'linux'",
    "(python_full_version == '3.10.*' and platform_machine != 'aarch64' and sys_platform == 'linux') or (python_full_version == '3.10.*' and sys_platform != 'darwin' and sys_platform != 'linux')",
    "python_full_version < '3.10' and platform_machine == 'arm64' and sys_platform == 'darwin'",
    "python_full_version < '3.10' and platform_machine == 'aarch64' and sys_platform == 'linux'",
    "(python_full_version < '3.10' and platform_machine != 'arm64' and sys_platform == 'darwin') or (python_full_version < '3.10' and platform_machine != 'aarch64' and sys_platform == 'linux') or (python_full_version < '3.10' and sys_platform != 'darwin' and sys_platform != 'linux')",
]
```

If we fail in one of those forks, do we know that it's about that fork and that it's not a more general, or maybe a specific subset? Is it about the platform component or the Python version component of the marker for those who have both? If we have multiple `python_full_version` components, which one do we show (and how do extract it from the `MarkerTree`)?

In user support, my experience was that users were lost with the current error message, but could figure it out when given the right keywords, which were reduce `requires-python` and setting `tool.uv.environments` respectively (and, not covered here, `tool.uv.required-environments` for installation). If we have specific cases where we have the confidence to make more concrete suggestions, I can add more specific branches.

---

_@zanieb approved on 2025-05-28 14:26_

---

_Comment by @konstin on 2025-05-28 14:41_

Ready to merge but the depot runners are down atm

---

_Comment by @zanieb on 2025-06-06 13:55_

@konstin this needs an update with `main`

---

_Merged by @konstin on 2025-06-06 14:17_

---

_Closed by @konstin on 2025-06-06 14:17_

---

_Branch deleted on 2025-06-06 14:17_

---
