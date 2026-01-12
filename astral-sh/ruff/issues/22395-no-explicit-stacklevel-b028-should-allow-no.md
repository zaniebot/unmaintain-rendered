```yaml
number: 22395
title: no-explicit-stacklevel (B028) should allow no stacklevel when skip_file_prefixes is provided
type: issue
state: open
author: abrahammurciano
labels:
  - good first issue
  - rule
assignees: []
created_at: 2026-01-05T09:17:16Z
updated_at: 2026-01-12T14:54:28Z
url: https://github.com/astral-sh/ruff/issues/22395
synced_at: 2026-01-12T15:54:58Z
```

# no-explicit-stacklevel (B028) should allow no stacklevel when skip_file_prefixes is provided

---

_@abrahammurciano_

### Summary

According to the [python documentation](https://docs.python.org/3/library/warnings.html#warnings.warn)

> The `skip_file_prefixes` keyword argument can be used to indicate which stack frames are ignored when counting stack levels. [...] When prefixes are supplied, `stacklevel` is implicitly overridden to be `max(2, stacklevel)`.
> [...]
> Changed in version 3.12: Added skip_file_prefixes.

If `skip_file_prefixes` is provided, then the stacklevel is already set to at least 2, and the user has already explicitly skipped certain stack frames by prefix. The resulting warning will already give the caller more context about the warning since it will output a stack frame higher up than the one the warning was issued from, most likely outside of the issuing package (i.e. the caller's actual call).

Therefore I believe [the B028 rule](https://docs.astral.sh/ruff/rules/no-explicit-stacklevel/) should be updated to allow either of the two parameters instead of only `stacklevel`.

---

_Comment by @MichaReiser on 2026-01-09 11:25_

Makes sense, thank you

---

_Label `good first issue` added by @MichaReiser on 2026-01-09 11:25_

---

_Label `rule` added by @MichaReiser on 2026-01-09 11:25_

---

_Comment by @caiquejjx on 2026-01-10 14:18_

Hi, I believe that this is already achieved here:
https://github.com/astral-sh/ruff/blob/046c5a46d88c930e4e5e4638ec6b3515ba919982/crates/ruff_linter/src/rules/flake8_bugbear/rules/no_explicit_stacklevel.rs#L79
I also tested locally and I don't get the warning if I set the `skip_file_prefixes` param, so maybe only update the message to also inform about the possibility of setting `skip_file_prefixes`?

---

_Comment by @abrahammurciano on 2026-01-12 14:54_

Weird, it was reporting it for me even with the `skip_file_prefixes` parameter set, but now it's not. Perhaps I was using an older version or something. Regardless, the documentation for this rule should probably also mention this parameter as an alternative fix for this rule.

---
