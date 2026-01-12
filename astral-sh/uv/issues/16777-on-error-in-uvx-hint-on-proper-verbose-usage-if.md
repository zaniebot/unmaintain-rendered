```yaml
number: 16777
title: "On error in `uvx`, hint on proper `--verbose` usage if it was provided to the subcommand"
type: issue
state: closed
author: zanieb
labels:
  - enhancement
  - help wanted
  - error messages
assignees: []
created_at: 2025-11-19T16:53:10Z
updated_at: 2025-12-09T16:15:08Z
url: https://github.com/astral-sh/uv/issues/16777
synced_at: 2026-01-12T16:02:38Z
```

# On error in `uvx`, hint on proper `--verbose` usage if it was provided to the subcommand

---

_@zanieb_

### Summary

When uv fails with an internal error, people will re-run it with `--verbose` but can accidentally provide `--verbose` to the subcommand instead. We should hint in this situation

### Example

e.g.,

```
uvx foo-does-not-exist --verbose
```

if we fail to resolve the package, we should notice the user provided `--verbose` and hint

```
error: ...

hint: You provided `--verbose` to `foo-does-not-exist`, did you mean to provide it to uv? e.g., with `uvx --verbose foo-does-not-exist`
```

---

_Label `enhancement` added by @zanieb on 2025-11-19 16:53_

---

_Label `error messages` added by @zanieb on 2025-11-19 16:53_

---

_Label `help wanted` added by @zanieb on 2025-11-19 16:53_

---

_Comment by @CarlosEduR on 2025-11-19 20:48_

I am new here, but I'd like to give it a try :). Can I work on it?

---

_Comment by @zanieb on 2025-11-19 20:58_

Feel free! It might be a little tricky to capture the error... maybe you'll want to write a function that wraps https://github.com/astral-sh/uv/blob/5c71b5c124a1ed98fb21502693b63069f97bec6c/crates/uv/src/commands/tool/run.rs#L85

---

_Closed by @zanieb on 2025-12-09 16:15_

---
