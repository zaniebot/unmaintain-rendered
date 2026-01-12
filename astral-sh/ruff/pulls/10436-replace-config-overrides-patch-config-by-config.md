```yaml
number: 10436
title: replace config_overrides.patch_config by config_overrides.to_ruff_args
type: pull_request
state: closed
author: hoel-bagard
labels: []
assignees: []
draft: true
base: main
head: hoel/use_config_in_ecosystem_checks
created_at: 2024-03-17T12:22:46Z
updated_at: 2024-12-30T09:07:34Z
url: https://github.com/astral-sh/ruff/pull/10436
synced_at: 2026-01-12T15:55:32Z
```

# replace config_overrides.patch_config by config_overrides.to_ruff_args

---

_@hoel-bagard_

## Summary

Closes #10345

This PR removes the manual patch of the config by the overrides when doing the ecosystem checks, and instead uses the
`--config` option.

## Test Plan

CI ?

---

_Comment by @github-actions[bot] on 2024-03-17 12:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/b61d90c6f5dc4600055916f98c0e63deec0546e9/stdlib/subprocess.pyi#L134'>stdlib/subprocess.pyi:134:1:</a> E999 SyntaxError: unexpected EOF while parsing
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E999 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @zanieb on 2024-04-12 20:06_

Sorry I lost track of this. Is it ready for review?

---

_Assigned to @zanieb by @zanieb on 2024-04-12 20:06_

---

_Comment by @hoel-bagard on 2024-04-13 01:16_

It's not ready for review yet.

I'll look again for a way to unset keys. Sorry to have let this PR/issue take so long.

---

_Comment by @hoel-bagard on 2024-04-13 09:53_

@zanieb I've tried to just set the value of the keys to unset to an empty string, but I don't know how to check whether this works or not.

When I run the `ruff-ecosystem check` locally I get  the same error for all the repos, but the CI is passing. It doesn't seem to be related to the config override, but I could be wrong (could it be linked to the `required-version` not being overwritten ?). I'm adding it below in case you know why it appears.
```
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
...
error: invalid value 'RUF9' for '--ignore <RULE_CODE>'
```

---

_@hoel-bagard reviewed on 2024-04-14 14:29_

---

_Review comment by @hoel-bagard on `python/ruff-ecosystem/ruff_ecosystem/projects.py`:95 on 2024-04-14 14:29_

@AlexWaygood Here I'm trying to unset / restore to default some values.

This is the part of the PR I'm the most unsure about, do you know if that's the correct way to do it ?

---

_Marked ready for review by @hoel-bagard on 2024-04-14 14:29_

---

_Converted to draft by @MichaReiser on 2024-04-15 06:51_

---

_Comment by @MichaReiser on 2024-12-30 09:07_

I'll close this because there was no activity in the last 8 months. 

---

_Closed by @MichaReiser on 2024-12-30 09:07_

---
