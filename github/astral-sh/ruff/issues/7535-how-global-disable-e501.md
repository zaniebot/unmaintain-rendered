---
number: 7535
title: How global disable E501?
type: issue
state: closed
author: daeeros
labels:
  - question
assignees: []
created_at: 2023-09-20T08:11:44Z
updated_at: 2023-09-20T13:05:42Z
url: https://github.com/astral-sh/ruff/issues/7535
synced_at: 2026-01-07T13:12:15-06:00
---

# How global disable E501?

---

_Issue opened by @daeeros on 2023-09-20 08:11_

Hello, I am using the extension for Vs Code, how can I disable the E501 error completely? Not in a specific file but globally in all files.

---

_Comment by @charliermarsh on 2023-09-20 13:05_

Hi!

My recommendation would be to create a `ruff.toml` file at `/Users/Alice/Library/Application Support/ruff/ruff.toml` on macOS or `/home/alice/.config/ruff/ruff.toml` on Linux with contents like:

```toml
ignore = ["E501"]
```

(See: "How can I change Ruff's default configuration?" in the [FAQ](https://docs.astral.sh/ruff/faq/#how-can-i-change-ruffs-default-configuration).)

Ruff will pick up that configuration whenever it runs, from the command line or VS Code.

Alternatively, you can set `--ignore=E501` in the VS Code extension settings itself:

<img width="398" alt="Screen Shot 2023-09-20 at 9 05 27 AM" src="https://github.com/astral-sh/ruff/assets/1309177/659b05d1-88a1-473b-a285-19ea71f97fd0">

(This will apply to the VS Code extension, but won't apply if you run Ruff from the command line.)


---

_Closed by @charliermarsh on 2023-09-20 13:05_

---

_Label `question` added by @charliermarsh on 2023-09-20 13:05_

---
