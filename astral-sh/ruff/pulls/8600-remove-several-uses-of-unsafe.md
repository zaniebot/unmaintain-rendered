```yaml
number: 8600
title: "remove several uses of `unsafe`"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/reduce-unsafe
created_at: 2023-11-10T14:41:41Z
updated_at: 2023-11-28T14:50:05Z
url: https://github.com/astral-sh/ruff/pull/8600
synced_at: 2026-01-12T15:55:26Z
```

# remove several uses of `unsafe`

---

_@BurntSushi_

This PR removes several uses of `unsafe`. I generally limited myself to low hanging fruit that I could see. There are still a few remaining uses of `unsafe` that looked a bit more difficult to remove (if possible at all). But this gets rid of a good chunk of them.

I put each `unsafe` removal into its own commit with a justification for why I did it. So I would encourage reviewing this PR commit-by-commit. That way, we can legislate them independently. It's no problem to drop a commit if we feel the `unsafe` should stay in that case.

---

_Review requested from @charliermarsh by @BurntSushi on 2023-11-10 14:41_

---

_Review requested from @zanieb by @BurntSushi on 2023-11-10 14:41_

---

_Review requested from @konstin by @BurntSushi on 2023-11-10 14:41_

---

_@zanieb approved on 2023-11-10 14:52_

LGTM, but I'm the least qualified :)

---

_Comment by @charliermarsh on 2023-11-10 14:57_

I'd prefer to have Micha review this when he's back since he has context on all of these, and I suspect they were done intentionally (though that doesn't necessarily mean they shouldn't be changed).

---

_Review requested from @MichaReiser by @BurntSushi on 2023-11-10 15:00_

---

_Comment by @github-actions[bot] on 2023-11-10 15:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @BurntSushi on 2023-11-10 16:20_

Benchmarks from codspeed seem to support my local benchmarks that there's little to no impact: https://codspeed.io/astral-sh/ruff/branches/ag/reduce-unsafe

---

_Review comment by @konstin on `crates/ruff_formatter/src/arguments.rs`:6 on 2023-11-10 16:21_

no more unsafe in the formatter :heart_eyes: (sorry micha)

---

_Review comment by @konstin on `crates/ruff_macros/src/newtype_index.rs`:50 on 2023-11-10 16:26_

Is this ever called in a const context? We had some `const fn` that didn't actually need to be const

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/map.rs`:546 on 2023-11-10 16:34_

I'm surprised there isn't a method in std that combines the assert and `NonZeroU32::new` (like `NonZeroU32::try_from(value + 1)`, but apparently `NonZeroU32` doesn't even have a "normal" add

---

_@konstin approved on 2023-11-10 16:35_

---

_@BurntSushi reviewed on 2023-11-10 16:48_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/comments/map.rs`:546 on 2023-11-10 16:48_

In this case, it would need to be a `TryFrom<usize> for NonZeroU32`. And if you fill out the full complement of those across all the `NonZero` types, it ends up being quite a lot of impls. Not clear that's worth doing when you can compose things, e.g., `u32::try_from(value).ok().and_then(std::num::NonZeroU32::new)` or something like that.

And yeah it doesn't have `Add` because there are some thorny decisions around its semantics. e.g., What happens when you do `NonZeroU32::MAX + 1`. And if you had subtraction, then `NonZeroU32::new(1).unwrap() - 1`. You'd have to panic I think.

I also don't think anyone has seriously proposed adding all of the arithmetic methods to the various `NonZero` types either. (They do have checked/saturation operations where appropriate.)

---

_@BurntSushi reviewed on 2023-11-10 16:57_

---

_Review comment by @BurntSushi on `crates/ruff_macros/src/newtype_index.rs`:50 on 2023-11-10 16:57_

It is yeah: https://github.com/astral-sh/ruff/blob/995649f45584af2dc8a1d128bfb1ed852dfcaf99/crates/ruff_macros/src/newtype_index.rs#L40

Anyway you slice it, `Self::MAX` having type `Self` means you need to build a `NonZeroU32` in a const context. And to do that for `u32::MAX - 1`, I believe that requires some kind of `unsafe` uttered somewhere. It's possible that `Self::MAX` doesn't need to be const and could be a method instead, but that's a bigger change.

I think we can probably just leave this one here for now.

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/format_element.rs`:347 on 2023-11-27 05:06_

We currently don't use best fitting in `ruff_python_formatter` so the cost of this change is unknown. 

My preferred solution for this would be to change `most_expanded` and `most_flat` to return `Result` and propagate the error inside of the `Printer` (with a proper message) as we now do for most (all) other invalid cases in the printer (instead of panicking).

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/arguments.rs`:6 on 2023-11-27 05:09_

This makes me sad :P

You can propose the same change to `std::fmt::Argument` ;)

---

_@MichaReiser reviewed on 2023-11-27 05:09_

---

_@BurntSushi reviewed on 2023-11-27 13:33_

---

_Review comment by @BurntSushi on `crates/ruff_formatter/src/format_element.rs`:347 on 2023-11-27 13:33_

Are you thinking about using a [`FormatError`](https://github.com/astral-sh/ruff/blob/f4d64720369e9de20bea76879a8f0693e1dc2b2c/crates/ruff_formatter/src/diagnostics.rs#L6-L27)? If so, which variant is appropriate here? I don't think I have enough context to write a good user facing error message here. (A panic looks appropriate to me here, since I imagine this is a bug in the code if the assert gets tripped. But I'm not completely certain.)

I suppose a behavior preserving change here would be to switch the `assert!` back to `debug_assert!` and document the panic conditions to `most_expanded` and `most_flat`.

---

_@BurntSushi reviewed on 2023-11-27 13:44_

---

_Review comment by @BurntSushi on `crates/ruff_formatter/src/arguments.rs`:6 on 2023-11-27 13:44_

I don't think it works there. In std, we're "escaping" out of multiple different trait impls: https://github.com/rust-lang/rust/blob/b29a1e00f850e548f3021ea523d0e143724fa9b7/library/core/src/fmt/rt.rs#L95-L130

Here it looks like we only need one particular trait impl, so we can just use a `dyn` and get what we need.

---

_@MichaReiser reviewed on 2023-11-27 23:04_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/format_element.rs`:347 on 2023-11-27 23:04_

It would be a new variant (or two) in `InvalidDocumentError` but I'm also okay keeping the panics in `most_expanded` and `most_flat` and pointing it out in the documentation that Printing will fail with a panic if passed with less than 2 variations. 

The debug assertion mainly exists to make debugging easier, because you can identify the incorrect call from the call-stack which you can't when it panics during printing (and document inspection in the printer is a pain).

---

_@MichaReiser approved on 2023-11-27 23:05_

Sorry. I didn't notice that this wasn't merged yet or I would have approved yesterday.

LGTM. I would prefer keeping the debug assertion in the `BestFitting` constructor and instead panic when using `most_expanded` or `most_flat` for performance reasons.

---

_Label `internal` added by @MichaReiser on 2023-11-27 23:06_

---

_Comment by @BurntSushi on 2023-11-28 14:26_

@MichaReiser Aye, done! I've updated the code to switch back to a `debug_assert!` at construction time, but panic in the `most_{flat,expanded}` methods.

---

_Merged by @BurntSushi on 2023-11-28 14:50_

---

_Closed by @BurntSushi on 2023-11-28 14:50_

---

_Branch deleted on 2023-11-28 14:50_

---
