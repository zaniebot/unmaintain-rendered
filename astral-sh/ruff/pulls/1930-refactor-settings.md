```yaml
number: 1930
title: Refactor settings
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: refactor-settings
created_at: 2023-01-17T09:48:58Z
updated_at: 2023-01-17T14:20:58Z
url: https://github.com/astral-sh/ruff/pull/1930
synced_at: 2026-01-12T15:55:07Z
```

# Refactor settings

---

_@not-my-profile_

_No description provided._

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:83 on 2023-01-17 12:31_

Is this an intentional newline?

---

_@charliermarsh reviewed on 2023-01-17 12:31_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:164 on 2023-01-17 12:32_

Can we move this into its own utility function, to keep the constructor declarative?

---

_@charliermarsh reviewed on 2023-01-17 12:32_

---

_@charliermarsh reviewed on 2023-01-17 12:32_

---

_Review comment by @charliermarsh on `src/settings/rule_table.rs`:26 on 2023-01-17 12:32_

Do you still plan on changing this to use macro-based lookups or no longer viable?

---

_@charliermarsh reviewed on 2023-01-17 12:33_

---

_Review comment by @charliermarsh on `src/settings/rule_table.rs`:36 on 2023-01-17 12:33_

Could we just implement `iter` here?

---

_@not-my-profile reviewed on 2023-01-17 12:50_

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:83 on 2023-01-17 12:50_

Yes I inserted it as a conceptual break between settings related to rule selection and other core settings (which are different from resolver or rule-specific settings).

---

_@not-my-profile reviewed on 2023-01-17 12:55_

---

_Review comment by @not-my-profile on `src/settings/rule_table.rs`:26 on 2023-01-17 12:55_

No I don't think macro-based lookups make sense (especially considering that they don't give any substantial performance gain). I think we should strive to decouple the ruff core from our linter implementations and requiring one boolean field in a struct for every rule is very much the opposite of decoupling.

---

_Review comment by @not-my-profile on `src/settings/rule_table.rs`:36 on 2023-01-17 12:57_

Yes we could but I think it would be less explicit ... iterate over what? All rules? All enabled rules? All rules that have autofix enabled? It's not unthinkable that we at some point end up having a need to also iterate over rules that have autofix enabled.

---

_@not-my-profile reviewed on 2023-01-17 12:57_

---

_@charliermarsh reviewed on 2023-01-17 13:01_

---

_Review comment by @charliermarsh on `src/settings/rule_table.rs`:36 on 2023-01-17 13:01_

Eh, I think it's reasonable to assume that iterating over a `RuleTable` iterates over the enabled or active rules. But, I'm not gonna push it.

---

_@not-my-profile reviewed on 2023-01-17 13:02_

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:164 on 2023-01-17 13:02_

We could however I don't really see the point since 1) that function won't be used anywhere else and 2) the function signature would have to be quite complex since it would need to take `config.fixable`, `config.unfixable`, `config.select` and `config.ignore` (we cannot just pass `&Configuration` because we want to avoid needless clones() and so we'd need to have 4 owned parameters for the borrow checker).

---

_@charliermarsh reviewed on 2023-01-17 13:03_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:164 on 2023-01-17 13:03_

I think the point would be to increase the readability of this constructor :)

---

_@not-my-profile reviewed on 2023-01-17 13:05_

---

_Review comment by @not-my-profile on `src/settings/rule_table.rs`:36 on 2023-01-17 13:05_

I agree that this is a reasonable assumption however I think `IntoIterator` implementations ideally don't require any assumptions.

---

_@charliermarsh reviewed on 2023-01-17 13:05_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:211 on 2023-01-17 13:05_

Great.

---

_@charliermarsh reviewed on 2023-01-17 13:08_

---

_Review comment by @charliermarsh on `src/settings/rule_table.rs`:36 on 2023-01-17 13:08_

Fair enough!

---

_@not-my-profile reviewed on 2023-01-17 13:15_

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:164 on 2023-01-17 13:15_

Fair enough, done :)

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:164 on 2023-01-17 13:37_

Thank you, will merge shortly :)

---

_@charliermarsh reviewed on 2023-01-17 13:37_

---

_Merged by @charliermarsh on 2023-01-17 14:20_

---

_Closed by @charliermarsh on 2023-01-17 14:20_

---
