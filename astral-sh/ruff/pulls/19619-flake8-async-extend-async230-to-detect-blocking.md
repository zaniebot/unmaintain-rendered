```yaml
number: 19619
title: "[`flake8-async`] Extend `ASYNC230` to detect blocking `pathlib` I/O methods"
type: pull_request
state: open
author: aidandj
labels:
  - rule
  - preview
assignees: []
base: main
head: aidan/async-230-pathlib-read-write
created_at: 2025-07-29T17:07:48Z
updated_at: 2025-08-05T12:30:20Z
url: https://github.com/astral-sh/ruff/pull/19619
synced_at: 2026-01-12T15:56:43Z
```

# [`flake8-async`] Extend `ASYNC230` to detect blocking `pathlib` I/O methods

---

_@aidandj_



<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Add detection for pathlib's blocking I/O methods (`read_text`, `read_bytes`, `write_text`, `write_bytes`) in async functions, beyond just the `open()` method. Under the hood, these methods call `open`, so they have the same issue that `open` does.

## Test Plan

Added test cases to the existing ASYNC230 test suite and updated the snapshot.


---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_open_call.rs`:82 on 2025-07-29 18:42_

I think we could use `matches!` here:


```suggestion
    if !matches!(attr.as_str(), "open"
        | "read_text"
        | "read_bytes"
        | "write_text"
        | "write_bytes")
    {
```

However, we will need two checks here for now because I think we should gate this behind preview since it's an expansion of a stable rule. You can see examples of these in `preview.rs`:

https://github.com/astral-sh/ruff/blob/d449c541cb2f232d06716cced5f0be27c437079b/crates/ruff_linter/src/preview.rs#L224-L227

and references to those functions.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC230.py`:25 on 2025-07-29 18:43_

Can you revert this? It will make the snapshots a bit easier to review since it won't shift everything down :)

---

_@ntBre reviewed on 2025-07-29 18:49_

Thanks! This change makes sense to me, but I think we should preview-gate it since the rule itself is stable.

Otherwise I just had a couple of small nits.

---

_Label `preview` added by @ntBre on 2025-07-29 18:50_

---

_Label `rule` added by @ntBre on 2025-07-29 18:50_

---

_@aidandj reviewed on 2025-07-29 18:50_

---

_Review comment by @aidandj on `crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC230.py`:25 on 2025-07-29 18:50_

Sorry, I kept trying to revert this but my formatter kept adding it back. I'll revert it again.

---

_Renamed from "[flake8-async] Extend ASYNC230 to detect blocking pathlib I/O methods" to "[`flake8-async`] Extend `ASYNC230` to detect blocking `pathlib` I/O methods" by @ntBre on 2025-07-29 18:51_

---

_Review comment by @aidandj on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_open_call.rs`:82 on 2025-07-29 18:51_

Thanks for the `matches` suggestion. Rust newbie here, still learning.

I'll look into the preview stuff.

---

_@aidandj reviewed on 2025-07-29 18:51_

---

_@ntBre reviewed on 2025-07-29 18:52_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC230.py`:25 on 2025-07-29 18:52_

No worries, I habitually hit my formatter binding and always have to hold back when modifying snapshots too.

---

_Review requested from @ntBre by @aidandj on 2025-07-29 19:33_

---

_Comment by @aidandj on 2025-07-29 19:39_

I did a double check in the pathlib library and found `touch` as well calls io.open. Added that in there.

I found a pattern of enabling preview tests in another file and copied that over, if there is a better method for preview tests let me know. It would be nice to tie them to those helper methods so that when you deleted the helper method you would know to move the test out of the preview call. Maybe that could be as simple as calling the helper function in the preview test to bring attention to it.

Also, who's responsibility is it to take it out of preview? The PR author? Or the ruff team?

---

_Comment by @MichaReiser on 2025-07-30 07:00_

> Also, who's responsibility is it to take it out of preview? The PR author? Or the ruff team?

We'll take it out of preview, following our versioning policy. 

Is this change in line with the upstream ASYNC rule?

---

_Comment by @ntBre on 2025-07-30 12:14_

> Is this change in line with the upstream ASYNC rule?

I should have mentioned this somewhere, but I looked into the upstream rule yesterday, and it looks like they only flag `open`, `io.open`, and `io.open_code`, so I concluded that we'd already diverged from upstream by handling `pathlib.Path.open`. But I'm certainly open to your input.

https://github.com/python-trio/flake8-async/blob/1bf61068d7cdaa25b8bdb91cbff017a785df1220/flake8_async/visitors/visitor2xx.py#L277-L278

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_async/snapshots/ruff_linter__rules__flake8_async__tests__ASYNC230_ASYNC230.py.snap`:108 on 2025-08-04 08:29_

The diagnostics here are fairly confusing as it's not clear to me where I'm calling `open`. Can we add a help text saying that `read_text` calls `open` internally`. We may also want to update the rule's documentation to mention the new (preview only behavior)

---

_@MichaReiser reviewed on 2025-08-04 08:31_

---

_Comment by @MichaReiser on 2025-08-04 08:40_

> I should have mentioned this somewhere, but I looked into the upstream rule yesterday, and it looks like they only flag open, io.open, and io.open_code, so I concluded that we'd already diverged from upstream by handling pathlib.Path.open. But I'm certainly open to your input.

Considering `Path.open` does make sense and seems less of a divergence to me than including extra methods. While the new methods are in the spirit of the rule, it does push its boundaries, e.g., a better rule name might be `blocking-file-operation-in-async-function` as the problem isn't just open (I suspect it's any file IO operation)?

Unlike other rules, the upstream async linter is very actively maintained. That makes it more likely that we'll run into conflicting rules. 

I tried to find a related issue or PR upstream but wasn't successful. @Zac-HD: Is this something that has come up with the async rules before? Is this a change you would be open to also make in `flake8-async` (I'm not expecting you to make the change yourself, just if it's in line with how you designed the rules)?


---

_@aidandj reviewed on 2025-08-04 14:51_

---

_Review comment by @aidandj on `crates/ruff_linter/src/rules/flake8_async/snapshots/ruff_linter__rules__flake8_async__tests__ASYNC230_ASYNC230.py.snap`:108 on 2025-08-04 14:51_

Another option would be to simplify and just remove the mention of open. And just say blocking methods.

---

_Comment by @Zac-HD on 2025-08-04 16:42_

I think this'd be the first time ruff was ahead of flake8-async, nice!

How about we move this new logic to a new ASYNC233 code, and open an upstream issue to implement it / reserve the number?  I think this is a much better experience for users updating their linter too, since they can use some temporary ignores without missing the current 230 issues.

---

_Comment by @aidandj on 2025-08-04 16:59_

> I think this'd be the first time ruff was ahead of flake8-async, nice!
> 
> How about we move this new logic to a new ASYNC233 code, and open an upstream issue to implement it / reserve the number? I think this is a much better experience for users updating their linter too, since they can use some temporary ignores without missing the current 230 issues.

This seems reasonable. Let me know if there is consensus and I could take a stab at that as well

---

_Comment by @Zac-HD on 2025-08-04 20:19_

Upstream issue opened!

Practically speaking, if you're interested in implementing more rules I'd pretty strongly prefer [pulling more into `ruff`](https://github.com/astral-sh/ruff/issues/8451) relative to moving this one in the other direction; I already mostly use ruff for speed 😁 

---

_Comment by @MichaReiser on 2025-08-05 08:16_

Thanks @Zac-HD 

> Practically speaking, if you're interested in implementing more rules I'd pretty strongly prefer https://github.com/astral-sh/ruff/issues/8451 relative to moving this one in the other direction; I already mostly use ruff for speed 😁

We're interested, it's just hard to prioritize with everything else going but we could mark some of those as help wanted (CC: @ntBre wdty?)

A new code sounds good to me. But this may justify removing the exsting pathlib check from `ASYNC230` (under preview)

---

_Comment by @jakkdl on 2025-08-05 10:52_

> Thanks @Zac-HD
> 
> > Practically speaking, if you're interested in implementing more rules I'd pretty strongly prefer #8451 relative to moving this one in the other direction; I already mostly use ruff for speed 😁
> 
> We're interested, it's just hard to prioritize with everything else going but we could mark some of those as help wanted (CC: @ntBre wdty?)
> 
> A new code sounds good to me. But this may justify removing the exsting pathlib check from `ASYNC230` (under preview)

I don't think we have any preference between adding `pathlib.Path.open` to `ASYNC230` vs adding all `pathlib` methods to an `ASYNC233` in upstream, so we can simply sync to what you end up deciding to do.

---

_Comment by @ntBre on 2025-08-05 12:30_

> We're interested, it's just hard to prioritize with everything else going but we could mark some of those as help wanted (CC: @ntBre wdty?)

Sounds good to me!

Thank y'all!

---
