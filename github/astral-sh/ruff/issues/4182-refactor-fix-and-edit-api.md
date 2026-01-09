---
number: 4182
title: "Refactor `Fix` and `Edit` API"
type: issue
state: closed
author: MichaReiser
labels:
  - internal
assignees: []
created_at: 2023-05-02T09:07:08Z
updated_at: 2023-05-08T09:57:05Z
url: https://github.com/astral-sh/ruff/issues/4182
synced_at: 2026-01-07T13:12:14-06:00
---

# Refactor `Fix` and `Edit` API

---

_Issue opened by @MichaReiser on 2023-05-02 09:07_

This issue is part of #4181. The goal is to prepare the `Fix` and `Edit` API to ease adding the `Applicability` to `Fix` in a follow-up PR. 

I propose the following changes to the API. Please feel free to deviate from or extend the proposal if I overlooked something.

* `Diagnostic`:
	* Change `fix` to `Option<Fix>`
	* Change `set_fix`, `with_fix` and `try_set_fix` to accept a `Fix`  
* `Fix`:
	* Rename `new` to `unspecified`
	* Create the new `unspecified_edits(edit: Edit, rest: impl IntoIterator<Item = Edit>) -> Self` factory function
	* Change `unspecified` to `unspecified(edit: Edit) -> Self`
	* Remove `is_empty()`, 
	* Remove the `FromIterator`, `From<Edit>` and `Default` implementations
	* Maybe: Add a `push_edit(&mut self, edit; Edit)` method


---

_Referenced in [astral-sh/ruff#4181](../../astral-sh/ruff/issues/4181.md) on 2023-05-02 09:07_

---

_Label `help wanted` added by @MichaReiser on 2023-05-02 09:12_

---

_Referenced in [astral-sh/ruff#4183](../../astral-sh/ruff/issues/4183.md) on 2023-05-02 09:16_

---

_Label `help wanted` removed by @MichaReiser on 2023-05-02 09:24_

---

_Label `internal` added by @MichaReiser on 2023-05-02 09:24_

---

_Referenced in [astral-sh/ruff#4198](../../astral-sh/ruff/pulls/4198.md) on 2023-05-03 01:42_

---

_Closed by @MichaReiser on 2023-05-08 09:57_

---
