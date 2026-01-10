```yaml
number: 12187
title: Update code owners for red knot
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: red-knot-update-code-owners
created_at: 2024-07-04T11:47:14Z
updated_at: 2024-07-04T12:14:25Z
url: https://github.com/astral-sh/ruff/pull/12187
synced_at: 2026-01-10T21:47:02Z
```

# Update code owners for red knot

---

_Pull request opened by @MichaReiser on 2024-07-04 11:47_

## Summary

Include the other newly introduced red knot crates.

## Test Plan

Not sure how to test this and I'm worrie dthat the wildcard won't workd but we'll see


---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-04 11:47_

---

_Label `internal` added by @MichaReiser on 2024-07-04 11:47_

---

_Label `internal` removed by @MichaReiser on 2024-07-04 11:47_

---

_Label `ci` added by @MichaReiser on 2024-07-04 11:47_

---

_Comment by @AlexWaygood on 2024-07-04 11:50_

GitHub reports it's valid syntax, at least!

<details>
<summary>Screenshot</summary>

![image](https://github.com/astral-sh/ruff/assets/66076021/185a6c5b-36cf-406d-91ca-08d9464a87de)

</details>

We use a lot of fancy globs and wildcards over at https://github.com/python/cpython/blob/main/.github/CODEOWNERS, so it's definitely possible to do this kind of thing

---

_Comment by @AlexWaygood on 2024-07-04 11:51_

CODEOWNERS docs on syntax here: https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners#codeowners-syntax

---

_@AlexWaygood approved on 2024-07-04 11:52_

LGTM

---

_Comment by @MichaReiser on 2024-07-04 11:56_

Oh nice, I didn't know that it validates the syntax. I did consult the syntax document. I just know that I'm very good at misinterpreting documentation :sweat_smile: 

---

_Merged by @MichaReiser on 2024-07-04 12:14_

---

_Closed by @MichaReiser on 2024-07-04 12:14_

---

_Branch deleted on 2024-07-04 12:14_

---
