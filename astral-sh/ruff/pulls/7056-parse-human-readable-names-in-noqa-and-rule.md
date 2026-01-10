```yaml
number: 7056
title: Parse human-readable names in noqa and rule selectors
type: pull_request
state: closed
author: hotpxl
labels: []
assignees: []
base: main
head: noqa
created_at: 2023-09-01T23:59:27Z
updated_at: 2024-04-08T07:02:47Z
url: https://github.com/astral-sh/ruff/pull/7056
synced_at: 2026-01-10T22:47:01Z
```

# Parse human-readable names in noqa and rule selectors

---

_Pull request opened by @hotpxl on 2023-09-01 23:59_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Allow using human-readable rule names in `noqa` and rule selectors
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

- ran `cargo test`
- manually tested putting human-readable rule name as `noqa` code 
- manually tested rule selector on the command line
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-09-02 00:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review requested from @charliermarsh by @MichaReiser on 2023-09-02 08:36_

---

_Review comment by @charliermarsh on `crates/ruff/src/noqa.rs`:204 on 2023-09-02 16:49_

We should probably add similar redirect support for named rules. This can be a TODO for now...

---

_Review comment by @charliermarsh on `crates/ruff/src/noqa.rs`:179 on 2023-09-02 16:51_

We could make this a bit stricter by first doing `lex_code`, then if it returns `None`, try a new `lex_name`, and change that method to require at least one dash (and only lowercase characters, no digits).

---

_Review comment by @charliermarsh on `crates/ruff/src/rule_selector.rs`:62 on 2023-09-02 16:55_

Maybe clearer as:

```rust
// If a human-readable name is provided, convert it back to a structured code.
// For example, given `unused-import`, convert to `F401`.
let s = if let Ok(rule) = Rule::from_name(&s) {
    Cow::Owned(rule.noqa_code().to_string())
} else {
    Cow::Borrowed(s)
};
```

---

_Review comment by @charliermarsh on `crates/ruff/src/rule_selector.rs`:62 on 2023-09-02 16:55_

We can probably improve the ergonomics of the rule APIs to reduce the number of lookups happening here, but it wouldn't need to be a requirement of this PR.

---

_Review comment by @charliermarsh on `crates/ruff/src/registry.rs`:29 on 2023-09-02 17:00_

I think there are some usages in `crates/ruff/src/checkers/noqa.rs` that need to be adjusted too (look for `if let Ok(rule) = Rule::from_code(code) {` and `let code = get_redirect_target(code).unwrap_or(code)`). In short, we need to audit all the usages of `noqa_code()`, in that file too.

---

_Review comment by @charliermarsh on `crates/ruff/src/noqa.rs`:179 on 2023-09-02 17:03_

These could even be separate methods, and then the caller would do `Self::lex_code(...).or_else(|| Self::lex_name(...))`.

---

_@charliermarsh reviewed on 2023-09-02 17:03_

---

_@charliermarsh reviewed on 2023-09-02 17:06_

---

_Review comment by @charliermarsh on `crates/ruff/src/registry.rs`:29 on 2023-09-02 17:06_

It may end up being useful to have separate wrapper types to make this all clearer (I haven't tried this myself but it's worth exploring), like:

```
pub struct RuleCodeId(String);
pub struct RuleNameId(String);
```

Which would ensure that we only compare `RuleCodeId` to `NoqaCode`, etc.

(In `crates/ruff/src/checkers/noqa.rs`, we often pass around the raw codes as `&str` because we don't yet _know_ if they're valid rule codes, and so we can't pass them around as `NoqaCode`.)


---

_@hotpxl reviewed on 2023-09-04 01:44_

---

_Review comment by @hotpxl on `crates/ruff/src/registry.rs`:29 on 2023-09-04 01:44_

I refactored the logic in `noqa.rs` a bit so we can pass around `Rule`s, which removes the ambiguity between a code and a name. Please let me know how it looks.

---

_@hotpxl reviewed on 2023-09-04 01:54_

---

_Review comment by @hotpxl on `crates/ruff/src/noqa.rs`:179 on 2023-09-04 01:54_

done

---

_@hotpxl reviewed on 2023-09-04 01:54_

---

_Review comment by @hotpxl on `crates/ruff/src/noqa.rs`:204 on 2023-09-04 01:54_

done

---

_@hotpxl reviewed on 2023-09-04 01:54_

---

_Review comment by @hotpxl on `crates/ruff/src/rule_selector.rs`:62 on 2023-09-04 01:54_

Done. I don't quite understand how `RuleCodePrefix` works yet since I don't understand macros :/ Otherwise I may be able to do something clever

---

_Review requested from @charliermarsh by @hotpxl on 2023-09-06 18:11_

---

_Comment by @hotpxl on 2023-09-22 19:10_

Is there any appetite in merging this soon, if so I'll rebase and resolve the merge conflicts.

---

_Comment by @LukeMarlin on 2024-01-18 09:32_

It would be great if this was merged, having names is way nicer when reading/reviewing code :)
We are starting to replace black/isort/pylint with ruff, and having to revert to codes is a step backward.

I'd gladly test it on our codebases if that can help

---

_Comment by @hotpxl on 2024-01-18 16:13_

@charliermarsh hi there! do you have any comments on the general direction?

---

_Assigned to @charliermarsh by @zanieb on 2024-03-12 17:26_

---

_Comment by @zanieb on 2024-03-12 17:27_

Hi sorry this has been left hanging. We've had a busy couple of months.

@charliermarsh could you take a look at this again or assign someone else?

---

_Comment by @william-almy-skydio on 2024-03-19 22:18_

Bump? 👀

Our company has recently migrated our repo to Ruff this past week, and this is the top feature developers have been asking for. Would be neat to get this over the line 🎉 

---

_Comment by @zanieb on 2024-03-20 01:10_

Basically, we will absolute support this in the future but we cannot say when it will happen — there's a lot of moving parts to consider here.

---

_Comment by @MichaReiser on 2024-04-08 07:02_

I close this PR because we're looking into this as part of #1774 and I don't think we want to merge any interim solution. Thank you @hotpxl for pushing this and we hope to ship some improvement on this front "soon". CC: @AlexWaygood it may be interesting to take a look at this PR to understand how human readable rule names were implemented.

---

_Closed by @MichaReiser on 2024-04-08 07:02_

---
