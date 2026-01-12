```yaml
number: 16186
title: "[`flake8-type-checking`] Avoid `TC004` false positives for submodules"
type: pull_request
state: open
author: Daverball
labels: []
assignees: []
base: main
head: bugfix/tc004-overlapping-import-bindings
created_at: 2025-02-16T15:25:04Z
updated_at: 2025-04-30T06:41:41Z
url: https://github.com/astral-sh/ruff/pull/16186
synced_at: 2026-01-12T15:55:53Z
```

# [`flake8-type-checking`] Avoid `TC004` false positives for submodules

---

_@Daverball_

Closes #15723

This is not a perfect solution, since we still end up with some false negatives, but it's still a lot better than having false positives.

## Summary

Submodule imports create a binding on the base module, if we have more than one submodule import for the same base module we may incorrectly attribute a runtime reference to a typing only import, if we don't make sure that the reference starts with the same name segments as the import.

## Test Plan

`cargo nextest run`

---

_Comment by @github-actions[bot] on 2025-02-16 15:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/scikit-build/scikit-build-core/blob/7568be0570ff095ffc63edc55413bc9c67fde4eb/src/scikit_build_core/resources/_editable_redirect.py#L11'>src/scikit_build_core/resources/_editable_redirect.py:11:12:</a> TC004 Move import `importlib.machinery` out of type-checking block. Import is used for more than type hinting.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TC004 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/mod.rs`:539 on 2025-02-24 08:52_

Actually it's still quite a bit more complex than that, since we can have partial overlaps and complete overlaps of bindings, so whether or not we can look at the references of a shadowing import depends on how it overlaps with our import and whether or not those overlapping imports are in a runtime context or not.

We also don't properly take into account the fact that the parent module will be fully imported by a submodule import. So for the parent module we would need to check if there are any bindings in runtime context at all in order to avoid false negatives, e.g. for a runtime use of `os.sep` with a typing only import of `os.path`. `os.path` should be flagged if there isn't also a runtime `os` import.

So for something like
```python
import a
from typing import TYPE_CHECKING
if TYPE_CHECKING:
    import a.b
    import a.b.d

x = a.b  # leads to TC004 for `import a.b`

import a.b.c

y = a.b  # fine
z = a.b.d  # leads to TC004 for `import a.b.d`
```
For reference `x` we would need to detect that the shadowed binding `a` for `import a` doesn't include `a.b`, but for reference `y` we would need to detect that `import a.b.c` fully covers the shadowed `import a.b`, so we don't have to worry about it. But for `z` `import a.b.c` doesn't fully cover `import a.b.d`, so we do need to look at the references to `import a.b.d` for checking whether or not `import a.b.d` has any runtime references. So the combinatorics quickly get out of hand here.

The approach chosen in this PR still seems like a reasonable compromise however, since it will catch some things with submodule imports, while incorrectly flagging fewer imports that shouldn't be flagged. I.e. we raise the false negative rate slightly, but we get rid of the false positives we're most likely to encounter in the wild.

Anything more accurate would probably need multi-file analysis and some degree of type inference, so we actually know which attributes are currently available at runtime in the current scope and through which binding.

Another approach we could take is to just never emit `TC004` if we're shadowing another binding. This would completely get rid of any false positives. But I feel the approach this PR has taken is more balanced.

---

_@Daverball reviewed on 2025-02-24 08:52_

---

_Comment by @MichaReiser on 2025-04-28 07:18_

@ntBre or @dylwil3 would you have time to review this pr and feel comfortable to do so? I'm leaning towards relying on @Daverball expertise here. 

---

_Comment by @Daverball on 2025-04-28 07:54_

@MichaReiser Currently, I would probably defer this PR in favor of getting #13965 to a state, where it can be merged, since that seems like a more complete solution for the problem to me, that benefits the whole semantic model and not just a single rule.

I'll try to find some time to take a closer look at the approach taken in the other PR to see if I can come up with some changes, beyond the ones that have already been proposed, that reduce the overhead. I had a similar idea for solving this problem more generally, but assumed it would add too much overhead, for too little benefit, so I didn't pursue it further at the time. But given that I now know that there are other rules that would benefit from this, it might be worth pursuing after all.

If we don't think we can push #13965 forward any time soon, and reviewing this PR can be done quickly, with a low amount of effort, then it still might be a reasonable stop gap measure, otherwise I'd probably put this into draft until we know for sure whether or not #13965 will happen. Especially since we will probably need to or at least want to revert this change, once it does.

---

_Comment by @MichaReiser on 2025-04-30 06:41_

Thanks @Daverball. @dylwil3 considers picking up #13965

---
