```yaml
number: 15924
title: support isolated PEP 723 scripts
type: pull_request
state: open
author: mohammedgqudah
labels:
  - enhancement
assignees: []
base: main
head: isolated-scripts
created_at: 2025-09-18T02:14:58Z
updated_at: 2025-12-13T15:46:07Z
url: https://github.com/astral-sh/uv/pull/15924
synced_at: 2026-01-10T05:49:14Z
```

# support isolated PEP 723 scripts

---

_Pull request opened by @mohammedgqudah on 2025-09-18 02:14_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Currently, `--isolated` with `--script` is a no-op, this PR adds support for isolated environments for PEP 723 scripts.

Closes #15919 and #15918

## Test Plan

1. create a pep723 script with two dependencies
2. run the script **without** `--isolated`, and make sure both packages can be imported
3. edit the script by removing the second dependency
4. run again with `--isolated`, it should fail because this new environment only has the first dependency. if it didn't fail then it's not using an isolated environment

## Notes to reviewers
- I don't really like passing the isolated flag to `ScriptInterpreter::discover`, I could remove that but then I would have to `PythonInstallation::find_or_download` and `validate_script_requires_python()` in `project/run.rs`. If you think that's fine - duplicating the requires_python logic - then I don't mind rewriting this part.
- ~~I needed to change the script lockfile logic by making sure it ignores the lockfile if `--isolated` was passed, but because of MSRV compatibility I couldn't use `!isolated && let` since it's unstable so I had to use a match statement, which explains the ugly git diff [9969352](https://github.com/astral-sh/uv/pull/15924/commits/996935224a6a89ddce69483c8f1667c94186b286)~~

---

_Renamed from "wip - support isolated PEP 723 scripts" to "support isolated PEP 723 scripts" by @mohammedgqudah on 2025-09-18 04:43_

---

_Marked ready for review by @mohammedgqudah on 2025-09-18 04:44_

---

_Label `enhancement` added by @konstin on 2025-09-18 07:44_

---

_Comment by @samypr100 on 2025-09-23 23:21_

>but because of MSRV compatibility I couldn't use !isolated && let since it's unstable so I had to use a match statement

@mohammedgqudah you can use let chains now

---

_Comment by @mohammedgqudah on 2025-09-24 00:39_

> @mohammedgqudah you can use let chains now

@samypr100 Thanks for the heads up, I've updated the PR


---

_Assigned to @zanieb by @zanieb on 2025-12-13 15:46_

---
