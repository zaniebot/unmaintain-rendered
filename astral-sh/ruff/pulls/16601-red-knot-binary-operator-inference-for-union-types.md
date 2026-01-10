```yaml
number: 16601
title: "[red-knot] Binary operator inference for union types"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/binary-operator-unions
created_at: 2025-03-10T14:27:28Z
updated_at: 2025-03-12T07:21:56Z
url: https://github.com/astral-sh/ruff/pull/16601
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Binary operator inference for union types

---

_Pull request opened by @sharkdp on 2025-03-10 14:27_

## Summary

This fixes a bug I noticed while looking at ecosystem results. The MRE version of it is this:

```py
def sub(x: float, y: float):
    # Red Knot: Operator `-` is unsupported between objects of type `int | float` and `int | float`
    return x - y
```

It's yet another instance of the general problem that our way of representing union types does not prevent us from handling unions (and intersections) incorrectly. The pattern is usually this: we implement operation X by explicitly matching on some cases that we handle explicitly (e.g. literals). For all other types, we fall back to some generic behavior (calling some functions that work for every `Type`). That generic behavior also *compiles* for `Type::Union`/`Type::Intersection`, but what we really want is to recursively call X for each element, which also gives us that special behavior for elements of the union.

For intersections, I'll open a ticket.

## Test Plan

- New Markdown tests.
- Expected diff in the ecosystem checks

---

_Label `red-knot` added by @sharkdp on 2025-03-10 14:27_

---

_Comment by @github-actions[bot] on 2025-03-10 14:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- 
-     |
-     |
- error: lint:unsupported-operator
-    --> /tmp/mypy_primer/projects/mypy_primer/mypy_primer/utils.py:114:79
- 112 |     if check and proc.returncode:
- 113 |         raise subprocess.CalledProcessError(proc.returncode, cmd, output=stdout, stderr=stderr)
- 114 |     return subprocess.CompletedProcess(cmd, proc.returncode, stdout, stderr), end_t - start_t
-     |                                                                               ^^^^^^^^^^^^^^^ Operator `-` is unsupported between objects of type `int | float` and `int | float`
- Found 50 diagnostics
+ Found 49 diagnostics

```
</details>


---

_Comment by @sharkdp on 2025-03-10 14:34_

> ```diff
> -     �[1m�[94m|�[0m
> -     �[1m�[94m|�[0m
> - �[1m�[91merror�[0m: �[1mlint:unsupported-operator�[0m
> ```

Ok, looks like I need the strip-ansi-codes step after all :smile: 

---

_Comment by @MichaReiser on 2025-03-10 14:40_

> Ok, looks like I need the strip-ansi-codes step after all 😄

Or you could ask @BurntSushi to implement the `color` terminal option or implement it yourself (see the CLI proposal, section `terminal`)

---

_Comment by @sharkdp on 2025-03-10 14:44_

> Or you could ask @BurntSushi to implement the `color` terminal option or implement it yourself (see the CLI proposal, section `terminal`)

But I want the CLI output in the GitHub actions run to be with colors (https://github.com/astral-sh/ruff/actions/runs/13767111167/job/38496238179#step:6:108). And I don't want to run it twice. So if stripping ANSI codes works fine, that seems like an acceptable option?

---

_Comment by @sharkdp on 2025-03-10 16:44_

Oh, now the diff is duplicated. I need to fix my `tee` command line, but will have to shift that to tomorrow.

---

_@sharkdp reviewed on 2025-03-11 21:19_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:4054 on 2025-03-11 21:19_

Instead of short-circuiting here, we could probably also build a better diagnostic, telling you which exact element-combination didn't work. But it seems low priority and doesn't necessarily have to be included here, I think.

---

_Marked ready for review by @sharkdp on 2025-03-11 21:20_

---

_Review requested from @carljm by @sharkdp on 2025-03-11 21:20_

---

_Review requested from @MichaReiser by @sharkdp on 2025-03-11 21:20_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-03-11 21:20_

---

_@carljm approved on 2025-03-11 23:44_

Thank you!

---

_Merged by @sharkdp on 2025-03-12 07:21_

---

_Closed by @sharkdp on 2025-03-12 07:21_

---

_Branch deleted on 2025-03-12 07:21_

---
