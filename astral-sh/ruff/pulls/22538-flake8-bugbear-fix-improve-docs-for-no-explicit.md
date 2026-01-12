```yaml
number: 22538
title: "[flake8_bugbear] fix: improve docs for `no_explicit_stacklevel`"
type: pull_request
state: open
author: caiquejjx
labels: []
assignees: []
base: main
head: fix-22395
created_at: 2026-01-12T18:45:18Z
updated_at: 2026-01-12T20:50:18Z
url: https://github.com/astral-sh/ruff/pull/22538
synced_at: 2026-01-12T21:25:53Z
```

# [flake8_bugbear] fix: improve docs for `no_explicit_stacklevel`

---

_@caiquejjx_

## Summary
Closes #22395 

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/no_explicit_stacklevel.rs`:20 on 2026-01-12 19:15_

The option to use `skip_file_prefixes` is only available for Python versions 3.12 and higher. So maybe more like:

```
/// more context about the warning. 
/// 
/// In Python 3.12 and higher, one may also use `skip_file_prefixes` to specify
/// which file prefixes are ignored when counting the stack level. This implicitly overrides the `stacklevel` to be
/// at least 2, according to the [Python documentation].
```

and then add a link to https://docs.python.org/3/library/warnings.html#warnings.warn at the bottom.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/no_explicit_stacklevel.rs`:55 on 2026-01-12 19:16_

I think we can just leave this fix title. It's still the case that we want the stacklevel to be at least 2, and it just happens to be the case that there's another way to achieve that. This also means we don't have to give a different message depending on the Python version.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/no_explicit_stacklevel.rs`:51 on 2026-01-12 19:18_

I don't think we need to mention `skip_file_prefixes` here, especially because it is version-dependent. (I think it is also sort of an "accidental" solution to the problem being linted for here.)

```suggestion
        "No explicit `stacklevel` provided".to_string()
```

---

_@dylwil3 requested changes on 2026-01-12 19:18_

Thanks!

---

_@caiquejjx reviewed on 2026-01-12 20:47_

---

_Review comment by @caiquejjx on `crates/ruff_linter/src/rules/flake8_bugbear/rules/no_explicit_stacklevel.rs`:20 on 2026-01-12 20:47_

ohh mb, i forgot this is only for python 3.12 and above.
Applied those changes and there is a link to the docs already so I didn't need to add it 

---

_Review requested from @dylwil3 by @caiquejjx on 2026-01-12 20:50_

---
