```yaml
number: 8586
title: Allow overriding pydocstyle convention rules
type: pull_request
state: merged
author: alanhdu
labels:
  - bug
  - docstring
assignees: []
merged: true
base: main
head: alan/override
created_at: 2023-11-09T17:42:04Z
updated_at: 2023-11-10T18:55:55Z
url: https://github.com/astral-sh/ruff/pull/8586
synced_at: 2026-01-10T23:40:55Z
```

# Allow overriding pydocstyle convention rules

---

_Pull request opened by @alanhdu on 2023-11-09 17:42_

## Summary

This fixes #2606 by moving where we apply the convention ignores -- instead of applying that at the very end, e track, we now track which rules have been specifically enabled (via `Specificity::Rule`). If they have, then we do *not* apply the docstring overrides at the end.

## Test Plan

Added unit tests to `ruff_workspace` and an integration test to `ruff_cli`

---

_Comment by @github-actions[bot] on 2023-11-09 17:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:795 on 2023-11-10 01:37_

I think it _could_ be fine to omit `docstring_override_updates` and just add to `docstring_overrides` whenever we see a direct selector. In other words, I don't know if this "reset" behavior is important for the purpose of these overrides. I'm trying to think of a case where this would matter.

As an example of why this exists (to the best of my memory): imagine you have `select=D` and `ignore=D401` in your configuration file. If you then pass `--select D4`, we want to ignore the `ignore`. So we treat a `--select` as resetting the selection chain.

Eh, thinking about it further, what you have here is probably right. Imagine you have `select=D415` in your configuration file, and then you run with `--select=D`. We probably still want to enforce the configuration override for `D415` in that case?

---

_Comment by @charliermarsh on 2023-11-10 01:38_

This looks good to me, thanks for putting it together! I'd suggest adding a test or two to `crates/ruff_cli/tests/integration_test.rs` which can roughly mirror the way you tested it on the CLI, but with snapshot testing.

---

_Review requested from @charliermarsh by @charliermarsh on 2023-11-10 01:38_

---

_Label `bug` added by @charliermarsh on 2023-11-10 01:38_

---

_Label `docstring` added by @charliermarsh on 2023-11-10 01:38_

---

_Comment by @charliermarsh on 2023-11-10 01:38_

(I think the implementation is fine. We already special-case conventions here, so I don't mind tracking a bit of extra state for it.)

---

_@charliermarsh reviewed on 2023-11-10 01:38_

---

_Marked ready for review by @alanhdu on 2023-11-10 14:24_

---

_@alanhdu reviewed on 2023-11-10 14:25_

---

_Review comment by @alanhdu on `crates/ruff_workspace/src/configuration.rs`:795 on 2023-11-10 14:25_

Hm... I was actually thinking about this in terms of having multiple `ruff.toml`s (e.g. in a monorepo or something). In that case, I think the intended behavior is that `select` "clears" the state of the "parent" `ruff.toml`s, and personally I think it'd be confusing if the docstring conventions were the only counter-example to that.

---

_Comment by @alanhdu on 2023-11-10 14:26_

Alright! Added some unit tests and an integration test, so this should be ready for review!

---

_Comment by @codspeed-hq[bot] on 2023-11-10 14:33_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alanhdu:alan/override)

### Merging #8586 will **not alter performance**

<sub>Comparing <code>alanhdu:alan/override</code> (e18d884) with <code>main</code> (a7dbe9d)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_Review comment by @alanhdu on `crates/ruff_cli/tests/integration_test.rs`:1618 on 2023-11-10 14:36_

FWIW, D417 was the rule I was surprised was disabled in the numpy convention.  It looks like there's specific code written to handle some of the quirks (e.g. https://github.com/astral-sh/ruff/blob/036b6bc0bdc6664e9d0d927acb7f0c224927b564/crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs#L1842), so it seemed odd that it was disabled.

---

_Review comment by @alanhdu on `crates/ruff_workspace/src/options.rs`:2400 on 2023-11-10 14:36_

Is there another place I should update the docs, or is this the right place?

---

_@alanhdu reviewed on 2023-11-10 14:36_

---

_@alanhdu reviewed on 2023-11-10 15:16_

---

_Review comment by @alanhdu on `crates/ruff_cli/tests/integration_test.rs`:1594 on 2023-11-10 15:16_

FWIW, I found some of this to be a bit repetitive -- would you be open to a PR that tries to refactor some of this Command construction into a custom builder struct?

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:795 on 2023-11-10 18:35_

Makes sense.

---

_@charliermarsh reviewed on 2023-11-10 18:35_

---

_@charliermarsh approved on 2023-11-10 18:38_

This is great -- thanks for taking it on!

---

_@charliermarsh reviewed on 2023-11-10 18:38_

---

_Review comment by @charliermarsh on `crates/ruff_cli/tests/integration_test.rs`:1594 on 2023-11-10 18:38_

Sure, I'm open to the idea!

---

_@charliermarsh reviewed on 2023-11-10 18:39_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/options.rs`:2400 on 2023-11-10 18:39_

This is the right place :)

---

_Comment by @charliermarsh on 2023-11-10 18:42_

Appreciate all the tests, thanks for following up there.

---

_Merged by @charliermarsh on 2023-11-10 18:47_

---

_Closed by @charliermarsh on 2023-11-10 18:47_

---

_Branch deleted on 2023-11-10 18:55_

---
