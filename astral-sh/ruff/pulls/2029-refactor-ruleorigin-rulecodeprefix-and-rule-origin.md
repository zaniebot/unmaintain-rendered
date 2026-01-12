```yaml
number: 2029
title: "refactor: RuleOrigin, RuleCodePrefix and Rule::origin"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: rename-ruleorigin-to-linter
created_at: 2023-01-20T13:41:54Z
updated_at: 2023-01-21T01:25:57Z
url: https://github.com/astral-sh/ruff/pull/2029
synced_at: 2026-01-12T15:55:07Z
```

# refactor: RuleOrigin, RuleCodePrefix and Rule::origin

---

_@not-my-profile_

I am about to implement the many to many mapping between codes and rules and would like to rename these two enums in advance because the names don't really make much sense anymore.

As a followup I will drop the all the partial prefixes from `RuleCodePrefix`, see https://github.com/charliermarsh/ruff/issues/1773#issuecomment-1378203413, i.e. you will only be able to select/ignore: one specific rule (e.g. `F401`, all rules from a linter (e.g. `F`) or all rules (`ALL`). While this is not strictly necessary to implement the many to many mapping (or human friendly linter names) it does enable the many to many mapping implementation to be prefix agnostic which I think is a really nice feature since ruff prefixes currently often diverge from the upstream prefixes and by having a prefix-agnostic mapping we can support both the existing ruff rule codes as well as the large legacy code bases that want to switch to ruff without having to rewrite all of their `noqa` directives.

---

_Renamed from "refactor: Rename RuleOrigin to Linter" to "refactor: Rename RuleOrigin and RuleCodePrefix" by @not-my-profile on 2023-01-20 13:51_

---

_Comment by @charliermarsh on 2023-01-20 14:24_

On the follow-up: I can write a longer comment when I’m back at my computer, but we have to be careful with how we handle that, as people do rely on that behavior today.

---

_Comment by @not-my-profile on 2023-01-20 14:34_

I think I can come up with a clever workaround to still support such partial prefix matches while still avoiding the disadvantageous hardcoding of prefixes ... this way we can stay backwards compatible for how long we want regarding such prefixes.

---

_Comment by @charliermarsh on 2023-01-20 14:40_

Yeah I just don’t want to break compatibility without a clear and thoughtful strategy. Sounds like we’re on the same page. (An edition or versioned configuration would help with this, in theory? Though it’ll also come automatically if users move to the kebab-case rule names.)

I’ll do a brief compilation of some of the common cases here via GitHub Code Search. For what it’s worth, I think the partial prefix match is mostly used for disabling subsets of docstring rules.

---

_Comment by @not-my-profile on 2023-01-20 14:47_

I think selectable kebab-case names are still some ways away since as soon as we have them every rule name change will become a breaking change, so I think before we can surface our rule names to the users we have to 1) establish some very thought out naming conventions and 2) rigorously rename all our rules to match that convention ... that will take some time.

---

_Converted to draft by @not-my-profile on 2023-01-20 18:00_

---

_Marked ready for review by @not-my-profile on 2023-01-20 18:28_

---

_Renamed from "refactor: Rename RuleOrigin and RuleCodePrefix" to "refactor: RuleOrigin, RuleCodePrefix and Rule::origin" by @not-my-profile on 2023-01-20 19:38_

---

_Comment by @charliermarsh on 2023-01-20 23:31_

Just read through this -- looks reasonable to me. (But now needs a rebase. Will merge whenever it's updated.)

---

_Comment by @not-my-profile on 2023-01-21 01:22_

Rebased :)

---

_Merged by @charliermarsh on 2023-01-21 01:25_

---

_Closed by @charliermarsh on 2023-01-21 01:25_

---
