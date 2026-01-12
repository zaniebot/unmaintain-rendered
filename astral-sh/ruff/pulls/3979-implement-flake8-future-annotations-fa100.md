```yaml
number: 3979
title: "Implement `flake8-future-annotations` FA100"
type: pull_request
state: merged
author: TylerYep
labels: []
assignees: []
merged: true
base: main
head: pr
created_at: 2023-04-15T18:47:45Z
updated_at: 2023-05-14T03:16:39Z
url: https://github.com/astral-sh/ruff/pull/3979
synced_at: 2026-01-12T15:55:14Z
```

# Implement `flake8-future-annotations` FA100

---

_@TylerYep_

This PR tackles the first part of #3072, which is to raise a lint error when a rewrite-able type is used without `from __future__ import annotations` present.

Test cases taken from: https://github.com/TylerYep/flake8-future-annotations

Some of the test cases I added (prefixed by `no_future`) do not raise lint warnings now (as expected), but will raise lint warnings once FA102 is implemented. I am adding them to ensure they do not raise extraneous errors to begin with.

This lint rule also found some available fixes in `scripts/check_ecosystem.py`, which tells me there's some value to having this around :)

The next two PRs will add the autofix functionality for FA100 and FA101 and/or FA102. Thanks for the review!

---

_Comment by @github-actions[bot] on 2023-04-15 19:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.2±0.29ms     2.5 MB/sec    1.00     16.1±0.24ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.07ms     4.2 MB/sec    1.00      4.0±0.06ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    488.4±9.48µs     6.0 MB/sec    1.00    487.9±9.25µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.8±0.09ms     3.8 MB/sec    1.00      6.7±0.13ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.12ms     5.2 MB/sec    1.01      7.9±0.08ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1723.3±13.22µs     9.7 MB/sec    1.00  1708.6±19.59µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    187.7±3.36µs    15.7 MB/sec    1.00    187.9±3.59µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.05ms     7.1 MB/sec    1.00      3.6±0.05ms     7.1 MB/sec
parser/large/dataset.py                    1.00      6.3±0.11ms     6.5 MB/sec    1.01      6.3±0.09ms     6.4 MB/sec
parser/numpy/ctypeslib.py                  1.00  1225.3±17.85µs    13.6 MB/sec    1.00  1225.1±24.36µs    13.6 MB/sec
parser/numpy/globals.py                    1.00    124.7±2.50µs    23.7 MB/sec    1.00    124.2±2.27µs    23.8 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.05ms     9.5 MB/sec    1.01      2.7±0.05ms     9.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     21.0±0.63ms  1985.7 KB/sec    1.00     20.6±0.66ms  2025.1 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.20ms     3.1 MB/sec    1.03      5.5±0.25ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   630.1±26.83µs     4.7 MB/sec    1.02  644.2±108.66µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.8±0.30ms     2.9 MB/sec    1.03      9.0±0.35ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.01     10.5±0.38ms     3.9 MB/sec    1.00     10.3±0.36ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.07ms     7.7 MB/sec    1.01      2.2±0.10ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   256.8±13.91µs    11.5 MB/sec    1.08   278.2±19.02µs    10.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.13ms     5.6 MB/sec    1.07      4.9±0.26ms     5.2 MB/sec
parser/large/dataset.py                    1.00      8.5±0.16ms     4.8 MB/sec    1.04      8.8±0.24ms     4.6 MB/sec
parser/numpy/ctypeslib.py                  1.00  1659.1±49.63µs    10.0 MB/sec    1.02  1698.0±70.38µs     9.8 MB/sec
parser/numpy/globals.py                    1.00    169.7±9.08µs    17.4 MB/sec    1.02    172.8±8.51µs    17.1 MB/sec
parser/pydantic/types.py                   1.00      3.7±0.17ms     6.8 MB/sec    1.01      3.8±0.12ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Implement flake8-future-annotations" to "Implement flake8-future-annotations FA100" by @TylerYep on 2023-04-15 19:40_

---

_Marked ready for review by @TylerYep on 2023-04-15 20:18_

---

_Review requested from @charliermarsh by @charliermarsh on 2023-04-15 20:55_

---

_Renamed from "Implement flake8-future-annotations FA100" to "Implement `flake8-future-annotations` FA100" by @TylerYep on 2023-04-30 20:40_

---

_Comment by @charliermarsh on 2023-05-02 20:54_

(Really sorry for the delay here, I will try to get to this soon, I just got distracted between launch and PyCon and some travel.)

---

_@MichaReiser approved on 2023-05-03 06:52_

---

_Review comment by @charliermarsh on `crates/ruff_python_stdlib/src/typing.rs`:204 on 2023-05-04 16:46_

@TylerYep - Could you speak a bit towards how this list was determined? I initially thought it'd only include builtin collections (like `List` and `Dict`), but including `DefaultDict` and `Deque` means we're asking users to import those from `collections`. So should it also include `AsyncIterator`, `Mapping`, etc. -- all the things that were moved from `typing` to `collections.abc` in Python 3.9? (I think `crates/ruff/src/rules/pyupgrade/rules/deprecated_import.rs` is our most comprehensive accounting of those moves, for reference.)

---

_@charliermarsh reviewed on 2023-05-04 16:46_

---

_@TylerYep reviewed on 2023-05-04 17:11_

---

_Review comment by @TylerYep on `crates/ruff_python_stdlib/src/typing.rs`:204 on 2023-05-04 17:11_

These are all of the data structures that can be rewritten by pyupgrade into lowercase form e.g.

List[int] becomes list[int]

DefaultDict[str, bool] becomes defaultdict[str, bool]

Union[int, str] becomes int | str

Changing the import path from typing to collections.abc is not important for this plugin because the resulting code looks the same.

---

_Review comment by @charliermarsh on `crates/ruff_python_stdlib/src/typing.rs`:204 on 2023-05-04 17:31_

By that logic, should `typing.AsyncIterator` and friends also be included?


---

_@charliermarsh reviewed on 2023-05-04 17:31_

---

_Review comment by @charliermarsh on `crates/ruff_python_stdlib/src/typing.rs`:204 on 2023-05-04 17:41_

To my knowledge, `pyupgrade` only rewrites the builtins: https://github.com/asottile/pyupgrade/blob/7c551d39dfa69851607d85392ffa296568e92436/pyupgrade/_plugins/typing_pep585.py#L15

---

_@charliermarsh reviewed on 2023-05-04 17:41_

---

_@TylerYep reviewed on 2023-05-04 18:08_

---

_Review comment by @TylerYep on `crates/ruff_python_stdlib/src/typing.rs`:204 on 2023-05-04 18:08_

Ah, in that case I guess we don't need to include Deque or DefaultDict.

(Technically, DefaultDict can be replaced by defaultdict and Deque can be replaced by deque and keep the same functionality, but Pyupgrade doesn't autofix them so I'll remove them until they are supported)

Fixing...

---

_Comment by @TylerYep on 2023-05-04 22:57_

I'm not sure why tests are failing - running `cargo test` locally passes cleanly for me.

Any idea @charliermarsh ?

---

_Comment by @charliermarsh on 2023-05-05 01:25_

@TylerYep - Just needed to merge, we recently hit a limit on a fixed-size vector that stores all the rules and it had to be increased.

---

_@MichaReiser approved on 2023-05-05 06:53_

---

_Comment by @TylerYep on 2023-05-07 09:14_

I went back to my original plugin and remembered why DefaultDict and Deque were originally included in FA100:

The main goal of this plugin is to signal to users when a type annotation could be rewritten in a more concise way when `from __future__ import annotations` is added.

1. This code could be simplified (and is currently autofixed by Pyupgrade):
```
from typing import List, Optional, Union

a: List[int] = []
b: Optional[Union[int, str]] = None
```

```
from __future__ import annotations

a: list[int] = []
b: int | str | None = None
```

2. This code could be simplified (but is not currently autofixed by Pyupgrade):

```
from collections import defaultdict, deque
from typing import DefaultDict, Deque

a: DefaultDict[int] = defaultdict(int)
b: Deque[int] = deque()
```

```
from __future__ import annotations

from collections import defaultdict, deque

a: defaultdict[int] = defaultdict(int)
b: deque[int] = deque()
```


https://github.com/asottile/pyupgrade/issues/799 provides context on why collection types are not rewritten using Pyupgrade (import aliasing is hard). It might be possible to extend the autofix functionality to rewrite all of them correctly, but that is out of scope for now.

Afaik, DefaultDict and Deque are the only two code changes included in PEP585, improved using `from __future__ import annotations` to use the common lowercase name, but not autofixed by Pyupgrade.

3. This code cannot be simplified, but can use a different import. These cases are not intended to be raised by this flake8 rule because the type annotation doesn't need to be changed by adding `from __future__ import annotations`:
```
from typing import AsyncIterator, Mapping

a: Mapping[int, str] = {}
b: AsyncIterator = ...
```

```
from collections.abc import AsyncIterator, Mapping

a: Mapping[int, str] = {}
b: AsyncIterator = ...

```

My view is that both 1 and 2 should be raised by this plugin, but not 3, because the code itself is not improved by adding `from __future__ import annotations`.

I reverted 4373751 to reflect the above explanation.

---

_Review comment by @calumy on `crates/ruff/src/rules/flake8_future_annotations/rules.rs`:18 on 2023-05-09 16:22_

This section should not have been removed when updating the docs. I'll update the error message to say something along the lines of "Rule `missing-future-annotations-with-imports` docs are not formatted. The example section should be rewritten to:"



---

_@calumy reviewed on 2023-05-09 16:22_

---

_@calumy reviewed on 2023-05-09 16:28_

---

_Review comment by @calumy on `crates/ruff/src/rules/flake8_future_annotations/rules.rs`:18 on 2023-05-09 16:28_

Ref #4312

---

_@TylerYep reviewed on 2023-05-09 17:19_

---

_Review comment by @TylerYep on `crates/ruff/src/rules/flake8_future_annotations/rules.rs`:18 on 2023-05-09 17:19_

Thanks, fixed! The error message looked copy+pastable but did not contain the full output. Hopefully that can be an autofix via pre-commit

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_future_annotations/rules.rs`:94 on 2023-05-12 01:46_

Sorry, last question: what's the benefit in flagging both the usages and the imports? Would flagging the import alone be insufficient?

---

_@charliermarsh reviewed on 2023-05-12 01:46_

---

_@TylerYep reviewed on 2023-05-12 03:36_

---

_Review comment by @TylerYep on `crates/ruff/src/rules/flake8_future_annotations/rules.rs`:94 on 2023-05-12 03:36_

If the user uses `import typing`, we can only flag `typing.List` at the usage. If they use `from typing import List`, then flagging the import alone is sufficient.

---

_Comment by @charliermarsh on 2023-05-12 16:55_

Will merge this after cutting the release.

---

_@charliermarsh reviewed on 2023-05-12 18:15_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:2451 on 2023-05-12 18:15_

These should only be activated for minimum-version < Python 3.9, right? Since PEP 585 was part of Python 3.9 (so future annotations aren't required to use the standard-library variants in 3.9 and later).

---

_Review comment by @TylerYep on `crates/ruff/src/checkers/ast/mod.rs`:2451 on 2023-05-12 18:30_

In practice, this works on Python 3.7+.

---

_@TylerYep reviewed on 2023-05-12 18:30_

---

_@charliermarsh reviewed on 2023-05-12 18:32_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:2451 on 2023-05-12 18:32_

But if your minimum-supported Python version is Python 3.9, you don't need `__future__` annotations to use the standard-library generics. So these errors would already be detected and fixed by the existing `pyupgrade` rules, right?

---

_@TylerYep reviewed on 2023-05-12 18:56_

---

_Review comment by @TylerYep on `crates/ruff/src/checkers/ast/mod.rs`:2451 on 2023-05-12 18:56_

Ah, you're asking about the upper version.

Unions like `str | None` didn't start working until Python 3.10, which are also covered by this plugin. 

After 3.10, the future annotations import is not as useful anymore.

---

_Comment by @calumy on 2023-05-13 06:59_

> Will merge this after cutting the release.

I thought it might be worth flagging that the changes to the ecosystem check seem to be causing it to error. I don't have time to investigate now but thought it would be worth saying something before this is merged. 

---

_Comment by @TylerYep on 2023-05-13 16:11_

What is the error? I see check_ecosystem imports Self, which must mean it's running on Python 3.11+, so none of my changes should impact it.

---

_Comment by @calumy on 2023-05-13 19:09_

> What is the error? I see check_ecosystem imports Self, which must mean it's running on Python 3.11+, so none of my changes should impact it.


Error in the ecosystem check above:
```
'async_generator' object does not support the asynchronous context manager protocol

```


---

_Comment by @TylerYep on 2023-05-13 19:21_

Was that caused by removing the asynccontextmanager import? It was an unused import that pre-commit told me to remove.

If that import is required due to side effects, then we probably need to add a `# noqa` to that line.

**Update: fixed the issue.**

---

_Review comment by @charliermarsh on `scripts/check_ecosystem.py`:39 on 2023-05-13 19:26_

It looks like the usage was removed here?

---

_@charliermarsh reviewed on 2023-05-13 19:26_

---

_Review comment by @TylerYep on `scripts/check_ecosystem.py`:39 on 2023-05-13 22:35_

Thanks, fixed the bad rebase.

---

_@TylerYep reviewed on 2023-05-13 22:35_

---

_Comment by @charliermarsh on 2023-05-14 02:57_

I made a few changes here that I _think_ are improvements based on some of the capabilities we have in Ruff, but if you disagree, just let me know and we can of course discuss further -- want to merge to keep momentum.

1. I removed the diagnostics that trigger on-import, and instead, I'm more closely following our pyupgrade rules. We flag all usages, regardless of whether the user does `import typing` or `from typing import List` -- when the user writes `typing.List` or `List`, we resolve it to `typing.List`. So it's always sufficient to just flag usages, given that we have enough binding tracking and semantic analysis to resolve those usages, regardless of how they're imported.
2. These rules now only flag if your minimum version is below Python 3.9 (or Python 3.10, for `Optional` and `Union`), since adding `__future__` on those versions has no effect with regards to simplifying the imports. (You can already use the standard library generics on those versions.)
3. We weren't flagging `Optional` or `Union`, so I added that too.


---

_Merged by @charliermarsh on 2023-05-14 03:00_

---

_Closed by @charliermarsh on 2023-05-14 03:00_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:2344 on 2023-05-14 03:10_

This condition is a lot to read, I'd like to clean this up... But the idea is: mirror the logic below for `Rule::NonPEP585Annotation`, but flip the `!self.ctx.future_annotations()`. So we're asking: would we trigger here if `__future__` annotations were enabled?

---

_@charliermarsh reviewed on 2023-05-14 03:10_

---
