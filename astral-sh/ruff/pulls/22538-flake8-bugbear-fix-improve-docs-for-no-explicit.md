```yaml
number: 22538
title: "[flake8_bugbear] fix: improve docs for `no_explicit_stacklevel`"
type: pull_request
state: open
author: caiquejjx
labels:
  - documentation
assignees: []
base: main
head: fix-22395
created_at: 2026-01-12T18:45:18Z
updated_at: 2026-01-20T20:04:48Z
url: https://github.com/astral-sh/ruff/pull/22538
synced_at: 2026-01-20T20:55:43Z
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

_Label `documentation` added by @MichaReiser on 2026-01-13 08:33_

---

_Assigned to @dylwil3 by @MichaReiser on 2026-01-13 08:33_

---

_@ntBre reviewed on 2026-01-20 18:25_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bugbear/rules/no_explicit_stacklevel.rs`:23 on 2026-01-20 18:25_

Does this link work? I don't think it will match up with the one below in the `## References` section, but I'd have to test the docs locally to be sure.

---

_Review comment by @caiquejjx on `crates/ruff_linter/src/rules/flake8_bugbear/rules/no_explicit_stacklevel.rs`:23 on 2026-01-20 19:54_

You're right! I added now a reusable doc link that serves for both docs references nicely:

https://github.com/user-attachments/assets/2b949f00-2852-4d31-b323-13953dfa7868



---

_@caiquejjx reviewed on 2026-01-20 19:54_

---

_Comment by @astral-sh-bot[bot] on 2026-01-20 20:04_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---
