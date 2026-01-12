```yaml
number: 18967
title: "[`flake8-executable`] Allow `uvx` in shebang line (`EXE003`)"
type: pull_request
state: merged
author: CodeMan62
labels:
  - rule
assignees: []
merged: true
base: main
head: codeman/18902
created_at: 2025-06-26T17:59:11Z
updated_at: 2025-06-30T13:38:18Z
url: https://github.com/astral-sh/ruff/pull/18967
synced_at: 2026-01-12T15:56:29Z
```

# [`flake8-executable`] Allow `uvx` in shebang line (`EXE003`)

---

_@CodeMan62_

## Summary
closes #18902 

## Test Plan
I have added a test case 

---

_Comment by @github-actions[bot] on 2025-06-26 18:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@jretz reviewed on 2025-06-26 18:13_

---

_Review comment by @jretz on `crates/ruff_linter/src/rules/flake8_executable/rules/shebang_missing_python.rs`:53 on 2025-06-26 18:13_

`run` is not a command available in `uvx`. This should be

```rust
        || shebang.contains("uvx")
```
and/or
```rust
        || shebang.contains("uv tool run")
```
I'd vote for allowing both.

---

_Renamed from "[`flake0-executables`] Allow `uvx run` in shebang line for EXE003" to "[`flake0-executables`] Allow `uvx` in shebang line for EXE003" by @CodeMan62 on 2025-06-26 18:19_

---

_@ntBre reviewed on 2025-06-26 18:39_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_executable/rules/shebang_missing_python.rs`:53 on 2025-06-26 18:39_

I agree, let's include both while we're here.

---

_@ntBre reviewed on 2025-06-26 18:41_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_executable/EXE003_uvx.py`:1 on 2025-06-26 18:41_

Can you make this file executable to resolve the EXE001 error in the snapshot? On a Unix-like system, a command like

```shell
chmod +x crates/ruff_linter/resources/test/fixtures/flake8_executable/EXE003_uvx.py
```

should work.

If we add support for `uv tool run` (and I think we should) we'll also need another test file.

---

_Label `rule` added by @MichaReiser on 2025-06-27 07:02_

---

_@MichaReiser reviewed on 2025-06-27 07:03_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/flake8_executable/EXE003_uvx.py`:1 on 2025-06-27 07:03_

Can we use a more realistic example. I don't think running that script as is would work. Instead, uvx needs to be called with a tool name (see the issue)

---

_@jretz reviewed on 2025-06-27 07:55_

---

_Review comment by @jretz on `crates/ruff_linter/resources/test/fixtures/flake8_executable/EXE003_uvx.py`:1 on 2025-06-27 07:55_

One idea for the `uvx` test case would be:

```python
#!/usr/bin/env -S uvx ruff check --isolated --select EXE003
print("hello world")
```

And for the `uv tool run` test case suggested above:

```python
#!/usr/bin/env -S uv tool run ruff check --isolated --select EXE003
print("hello world")
```

While these are not examples you would commonly see in the real world, they demonstrate the functionality involved and would run without error (after the resolution of this issue is released). They also have a good chance of being easily understood by people working on Astral projects.

Plus, who can resist the fun of a little self-referential code. 😉

---

_Review requested from @ntBre by @CodeMan62 on 2025-06-29 12:26_

---

_Review requested from @MichaReiser by @CodeMan62 on 2025-06-29 12:26_

---

_Comment by @CodeMan62 on 2025-06-29 12:26_

everything is done :+1: 

---

_@ntBre approved on 2025-06-30 13:37_

Looks good, thank you!

---

_Renamed from "[`flake0-executables`] Allow `uvx` in shebang line for EXE003" to "[`flake8-executable`] Allow `uvx` in shebang line (`EXE003`)" by @ntBre on 2025-06-30 13:37_

---

_Merged by @ntBre on 2025-06-30 13:38_

---

_Closed by @ntBre on 2025-06-30 13:38_

---
