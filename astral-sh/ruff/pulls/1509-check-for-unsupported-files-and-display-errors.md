```yaml
number: 1509
title: Check for Unsupported Files and Display Errors and Warnings
type: pull_request
state: merged
author: saadmk11
labels: []
assignees: []
merged: true
base: main
head: check-unsupported-files
created_at: 2022-12-31T11:29:09Z
updated_at: 2023-01-01T23:40:58Z
url: https://github.com/astral-sh/ruff/pull/1509
synced_at: 2026-01-12T05:36:31Z
```

# Check for Unsupported Files and Display Errors and Warnings

---

_Pull request opened by @saadmk11 on 2022-12-31 11:29_

closes https://github.com/charliermarsh/ruff/issues/1473

**Reference:**

> - (Optional) If we don't find _any_ files, show a warning.
> - If we're passed a non `.py` or `.pyi` file, raise an error for that file (but process any valid files).

_Originally posted by @charliermarsh in https://github.com/charliermarsh/ruff/issues/1473#issuecomment-1368082842_
      

---

_Comment by @charliermarsh on 2022-12-31 13:12_

Thank you! I decided to make the "unsupported file type" thing a hard error. I may regret it, we'll see!

---

_Merged by @charliermarsh on 2022-12-31 13:12_

---

_Closed by @charliermarsh on 2022-12-31 13:12_

---

_Branch deleted on 2022-12-31 13:14_

---

_Comment by @andersk on 2023-01-01 23:22_

Zulip has many Python scripts that are marked executable with a shebang line but without a `.py` extension. I think this should be reverted.

---

_Comment by @charliermarsh on 2023-01-01 23:31_

Do you want / expect those to be linted? If so, how have you linted them in the past, given that we only check `.py` and `.pyi` files?

---

_Comment by @charliermarsh on 2023-01-01 23:33_

We can change this to just attempt to lint them, which would raise a syntax error for any non-Python files that are passed directly.

---

_Comment by @andersk on 2023-01-01 23:34_

We linted all files passed directly on the command line, regardless of extension, until pretty recently (I’m checking where this changed).

---

_Comment by @charliermarsh on 2023-01-01 23:35_

Okay, I'm happy to revert to that behavior.

---

_Comment by @charliermarsh on 2023-01-01 23:36_

(I guess my question was: are you passing those directly on the command line?)

---

_Comment by @andersk on 2023-01-01 23:36_

Yes, Zulip has a wrapper script that finds all the Python files in the Git repository via extensions and shebang lines, and passes them directly on the command line.

---

_Comment by @charliermarsh on 2023-01-01 23:37_

Ok, I can fix this later tonight. Or feel free to send a PR of course. Sorry for the trouble.

---

_Comment by @andersk on 2023-01-01 23:39_

`git bisect` shows this changed in #1241.

(The problem isn’t urgent for Zulip, since Zulip uses a pinned version of Ruff.)

---

_Comment by @charliermarsh on 2023-01-01 23:40_

Tracking here: #1541

---
