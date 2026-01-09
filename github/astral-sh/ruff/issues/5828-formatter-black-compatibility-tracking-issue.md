---
number: 5828
title: 📋 Formatter black compatibility tracking issue
type: issue
state: closed
author: konstin
labels:
  - formatter
assignees: []
created_at: 2023-07-17T09:32:23Z
updated_at: 2023-09-04T13:28:23Z
url: https://github.com/astral-sh/ruff/issues/5828
synced_at: 2026-01-07T13:12:15-06:00
---

# 📋 Formatter black compatibility tracking issue

---

_Issue opened by @konstin on 2023-07-17 09:32_

We will continuously update this issue with compatibility measure between our formatter and black and the general status of the formatter.


### Not Implemented

`not_yet_implemented` usages: 0
`not_yet_implemented_custom_text` usages: 0

### Select Projects / Small ecosystem check

To condense the compatibility over an entire repository, we use the similarity index. It is defined as the percentage of lines that remains unchanged between black's formatting and our formatting. You could compute it as the number of neutral lines in a diff divided by the neutral plus the removed lines.

I chose 6 different projects to check for now, we may update this list in the future. 
To reproduce these numbers or to check your own changes, you can run `scripts/formatter_progress.sh`:

**django** (web framework): 2752 files, similarity index 0.99805:

- Tracked in https://github.com/astral-sh/ruff/issues/6069

**transformers** (ML framework): 2527 files, similarity index 0.99620

- https://github.com/astral-sh/ruff/issues/5824 (The project doesn’t use `black .` but `black examples tests src utils` , same thing for ruff)

**twine** (small util library): 32 files, similarity index 0.99876

**typeshed** (stub files): 3627 files, similarity index 0.99953

- https://github.com/astral-sh/ruff/issues/5821
- https://github.com/astral-sh/ruff/issues/5822
- We're missing black's exact empty line rules: https://github.com/astral-sh/ruff/issues/6723

**warehouse** (pypi backend): 641 files, similarity index 0.99601

**zulip** (a django user): 1424 files, similarity index 0.99727

### Large ecosystem check

There is a number of formatter bugs showing up in the ecosystem checks, a list from 2023-08-21 is in https://gist.github.com/konstin/c7183dda99e65c7a1acbd970d2f44b42. Implicit string concatenation and fluent style seem to be the two common causes. Gist created by: 

```shell
target/release/ruff_dev format-dev --stability-check --error-file target/errors-$(date --iso-8601).txt --multi-project target/checkouts > target/stability-check-release-$(date --iso-8601).txt
```
and manually removing e.g. slow formatting.

---

_Label `formatter` added by @konstin on 2023-07-17 09:32_

---

_Comment by @cnpryer on 2023-07-17 13:28_

I know it's comment placement, but is #5630 related to black compatibility?

---

_Comment by @konstin on 2023-07-18 17:30_

> I know it's comment placement, but is https://github.com/astral-sh/ruff/issues/5630 related to black compatibility?

i'd expect fixing this would be a minor improvement in the compatibility score, but it's unfortunately hard to tell beforehand

---

_Referenced in [astral-sh/ruff#5355](../../astral-sh/ruff/issues/5355.md) on 2023-07-18 17:32_

---

_Renamed from "Formatter black compatibility tracking issue" to ":clipboard: Formatter black compatibility tracking issue" by @konstin on 2023-07-19 13:20_

---

_Renamed from ":clipboard: Formatter black compatibility tracking issue" to "📋 Formatter black compatibility tracking issue" by @konstin on 2023-07-19 13:20_

---

_Comment by @konstin on 2023-07-19 13:24_

Old numbers:

build: similarity index 0.738
django: similarity index 0.978
transformers: similarity index 0.972
typeshed: similarity index 0.827
zulip: similarity index 0.970

New numbers:

build: 40 files, similarity index 0.740
django: 2752 files, similarity index 0.983
transformers: 2527 files, similarity index 0.976
typeshed: 3627 files, similarity index 0.766
zulip: 1424 files, similarity index 0.975

I'm unsure what happened with typeshed

---

_Comment by @konstin on 2023-07-19 14:32_

https://github.com/astral-sh/ruff/pull/5882 fixes the transformers bugs, after that all test projects pass without errors (except for those files that already have syntax errors in the input)

---

_Comment by @konstin on 2023-07-19 19:45_

Updated the gist to today (https://gist.github.com/konstin/edd2a914fdf4196020d9efa3f2c7e6ff) and filed issues for recurring themes.

---

_Comment by @konstin on 2023-07-20 09:51_

I minimized all cases of unstable formatting from the ecosystem checks: https://gist.github.com/konstin/61adab19d5615160750171944653673d . This gives us a single file that checks for all known cases of unstable formatting

---

_Referenced in [astral-sh/ruff#5915](../../astral-sh/ruff/pulls/5915.md) on 2023-07-20 10:52_

---

_Referenced in [astral-sh/ruff#5943](../../astral-sh/ruff/pulls/5943.md) on 2023-07-21 08:24_

---

_Comment by @konstin on 2023-07-24 17:44_

#6035 documents how to run the formatter progress tracking script yourself, and it's also part of CI now

---

_Comment by @konstin on 2023-07-25 11:26_

Tracking issue specifically for django: https://github.com/astral-sh/ruff/issues/6069


---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-07-31 16:16_

---

_Comment by @konstin on 2023-08-21 13:49_

Updated the original post

---

_Referenced in [shap/shap#3111](../../shap/shap/pulls/3111.md) on 2023-08-22 01:19_

---

_Comment by @cnpryer on 2023-08-28 16:30_

> not_yet_implemented_custom_text usages: 8

Was browsing. Figured I could cc https://github.com/astral-sh/ruff/pull/6905#issuecomment-1694508204. Looks to be no longer used :)



---

_Comment by @konstin on 2023-09-04 13:28_

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| django       |           0.99957 |              2760 |                67 |
| transformers |           0.99928 |              2587 |               456 |
| twine        |           0.99982 |                33 |                 1 |
| typeshed     |           0.99978 |              3496 |              2173 |
| warehouse    |           0.99818 |               648 |                24 |
| zulip        |           0.99942 |              1437 |                32 |

We've reached >99.9% in 5 out of 6 projects! There's still some differences remaining, but all nodes are implemented and all the big recurring themes are fixed, so i'm closing this issue.

---

_Closed by @konstin on 2023-09-04 13:28_

---
