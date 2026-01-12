```yaml
number: 7819
title: Refactor applicability to use safe and unsafe
type: pull_request
state: closed
author: zanieb
labels:
  - internal
assignees: []
base: main
head: zanie/app-refactor-safe
created_at: 2023-10-04T19:05:10Z
updated_at: 2023-10-05T18:04:11Z
url: https://github.com/astral-sh/ruff/pull/7819
synced_at: 2026-01-12T15:55:24Z
```

# Refactor applicability to use safe and unsafe

---

_@zanieb_

Following much discussion for #4181 at https://github.com/astral-sh/ruff/pull/5119, https://github.com/astral-sh/ruff/discussions/5476, #7769, and in [Discord](https://discord.com/channels/1039017663004942429/1082324250112823306/1159144114231709746), this pull request changes `Applicability` from using `Suggested` for uncertain fixes to `Unsafe`.

Also removes `Applicability::Unspecified` (replacing #7792).

I have some qualms about how we will explain "manual" fixes to users which are a type of unsafe fix that cannot be applied programmatically. However, since manual fixes are not presented to the user after #7769 we can worry about this in the future.

---

_Label `internal` added by @zanieb on 2023-10-04 19:13_

---

_Comment by @zanieb on 2023-10-04 19:18_

Here we structure applicability as two enumerations

- Automatic
    - Safe
    - Unsafe
- Manual

We could also consider a flat structure

- Safe
- Unsafe
- Hint / Suggestion / Manual / Display / etc.

or a structure that more closely matches _when_ it is applicable / safe

- Always
- Sometimes
- Never

(implemented in https://github.com/astral-sh/ruff/pull/7821)



---

_Comment by @codspeed-hq[bot] on 2023-10-04 19:23_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zanie/app-refactor-safe)

### Merging #7819 will **degrade performances by 3.35%**

<sub>Comparing <code>zanie/app-refactor-safe</code> (c5ae2f1) with <code>main</code> (59c00b5)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/zanie/app-refactor-safe)._

### Benchmarks breakdown

|     | Benchmark | `main` | `zanie/app-refactor-safe` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[large/dataset.py]` | 92.6 ms | 95.8 ms | -3.35% |


---

_Comment by @github-actions[bot] on 2023-10-04 19:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Closed by @zanieb on 2023-10-05 18:04_

---

_Comment by @zanieb on 2023-10-05 18:04_

Closing in favor of #7821 

---
