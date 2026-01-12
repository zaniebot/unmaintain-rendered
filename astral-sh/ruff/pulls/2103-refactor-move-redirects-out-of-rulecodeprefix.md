```yaml
number: 2103
title: "refactor: Move redirects out of RuleCodePrefix "
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: refactor-redirects
created_at: 2023-01-23T08:46:01Z
updated_at: 2023-01-24T14:26:19Z
url: https://github.com/astral-sh/ruff/pull/2103
synced_at: 2026-01-12T04:52:00Z
```

# refactor: Move redirects out of RuleCodePrefix 

---

_Pull request opened by @not-my-profile on 2023-01-23 08:46_

(See the 2nd commit message for an explanation & the reasoning.)

---

_@charliermarsh reviewed on 2023-01-23 17:55_

---

_Review comment by @charliermarsh on `src/rule_redirects.rs`:22 on 2023-01-23 17:55_

Does this correctly trigger once, _per rule_? Or is this going to trigger once, ever?

---

_@not-my-profile reviewed on 2023-01-23 18:29_

---

_Review comment by @not-my-profile on `src/rule_redirects.rs`:22 on 2023-01-23 18:29_

Hahaha oh yeah ... this indeed didn't work as intended.

I just changed it to just use `warn_user!` since I cannot think of an easy alternative to preserve the limitation that it only prints once since a `FromStr` impl cannot easily keep state.

I think we'd have to move the warning call to the CLI crate and `flake8_to_ruff/parser.rs` because these can keep state of which warnings have already been reported ... but I am still uncertain how the integration with `clap` (w|c)ould work ... if you think that should be done I can take a look tomorrow.

---

_@charliermarsh reviewed on 2023-01-23 19:15_

---

_Review comment by @charliermarsh on `src/rule_redirects.rs`:22 on 2023-01-23 19:15_

Hmm... It'd be nice to preserve, although it's not the end of the world. It looks like it appears once per inclusion, and never when parsing `noqa` redirects (haven't looked at the code to understand how / why the latter behavior works -- does that make sense to you?).

Given `foo.py`:

```py
import pandas as x # noqa: IC001
```

I get:

```
❯ cargo run foo.py --select IC001 --fixable IC001
    Finished dev [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/ruff foo.py --select IC001 --fixable IC001`
warning: `IC001` has been remapped to `ICN001`
warning: `IC001` has been remapped to `ICN001`
warning: debug build without --no-cache.
```

---

_Review comment by @charliermarsh on `src/rule_redirects.rs`:22 on 2023-01-23 19:17_

Oh, right, ok, it's because the `noqa` redirect goes through the `REDIRECTS` map directly.

---

_@charliermarsh reviewed on 2023-01-23 19:17_

---

_Converted to draft by @not-my-profile on 2023-01-24 04:48_

---

_@not-my-profile reviewed on 2023-01-24 12:18_

---

_Review comment by @not-my-profile on `src/rule_redirects.rs`:22 on 2023-01-24 12:18_

ruff didn't warn about redirects in `noqa` comments previously ... my PR doesn't change that.

Anyway I have updated the PR to only warn about redirects once when they occur multiple times (in the CLI or `pyproject.toml` files) :)

---

_Marked ready for review by @not-my-profile on 2023-01-24 12:19_

---

_Converted to draft by @not-my-profile on 2023-01-24 12:23_

---

_Comment by @not-my-profile on 2023-01-24 12:27_

I split the first 7 commits into their own PR (#2122) to make them easier to review ... once that has been merged, I'll rebase this PR.

---

_Renamed from "refactor: Move redirects out of RuleSelector " to "refactor: Move redirects out of RuleCodePrefix " by @not-my-profile on 2023-01-24 12:31_

---

_Marked ready for review by @not-my-profile on 2023-01-24 12:39_

---

_Review requested from @charliermarsh by @not-my-profile on 2023-01-24 12:50_

---

_Comment by @not-my-profile on 2023-01-24 12:54_

(Note that I have realized that code redirects should be supported everywhere `RuleSelector` is used and have hence refactored `RuleSelector` accordingly as opposed to introducing a `MaybeRedirectedRuleSelector` wrapper type that I initially proposed, since it would have to be used everywhere.)

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:317 on 2023-01-24 13:20_

Can we use `FxHashMap` for this, just for consistency?

---

_@charliermarsh reviewed on 2023-01-24 13:20_

---

_@charliermarsh reviewed on 2023-01-24 13:23_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:361 on 2023-01-24 13:23_

This will still warn twice if you specify an invalid code in both `select` and `fixable`, right? I'm ok with that, but I want to make sure I understand what's going on. (We could lift the redirect map up a level, or something, but seems annoying.)

---

_@not-my-profile reviewed on 2023-01-24 14:09_

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:317 on 2023-01-24 14:09_

Sure ... changed to `FxHashMap`.

---

_@not-my-profile reviewed on 2023-01-24 14:16_

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:361 on 2023-01-24 14:16_

Yes indeed ... good observation.

I honestly think that reporting config issues this way is generally suboptimal because ruff configuration can come from many sources, so it could be hard to track down where exactly a deprecated redirect is coming from. I think we should rather report exactly where a redirect is used, i.e. in which config file and key (e.g. `tool.ruff.select`) or in which CLI parameter. However implementing that would take some effort and probably only really be worth it if we had more lints for our configuration (#1472 seems related).

So yes the current reporting isn't optimal ... but neither would be moving the redirect map up a level as far as I'm concerned ... so I'd also be ok with keeping this for now and cycle back to it once we have addressed the more pressing matters (like the many to many mapping, documentation, human-friendly rule names etc.).

---

_Merged by @charliermarsh on 2023-01-24 14:26_

---

_Closed by @charliermarsh on 2023-01-24 14:26_

---
