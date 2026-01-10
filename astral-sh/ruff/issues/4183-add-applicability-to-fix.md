```yaml
number: 4183
title: "Add `Applicability` to `Fix`"
type: issue
state: closed
author: MichaReiser
labels:
  - internal
  - help wanted
assignees: []
created_at: 2023-05-02T09:12:12Z
updated_at: 2023-05-10T06:42:48Z
url: https://github.com/astral-sh/ruff/issues/4183
synced_at: 2026-01-10T11:09:47Z
```

# Add `Applicability` to `Fix`

---

_Issue opened by @MichaReiser on 2023-05-02 09:12_

Part of #4181 and depends on #4182

* Add a new `Applicability` enum in `fix.rs` similar to [Rome's](https://github.com/rome/tools/blob/38104b339da8ff688f469799e1fc3f4a46f3d2ec/crates/rome_diagnostics/src/suggestion.rs#L6-L16) or [Clippy's](https://doc.rust-lang.org/stable/nightly-rustc/rustc_errors/enum.Applicability.html) `Applicability` enums with three variants: 
	* `Safe` (or `Automatic`, `Always`, `MachineApplicable`?)
	* `MaybeIncorrect`
	* `Unspecified`
* Add the `applicability` field to `Fix`
* Add the new `safe` and `safe_edits` constructor functions (identical to `unspecified` and `unspecified_edits)`
* Add the new `maybe_incorrect` and `maybe_incorrect_edits` constructor functions (identical to `unspecified` and `unspecified_edits)`
* Mark `unspecified` and `unspecified_edits` as `deprecated` (new lints should use either `safe_*` or `maybe_incorrect_*). Add `allow(deprecated)` at the call-sites. 
* Change the [`Diff`](https://github.com/charliermarsh/ruff/blob/fdd5fd2ad1999ef43a418da29f37858fd98cb151/crates/ruff/src/message/diff.rs#L54) implementation to write `Suggested Fix` for `MaybeIncorrect` and `Unspecified`, and `Safe fix` for `Applicability::Safe`. 

---

_Label `help wanted` added by @MichaReiser on 2023-05-02 09:16_

---

_Label `internal` added by @MichaReiser on 2023-05-02 09:24_

---

_Comment by @zanieb on 2023-05-03 01:58_

FYI I've started working on this branched off of #4198 

---

_Assigned to @zanieb by @MichaReiser on 2023-05-03 07:56_

---

_Comment by @MichaReiser on 2023-05-03 14:42_

This will allow us to change `min_start` to return a `TextSize` rather than `Option<TextSize>` because it is guaranteed that the `Fix` always contains at least one edit.

---

_Comment by @zanieb on 2023-05-04 22:03_

I'm working on the names here and looking for something where they're coherent with each other. I'm curious if we're likely to add more levels in the future?

For example, here's an option centered on "certainty" that's consistent while leaving room for feature options

```rust
enum Applicability {
    /// The change is guaranteed to be correct and safe to apply automatically.
    Certain,

    /// The change is likely to be correct, but may require a manual review to ensure correctness.
    Likely,

    /// The change might not be correct, and requires manual review before applying.
    Uncertain,

    /// The change could introduce errors or break the code, and should be applied with caution.
    Risky,

    /// The applicability of the change is not specified or cannot be determined.
    Unspecified,
}
```

Or if we do not need more options in the future

```rust
enum Applicability {
    Safe,
    Unsafe,
    Unspecified
}
```

This option matches your safe/unsafe framing in the primary issue https://github.com/charliermarsh/ruff/issues/4181 — I think this may be the clearest framing if there are not going to be additional options?

Or if we want to center on how it will be applied

```rust
enum Applicability {
    Automatic,
    Suggested,
    Unspecified
}
```

---

_Comment by @MichaReiser on 2023-05-05 07:14_

> I'm working on the names here and looking for something where they're coherent with each other. I'm curious if we're likely to add more levels in the future?

I don't think we can rule it out completely. For example, Clippy has a `HasPlaceholders` `Applicability` which could be useful. 

Thank you for your thoughtful suggestions. They are way more consistent than my initial proposal :)

The challenge I'm facing right now evaluating the proposals is that we need to make a call on what a *future* `Appicability` level could be: Will it represent a more risky code fixe, or mark incomplete code fixes (that e.g. require manual edits from users)? Depending on that, an `Applicability level around `certainty` or how it is applied is more appropriate. 


> For example, here's an option centered on "certainty" that's consistent while leaving room for feature options

I'm trying to think about how the fix review workflows for `Likely`, `Uncertain`, and `Risky` would differ because it seems all of them require manual review. We could show some scary warnings around `Uncertain` and `Risky` fixes but I fear that it will be difficult for users to understand the differences (or why some fixes have scary warnings and others don't). 

> Or if we want to center on how it will be applied

That's why I'm leaning more towards this naming, but I fail to come up with a good name for a fix that we "propose" to a user but e.g. don't support applying automatically because we want users to manually apply, either because the code is incomplete or has a good chance to be incorrect. Would that be `Manual`? 

What's your preference?



---

_Comment by @zanieb on 2023-05-07 15:59_

I think the following makes sense

```rust
pub enum Applicability {
    /// The fix is definitely what the user intended, or maintains the exact meaning of the code.
    /// This fix should be automatically applied.
    Automatic,

    /// The fix may be what the user intended, but it is uncertain.
    /// The fix should result in valid code if it is applied.
    /// The fix can be applied with user opt-in.
    Suggested,

    /// The fix has a good chance of being incorrect or the code is incomplete.
    /// The fix may result in invalid code if it is applied.
    /// The fix should only be manually applied by the user.
    Manual,

    /// The applicability of the fix is unknown.
    #[default]
    Unspecified,
}
```

I agree that "certainty" doesn't really help with the typical user workflow. 

---

_Comment by @MichaReiser on 2023-05-08 06:27_

> I think the following makes sense

I like it. Thanks for pushing for, and improving the naming! 

---

_Closed by @MichaReiser on 2023-05-10 06:42_

---
