```yaml
number: 3229
title: "[pydocstyle]: Implement `ignore-decorators`"
type: pull_request
state: merged
author: edgarrmondragon
labels:
  - configuration
assignees: []
merged: true
base: main
head: feat/pydocstyle-ignore-decorators
created_at: 2023-02-25T23:35:14Z
updated_at: 2023-02-26T23:02:33Z
url: https://github.com/astral-sh/ruff/pull/3229
synced_at: 2026-01-12T15:55:12Z
```

# [pydocstyle]: Implement `ignore-decorators`

---

_@edgarrmondragon_

Related to https://github.com/charliermarsh/ruff/issues/2205

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pydocstyle/helpers.rs`:85 on 2023-02-26 00:28_

Why check twice? Can we not just use the second version (going through `resolve_call_path`)?

---

_@charliermarsh reviewed on 2023-02-26 00:28_

---

_@edgarrmondragon reviewed on 2023-02-26 00:54_

---

_Review comment by @edgarrmondragon on `crates/ruff/src/rules/pydocstyle/helpers.rs`:85 on 2023-02-26 00:54_

That's not catching the `ignored_decorator` here:

https://github.com/charliermarsh/ruff/blob/cd8be8c0becbbcb5397d65b957c4b1821a600382/crates/ruff/resources/test/fixtures/pydocstyle/D.py#L409-L422

I guess because `BindingKind::FunctionDefinition` is mapped to `None` here:

https://github.com/charliermarsh/ruff/blob/d5abcda35ae58cedac2da2536bbecff7af40f824/crates/ruff/src/checkers/ast.rs#L248-L285

I can try fixing that if it makes sense.

---

_@charliermarsh reviewed on 2023-02-26 03:28_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pydocstyle/helpers.rs`:85 on 2023-02-26 03:28_

Ohh interesting. I'd actually vote _not_ to support the former, at least for now... It seems rare that you'd define a decorator that you'd want to ignore in this way within the same file that it's used, and it's consistent with the rest of these configuration options that they rely on import tracking. If we want to support inter-file definitions like this, we probably want to support them across-the-board, rather than for this one option. Does that make sense?

---

_@edgarrmondragon reviewed on 2023-02-26 05:20_

---

_Review comment by @edgarrmondragon on `crates/ruff/src/rules/pydocstyle/helpers.rs`:85 on 2023-02-26 05:20_

Yeah, that makes sense. I guess users could still ignore their custom decorator in other modules where it's imported.

---

_Review comment by @edgarrmondragon on `crates/ruff/src/rules/pydocstyle/helpers.rs`:83 on 2023-02-26 05:41_

Do you think it's worth returning early if `ignore_decorators` is empty?

EDIT: Probably doesn't hurt 5b719a242a8a24e970d25f667c65a1a4aa2fc317

---

_@edgarrmondragon reviewed on 2023-02-26 05:41_

---

_Label `configuration` added by @charliermarsh on 2023-02-26 21:37_

---

_Merged by @charliermarsh on 2023-02-26 21:40_

---

_Closed by @charliermarsh on 2023-02-26 21:40_

---

_Branch deleted on 2023-02-26 23:02_

---
