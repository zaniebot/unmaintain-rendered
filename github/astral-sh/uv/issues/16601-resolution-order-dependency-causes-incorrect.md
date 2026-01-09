---
number: 16601
title: Resolution Order Dependency Causes Incorrect Backtracking
type: issue
state: open
author: qiulang
labels:
  - question
assignees: []
created_at: 2025-11-05T14:52:13Z
updated_at: 2025-11-21T18:49:51Z
url: https://github.com/astral-sh/uv/issues/16601
synced_at: 2026-01-07T13:12:19-06:00
---

# Resolution Order Dependency Causes Incorrect Backtracking

---

_Issue opened by @qiulang on 2025-11-05 14:52_

## Summary

When resolving dependencies, uv makes different (incorrect) resolution decisions depending on whether transitive dependency constraints are also specified at the top level. This causes it to pick incompatible package versions and backtrack to ancient versions instead of finding valid solutions.

## Minimal Reproduction

### Environment

- Index: Any (tested with official PyPI and Chinese mirrors)
- What matter is the date (as in Nov.05 2025) with **huggingface-hub has released 1.0.1 while transformers 4.57.1 is the latest**

### Case 1: Bug - Just install xinference

```bash
uv pip compile - <<EOF
xinference
EOF
```

**Result (WRONG):**

```
transformers==4.12.2       # Ancient version from 2021!
huggingface-hub==1.0.1     # Violates transformers' <1.0 constraint
tokenizers==0.10.3         # Ancient version
Resolution time: ~1.6s
```

### Case 2: Works - Add explicit transformers constraint

```bash
uv pip compile - <<EOF
transformers>=4.40.0
xinference
EOF
```

**Result (CORRECT):**

```
transformers==4.57.1
huggingface-hub==0.36.0    # Correctly satisfies both constraints
tokenizers==0.22.1
Resolution time: ~167ms
```

### Case 3: Also works - Order doesn't matter when both specified

```bash
uv pip compile - <<EOF
xinference
transformers>=4.40.0
EOF
```

**Result (CORRECT):**

```
transformers==4.57.1
huggingface-hub==0.36.0
tokenizers==0.22.1
Resolution time: ~21ms
```

## Root Cause Analysis

The dependency constraints are:

- `xinference` → `huggingface-hub>=0.19.4` (no upper bound)
- `xinference` → `transformers` (via `peft`)
- `transformers>=4.40` → `huggingface-hub>=0.34.0,<1.0`
- `gradio` (xinference dependency) → `huggingface-hub>=0.33.5,<2.0`

Valid solution space: `huggingface-hub` in range `[0.34.0, 1.0)` (e.g., 0.36.0)

### What uv does wrong (Case 1):

1. Processes xinference's loose `huggingface-hub>=0.19.4` constraint
2. Sees gradio's `<2.0` upper bound
3. **Picks `huggingface-hub==1.0.1`** (satisfies gradio but violates transformers)
4. Later discovers transformers needs `<1.0`
5. **Backtracks transformers to 4.12.2** instead of reconsidering huggingface-hub
6. Results in ancient, incompatible versions

BTW, `pip install xinference` works

### What uv does right (Cases 2 & 3):

1. Has `transformers>=4.40.0` as a top-level constraint
2. Considers transformers' `huggingface-hub<1.0` requirement from the start
3. **Correctly picks `huggingface-hub==0.36.0`** (in the valid range)
4. Everyone gets modern, compatible versions

## Expected Behavior

The resolver should produce the same result regardless of whether transitive constraints are explicitly listed at the top level. In this case:

- `uv pip compile "xinference"` should produce the same resolution as
- `uv pip compile "transformers>=4.40.0\nxinference"`

Since a valid solution exists (huggingface-hub 0.36.0), uv should find it in both cases.

## Impact

This affects real-world packages like `xinference` which cannot be installed with uv without workarounds:

```bash
# Fails
uv pip install xinference

# Workaround 1: Pre-constrain huggingface-hub
uv pip install "huggingface-hub>=0.34.0,<1.0" xinference

# Workaround 2: Explicitly add transformers constraint
uv pip install transformers xinference
```

Users are forced to understand the internal dependency tree and manually add constraints, which defeats the purpose of automatic dependency resolution.

## Additional Notes

- `pip` resolves this correctly in all cases (always picks modern versions)
- The resolution time difference (1.6s vs 167ms) suggests uv is doing significant backtracking
- The bug occurs regardless of which package index is used

## Suggested Fix

When uv encounters a conflict during backtracking, it should reconsider earlier choices (like huggingface-hub version) rather than always downgrading the package that exposed the conflict (transformers).

### Platform

Ubuntu22 / macOS 15

### Version

uv 0.9.7

### Python version

3.11/3/12 (does not matter)

---

_Label `bug` added by @qiulang on 2025-11-05 14:52_

---

_Referenced in [xorbitsai/inference#4215](../../xorbitsai/inference/issues/4215.md) on 2025-11-05 14:58_

---

_Comment by @konstin on 2025-11-05 17:20_

In a case where the latest version of two packages conflict, both resolutions are valid. From uv's perspective, when the only direct dependency is xinference, the choices are downgrading transformers or downgrading huggingface-hub and both look equally bad. Either choice is a correct one: Both fulfill the requirements you set. As a heuristic, uv does prefer higher versions for direct dependencies over indirect dependencies, but for indirect dependencies it only guarantees it's deterministic (running `uv pip compile` twice on the same requirements with the same releases gives the same resolution), but not any specific order. If you need a specific resolution, you need to specify those additional constraints, as you e.g. did with by adding `transformers>=4.40.0`.

---

_Label `bug` removed by @konstin on 2025-11-05 17:20_

---

_Label `question` added by @konstin on 2025-11-05 17:20_

---

_Comment by @qiulang on 2025-11-06 00:41_

I understand what you said but from end user point of view, using uv to install xinference would fail, while pip wouldn't. That is the problem. Besides, uv backtracks to a version that satisfying the requirement  but then fails to install, I can't say that is a good result.

I check uv installation log and find it output these lines

```
DEBUG Recording dependency conflict of transformers==4.55.4 from incompatibility of (transformers, huggingface-hub)
DEBUG Package transformers has too many conflicts (affected), prioritizing
DEBUG Package huggingface-hub has too many conflicts (culprit), deprioritizing and backtracking
```

But in the end uv still choose huggingface-hub==1.0.1 

---

_Comment by @konstin on 2025-11-21 12:29_

> I understand what you said but from end user point of view, using uv to install xinference would fail, while pip wouldn't. That is the problem.

pip doesn't guarantee this either, it picks the correct solution "accidentally", and this may change when more packages or new versions are published.

---

_Comment by @notatallshaw on 2025-11-21 17:18_

Yeah, as a pip maintainer I don't follow how pip could guarantee not to choose an older transformers and tokenizers:

```
transformers==4.12.2
huggingface-hub==1.0.1
tokenizers==0.10.3
```

Over an older huggingface-hub:

```
transformers==4.57.1
huggingface-hub==0.36.0
tokenizers==0.22.1
```

Unless I am missing something here?

As a side note, I do wonder if there should be some option that would allow users to do something like: If packages have published packages with the last N years only select those packages, not older packages. But I don't have a specific proposal for that the UX of what would look like, and it would definitely be a new issue.

---

_Comment by @zanieb on 2025-11-21 18:01_

The naive pattern would be `exclude-older`? I guess something like `exclude-stale` would be possible too, but harder because we need to visit the latest versions first and get their dates.

---

_Comment by @notatallshaw on 2025-11-21 18:49_

> I guess something like `exclude-stale` would be possible too, but harder because we need to visit the latest versions first and get their dates.

Yeah, if the intent is for it to be an easy "just fix things" option there's a bunch of design choices that need to be made, I'm probably not going to propose it for pip until there is prior art.

---
