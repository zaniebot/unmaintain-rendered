```yaml
number: 16927
title: " [`refurb`] Document why `UserDict`, `UserList`, `UserString` are preferred over `dict`, `list`, `str` (`FURB189`)"
type: pull_request
state: merged
author: dan-wilton
labels:
  - documentation
assignees: []
merged: true
base: main
head: update-subclass-builtin-docs
created_at: 2025-03-23T15:24:19Z
updated_at: 2025-03-23T17:24:40Z
url: https://github.com/astral-sh/ruff/pull/16927
synced_at: 2026-01-12T15:55:59Z
```

#  [`refurb`] Document why `UserDict`, `UserList`, `UserString` are preferred over `dict`, `list`, `str` (`FURB189`)

---

_@dan-wilton_

## Summary

This PR addresses docs issue https://github.com/astral-sh/ruff/issues/14328.


---

_Label `documentation` added by @ntBre on 2025-03-23 16:11_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/subclass_builtin.rs`:26 on 2025-03-23 16:17_

I actually got `{'a': 1, 'b': 2}` when running this. It looks like `dict.__init__` bypasses `__setitem__` too because the `Use instead` example uppercases them both.

```suggestion
/// d = UppercaseDict({"a": 1})
/// d.update({"b": 2})  # Bypasses __setitem__
/// print(d)  # {'a': 1, 'b': 2}
```

---

_Comment by @github-actions[bot] on 2025-03-23 16:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-03-23 16:25_

Thanks! I had a one-character nit and CI wants you to reformat the example, but this looks great otherwise.

---

_Review comment by @dan-wilton on `crates/ruff_linter/src/rules/refurb/rules/subclass_builtin.rs`:26 on 2025-03-23 16:35_

Thanks for the review, and good catch! I'll update the example to be a bit more accurate 😄 

---

_@dan-wilton reviewed on 2025-03-23 16:35_

---

_Comment by @dan-wilton on 2025-03-23 16:37_

@ntBre 
> Thanks! I had a one-character nit and CI wants you to reformat the example, but this looks great otherwise.

Thanks for the review! I ran with the pre-commit hooks, but doesn't seem like they caught the docs formatting - I'll double check my set up 🚀 

---

_Comment by @ntBre on 2025-03-23 16:45_

No worries, I don't think pre-commit catches this unfortunately. I usually run

```shell
uv run --with mdformat --with mdformat-mkdocs --with pyyaml scripts/check_docs_formatted.py --generate-docs
```

but there might be an easier way.

---

_Renamed from " [`subclass-builtin`] Document why `UserDict`, `UserList`, `UserString` are preferred over `dict`, `list`, `str` (`FURB189`)" to " [`refurb`] Document why `UserDict`, `UserList`, `UserString` are preferred over `dict`, `list`, `str` (`FURB189`)" by @ntBre on 2025-03-23 17:18_

---

_Merged by @ntBre on 2025-03-23 17:24_

---

_Closed by @ntBre on 2025-03-23 17:24_

---
