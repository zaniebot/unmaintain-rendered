```yaml
number: 17869
title: "[ty] Increase worker-thread stack size"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/increase-default-stack-size
created_at: 2025-05-05T19:16:20Z
updated_at: 2025-05-05T21:15:18Z
url: https://github.com/astral-sh/ruff/pull/17869
synced_at: 2026-01-10T18:57:03Z
```

# [ty] Increase worker-thread stack size

---

_Pull request opened by @sharkdp on 2025-05-05 19:16_

## Summary

closes #17472 

This is obviously just a band-aid solution to this problem (in that you can always make your [pathological inputs](https://github.com/sympy/sympy/blob/28994edd82a18c22a98a52245c173f3c836dcad3/sympy/polys/numberfields/resolvent_lookup.py) bigger and it will still crash), but I think this is not an unreasonable change — even if we add more sophisticated solutions later. I tried using `stacker` as suggested by @MichaReiser, and it works. But it's unclear where exactly would be the right place to put it, and even for the `sympy` problem, we would need to add it both in the semantic index builder AST traversal and in type inference. Increasing the default stack size for worker threads, as proposed here, doesn't solve the underlying problem (that there is a hard limit), but it is more universal in the sense that it is not specific to large binary-operator expression chains.

To determine a reasonable stack size, I created files that look like

*right associative*:
```py
from typing import reveal_type
total = (1 + (1 + (1 + (1 + (… + 1)))))
reveal_type(total)
```

*left associative*
```py
from typing import reveal_type
total = 1 + 1 + 1 + 1 + … + 1
reveal_type(total)
```

with a variable amount of operands (`N`). I then chose the stack size large enough to still be able to handle cases that existing type checkers can not:

```
right

  N = 20: mypy takes ~ 1min
  N = 350: pyright crashes with a stack overflow (mypy fails with "too many nested parentheses")
  N = 800: ty(main) infers Literal[800] instantly
  N = 1000: ty(main) crashes with "thread '<unknown>' has overflowed its stack"

  N = 7000: ty(this branch) infers Literal[7000] instantly
  N = 8000+: ty(this branch) crashes


left

  N = 300: pyright emits "Maximum parse depth exceeded; break expression into smaller sub-expressions"
           total is inferred as Unknown
  N = 5500: mypy crashes with "INTERNAL ERROR"
  N = 2500: ty(main) infers Literal[2500] instantly
  N = 3000: ty(main) crashes with "thread '<unknown>' has overflowed its stack"

  N = 22000: ty(this branch) infers Literal[22000] instantly
  N = 23000+: ty(this branch) crashes
```

## Test Plan

New regression test.

---

_Label `ty` added by @sharkdp on 2025-05-05 19:16_

---

_Review requested from @carljm by @sharkdp on 2025-05-05 19:16_

---

_Review requested from @MichaReiser by @sharkdp on 2025-05-05 19:16_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-05 19:16_

---

_Review requested from @dcreager by @sharkdp on 2025-05-05 19:16_

---

_Comment by @github-actions[bot] on 2025-05-05 19:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-05 19:20_

---

_@sharkdp reviewed on 2025-05-05 19:23_

---

_Review comment by @sharkdp on `crates/ty/src/main.rs`:409 on 2025-05-05 19:23_

It's probably worth noting that this does not generally increase memory usage by 16 MiB × num_threads. It's just the max. available stack space that a single thread can use.

---

_Review comment by @carljm on `crates/ty/src/main.rs`:409 on 2025-05-05 19:25_

And 16MiB/thread is not even detectable, relative to the memory we need for source texts and AST, for any project large enough to care about memory usage.

---

_@carljm approved on 2025-05-05 19:25_

This looks great, thank you

---

_@sharkdp reviewed on 2025-05-05 19:28_

---

_Review comment by @sharkdp on `crates/ty/tests/cli.rs`:1209 on 2025-05-05 19:28_

I chose a *much* smaller limit than what we can handle (but still bigger than what we can handle on `main`) for two reasons:

* Debug builds take up more stack space and crash at smaller input sizes
* We don't want minor increases of stack frame sizes to lead to a test failure here.

---

_Merged by @sharkdp on 2025-05-05 19:31_

---

_Closed by @sharkdp on 2025-05-05 19:31_

---

_Branch deleted on 2025-05-05 19:31_

---

_Comment by @MichaReiser on 2025-05-05 21:15_

I think the long term solution here would be to implement binary expression traversal in a loop rather than with recursion. But this is fine for now

---
