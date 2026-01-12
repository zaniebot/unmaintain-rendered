```yaml
number: 17108
title: "CI: Use a native version of Shellcheck instead of WASI"
type: pull_request
state: closed
author: akx
labels:
  - ci
assignees: []
base: main
head: ci-native-shellcheck
created_at: 2025-04-01T06:04:41Z
updated_at: 2025-04-01T13:25:44Z
url: https://github.com/astral-sh/ruff/pull/17108
synced_at: 2026-01-12T15:56:00Z
```

# CI: Use a native version of Shellcheck instead of WASI

---

_@akx_

## Summary

Refs https://github.com/astral-sh/ruff/pull/17092#issuecomment-2768137890, this switches the CI pipeline to use a native version of Shellcheck pulled from GitHub Releases instead of the WASI compilation.

cc @AlexWaygood 

## Test Plan

I tried this out in my fork of `ruff` (where it made a cold-cache `pre-commit` CI step faster than a hot-cache run), and we're probably about to find out how it works here in upstream land.

---

_Comment by @github-actions[bot] on 2025-04-01 06:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-04-01 07:24_

---

_Comment by @AlexWaygood on 2025-04-01 12:29_

Nice, that's quite an impressive speedup... I knew that the actionlint step was slow (it's why it's only run if you pass `--hook-stage=manual`), but I didn't realise it was taking up _so_ much time in the GitHub Actions run.

Did you verify that the shellcheck integration is still working as expected? I.e., does it still catch shell-scripting errors embedded in GitHub Actions `run:` steps?

A thing I don't really like about this approach is that it means you won't be able to run the pre-commit hook manually locally unless you have shellcheck preinstalled. (Or, well, you _will_ be able to run the hook, but it won't actually check the same things as it checks in CI unless you have shellcheck preinstalled.) That seems confusing enough that we'd probably have to document it in our `CONTRIBUTING.md` file -- but I'd much rather contributors didn't have to install any other tools at all. Sorta the point of pre-commit, in my opinion, is that it makes it very easy to locally run exactly the same commands that are run in CI.

You mentioned in https://github.com/astral-sh/ruff/pull/17092#issuecomment-2768137890 that a big reason this might be so slow in CI is because the GitHub Actions runners are starved of cores -- we could see if switching to the larger depot runners helps. We use them in some other jobs already, e.g. https://github.com/astral-sh/ruff/blob/d0c8eaa0923352ccc4ec30c9fac1f573f20998b3/.github/workflows/ci.yaml#L201-L203

---

_Comment by @akx on 2025-04-01 12:58_

> Did you verify that the shellcheck integration is still working as expected? I.e., does it still catch shell-scripting errors embedded in GitHub Actions `run:` steps?

Yes – an earlier version of this had `cd $(mktemp -d)`, which [shellcheck dutifully did flag me to make `cd "$(mktemp -d)"` instead.](https://github.com/akx/ruff/actions/runs/14188273689/job/39747397724)

> A thing I don't really like about this approach is that it means you won't be able to run the pre-commit hook manually locally unless you have shellcheck preinstalled. 

I was thinking the same – but then again, this check is disabled by default anyway, so I doubt too many contributors would know to run `--hook-stage=manual` if they hadn't peeked in the pre-commit configuration to find the comment about this. 😅 

> You mentioned in https://github.com/astral-sh/ruff/pull/17092#issuecomment-2768137890 that a big reason this might be so slow in CI is because the GitHub Actions runners are starved of cores

That was, tbh, before I noticed it's running WASM in a Go-based interpreter. 😄


---

_Comment by @AlexWaygood on 2025-04-01 13:05_

> Yes – an earlier version of this had `cd $(mktemp -d)`, which [shellcheck dutifully did flag me to make `cd "$(mktemp -d)"` instead.](https://github.com/akx/ruff/actions/runs/14188273689/job/39747397724)

Nice, thank you ❤️

> I was thinking the same – but then again, this check is disabled by default anyway, so I doubt too many contributors would know to run `--hook-stage=manual` if they hadn't peeked in the pre-commit configuration to find the comment about this. 😅

hmm, true. It's still nice for me, personally, to be able to run exactly the same thing locally as is run in CI, though ;-)

I'd like to experiment with switching to the depot runners and see how much that buys us

---

_Label `ci` added by @AlexWaygood on 2025-04-01 13:07_

---

_Comment by @akx on 2025-04-01 13:08_

> I'd like to experiment with switching to the depot runners and see how much that buys us

Sure, but I wouldn't be holding my breath for great gains there. I can whip up a sibling PR anyway, just a sec.

---

_Comment by @AlexWaygood on 2025-04-01 13:22_

Thanks for your work on this!! https://github.com/astral-sh/ruff/pull/17120 got this down to 30s, which I think is "fast enough" now :-D

---

_Closed by @AlexWaygood on 2025-04-01 13:22_

---
