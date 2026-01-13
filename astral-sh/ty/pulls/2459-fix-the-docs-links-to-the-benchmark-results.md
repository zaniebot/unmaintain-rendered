```yaml
number: 2459
title: Fix the docs links to the benchmark results
type: pull_request
state: merged
author: Iain-S
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix/bar-chart-svg
created_at: 2026-01-12T10:42:14Z
updated_at: 2026-01-13T05:29:52Z
url: https://github.com/astral-sh/ty/pull/2459
synced_at: 2026-01-13T06:26:55Z
```

# Fix the docs links to the benchmark results

---

_@Iain-S_

## Summary

This PR corrects the HTML `<src=...>` attributes in `index.md` such that they point to the right SVG files.

Note:

1. There is a bar chart on the docs home page <https://docs.astral.sh/ty/>.
2. There are two SVG files and we choose which one to display depending on the theme (light or dark).
3. We currently get:
    1. `ty-benchmark-cli.svg`, if we use the light theme. This has light writing, which cannot be seen against the light theme background.
    2. `ty-benchmark-cli-dark.svg`, if we use the dark theme. This does not exist.
4. This PR changes these links so that we use `ty-benchmark-cli-light.svg` for the light theme and `ty-benchmark-cli.svg` for the dark theme. Both of these images already existed.

## Test Plan

We manually compare the before and after appearance of the docs site.

### Light

| before | after |
| - | - | 
| <img width="775" height="300" alt="Screenshot 2026-01-12 at 10 17 27" src="https://github.com/user-attachments/assets/da3854d2-49fd-4a93-980b-2fd43c059155" /> | <img width="736" height="390" alt="Screenshot 2026-01-12 at 10 21 47" src="https://github.com/user-attachments/assets/35ae397f-163a-411d-9470-da1a727eb261" /> |

### Dark

| before | after |
| - | - | 
| <img width="775" height="300" alt="Screenshot 2026-01-12 at 10 17 19" src="https://github.com/user-attachments/assets/48f0f19a-e57b-4b64-89d0-08bad5bcef60" /> | <img width="775" height="300" alt="Screenshot 2026-01-12 at 10 21 43" src="https://github.com/user-attachments/assets/f6c2388b-acfa-4ff1-92fd-2428c9e80d84" /> |

## To the reviewer

The `CONTRIBUTING.md` guidelines recommend running

```shell
npx prettier --prose-wrap always --write "**/*.md"
```

after editing any markdown files. However, this changes 24 files, not only `index.md`. Should I run this as a separate commit, a separate PR or neither?

---

_@dhruvmanila approved on 2026-01-13 05:23_

Thank you!

---

_Label `documentation` added by @dhruvmanila on 2026-01-13 05:24_

---

_Comment by @dhruvmanila on 2026-01-13 05:25_

> However, this changes 24 files, not only `index.md`. Should I run this as a separate commit, a separate PR or neither?

Let's leave that for now, I'm not sure about that as I don't think we enforce it in the CI (cc @zanieb)

---

_Merged by @dhruvmanila on 2026-01-13 05:29_

---

_Closed by @dhruvmanila on 2026-01-13 05:29_

---
