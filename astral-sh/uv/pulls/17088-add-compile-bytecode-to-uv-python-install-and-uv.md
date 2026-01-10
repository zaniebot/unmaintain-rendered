---
number: 17088
title: "Add `--compile-bytecode` to `uv python install` and `uv python upgrade` to compile the standard library"
type: pull_request
state: open
author: EliteTK
labels: []
assignees: []
base: main
head: tk/py-install-bytecomp
created_at: 2025-12-11T16:37:16Z
updated_at: 2025-12-11T17:40:41Z
url: https://github.com/astral-sh/uv/pull/17088
synced_at: 2026-01-10T01:26:21Z
---

# Add `--compile-bytecode` to `uv python install` and `uv python upgrade` to compile the standard library

---

_Pull request opened by @EliteTK on 2025-12-11 16:37_

## Summary

Implement #16408.

Currently doesn't avoid recompiling the bytecode when it is already compiled, which I initially thought could be fine but then realised that it would be annoying if you have the environment variable set.

Covers upgrades, installs, reinstalls, and compiling existing installs. But not certain about the UX.

pyodide is weird and currently gets skipped by the heuristic I came up with.

## Test Plan

Styling of the status report was manually tested, there is a new test for testing the actual functionality.


---

_Review requested from @konstin by @EliteTK on 2025-12-11 16:37_

---

_Comment by @codspeed-hq[bot] on 2025-12-11 16:52_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/tk%2Fpy-install-bytecomp?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #17088 will **not alter performance**

<sub>Comparing <code>tk/py-install-bytecomp</code> (cc0d3f9) with <code>main</code> (59d73fd)</sub>



### Summary

`✅ 5` untouched  





---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:5871 on 2025-12-11 17:13_

I'd put something other than "critical", like "For cases where first-start time is important"

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:5875 on 2025-12-11 17:15_

I wouldn't compare to pip in a non-pip interface.

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:445 on 2025-12-11 17:19_

Is there a specific reason to sort inside here, vs. ensuring we're getting a fixed order from previous code?

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:484 on 2025-12-11 17:21_

That's elegant

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:519 on 2025-12-11 17:21_

@oconnor663 Since we talked about `.await`ing while another future is pending - you and @EliteTK both have a better understanding of this than me.

The tl;dr from that conversation is that if we await here, the download futures stall. I think we actually have to do the bytecode compilation inside the download stream and use a semaphore to limit the number of parallel stdlib compilations?

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:1123 on 2025-12-11 17:25_

Is that for pyodide?

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:5872 on 2025-12-11 17:27_

Notably, also larger directory sizes, which is the reason why the pyc files are often not shipped by default

---

_Review comment by @konstin on `crates/uv/tests/it/python_install.rs`:4004 on 2025-12-11 17:28_

Doesn't that count change when we bump the patch version?

---

_Review comment by @konstin on `crates/uv/tests/it/help.rs`:583 on 2025-12-11 17:32_

The option can be in the main options section, "Installer options" generally refers to things that control wheel installs, not Python installs.

---

_@konstin reviewed on 2025-12-11 17:34_

Looks good! Added some smaller comments.

---

_@zanieb reviewed on 2025-12-11 17:40_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:5872 on 2025-12-11 17:40_

Yeah this will also increase your Docker image size, so it's a trade-off in that regard

---

_@zanieb reviewed on 2025-12-11 17:40_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:5866 on 2025-12-11 17:40_

We should also document at https://docs.astral.sh/uv/guides/integration/docker/#compiling-bytecode

---
