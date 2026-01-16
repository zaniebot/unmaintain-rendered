```yaml
number: 17501
title: "Add `-t` shortform for `--target` to `uv pip` subcommands"
type: pull_request
state: open
author: fifty-six
labels:
  - compatibility
assignees: []
base: main
head: shortform-t
created_at: 2026-01-15T21:36:12Z
updated_at: 2026-01-15T23:11:19Z
url: https://github.com/astral-sh/uv/pull/17501
synced_at: 2026-01-16T00:03:20Z
```

# Add `-t` shortform for `--target` to `uv pip` subcommands

---

_@fifty-six_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This adds `-t` to `uv pip` to preserve drop-in compatibility with pip's `-t` shortform

Closes #17495

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Just interactively checked to make sure it works in the same places as `--test`
```bash
uv pip install -t test ansible
uv pip list -t test
echo 'ansible' > requirements.txt
uv pip sync -t test requirements.txt
```

---

_@zanieb approved on 2026-01-15 23:11_

cc @charliermarsh 

---

_Label `compatibility` added by @zanieb on 2026-01-15 23:11_

---
