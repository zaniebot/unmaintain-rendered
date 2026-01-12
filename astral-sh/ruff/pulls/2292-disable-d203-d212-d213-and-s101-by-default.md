```yaml
number: 2292
title: Disable D203, D212, D213 and S101 by default
type: pull_request
state: closed
author: not-my-profile
labels: []
assignees: []
draft: true
base: main
head: nursery
created_at: 2023-01-28T06:12:36Z
updated_at: 2023-02-01T15:32:02Z
url: https://github.com/astral-sh/ruff/pull/2292
synced_at: 2026-01-12T04:52:00Z
```

# Disable D203, D212, D213 and S101 by default

---

_Pull request opened by @not-my-profile on 2023-01-28 06:12_

(see our discussion in #2289 as well as the commit messages for the reasoning)

---

_Converted to draft by @not-my-profile on 2023-01-28 06:27_

---

_Marked ready for review by @not-my-profile on 2023-01-28 06:51_

---

_Comment by @JonathanPlasse on 2023-01-28 10:36_

Does specifying `tool.ruff.pydocstyle.convention` still activates the rules `D203`, `D212`, and `D213`?

---

_Comment by @not-my-profile on 2023-01-28 11:47_

Good question. Previously `pydocstyle.convention`  only disabled rules, it never enabled them, so if you didn't also select them via `D` it wouldn't turn them on. I do not want the new `explicitly_enabled` logic to somehow take the presence of a `D` selector into account since I think that would overcomplicate the rule selection behavior ... so I'll change `pydocstyle.convention`  to enable these rules even if `D` isn't selected ... even if that is a bit of a breaking change I think it much better aligns with the probable user intention.

(Marking this as a draft till I implement that ... probably only have time for that in a couple of hours.)

---

_Converted to draft by @not-my-profile on 2023-01-28 11:47_

---

_Comment by @charliermarsh on 2023-01-28 12:20_

`D203` and `D213` aren't part of any of the valid conventions. (`D212` _is_, and shouldn't be part of any sort of automatic disable list IMO.)

---

_@charliermarsh reviewed on 2023-01-28 12:21_

---

_Review comment by @charliermarsh on `README.md`:1227 on 2023-01-28 12:21_

We ignore comparisons to strings by default. At least, that's the intent (it's parameterizable). If so, this message is incorrect, I think?

---

_Review comment by @charliermarsh on `src/settings/nursery.rs`:11 on 2023-01-28 12:30_

I'd like to remove this, `Rule::MagicValueComparison`, and `Rule::MultiLineSummaryFirstLine` from this list for now. We can always add them later. But I don't want to couple the debate here to the removal of specific rules, apart from those that motivated the issue. The goal here is really to solve the infinite iteration loop for new users.

---

_@charliermarsh reviewed on 2023-01-28 12:30_

---

_@charliermarsh reviewed on 2023-01-28 12:31_

---

_Review comment by @charliermarsh on `src/settings/nursery.rs`:8 on 2023-01-28 12:31_

Here's where I'm torn: assume that no user _ever_ enabled this rule. Why support it? Why maintain the code? The tests? Etc.

---

_@not-my-profile reviewed on 2023-01-28 14:22_

---

_Review comment by @not-my-profile on `README.md`:1227 on 2023-01-28 14:22_

Ah ... I hadn't noticed the `allow-magic-value-types` setting you introduced in #1987, which even defaults to `["str"]`. Ok yes this addresses the problem.

---

_Renamed from "Disable D203, D212, D213, PLR2004 and S101 by default" to "Disable D203, D212, D213 and S101 by default" by @not-my-profile on 2023-01-28 14:28_

---

_@not-my-profile reviewed on 2023-01-28 14:29_

---

_Review comment by @not-my-profile on `README.md`:1227 on 2023-01-28 14:29_

I have dropped the default disabling of MagicValueComparison from this PR.

---

_@not-my-profile reviewed on 2023-01-28 14:59_

---

_Review comment by @not-my-profile on `src/settings/nursery.rs`:8 on 2023-01-28 14:59_

I am alright with dropping these rules completely. However you don't want to break configs that currently ignore these rules and since the `RuleCodePrefix` serde impls are currently automatically derived, I think the easiest way to achieve that would be to introduce a no-op rule and map the legacy codes to that no-op violation that simply never gets triggered ... however since we don't yet have the many-to-many mapping we'd need to assign a rule code to the no-op rule ... which feels wrong since it's very much an implementation detail. So I'd rather have us wait with the dropping of these rules until after the many-to-many mapping has been implemented.

And I also think it would be nice to have some deprecation period with ruff still supporting the rules but printing a warning that we are planning on dropping them with a link to a GitHub issue about that, so that in case these rules are in fact used by some users they can voice their opinions about it.

---

_@not-my-profile reviewed on 2023-01-28 15:03_

---

_Review comment by @not-my-profile on `src/settings/nursery.rs`:11 on 2023-01-28 15:03_

> D212 (MultiLineSummaryFirstLine) is [part of a valid convention], and shouldn't be part of any sort of automatic disable list IMO

It is not part of PEP 257, which I'd consider to be the only default convention. I don't think it makes sense to enable convention-specific rules by default just because they are part of some convention ... there are many conventions and unless the user has configured `convention` we don't  know which convention they use. Case in point: pydocstyle also does not enable D212 by default.

---

_@charliermarsh reviewed on 2023-01-28 15:14_

---

_Review comment by @charliermarsh on `src/settings/nursery.rs`:8 on 2023-01-28 15:14_

This is reasonable. I'd like to get these rules out of `--select D` and `--select ALL` as soon as we can. And the approach you've proposed here does achieve that (as long as we preserve retaining `D212` when the appropriate conventions are set, as per the discussion above).

I do think it's strange to put these rules in `nursery` rather than mark them as explicitly deprecated (and use the strategy you've implemented here to handle those deprecations), since we know we want to remove them. My preference would be to leave `S101` and `D212` for now, and use the strategy here for `D203` and `D213`, but mark them as deprecated rather than in nursery. To me, nursery suggests that there's a path to stabilizing these rules, but the intent here really is to discourage and stop supporting them.


---

_Comment by @not-my-profile on 2023-01-28 20:53_

Just an update:

* I'd firstly like to fix the config resolution situation #2312.
* Then I'd like to convert the `pydocstyle.convention` setting into a rule selector ... so you could e.g. `--select pydocstyle:google` and then ignore a specific rule from that rule set with `--ignore`
* And then finally disable these problematic pydocstyle rules by default by implementing the changes in this PR.

---

_@not-my-profile reviewed on 2023-01-28 20:55_

---

_Review comment by @not-my-profile on `src/settings/nursery.rs`:8 on 2023-01-28 20:55_

I am not convinced that keeping these rules around actually is much of a maintenance burden ... once the conflict / endless iteration issue has been fixed by disabling them by default they really shouldn't be such a hassle anymore ... but if you want to remove them that's very much your call.

---

_Comment by @bluetech on 2023-01-28 21:36_

Regarding the `assert` one, clippy deals with this by having a [`clippy::restriction`](https://doc.rust-lang.org/clippy/lints.html#restriction) group that is off by default, and is not meant to be enabled wholesale, but contains lints which can be enabled for specific code bases which want to enforce that some feature is not used (see lint list [here](https://rust-lang.github.io/rust-clippy/stable/index.html)). I can imagine this being useful for some Python projects, e.g. enforce no `assert`, [no `async`/`await`](https://github.com/gevent/gevent), no `:=`, no `breakpoint()`, or even [no exceptions](https://github.com/dry-python/returns).

---

_Comment by @charliermarsh on 2023-01-30 22:54_

@not-my-profile - This came up again recently (#2368). What can we do to prioritize turning those `D` rules off by default?

I'd like to start by _just_ disabling the `D` rules, and we can revisit the other cases later.


---

_Comment by @not-my-profile on 2023-02-01 15:32_

Closing this for now since ba26a60e2a49716da4bd0a01b7faad17b9094216 has addressed the issue that prompted this PR.

The tracking issue for the more flexible rule categorization is #1774.

---

_Closed by @not-my-profile on 2023-02-01 15:32_

---
