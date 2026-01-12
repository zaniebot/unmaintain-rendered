```yaml
number: 7706
title: "Improve performance of `commented-out-code` (~50-80%)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/eradicate
created_at: 2023-09-29T03:21:13Z
updated_at: 2023-09-29T20:20:33Z
url: https://github.com/astral-sh/ruff/pull/7706
synced_at: 2026-01-12T15:55:24Z
```

# Improve performance of `commented-out-code` (~50-80%)

---

_@charliermarsh_

## Summary

This PR implements a variety of optimizations to improve performance of the Eradicate rule, which always shows up in all-rules benchmarks and bothers me. (These improvements are not hugely important, but it was kind of a fun Friday thing to spent a bit of time on.)

The improvements include:

- Doing cheaper work first (checking for some explicit substrings upfront).
- Using `aho-corasick` to speed an exact substring search.
- Merging multiple regular expressions using a `RegexSet`.
- Removing some unnecessary `\s*` and other pieces from the regular expressions (since we already trim strings before matching on them).

## Test Plan

I benchmarked this function in a standalone crate using a variety of cases. Criterion reports that this version is up to 80% faster, and almost every case is at least 50% faster:

```
Eradicate/Detection/# Warn if we are installing over top of an existing installation. This can
                        time:   [101.84 ns 102.32 ns 102.82 ns]
                        change: [-77.166% -77.062% -76.943%] (p = 0.00 < 0.05)
                        Performance has improved.
Found 3 outliers among 100 measurements (3.00%)
  3 (3.00%) high mild
Eradicate/Detection/#from foo import eradicate
                        time:   [74.872 ns 75.096 ns 75.314 ns]
                        change: [-84.180% -84.131% -84.079%] (p = 0.00 < 0.05)
                        Performance has improved.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high mild
Eradicate/Detection/# encoding: utf8
                        time:   [46.522 ns 46.862 ns 47.237 ns]
                        change: [-29.408% -28.918% -28.471%] (p = 0.00 < 0.05)
                        Performance has improved.
Found 7 outliers among 100 measurements (7.00%)
  6 (6.00%) high mild
  1 (1.00%) high severe
Eradicate/Detection/# Issue #999
                        time:   [16.942 ns 16.994 ns 17.058 ns]
                        change: [-57.243% -57.064% -56.815%] (p = 0.00 < 0.05)
                        Performance has improved.
Found 3 outliers among 100 measurements (3.00%)
  2 (2.00%) high mild
  1 (1.00%) high severe
Eradicate/Detection/# type: ignore
                        time:   [43.074 ns 43.163 ns 43.262 ns]
                        change: [-17.614% -17.390% -17.152%] (p = 0.00 < 0.05)
                        Performance has improved.
Found 5 outliers among 100 measurements (5.00%)
  3 (3.00%) high mild
  2 (2.00%) high severe
Eradicate/Detection/# user_content_type, _ = TimelineEvent.objects.using(db_alias).get_or_create(
                        time:   [209.40 ns 209.81 ns 210.23 ns]
                        change: [-32.806% -32.630% -32.470%] (p = 0.00 < 0.05)
                        Performance has improved.
Eradicate/Detection/# this is = to that :(
                        time:   [72.659 ns 73.068 ns 73.473 ns]
                        change: [-68.884% -68.775% -68.655%] (p = 0.00 < 0.05)
                        Performance has improved.
Found 9 outliers among 100 measurements (9.00%)
  7 (7.00%) high mild
  2 (2.00%) high severe
Eradicate/Detection/#except Exception:
                        time:   [92.063 ns 92.366 ns 92.691 ns]
                        change: [-64.204% -64.052% -63.909%] (p = 0.00 < 0.05)
                        Performance has improved.
Found 4 outliers among 100 measurements (4.00%)
  2 (2.00%) high mild
  2 (2.00%) high severe
Eradicate/Detection/#print(1)
                        time:   [68.359 ns 68.537 ns 68.725 ns]
                        change: [-72.424% -72.356% -72.278%] (p = 0.00 < 0.05)
                        Performance has improved.
Found 2 outliers among 100 measurements (2.00%)
  1 (1.00%) low mild
  1 (1.00%) high mild
Eradicate/Detection/#'key': 1 + 1,
                        time:   [79.604 ns 79.865 ns 80.135 ns]
                        change: [-69.787% -69.667% -69.549%] (p = 0.00 < 0.05)
                        Performance has improved.
```

---

_@charliermarsh reviewed on 2023-09-29 03:22_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/eradicate/detection.rs`:45 on 2023-09-29 03:22_

Put the cheapest and most discriminant step first. This should exclude most comments.

---

_@charliermarsh reviewed on 2023-09-29 03:22_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/eradicate/detection.rs`:23 on 2023-09-29 03:22_

Combine into a single regex, rather than five separate.

---

_@charliermarsh reviewed on 2023-09-29 03:23_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/eradicate/detection.rs`:14 on 2023-09-29 03:23_

I actually don't know how much this improves performance over a manual iteration. Having trouble getting this function to show up in Instruments.

---

_Comment by @codspeed-hq[bot] on 2023-09-29 03:31_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/eradicate)

### Merging #7706 will **improve performances by 3.56%**

<sub>Comparing <code>charlie/eradicate</code> (91aede8) with <code>main</code> (e9f8b91)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/eradicate` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[large/dataset.py]` | 95.7 ms | 92.4 ms | +3.56% |


---

_Comment by @charliermarsh on 2023-09-29 03:32_

Think I need to run the benchmarks locally with just this rule or something.

---

_Comment by @github-actions[bot] on 2023-09-29 03:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/eradicate/detection.rs`:66 on 2023-09-29 08:35_

Not sure if it helps performance but some of the above regex could be combined with https://docs.rs/regex/latest/regex/struct.RegexSet.html . But we may also be able to join the Regex by simply ORing them together (A | B | C) instead of having multiple regex expressions

---

_@MichaReiser reviewed on 2023-09-29 08:35_

---

_@MichaReiser reviewed on 2023-09-29 08:36_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/eradicate/detection.rs`:14 on 2023-09-29 08:36_

You may need a test file with a ton of comments

---

_Marked ready for review by @charliermarsh on 2023-09-29 19:18_

---

_@charliermarsh reviewed on 2023-09-29 19:18_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/eradicate/detection.rs`:153 on 2023-09-29 19:18_

This is Python 2 code.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/eradicate/detection.rs`:130 on 2023-09-29 19:19_

The method assumes you pass a comment, so this test doesn't really make sense.

---

_@charliermarsh reviewed on 2023-09-29 19:19_

---

_@charliermarsh reviewed on 2023-09-29 19:19_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/eradicate/detection.rs`:72 on 2023-09-29 19:19_

Interestingly, combining `HASH_NUMBER` and `ALLOWLIST_REGEX` into a single regex ended up _hurting_ performance, so I left those as two separate passes.

---

_Review comment by @konstin on `crates/ruff_linter/Cargo.toml`:74 on 2023-09-29 19:43_

which of these styles do we want (my preference is `<pkg> = "x.y.z"`)?

---

_Label `performance` added by @charliermarsh on 2023-09-29 19:55_

---

_@konstin approved on 2023-09-29 20:00_

---

_Review comment by @charliermarsh on `crates/ruff_linter/Cargo.toml`:74 on 2023-09-29 20:02_

Oh I prefer the `version` one but I broke my rule.

---

_@charliermarsh reviewed on 2023-09-29 20:02_

---

_Renamed from "Improve performance of commented-out-code rule" to "Improve performance of commented-out-code rule by 50-80%" by @charliermarsh on 2023-09-29 20:06_

---

_Renamed from "Improve performance of commented-out-code rule by 50-80%" to "Improve performance of `commented-out-code` (50-80% improvement)" by @charliermarsh on 2023-09-29 20:06_

---

_Renamed from "Improve performance of `commented-out-code` (50-80% improvement)" to "Improve performance of `commented-out-code` (~50-80%)" by @charliermarsh on 2023-09-29 20:06_

---

_Merged by @charliermarsh on 2023-09-29 20:13_

---

_Closed by @charliermarsh on 2023-09-29 20:13_

---

_Branch deleted on 2023-09-29 20:13_

---
