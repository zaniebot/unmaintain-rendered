```yaml
number: 15001
title: Update Rust crate chrono to v0.4.39
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/chrono-0.x-lockfile
created_at: 2024-12-16T01:14:32Z
updated_at: 2024-12-16T01:28:30Z
url: https://github.com/astral-sh/ruff/pull/15001
synced_at: 2026-01-10T20:42:27Z
```

# Update Rust crate chrono to v0.4.39

---

_Pull request opened by @renovate on 2024-12-16 01:14_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [chrono](https://redirect.github.com/chronotope/chrono) | workspace.dependencies | patch | `0.4.38` -> `0.4.39` |

---

### Release Notes

<details>
<summary>chronotope/chrono (chrono)</summary>

### [`v0.4.39`](https://redirect.github.com/chronotope/chrono/releases/tag/v0.4.39): 0.4.39

[Compare Source](https://redirect.github.com/chronotope/chrono/compare/v0.4.38...v0.4.39)

#### What's Changed

-   [#&#8203;1577](https://redirect.github.com/chronotope/chrono/issues/1577): Changed years_since documentation to match its implementation by [@&#8203;Taxalo](https://redirect.github.com/Taxalo) in [https://github.com/chronotope/chrono/pull/1578](https://redirect.github.com/chronotope/chrono/pull/1578)
-   Remove obsolete weird feature guard by [@&#8203;djc](https://redirect.github.com/djc) in [https://github.com/chronotope/chrono/pull/1582](https://redirect.github.com/chronotope/chrono/pull/1582)
-   Fix format::strftime docs link by [@&#8203;frederikhors](https://redirect.github.com/frederikhors) in [https://github.com/chronotope/chrono/pull/1581](https://redirect.github.com/chronotope/chrono/pull/1581)
-   Fix micros (optional) limit in and_hms_micro_opt by [@&#8203;qrilka](https://redirect.github.com/qrilka) in [https://github.com/chronotope/chrono/pull/1584](https://redirect.github.com/chronotope/chrono/pull/1584)
-   Update windows-bindgen requirement from 0.56 to 0.57 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/chronotope/chrono/pull/1589](https://redirect.github.com/chronotope/chrono/pull/1589)
-   native/date: Improve DelayedFormat doc re Panics by [@&#8203;behnam-oneschema](https://redirect.github.com/behnam-oneschema) in [https://github.com/chronotope/chrono/pull/1590](https://redirect.github.com/chronotope/chrono/pull/1590)
-   Fix typo in rustdoc of `from_timestamp_nanos()` by [@&#8203;sgoll](https://redirect.github.com/sgoll) in [https://github.com/chronotope/chrono/pull/1591](https://redirect.github.com/chronotope/chrono/pull/1591)
-   Update windows-bindgen requirement from 0.57 to 0.58 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/chronotope/chrono/pull/1594](https://redirect.github.com/chronotope/chrono/pull/1594)
-   docs: document century cutoff for %y by [@&#8203;MarcoGorelli](https://redirect.github.com/MarcoGorelli) in [https://github.com/chronotope/chrono/pull/1598](https://redirect.github.com/chronotope/chrono/pull/1598)
-   Checked `NaiveWeek` methods by [@&#8203;bragov4ik](https://redirect.github.com/bragov4ik) in [https://github.com/chronotope/chrono/pull/1600](https://redirect.github.com/chronotope/chrono/pull/1600)
-   Impl serde::Serialize and serde::Deserialize for TimeDelta by [@&#8203;Awpteamoose](https://redirect.github.com/Awpteamoose) in [https://github.com/chronotope/chrono/pull/1599](https://redirect.github.com/chronotope/chrono/pull/1599)
-   Derive `PartialEq`,`Eq`,`Hash`,`Copy` and `Clone` on `NaiveWeek` by [@&#8203;DSeeLP](https://redirect.github.com/DSeeLP) in [https://github.com/chronotope/chrono/pull/1618](https://redirect.github.com/chronotope/chrono/pull/1618)
-   Support ohos tzdata since ver.oh35 by [@&#8203;MirageLyu](https://redirect.github.com/MirageLyu) in [https://github.com/chronotope/chrono/pull/1613](https://redirect.github.com/chronotope/chrono/pull/1613)
-   Use Formatter::pad (instead of write_str) for Weekdays by [@&#8203;horazont](https://redirect.github.com/horazont) in [https://github.com/chronotope/chrono/pull/1621](https://redirect.github.com/chronotope/chrono/pull/1621)
-   Fix typos by [@&#8203;szepeviktor](https://redirect.github.com/szepeviktor) in [https://github.com/chronotope/chrono/pull/1623](https://redirect.github.com/chronotope/chrono/pull/1623)
-   Fix comment. by [@&#8203;khuey](https://redirect.github.com/khuey) in [https://github.com/chronotope/chrono/pull/1624](https://redirect.github.com/chronotope/chrono/pull/1624)
-   chore: add `#[inline]` to `num_days` by [@&#8203;CommanderStorm](https://redirect.github.com/CommanderStorm) in [https://github.com/chronotope/chrono/pull/1627](https://redirect.github.com/chronotope/chrono/pull/1627)
-   fix typo by [@&#8203;futreall](https://redirect.github.com/futreall) in [https://github.com/chronotope/chrono/pull/1633](https://redirect.github.com/chronotope/chrono/pull/1633)
-   Update mod.rs by [@&#8203;donatik27](https://redirect.github.com/donatik27) in [https://github.com/chronotope/chrono/pull/1638](https://redirect.github.com/chronotope/chrono/pull/1638)

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

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS41OC4xIiwidXBkYXRlZEluVmVyIjoiMzkuNTguMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2024-12-16 01:14_

---

_Merged by @charliermarsh on 2024-12-16 01:25_

---

_Closed by @charliermarsh on 2024-12-16 01:25_

---

_Branch deleted on 2024-12-16 01:25_

---

_Comment by @github-actions[bot] on 2024-12-16 01:28_

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
