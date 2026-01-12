```yaml
number: 2802
title: Improve renovate config
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate-sync
created_at: 2024-04-03T11:17:17Z
updated_at: 2024-04-03T14:23:38Z
url: https://github.com/astral-sh/uv/pull/2802
synced_at: 2026-01-12T16:05:13Z
```

# Improve renovate config

---

_@AlexWaygood_

## Summary

A bunch of changes were made to Ruff's renovate config on Monday; this PR syncs uv's config to apply the same updates. Changes made:

- Give renovate more time in which to create PRs on Mondays (same as https://github.com/astral-sh/ruff/commit/2ea0c3dce6412990d522e262828a247c746d9e07)
- Allow renovate to create more than 2 PRs an hour (same as https://github.com/astral-sh/ruff/commit/8d547ef83a85a5cabe749a4cf09a41f931dc8116)
- Run prettier on the config file (done at Ruff in https://github.com/astral-sh/ruff/commit/877a9145ae7cd92a52a54f10b58d95366369b084)

## Test Plan

I validated the config using renovate's CLI tool:

```
(renovate-sync) % npx --yes --package renovate -- renovate-config-validator                                                               ~/dev/uv
(node:78418) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
(Use `node --trace-deprecation ...` to show where the warning was created)
 INFO: Validating .github/renovate.json5
 INFO: Config validated successfully
```


---

_Comment by @konstin on 2024-04-03 11:22_

We have a prettier ci job, can you add json5 to the pattern there?

---

_Comment by @AlexWaygood on 2024-04-03 11:23_

> We have a prettier ci job, can you add json5 to the pattern there?

good call

---

_Comment by @konstin on 2024-04-03 11:32_

Sorry, i meant https://github.com/astral-sh/uv/blob/4b2e67955fc355c69766101f60e182e88094b4da/.github/workflows/ci.yml#L34

---

_Comment by @AlexWaygood on 2024-04-03 11:34_

> Sorry, i meant
> 
> https://github.com/astral-sh/uv/blob/4b2e67955fc355c69766101f60e182e88094b4da/.github/workflows/ci.yml#L34

oh, sure. Do you want/mind the pre-commit change as well, or should I revert that?

---

_Comment by @konstin on 2024-04-03 11:39_

I don't use precommit so i don't know

---

_Comment by @AlexWaygood on 2024-04-03 11:44_

I verified locally that prettier-run-via-pre-commit formats both YAML and JSON5 files with this change, so I think we're good.

---

_Merged by @AlexWaygood on 2024-04-03 11:48_

---

_Closed by @AlexWaygood on 2024-04-03 11:48_

---

_Branch deleted on 2024-04-03 11:48_

---

_Label `internal` added by @zanieb on 2024-04-03 14:15_

---

_Comment by @zanieb on 2024-04-03 14:15_

@AlexWaygood please include the `internal` label or this ends up in the changelog.

---

_Comment by @AlexWaygood on 2024-04-03 14:23_

> @AlexWaygood please include the `internal` label or this ends up in the changelog.

Thanks for the reminder. Sorry I keep forgetting :(

---
