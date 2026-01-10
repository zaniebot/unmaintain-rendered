```yaml
number: 651
title: Precommit hook not catching the latest version
type: issue
state: closed
author: jiwidi
labels: []
assignees: []
created_at: 2022-11-07T21:40:08Z
updated_at: 2022-11-07T21:56:59Z
url: https://github.com/astral-sh/ruff/issues/651
synced_at: 2026-01-10T15:56:05Z
```

# Precommit hook not catching the latest version

---

_Issue opened by @jiwidi on 2022-11-07 21:40_

Hi!

precommit hook is missing the latest versions. When i use

```yaml
  - repo: https://github.com/charliermarsh/ruff-pre-commit
    rev: v0.0.106
    hooks:
      - id: ruff
        name: ruff (python)
```

I get:
```bash
[INFO] Initializing environment for https://github.com/charliermarsh/ruff-pre-commit.
An unexpected error has occurred: CalledProcessError: command: ('/usr/bin/git', 'checkout', 'v0.0.106')
return code: 1
expected return code: 0
stdout: (none)
stderr:
    error: pathspec 'v0.0.106' did not match any file(s) known to git

Check the log at /Users/jiwidi/.cache/pre-commit/pre-commit.log
```

I see you have this exact version in git so not exactly sure what is happening. Tried cleaning the cache for pre-commit but still not working.

It does work with 0.99 (the previous version i used)


---

_Comment by @charliermarsh on 2022-11-07 21:54_

I think the issue is that the GitHub Action to publish newer versions doesn't run automatically, it's on a once-a-day cron job. So there's often a delay between publishing a new Ruff version and publishing a new pre-commit version.

I should just run it automatically whenever I publish, and/or wait to bump the version in the README until a new version is published...

(I just kicked it off, so v0.0.106 should be published shortly.)


---

_Comment by @charliermarsh on 2022-11-07 21:56_

(v0.0.106 is up now.)

---

_Comment by @jiwidi on 2022-11-07 21:56_

Great! thanks for the swift response and fix!

---

_Closed by @jiwidi on 2022-11-07 21:56_

---
