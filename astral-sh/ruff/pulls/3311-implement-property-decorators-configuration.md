```yaml
number: 3311
title: "Implement `property-decorators` configuration option for pydocstyle"
type: pull_request
state: merged
author: staticssleever668
labels:
  - configuration
assignees: []
merged: true
base: main
head: feat_pydocstyle_property_decorator
created_at: 2023-03-02T20:48:40Z
updated_at: 2023-03-02T22:00:21Z
url: https://github.com/astral-sh/ruff/pull/3311
synced_at: 2026-01-12T04:39:44Z
```

# Implement `property-decorators` configuration option for pydocstyle

---

_Pull request opened by @staticssleever668 on 2023-03-02 20:48_

Implements the second part of #2205.
(I did not run pre-commit locally as it's installing the environment indefinitely)

---

_@staticssleever668 reviewed on 2023-03-02 20:49_

---

_Review comment by @staticssleever668 on `crates/ruff/resources/test/fixtures/pydocstyle/D401.py`:6 on 2023-03-02 20:49_

Is such third-party import in tests okay?

---

_@charliermarsh reviewed on 2023-03-02 20:50_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/pydocstyle/D401.py`:6 on 2023-03-02 20:50_

Yeah totally fine. We don't actually run the code, only analyze it.

---

_Comment by @charliermarsh on 2023-03-02 20:50_

> (I did not run pre-commit locally as it's installing the environment indefinitely)

That's fine, only passing CI is strictly required.

---

_Review comment by @staticssleever668 on `crates/ruff/src/rules/pydocstyle/helpers.rs`:122 on 2023-03-02 20:51_

A portion of ruff::ast::function_type::classify is basically the same snippet. Would it make sense to make this a helper somewhere higher than in ruff::rules::pydocstyle::helpers?

---

_@staticssleever668 reviewed on 2023-03-02 20:51_

---

_@staticssleever668 reviewed on 2023-03-02 20:52_

---

_Review comment by @staticssleever668 on `crates/ruff/src/rules/pydocstyle/helpers.rs`:122 on 2023-03-02 20:52_

(also, function name and description look bad to me, but I can't name them better - language barrier -, any suggestions on improving them?)

---

_Review comment by @staticssleever668 on `crates/ruff/src/rules/pydocstyle/settings.rs`:110 on 2023-03-02 20:56_

I think this behavior makes much more sense, but I can make it match pydocstyle. What do you think?

---

_@staticssleever668 reviewed on 2023-03-02 20:56_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pydocstyle/helpers.rs`:122 on 2023-03-02 20:56_

Just looking at some of the existing conventions... one sec...

---

_@charliermarsh reviewed on 2023-03-02 20:56_

---

_@charliermarsh reviewed on 2023-03-02 20:57_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pydocstyle/settings.rs`:110 on 2023-03-02 20:57_

I prefer what you have here.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pydocstyle/settings.rs`:110 on 2023-03-02 20:58_

Oh, although I guess `staticmethod_decorators` and `classmethod_decorators` don't work that way. I feel like they _should_ though. Fine to leave it for now, maybe we change those other settings semantics in a follow-up.

---

_@charliermarsh reviewed on 2023-03-02 20:58_

---

_@charliermarsh reviewed on 2023-03-02 21:02_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pydocstyle/rules/non_imperative_mood.rs`:34 on 2023-03-02 21:02_

I think instead, maybe we should just pass the extra `property_decorators` to `is_property`. It's not super consistent with the other utilities in that file, which don't accept "extra" decorators, but it does mean one centralized function for determining "decorator status", _and_ it's more efficient since we only do the `checker.resolve_call_path` once per decorator.

---

_@charliermarsh reviewed on 2023-03-02 21:42_

---

_Review comment by @charliermarsh on `crates/ruff/src/visibility.rs`:86 on 2023-03-02 21:42_

It's preferable to just take `&[CallPath]` here, since `extra_properties.iter().any(...)` should return `false` if it's empty anyway.

---

_Review comment by @staticssleever668 on `crates/ruff/src/rules/pydocstyle/rules/non_imperative_mood.rs`:34 on 2023-03-02 21:47_

Made it like so

---

_@staticssleever668 reviewed on 2023-03-02 21:47_

---

_@staticssleever668 reviewed on 2023-03-02 21:47_

---

_Review comment by @staticssleever668 on `crates/ruff/src/rules/pydocstyle/helpers.rs`:122 on 2023-03-02 21:47_

Not relevant not that the function is gone :)

---

_@staticssleever668 reviewed on 2023-03-02 21:48_

---

_Review comment by @staticssleever668 on `crates/ruff/src/visibility.rs`:86 on 2023-03-02 21:48_

Sure!

---

_@charliermarsh reviewed on 2023-03-02 21:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/visibility.rs`:86 on 2023-03-02 21:49_

Thanks :)

---

_Merged by @charliermarsh on 2023-03-02 21:59_

---

_Closed by @charliermarsh on 2023-03-02 21:59_

---

_Label `configuration` added by @charliermarsh on 2023-03-02 22:00_

---

_Comment by @charliermarsh on 2023-03-02 22:00_

Rock! Thank you!

---

_Branch deleted on 2023-03-02 22:00_

---

_Comment by @staticssleever668 on 2023-03-02 22:00_

Thank you for reviewing and merging! :)

---
