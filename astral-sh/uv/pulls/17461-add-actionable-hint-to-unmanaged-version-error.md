```yaml
number: 17461
title: Add actionable hint to unmanaged version error message
type: pull_request
state: open
author: originell
labels: []
assignees: []
base: main
head: fix/python-download-hint
created_at: 2026-01-14T11:59:17Z
updated_at: 2026-01-14T16:28:00Z
url: https://github.com/astral-sh/uv/pull/17461
synced_at: 2026-01-14T16:39:18Z
```

# Add actionable hint to unmanaged version error message

---

_@originell_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

When a project requests a newer Python version than the current UV version knows about, the existing error message isn't all that useful. Suggesting updating `uv` seems like a good hint for humans (and future LLM overlords) alike.

I just ran into this with some colleagues of mine. I upgraded a project to 3.14. They pulled and did a `uv sync`, but it failed. It turned out that their uv installation was at 0.5. Running a `uv self update` to update to 0.9.x made it work again.

Before:

> error: No interpreter found for Python ==3.14.* in managed installations or search path

After:

> error: No interpreter found for Python ==3.14.* in managed installations or search path
>
> hint: No Python download is available for Python 3.14. This version might be newer than your uv. Update uv and retry.

The current idea for the hint does not explicitly mention running `uv self update`. That's because I felt like this wouldn't be a good idea due to the various ways uv could be installed. If you say `uv self update` is always safe, I'd be happy to adjust the hint and explicitly mention that command.

## Test Plan

<!-- How was it tested? -->
`cargo nextest` as laid out in the contributing docs.


---

_Review requested from @zanieb by @konstin on 2026-01-14 12:00_

---

_Review comment by @EliteTK on `crates/uv-python/src/installation.rs`:165 on 2026-01-14 13:17_

If the version request was for `Default` or `Any` and we're down this path then something else is likely broken and we should just not show this hint.

---

_Comment by @originell on 2026-01-14 13:21_

note: initial ci run rightfully discovered an error in my snapshot. i updated that. the new failure doesn't seem to be related to my code. if it is, pls let me know and i'll adjust :) i'm not too familiar with the uv codebase and rust in general

---

_Review comment by @EliteTK on `crates/uv-python/src/installation.rs`:167 on 2026-01-14 13:29_

This should say something closer to:

"This version of uv is unaware of a managed Python download for {request}. Consider updating uv if the requested version is newer."

Or something like that, I'm not 100% on the wording.

"no ... is available" implies it's unavailable which we're contradicting by suggesting 

---

_Review comment by @EliteTK on `crates/uv/tests/it/sync.rs`:13287 on 2026-01-14 13:31_

Use `.with_filtered_python_sources()` on the context instead. i.e. `let context = TestContext::new_with_versions(&["3.12"]).with_managed_python_dirs().with_filtered_python_sources()`.

Then you can drop this.

---

_@EliteTK reviewed on 2026-01-14 13:31_

---

_@originell reviewed on 2026-01-14 15:08_

---

_Review comment by @originell on `crates/uv-python/src/installation.rs`:165 on 2026-01-14 15:08_

that makes sense. thanks for that! I changed it in the latest commit

---

_@originell reviewed on 2026-01-14 15:09_

---

_Review comment by @originell on `crates/uv-python/src/installation.rs`:167 on 2026-01-14 15:09_

I get where you are going with this. I added another suggestion to the code. What do you think about:

> This uv release may not support managed Python 3.100 yet. Update uv and retry.



---

_@originell reviewed on 2026-01-14 15:09_

---

_Review comment by @originell on `crates/uv/tests/it/sync.rs`:13287 on 2026-01-14 15:09_

updated!

---

_@EliteTK reviewed on 2026-01-14 15:30_

---

_Review comment by @EliteTK on `crates/uv-python/src/installation.rs`:167 on 2026-01-14 15:30_

I am not sure about "support" there but maybe @zanieb can advise on this detail before you make any further changes. I don't want to end up being overly nitpicky here.

---

_Review comment by @originell on `crates/uv-python/src/installation.rs`:167 on 2026-01-14 15:37_

True. "Support" might be too strong of a word here. 

I don't think you are overly picky. Words are important.

I'll also sit some more on this. :) 

---

_@originell reviewed on 2026-01-14 15:37_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:13291 on 2026-01-14 16:27_

Maybe
```suggestion
    hint: uv embeds available Python downloads and may require an update to install new versions. Consider retrying on a newer version of uv.
```

---

_@zanieb reviewed on 2026-01-14 16:28_

---
