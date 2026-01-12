```yaml
number: 2753
title: Update Rust crate chrono to v0.4.37
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/chrono-0.x-lockfile
created_at: 2024-04-01T03:55:16Z
updated_at: 2024-04-01T07:27:12Z
url: https://github.com/astral-sh/uv/pull/2753
synced_at: 2026-01-12T16:05:12Z
```

# Update Rust crate chrono to v0.4.37

---

_@renovate_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [chrono](https://togithub.com/chronotope/chrono) | workspace.dependencies | patch | `0.4.35` -> `0.4.37` |

---

### Release Notes

<details>
<summary>chronotope/chrono (chrono)</summary>

### [`v0.4.37`](https://togithub.com/chronotope/chrono/releases/tag/v0.4.37)

[Compare Source](https://togithub.com/chronotope/chrono/compare/v0.4.36...v0.4.37)

Version 0.4.36 introduced an unexpected breaking change and was yanked. In it `LocalResult` was renamed to `MappedLocalTime` to avoid the impression that it is a `Result` type were some of the results are errors. For backwards compatibility a type alias with the old name was added.

As it turns out there is one case where a type alias behaves differently from the regular enum: you can't import enum variants from a type alias with `use chrono::LocalResult::*`. With 0.4.37 we make the new name `MappedLocalTime` the alias, but keep using it in function signatures and the documentation as much as possible.

See also the release notes of [chrono 0.4.36](https://togithub.com/chronotope/chrono/releases/tag/v0.4.36) from yesterday for the yanked release.

### [`v0.4.36`](https://togithub.com/chronotope/chrono/releases/tag/v0.4.36)

[Compare Source](https://togithub.com/chronotope/chrono/compare/v0.4.35...v0.4.36)

This release un-deprecates the methods on `TimeDelta` that were deprecated with the 0.4.35 release because of the churn they are causing for the ecosystem.

New is the `DateTime::with_time()` method. As an example of when it is useful:

```rust
use chrono::{Local, NaiveTime};
// Today at 12:00:00
let today_noon = Local::now().with_time(NaiveTime::from_hms_opt(12, 0, 0).unwrap());
```

### Additions

-   Add `DateTime::with_time()` ([#&#8203;1510](https://togithub.com/chronotope/chrono/issues/1510))

### Deprecations

-   Revert `TimeDelta` deprecations ([#&#8203;1543](https://togithub.com/chronotope/chrono/issues/1543))
-   Deprecate `TimeStamp::timestamp_subsec_nanos`, which was missed in the 0.4.35 release ([#&#8203;1486](https://togithub.com/chronotope/chrono/issues/1486))

### Documentation

-   Correct version number of deprecation notices ([#&#8203;1486](https://togithub.com/chronotope/chrono/issues/1486))
-   Fix some typos ([#&#8203;1505](https://togithub.com/chronotope/chrono/issues/1505))
-   Slightly improve serde documentation ([#&#8203;1519](https://togithub.com/chronotope/chrono/issues/1519))
-   Main documentation: simplify links and reflow text ([#&#8203;1535](https://togithub.com/chronotope/chrono/issues/1535))

### Internal

-   CI: Lint benchmarks ([#&#8203;1489](https://togithub.com/chronotope/chrono/issues/1489))
-   Remove unnessary `Copy` and `Send` impls ([#&#8203;1492](https://togithub.com/chronotope/chrono/issues/1492), thanks [@&#8203;erickt](https://togithub.com/erickt))
-   Backport streamlined `NaiveDate` unit tests ([#&#8203;1500](https://togithub.com/chronotope/chrono/issues/1500), thanks [@&#8203;Zomtir](https://togithub.com/Zomtir))
-   Rename `LocalResult` to `TzResolution`, add alias ([#&#8203;1501](https://togithub.com/chronotope/chrono/issues/1501))
-   Update windows-bindgen to 0.55 ([#&#8203;1504](https://togithub.com/chronotope/chrono/issues/1504))
-   Avoid duplicate imports, which generate warnings on nightly ([#&#8203;1507](https://togithub.com/chronotope/chrono/issues/1507))
-   Add extra debug assertions to `NaiveDate::from_yof` ([#&#8203;1518](https://togithub.com/chronotope/chrono/issues/1518))
-   Some small simplifications to `DateTime::date_naive` and `NaiveDate::diff_months` ([#&#8203;1530](https://togithub.com/chronotope/chrono/issues/1530))
-   Remove `unwrap` in Unix `Local` type ([#&#8203;1533](https://togithub.com/chronotope/chrono/issues/1533))
-   Use different method to ignore feature-dependent doctests ([#&#8203;1534](https://togithub.com/chronotope/chrono/issues/1534))

Thanks to all contributors on behalf of the chrono team, [@&#8203;djc](https://togithub.com/djc) and [@&#8203;pitdicker](https://togithub.com/pitdicker)!

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR has been generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View repository job log [here](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzNy4yNjkuMiIsInVwZGF0ZWRJblZlciI6IjM3LjI2OS4yIiwidGFyZ2V0QnJhbmNoIjoibWFpbiJ9-->


---

_Label `internal` added by @renovate[bot] on 2024-04-01 03:55_

---

_Merged by @AlexWaygood on 2024-04-01 07:27_

---

_Closed by @AlexWaygood on 2024-04-01 07:27_

---

_Branch deleted on 2024-04-01 07:27_

---
