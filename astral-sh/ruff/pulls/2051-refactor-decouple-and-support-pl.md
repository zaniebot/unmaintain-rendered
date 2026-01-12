```yaml
number: 2051
title: "Refactor, decouple and support \"PL\""
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: refactor-and-decouple
created_at: 2023-01-21T07:18:06Z
updated_at: 2023-01-22T16:51:39Z
url: https://github.com/astral-sh/ruff/pull/2051
synced_at: 2026-01-12T04:52:00Z
```

# Refactor, decouple and support "PL"

---

_Pull request opened by @not-my-profile on 2023-01-21 07:18_

Except for the `Support prefix "PL" to select all of Pylint` commit and the minor README change this is purely a refactor.

---

_Comment by @not-my-profile on 2023-01-22 04:12_

Rebased since #2078 in the meantime has implemented the `--prefix` argument I had also implemented here.

Also reworded the message of the now 2nd last commit to be more accurate.

---

_Converted to draft by @not-my-profile on 2023-01-22 04:27_

---

_Marked ready for review by @not-my-profile on 2023-01-22 04:50_

---

_Comment by @not-my-profile on 2023-01-22 05:05_

Updated the 2nd last commit to simplify the logic/be more declarative.

After this PR my next plan is to rework the `RuleSelector` implementation to enable the introduction of the flexible many to many mapping between codes and rules, I have in mind.

---

_Review comment by @charliermarsh on `src/settings/configuration.rs`:37 on 2023-01-22 05:15_

Why pull these up, rather than retain the sort? Semantically, what's the grouping here?

---

_Review comment by @charliermarsh on `README.md`:1111 on 2023-01-22 05:16_

I feel like this was changed intentionally at some point, to spell out all of the rules, but maybe that was actually for Pycodestyle, and Pylint ended up this way as a result of that convention?

---

_Review comment by @charliermarsh on `ruff.schema.json`:1558 on 2023-01-22 05:16_

To be clear, does `PLC` still work to select the `PLC` codes? Or just `PL`?

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:324 on 2023-01-22 05:17_

How does the specificity work between `PL` and (e.g.) `PLC`? What would `--select PLC --ignore PL` do?

---

_@charliermarsh reviewed on 2023-01-22 05:17_

---

_@not-my-profile reviewed on 2023-01-22 05:21_

---

_Review comment by @not-my-profile on `src/settings/configuration.rs`:37 on 2023-01-22 05:21_

These fields control which rules are enabled ... the others don't. Except `per_file_ignores` all of these fields become a `RuleTable` in the `Settings` struct.

---

_@not-my-profile reviewed on 2023-01-22 05:23_

---

_Review comment by @not-my-profile on `README.md`:1111 on 2023-01-22 05:23_

I don't know the history behind this  line. But since 9dc66b5a6546926aff0d04a0280401f939f3a3d8 we have subheadings for these codes anyway, so I think listing all of them here is a bit redundant ... besides this change servers to document the new "PL" selector.

---

_Review comment by @not-my-profile on `ruff.schema.json`:1558 on 2023-01-22 05:25_

Yes PLC, PLE, PLR, PLW all still work as previously ... it's just that PL now also works ... i.e. the change is backwards compatible.

---

_@not-my-profile reviewed on 2023-01-22 05:25_

---

_@not-my-profile reviewed on 2023-01-22 05:48_

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:324 on 2023-01-22 05:48_

Very good question! ... thanks that was indeed broken ... I just force pushed a fix that makes this work as expected (the more specific selector takes precedence).

Please note that all the special logic that was necessary to implement the `PL` code will be removed in my followup PR that changes how `RuleSelector` works (which is also why I didn't implement special test cases for PL since these will very shortly be redundant since PL will be just a prefix like any other prefix that's configured via the `#[prefix]` macro attribute).

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:324 on 2023-01-22 05:57_

Is that alright?

---

_@not-my-profile reviewed on 2023-01-22 05:57_

---

_@charliermarsh reviewed on 2023-01-22 06:00_

---

_Review comment by @charliermarsh on `src/settings/configuration.rs`:37 on 2023-01-22 06:00_

Sounds good. It might be nice to have a comment like `// Rule enablement` or similar, and something else for the next section, even just to show that the distinction is intentional and not overlooked formatting.

---

_@charliermarsh reviewed on 2023-01-22 06:01_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:324 on 2023-01-22 06:01_

Ah, ok, that's reasonable. I see the special-casing now. It effectively treats the `R` in `PLR0` as a numerical piece, IIUC.

---

_@not-my-profile reviewed on 2023-01-22 06:09_

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:324 on 2023-01-22 06:09_

Yes exactly that's the fix I just implemented.

And my follow up PR will change the rule selector parsing to firstly parse the configured `#[prefix]`es so `PLR0` will be split to `PL` (= `Pylint`), `R0`  ... which will let us remove that special casing :)

---

_Merged by @charliermarsh on 2023-01-22 16:51_

---

_Closed by @charliermarsh on 2023-01-22 16:51_

---

_Comment by @charliermarsh on 2023-01-22 16:51_

Thanks, sorry for not merging last night, I had to sleep :joy:

---
