```yaml
number: 20675
title: "[ty] improve base conda distinction from child conda"
type: pull_request
state: merged
author: Danielkonge
labels:
  - ty
assignees: []
merged: true
base: main
head: conda-base
created_at: 2025-10-01T22:36:18Z
updated_at: 2025-10-03T13:56:06Z
url: https://github.com/astral-sh/ruff/pull/20675
synced_at: 2026-01-12T15:57:07Z
```

# [ty] improve base conda distinction from child conda

---

_@Danielkonge_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

#19990 didn't completely fix the base vs. child conda environment distinction, since it detected slightly different behavior than what I usually see in conda. E.g., I see something like the following:
```
(didn't yet activate conda, but base is active)
➜ printenv | grep CONDA
CONDA_PYTHON_EXE=/opt/anaconda3/bin/python
CONDA_PREFIX=/opt/anaconda3
CONDA_DEFAULT_ENV=base
CONDA_EXE=/opt/anaconda3/bin/conda
CONDA_SHLVL=1
CONDA_PROMPT_MODIFIER=(base)

(activating conda)
➜ conda activate test

(test is an active conda environment)
❯ printenv | grep CONDA
CONDA_PREFIX=/opt/anaconda3/envs/test
CONDA_PYTHON_EXE=/opt/anaconda3/bin/python
CONDA_SHLVL=2
CONDA_PREFIX_1=/opt/anaconda3
CONDA_DEFAULT_ENV=test
CONDA_PROMPT_MODIFIER=(test)
CONDA_EXE=/opt/anaconda3/bin/conda
```

But the current behavior looks for `CONDA_DEFAULT_ENV = basename(CONDA_PREFIX)` for the base environment instead of the child environment, where we actually see this equality. 

This pull request fixes that and updates the tests correspondingly.

## Test Plan

I updated the existing tests with the new behavior. Let me know if you want more tests. Note: It shouldn't be necessary to test for the case where we have `conda/envs/base`, since one should not be able to create such an environment (one with the name of `CONDA_DEFAULT_ENV`). 


---

_Review requested from @carljm by @Danielkonge on 2025-10-01 22:36_

---

_Review requested from @AlexWaygood by @Danielkonge on 2025-10-01 22:36_

---

_Review requested from @sharkdp by @Danielkonge on 2025-10-01 22:36_

---

_Review requested from @dcreager by @Danielkonge on 2025-10-01 22:36_

---

_Review requested from @MichaReiser by @Danielkonge on 2025-10-01 22:36_

---

_Renamed from "[ty] improve base conda distinction  child conda" to "[ty] improve base conda distinction from child conda" by @Danielkonge on 2025-10-01 22:39_

---

_Review requested from @Gankra by @MichaReiser on 2025-10-02 06:26_

---

_Label `ty` added by @MichaReiser on 2025-10-02 06:26_

---

_Comment by @github-actions[bot] on 2025-10-02 06:57_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-02 06:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review request for @sharkdp removed by @sharkdp on 2025-10-02 07:36_

---

_Comment by @Gankra on 2025-10-02 14:05_

Hmm this implementation has seemingly been working fine for uv for quite a while? 

https://github.com/astral-sh/uv/blob/8da9df3654f255a2f2e8e83c51257c0699ece9f7/crates/uv-python/src/virtualenv.rs#L79

Did I invert something in my implementation?

---

_Comment by @Gankra on 2025-10-02 14:33_

Aha! uv just shipped this fix (and a bonus one) a couple weeks ago.

https://github.com/astral-sh/uv/pull/15679/
https://github.com/astral-sh/uv/pull/15680/

---

_@Gankra approved on 2025-10-02 14:37_

Approving the basic idea, but some more comment/naming cleanups are in-order, ala the equivalent uv PRs. Would you be interested in doing those extra changes? If not I can.

---

_Comment by @Danielkonge on 2025-10-02 15:28_

> Approving the basic idea, but some more comment/naming cleanups are in-order, ala the equivalent uv PRs. Would you be interested in doing those extra changes? If not I can.

I can try to add these changes later today.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-10-02 15:30_

---

_@Danielkonge reviewed on 2025-10-02 20:31_

---

_Review comment by @Danielkonge on `crates/ty/docs/environment.md`:45 on 2025-10-02 20:31_

uv writes `CONDA_ROOT` instead of `_CONDA_ROOT`, but the section is "Externally-defined variables", and the actual environment variable is with the `_`, so I wrote it like this.

---

_Comment by @Danielkonge on 2025-10-02 20:33_

@Gankra: I think I added what the mentioned uv PRs covered now. Let me know if you wanted more than this? 

Also: I left it is in a separate commit for now, but should I just rebase?

---

_@Gankra approved on 2025-10-03 13:34_

Rad, thanks so much!

---

_Merged by @Gankra on 2025-10-03 13:56_

---

_Closed by @Gankra on 2025-10-03 13:56_

---
