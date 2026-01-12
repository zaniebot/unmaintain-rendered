```yaml
number: 4915
title: Generate one fix per statement for flake8-type-checking rules
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/consolidated-typing-errors
created_at: 2023-06-07T03:01:29Z
updated_at: 2023-06-13T13:19:52Z
url: https://github.com/astral-sh/ruff/pull/4915
synced_at: 2026-01-12T03:43:29Z
```

# Generate one fix per statement for flake8-type-checking rules

---

_Pull request opened by @charliermarsh on 2023-06-07 03:01_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

We have special-case handling for the unused imports diagnostic, whereby we generate one diagnostic per import member, but apply a unified fix on a per-statement basis. This strikes the balance between providing granular feedback in the LSP (i.e., being able to highlight individual members as unused), but allowing Code Actions to fix the entire statement in one pass.

This PR applies the same treatment to the flake8-type-checking rules. So, e.g., if you have `from pandas import Series, DataFrame` and both `Series` and `DataFrame` are only used in typing contexts, we'll generate two diagnostics (one for each member), but those diagnostics will share a fix, and that fix will move _both_ members.

## Test Plan

Added some additional snapshots.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-06-07 03:01_

---

_Review requested from @konstin by @charliermarsh on 2023-06-07 03:01_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyflakes/rules/unused_import.rs`:240 on 2023-06-07 03:02_

I cleaned the implementation here up a bit, to use more of our modern conveniences (like `binding.trimmed_range`).

---

_@charliermarsh reviewed on 2023-06-07 03:02_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs`:68 on 2023-06-07 03:05_

This, `typing_only_runtime_import`, and `unused_import` are structurally very similar. They all follow the same pattern:

- Iterate over individual imports, and group them by statement of origin.
- Iterate over the statements, and generate a single fix for all imports (taking into account suppression comments).
- Generate a diagnostic for each import in the statement, using that single fix.
- (Generate a diagnostic for each suppressed violation, so that the suppression comments aren't marked as unused.)

A lot of the complexity comes from handling suppression comments, since we need the unified fix to omit those members that are suppressed, and we rarely do that kind of thing in lint rules (we typically leave suppression to the next phase).

Also, while these three files are structurally similar, there are a bunch of nuanced differences between them that make me not want to bother DRYing them up much further. For example, in the typing-only import rule, we have to not only group by statement, but also by import _type_, since a single import statement can contain imports of multiple types (e.g., `import os, pandas`), which means we need a fix for each import _type_.

---

_@charliermarsh reviewed on 2023-06-07 03:05_

---

_Comment by @github-actions[bot] on 2023-06-07 03:23_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+5, -0, 0 error(s))

<details><summary>disnake (+5, -0)</summary>
<p>

```diff
+ disnake/ext/commands/common_bot_base.py:35:12: TCH004 Move import `importlib.machinery` out of type-checking block. Import is used for more than type hinting.
+ tests/ext/commands/test_core.py:9:35: TCH004 Move import `typing_extensions.assert_type` out of type-checking block. Import is used for more than type hinting.
+ tests/ui/test_action_row.py:13:35: TCH004 Move import `typing_extensions.assert_type` out of type-checking block. Import is used for more than type hinting.
+ tests/ui/test_action_row.py:15:28: TCH004 Move import `disnake.ui.MessageUIComponent` out of type-checking block. Import is used for more than type hinting.
+ tests/ui/test_action_row.py:15:48: TCH004 Move import `disnake.ui.ModalUIComponent` out of type-checking block. Import is used for more than type hinting.
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| TCH004 | 5 | 5 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.5±0.02ms     7.4 MB/sec    1.00      5.5±0.02ms     7.4 MB/sec
formatter/numpy/ctypeslib.py               1.02   1092.4±5.86µs    15.2 MB/sec    1.00   1075.0±1.42µs    15.5 MB/sec
formatter/numpy/globals.py                 1.04    124.3±0.40µs    23.7 MB/sec    1.00    119.7±0.19µs    24.6 MB/sec
formatter/pydantic/types.py                1.01      2.4±0.00ms    10.6 MB/sec    1.00      2.4±0.01ms    10.6 MB/sec
linter/all-rules/large/dataset.py          1.01     14.3±0.05ms     2.8 MB/sec    1.00     14.2±0.09ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    416.2±0.63µs     7.1 MB/sec    1.00    417.9±4.11µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.03ms     4.3 MB/sec    1.01      6.0±0.06ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.03ms     6.0 MB/sec    1.00      6.8±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1460.5±1.78µs    11.4 MB/sec    1.00   1451.3±3.94µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    162.8±0.26µs    18.1 MB/sec    1.00    161.4±0.76µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.03ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.9±0.08ms     6.8 MB/sec    1.00      6.0±0.07ms     6.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1145.8±32.68µs    14.5 MB/sec    1.00  1140.4±19.83µs    14.6 MB/sec
formatter/numpy/globals.py                 1.00    126.3±3.50µs    23.4 MB/sec    1.01    127.1±6.49µs    23.2 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.06ms     9.8 MB/sec    1.00      2.6±0.05ms     9.8 MB/sec
linter/all-rules/large/dataset.py          1.00     14.8±0.17ms     2.8 MB/sec    1.00     14.8±0.15ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.06ms     4.4 MB/sec    1.00      3.8±0.06ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.01    439.3±9.18µs     6.7 MB/sec    1.00    435.6±6.53µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.10ms     4.1 MB/sec    1.01      6.3±0.09ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.07ms     5.6 MB/sec    1.00      7.3±0.08ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1546.3±59.03µs    10.8 MB/sec    1.00  1551.7±25.40µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.01    174.2±3.60µs    16.9 MB/sec    1.00    172.8±4.72µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.04ms     7.7 MB/sec    1.01      3.3±0.05ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs`:186 on 2023-06-07 11:44_

Is this like `List` in `from typing import List, Sequence`?

---

_@konstin approved on 2023-06-07 11:50_

---

_@konstin reviewed on 2023-06-07 11:52_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_type_checking/snapshots/ruff__rules__flake8_type_checking__tests__runtime-import-in-type-checking-block_TCH004_5.py.snap`:18 on 2023-06-07 11:52_

The is an unnecessary pass state; i'd expected it to be cleaned up by another rule but it seems there is no rule currently that detects extra `pass` statements? not sure if this is actionable for this PR tough

---

_@charliermarsh reviewed on 2023-06-08 02:07_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_type_checking/snapshots/ruff__rules__flake8_type_checking__tests__runtime-import-in-type-checking-block_TCH004_5.py.snap`:18 on 2023-06-08 02:07_

We do have a rule that detects empty type-checking blocks, so this would be picked up from that at least.

---

_@charliermarsh reviewed on 2023-06-08 02:08_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs`:186 on 2023-06-08 02:08_

Yeah, I'll add a note.

---

_Comment by @charliermarsh on 2023-06-08 02:10_

I think the ecosystem CI checks must be stale -- I've checked multiple times locally, and I get those same results on main as on this branch. Perhaps the repo was updated between runs?

---

_Merged by @charliermarsh on 2023-06-08 02:22_

---

_Closed by @charliermarsh on 2023-06-08 02:22_

---

_Branch deleted on 2023-06-08 02:22_

---

_Comment by @henryiii on 2023-06-13 02:17_

I think maybe this PR did it (?), but the following code, setting up a type alias:

```python
if typing.TYPE_CHECKING:
    import numpy.typing

    FloatArray = numpy.typing.NDArray[numpy.float64]
else:
    FloatArray = numpy.ndarray
```

Is now being flagged in 0.272, and wasn't in 0.270.

---

_Comment by @charliermarsh on 2023-06-13 02:22_

Thanks, I'll take a look.

---

_Comment by @charliermarsh on 2023-06-13 02:23_

Are you able to include a snippet to reproduce whatever you're seeing? Or link me to the file?

---

_Comment by @henryiii on 2023-06-13 02:28_

Sure, it's:
* PR: https://github.com/scikit-hep/vector/pull/352
* CI: https://github.com/scikit-hep/vector/actions/runs/5250301792/jobs/9484106056?pr=352
* Code: https://github.com/scikit-hep/vector/blob/8ca75f60e125c3b993da84b297f5636843e3210b/src/vector/_typeutils.py#L79-L84

I'm not sure how the check is setup, but I could make the TypeAlias explicit if needed.

---

_Comment by @charliermarsh on 2023-06-13 02:39_

It has to do with how we treat submodule imports. Generally, if you `import a` and `import a.b`, then if you use `a` at runtime, we don't treat `a.b` usages as "typing-only" unless you enable the "strict" setting -- since `import a` typically imports `a.b` anyway.

`numpy.typing` and a few other modules don't behave this way. I honestly might just special-case them.

---

_Comment by @charliermarsh on 2023-06-13 02:39_

I don't quite know the right language to describe it, but here's the same issue in `importlib`: https://github.com/astral-sh/ruff/issues/3821.

---

_Comment by @henryiii on 2023-06-13 02:42_

It's somewhat common to have some typing-only code in a submodule (somewhat often called `*.typing`). The module above, `vector._typeutils`, is the same way (though really only intended for internal use).

What strict setting? I usually like strict settings. :)

---

_Comment by @charliermarsh on 2023-06-13 02:44_

Hah, I was thinking of https://beta.ruff.rs/docs/settings/#flake8-type-checking-strict, but it doesn't actually fix your issue, let me look at it again.

---

_Comment by @charliermarsh on 2023-06-13 02:46_

This does fix the specific issue here:

```py
if typing.TYPE_CHECKING:
    import numpy.typing

    FloatArray = numpy.typing.NDArray[numpy.float64]
else:
    import numpy

    FloatArray = numpy.ndarray
```

In the end, I think it's actually about the lack of branch analysis. Once we see the `import numpy.typing` in the `TYPE_CHECKING` block, we then attribute `FloatArray = numpy.ndarray` to _that_ import, even though it's in a different branch.


---

_Comment by @henryiii on 2023-06-13 13:19_

I applied that method to `vector`, and it's passing. I'm fine with that for now, especially since that's the only reason numpy is imported in this case, maybe it could be improved eventually.

---
