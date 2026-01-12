```yaml
number: 19049
title: "[`flake8-bandit`] Make `S604` and `S609` examples error out-of-the-box"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-5
created_at: 2025-06-30T17:28:55Z
updated_at: 2025-06-30T21:22:00Z
url: https://github.com/astral-sh/ruff/pull/19049
synced_at: 2026-01-12T15:56:30Z
```

# [`flake8-bandit`] Make `S604` and `S609` examples error out-of-the-box

---

_@MeGaGiGaGon_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #18972

Both in one PR since they are in the same file.

S604
---

This PR makes [call-with-shell-equals-true (S604)](https://docs.astral.sh/ruff/rules/call-with-shell-equals-true/#call-with-shell-equals-true-s604)'s example error out-of-the-box

[Old example](https://play.ruff.rs/a054fb79-7653-47f7-9ab5-3d8b7540c810)
```py
import subprocess

user_input = input("Enter a command: ")
subprocess.run(user_input, shell=True)
```

[New example](https://play.ruff.rs/6fea81b4-e745-4b85-8bea-faaabea5c86d)
```py
import my_custom_subprocess

user_input = input("Enter a command: ")
my_custom_subprocess.run(user_input, shell=True)
```

The old example doesn't raise `S604` because it gets overwritten by [subprocess-popen-with-shell-equals-true (S602)](https://docs.astral.sh/ruff/rules/subprocess-popen-with-shell-equals-true/#subprocess-popen-with-shell-equals-true-s602) (which is a good idea to prevent two lints saying the same thing from being raised)

S609
---

This PR makes [unix-command-wildcard-injection (S609)](https://docs.astral.sh/ruff/rules/unix-command-wildcard-injection/#unix-command-wildcard-injection-s609)'s example error out-of-the-box

[Old example](https://play.ruff.rs/849860fa-0d12-4916-bdbc-64a0fa14cd9b)
```py
import subprocess

subprocess.Popen(["chmod", "777", "*.py"])
```

[New example](https://play.ruff.rs/77a54d7c-cf78-4158-bcf8-96dd698cf366)
```py
import subprocess

subprocess.Popen(["chmod", "777", "*.py"], shell=True)
```

I'm not familiar enough with `subprocess` to know why `shell=True` is required to make `S609` raise here, but it works.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-06-30 17:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-06-30 21:10_

makes sense, thanks!

---

_Label `documentation` added by @dylwil3 on 2025-06-30 21:10_

---

_Merged by @dylwil3 on 2025-06-30 21:10_

---

_Closed by @dylwil3 on 2025-06-30 21:10_

---

_Branch deleted on 2025-06-30 21:22_

---
