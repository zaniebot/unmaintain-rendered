```yaml
number: 982
title: Bump version to 0.0.1a18
type: pull_request
state: merged
author: MichaReiser
labels:
  - release
assignees: []
merged: true
base: main
head: release-0.0.1a18
created_at: 2025-08-14T09:36:40Z
updated_at: 2025-08-14T10:21:32Z
url: https://github.com/astral-sh/ty/pull/982
synced_at: 2026-01-10T02:34:10Z
```

# Bump version to 0.0.1a18

---

_Pull request opened by @MichaReiser on 2025-08-14 09:36_

_No description provided._

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-08-14 09:44_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:8 on 2025-08-14 09:46_

```suggestion
- Fix goto-definition on imports ([#19834](https://github.com/astral-sh/ruff/pull/19834))
- Support non-generic recursive type aliases that use [the `type` statement](https://docs.python.org/3/reference/simple_stmts.html#grammar-token-python-grammar-type_stmt) ([#19805](https://github.com/astral-sh/ruff/pull/19805))
```

I think this is an important distinction because (while it's a very important first step), we still don't support most recursive type aliases that users would have in their code. We don't yet support:
- generic recursive type aliases
- explicit recursive type aliases that use `typing.TypeAlias`
- explicit recursive type aliases that use `typing.TypeAliasType`
- implicit recursive type aliases 

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:15 on 2025-08-14 09:47_

```suggestion
- Enable go-to-definition to jump to the runtime definition in the standard library for stdlib symbols (rather than the type definition in typeshed's stubs) ([#19529](https://github.com/astral-sh/ruff/pull/19529))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:19 on 2025-08-14 09:48_

```suggestion
- Warn users if the server received unknown options ([#19779](https://github.com/astral-sh/ruff/pull/19779))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:21 on 2025-08-14 09:48_

```suggestion
- Render docstrings in hover ([#19882](https://github.com/astral-sh/ruff/pull/19882))
- Resolve docstrings for modules ([#19898](https://github.com/astral-sh/ruff/pull/19898))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:27 on 2025-08-14 09:49_

```suggestion
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:30 on 2025-08-14 09:49_

```suggestion
- Improve performance around tuple types ([#19840](https://github.com/astral-sh/ruff/pull/19840))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:33 on 2025-08-14 09:50_

```suggestion
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:38 on 2025-08-14 09:51_

```suggestion
- Improve subscript narrowing for `collections.ChainMap`, `collections.Counter`, `collections.deque` and `collections.OrderedDict` ([#19781](https://github.com/astral-sh/ruff/pull/19781))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:41 on 2025-08-14 09:52_

```suggestion
- Extend all tuple special casing to tuple subclasses ([#19669](https://github.com/astral-sh/ruff/pull/19669))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:42 on 2025-08-14 09:52_

```suggestion
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:43 on 2025-08-14 09:52_

```suggestion
- Improve multithreaded performance for large codebases ([#19867](https://github.com/astral-sh/ruff/pull/19867))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:32 on 2025-08-14 09:53_

not a great description of the user-facing impact, but I'm not sure how to describe this better...

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:47 on 2025-08-14 09:54_

```suggestion
- Fix deferred name loading in PEP695 generic classes/functions ([#19888](https://github.com/astral-sh/ruff/pull/19888))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:48 on 2025-08-14 09:55_

```suggestion
- Improve handling of symbol-lookup edge cases involving class scopes ([#19795](https://github.com/astral-sh/ruff/pull/19795))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:49 on 2025-08-14 09:55_

```suggestion
```

---

_@AlexWaygood approved on 2025-08-14 09:55_

---

_Comment by @dhruvmanila on 2025-08-14 10:07_

Thank you @AlexWaygood !

---

_Marked ready for review by @dhruvmanila on 2025-08-14 10:17_

---

_Label `release` added by @dhruvmanila on 2025-08-14 10:19_

---

_Merged by @dhruvmanila on 2025-08-14 10:21_

---

_Closed by @dhruvmanila on 2025-08-14 10:21_

---

_Branch deleted on 2025-08-14 10:21_

---
