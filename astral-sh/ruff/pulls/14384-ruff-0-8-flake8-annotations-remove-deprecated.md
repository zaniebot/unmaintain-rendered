```yaml
number: 14384
title: "[ruff 0.8] [`flake8-annotations`] Remove deprecated rules ANN101 and ANN102"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - breaking
assignees: []
merged: true
base: ruff-0.8
head: alex/remove-ann101-102
created_at: 2024-11-16T19:06:15Z
updated_at: 2024-11-19T09:00:43Z
url: https://github.com/astral-sh/ruff/pull/14384
synced_at: 2026-01-12T15:55:47Z
```

# [ruff 0.8] [`flake8-annotations`] Remove deprecated rules ANN101 and ANN102

---

_@AlexWaygood_

## Summary

Remove deprecated rules ANN101 and ANN102. These have been deprecated since [Ruff 0.2](https://github.com/astral-sh/ruff/pull/9680), released in February. There are no open issues on the tracker asking us to undeprecate them.

## Test Plan

- `cargo test`
- `git grep ANN101`, `git grep ANN102`, `git grep MissingTypeCls` and `git grep MissingTypeSelf` all show no results
- Wait to see how impactful the ecosystem report shows this to be


---

_Label `breaking` added by @AlexWaygood on 2024-11-16 19:06_

---

_Comment by @github-actions[bot] on 2024-11-16 19:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-11-16 19:27_

All the errors in the ecosystem check are due to people having already explicitly ignored the rule in their `pyproject.toml` files. I think that validates removing the rule.

---

_Renamed from "[ruff 0.8] Remove deprecated rules ANN101 and ANN102" to "[ruff 0.8] [`flake8-annotations`] Remove deprecated rules ANN101 and ANN102" by @AlexWaygood on 2024-11-16 19:38_

---

_Comment by @AlexWaygood on 2024-11-16 19:53_

I feel like my main concern with merging this _now_ is it will make the ecosystem report less useful for other PRs targeting the `ruff-0.8` branch, since, once this is merged, Ruff just can't parse the configuration file at all for repos that explicitly list these rules in `tool.ruff.lint.ignore`. If we want to land this, I think I'd prefer to merge it after all other changes have made it into the `ruff-0.8` branch.

---

_@MichaReiser approved on 2024-11-16 21:36_

---

_Label `do-not-merge` added by @AlexWaygood on 2024-11-16 21:43_

---

_Comment by @MichaReiser on 2024-11-17 15:03_

Landing that PR late in the 0.8 release process might not be enough because it means that we go blind on ecosystem checks after the 0.8 release until all ecosystem projects have updated their configurations (which may take a while).

I think we have to change the ecosystem script to remove individual rules. 

---

_Comment by @AlexWaygood on 2024-11-17 15:10_

> Landing that PR late in the 0.8 release process might not be enough because it means that we go blind on ecosystem checks after the 0.8 release until all ecosystem projects have updated their configurations (which may take a while).

Right. I added a step to the 0.8 plan which is "file PRs with other projects helping them to update to 0.8" after we've actually cut the release. But maybe even that won't be sufficient.

---

_Comment by @MichaReiser on 2024-11-17 15:18_

> Right. I added a step to the 0.8 plan which is "file PRs with other projects helping them to update to 0.8" after we've actually cut the release. But maybe even that won't be sufficient.

I haven't checked but I worry that many projects aren't even using Ruff 0.7 at this point. That means they might have to upgrade from an arbitrary old Ruff version to 0.8, and upgrading might just not be a priority for them. 

---

_Comment by @AlexWaygood on 2024-11-17 17:34_

Okay, so for the various affected projects:
- `setuptools`:
  - Broken by https://github.com/astral-sh/ruff/pull/14382
  - Currently pins ruff to `>=0.7.1`:
    - https://github.com/pypa/setuptools/blob/540001561bc2c6766940ba5fd6247735c1a3a290/pyproject.toml#L118
    - https://github.com/pypa/setuptools/blob/540001561bc2c6766940ba5fd6247735c1a3a290/.pre-commit-config.yaml#L3
- `latch`:
  - Broken by this PR
  - Currently pins ruff to `>=0.7.0`: https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/pyproject.toml#L90
- `scikit-build-core`:
  - Broken by this PR _and_ #14385
  - Currently pins ruff to `0.7.2`: https://github.com/scikit-build/scikit-build-core/blob/acc69a86cd1995b8baff4e631e453906226801dd/.pre-commit-config.yaml#L28
- `zulip`:
  - Broken by this PR
  - Currently pins ruff to `0.7.1`: https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/requirements/dev.txt#L2910
- `trio`:
  - Broken by this PR
  - Currently pins ruff to `0.7.3`: https://github.com/python-trio/trio/blob/785dc59309ca4f491562b817836092f54b7da4b1/test-requirements.txt#L116
- `airflow`
  - Broken by https://github.com/astral-sh/ruff/pull/14385
  - Currently pins ruff to `0.7.3`: https://github.com/apache/airflow/blob/c807762ec65db6bd699c1eb638dd79d173717df2/.pre-commit-config.yaml#L363
- `pandas`:
  - Broken by #14385
  - Currently pins ruff to `0.7.3`: https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/.pre-commit-config.yaml#L22
- `cibuildwheel`:
  - Broken by #14385
  - Currently pins ruff to `0.7.3`: https://github.com/pypa/cibuildwheel/blob/df6f8863900b4519a8dfee992548b11b4329d12b/.pre-commit-config.yaml#L17
- `pip`
  - Broken by #14385
  - Currently pins ruff to `0.5.6`: https://github.com/pypa/pip/blob/fe0925b3c00bf8956a0d33408df692ac364217d4/.pre-commit-config.yaml#L25
- `indico`
  - Broken by #14385
  - Currently pins ruff to `0.7.2`: https://github.com/indico/indico/blob/e63bce8b88b4a3bcfc8397d83fceadd32b81712c/requirements.dev.txt#L166

So nearly all of those look like they should be _fairly_ easy upgrades, and it looks like they try hard to keep their ruff pin up to date. The only exception is perhaps the `pip` one. But the sheer quantity of affected projects means that we should probably adopt some interim solution anyway to avoid the ecosystem check breaking for all PRs.

---

_Comment by @Avasam on 2024-11-18 00:04_

I commented the same thing in https://github.com/astral-sh/ruff/pull/14383#issuecomment-2481685777, but I think it's even more relevant here.

Making "unknown rule ignored" a warning instead of a parsing error may help keep the ecosystem checks relevant when removing a rule. This is already a feature request there: https://github.com/astral-sh/ruff/issues/13505

---

_Comment by @MichaReiser on 2024-11-18 07:31_

> Making "unknown rule ignored" a warning instead of a parsing error may help keep the ecosystem checks relevant when removing a rule. This is already a feature request there: https://github.com/astral-sh/ruff/issues/13505

That's a good point. I worry that the change is a bit more work. It would require an error resilient `RuleSelector` for deserialization. 

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-11-18 11:40_

---

_Label `do-not-merge` removed by @MichaReiser on 2024-11-19 08:45_

---

_Merged by @MichaReiser on 2024-11-19 09:00_

---

_Closed by @MichaReiser on 2024-11-19 09:00_

---

_Branch deleted on 2024-11-19 09:00_

---
