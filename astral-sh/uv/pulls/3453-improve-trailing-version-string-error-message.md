```yaml
number: 3453
title: Improve trailing version string error message
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/fix-trailing-version-error-message
created_at: 2024-05-08T09:07:40Z
updated_at: 2024-05-08T12:50:30Z
url: https://github.com/astral-sh/uv/pull/3453
synced_at: 2026-01-10T14:37:54Z
```

# Improve trailing version string error message

---

_Pull request opened by @konstin on 2024-05-08 09:07_

We would previously show the parsed version when erroring due to trailing content after a valid version, which can look different than the input. E.g. when encountering `0.1-bulbasaur`, we would display:

```
after parsing '0.1b0', found 'ulbasaur', which is not part of a valid version
```

With storing the input string instead of the input version, we now show:

```
after parsing '0.1-b', found 'ulbasaur', which is not part of a valid version
```


---

_Label `error messages` added by @konstin on 2024-05-08 09:07_

---

_Review requested from @BurntSushi by @konstin on 2024-05-08 09:07_

---

_Comment by @codspeed-hq[bot] on 2024-05-08 09:10_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/konsti/fix-trailing-version-error-message)

### Merging #3453 will **degrade performances by 7.12%**

<sub>Comparing <code>konsti/fix-trailing-version-error-message</code> (44c56ce) with <code>main</code> (5a07923)</sub>



### Summary

`❌ 1 (👁 1)` regressions
`✅ 11` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `konsti/fix-trailing-version-error-message` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `wheelname_tag_compatibility[flyte-short-incompatible]` | 1.1 µs | 1.2 µs | -7.12% |


---

_@BurntSushi approved on 2024-05-08 12:00_

Love it.

I don't get the benchmark regression. I thought maybe this change was doing something to the size of the error type, but it's already boxed, so this shouldn't have an effect?

---

_Comment by @konstin on 2024-05-08 12:50_

This looks like flakiness, i'll ignore it:

![image](https://github.com/astral-sh/uv/assets/6826232/f8f9df6b-816f-451f-bf45-322d29eb9c83)


---

_Merged by @konstin on 2024-05-08 12:50_

---

_Closed by @konstin on 2024-05-08 12:50_

---

_Branch deleted on 2024-05-08 12:50_

---
