```yaml
number: 19754
title: "[ty] Fix typing repository commit output in CI"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: dhruv/fix-conformance-commit
created_at: 2025-08-05T07:39:57Z
updated_at: 2025-08-05T09:27:25Z
url: https://github.com/astral-sh/ruff/pull/19754
synced_at: 2026-01-12T15:56:46Z
```

# [ty] Fix typing repository commit output in CI

---

_@dhruvmanila_

## Summary

This PR fixes the issue mentioned in https://github.com/astral-sh/ruff/pull/19736#issuecomment-3151903662 ~~but I can't test it without merging it on `main` because GitHub Actions still pickup the old version of the workflow file.~~ and is tested by manually triggering the workflow, refer to the comment on this PR (https://github.com/astral-sh/ruff/pull/19754#issuecomment-3153894179) which has the commit hash.

---

_Comment by @github-actions[bot] on 2025-08-05 07:41_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @dhruvmanila on 2025-08-05 07:51_

Oh, I think I'll have to merge this PR to even test this out as without that it still uses the old version of the workflow file.

---

_Label `ci` added by @dhruvmanila on 2025-08-05 07:53_

---

_Label `ty` added by @dhruvmanila on 2025-08-05 07:53_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-08-05 07:53_

---

_Marked ready for review by @dhruvmanila on 2025-08-05 07:53_

---

_Comment by @MichaReiser on 2025-08-05 08:11_

Did you try running it from the actions view (or using the `gh` workflow commands)?

<img width="2511" height="859" alt="Screenshot 2025-08-05 at 10 11 09" src="https://github.com/user-attachments/assets/637a180e-88bf-46b1-8401-cc3db698ac9d" />


---

_Comment by @dhruvmanila on 2025-08-05 09:22_

> Did you try running it from the actions view (or using the `gh` workflow commands)?

I did not, thank you for pointing that out.

I triggered it and it works, refer to the comment in this PR (https://github.com/astral-sh/ruff/pull/19754#issuecomment-3153894179) which has the commit hash.

---

_Merged by @dhruvmanila on 2025-08-05 09:23_

---

_Closed by @dhruvmanila on 2025-08-05 09:23_

---

_Branch deleted on 2025-08-05 09:23_

---

_@AlexWaygood approved on 2025-08-05 09:24_

Thank you!! I still can't say I totally understand what the problem with the previous approach was, but if this approach works then it works 😆

---

_Comment by @dhruvmanila on 2025-08-05 09:27_

> Thank you!! I still can't say I totally understand what the problem with the previous approach was, but if this approach works then it works 😆

It's because the `GITHUB_OUTPUT` only populates the output for that specific step and it cannot be accessed via the environment variable, you need to access it using the `steps.<name>.outputs.<key>` format. Refer to my initial approach in this PR (https://github.com/astral-sh/ruff/pull/19754/commits/d213e76cc8f3ad45f389d961f828493950e3f744) but then I realized there's no need to do this output game when I can just get the value from the file itself. I think the reason the `pr-number` requires going through the output loop is because it's being used as an input to another step while the conformance commit hash is directly being utilized in the shell script.

---
