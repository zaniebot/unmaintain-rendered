```yaml
number: 10270
title: "Encapsulate `Program` fields"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/encapsulate-program
created_at: 2024-03-07T08:51:26Z
updated_at: 2024-03-07T16:19:12Z
url: https://github.com/astral-sh/ruff/pull/10270
synced_at: 2026-01-10T22:47:01Z
```

# Encapsulate `Program` fields

---

_Pull request opened by @dhruvmanila on 2024-03-07 08:51_

## Summary

This PR updates the fields in `Program` struct to be private and exposes methods to get the values. The motivation behind this is to encapsulate the internal representation of the parsed program which we could alter in the future.


---

_Label `parser` added by @dhruvmanila on 2024-03-07 08:51_

---

_Renamed from "Encapsulte `Program` fields" to "Encapsulate `Program` fields" by @dhruvmanila on 2024-03-07 08:53_

---

_Comment by @codspeed-hq[bot] on 2024-03-07 10:47_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/encapsulate-program)

### Merging #10270 will **not alter performance**

<sub>Comparing <code>dhruv/encapsulate-program</code> (0035ec2) with <code>dhruv/parser</code> (035ac75)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Marked ready for review by @dhruvmanila on 2024-03-07 13:11_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-07 13:11_

---

_@MichaReiser approved on 2024-03-07 15:19_

You know how to make me happy :)

---

_Merged by @dhruvmanila on 2024-03-07 16:19_

---

_Closed by @dhruvmanila on 2024-03-07 16:19_

---

_Branch deleted on 2024-03-07 16:19_

---
