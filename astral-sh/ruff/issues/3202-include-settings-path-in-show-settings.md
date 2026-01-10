```yaml
number: 3202
title: "Include settings path in `--show-settings`"
type: issue
state: closed
author: charliermarsh
labels:
  - cli
assignees: []
created_at: 2023-02-24T05:07:15Z
updated_at: 2023-05-04T06:22:32Z
url: https://github.com/astral-sh/ruff/issues/3202
synced_at: 2026-01-10T11:09:46Z
```

# Include settings path in `--show-settings`

---

_Issue opened by @charliermarsh on 2023-02-24 05:07_

We should tell you _which_ settings file was loaded on-disk.

---

_Label `cli` added by @charliermarsh on 2023-02-24 05:07_

---

_Comment by @charliermarsh on 2023-02-24 05:07_

This is surprisingly difficult (or, at least, requires some refactoring).

---

_Comment by @dhruvmanila on 2023-04-10 18:38_

I think I can take this up tomorrow, have been looking at the CLI code recently.

One doubt is what to output when ran in `--isolated` mode or using the default settings as a fallback. Maybe don't include the path or leave it blank like `"-"`:

https://github.com/charliermarsh/ruff/blob/333f1bd9ceff6390ae652790abb22cd175042bc7/crates/ruff_cli/src/resolve.rs#L65-L72

The output I'm thinking of:

```console
$ ruff check --show-settings .
Resolved settings for: "..."
Settings path: "..."
Settings {
    rules: RuleTable {
        enabled: {
			...
```

---

_Closed by @MichaReiser on 2023-05-04 06:22_

---
