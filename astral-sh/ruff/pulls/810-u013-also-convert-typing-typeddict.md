```yaml
number: 810
title: "U013: Also convert typing.TypedDict"
type: pull_request
state: merged
author: martinlehoux
labels: []
assignees: []
merged: true
base: main
head: fix-u0013-namespaced-typed-dict
created_at: 2022-11-18T22:48:58Z
updated_at: 2022-11-27T12:23:44Z
url: https://github.com/astral-sh/ruff/pull/810
synced_at: 2026-01-12T15:55:05Z
```

# U013: Also convert typing.TypedDict

---

_@martinlehoux_

- This occured to me while working on NamedTuple
- I don't know if it's better to make small PRs, or if I can bring these fixes along the NamedTuple one

---

_@andersk reviewed on 2022-11-18 22:53_

---

_Review comment by @andersk on `src/pyupgrade/plugins/convert_typed_dict_functional_to_class.rs`:37 on 2022-11-18 22:53_

We have `crate::ast::helpers::match_module_member` for doing this more correctly.

---

_Review comment by @martinlehoux on `src/pyupgrade/plugins/convert_typed_dict_functional_to_class.rs`:37 on 2022-11-19 12:42_

Nice one, thanks!

---

_@martinlehoux reviewed on 2022-11-19 12:42_

---

_Merged by @charliermarsh on 2022-11-19 14:29_

---

_Closed by @charliermarsh on 2022-11-19 14:29_

---

_Branch deleted on 2022-11-27 12:23_

---
