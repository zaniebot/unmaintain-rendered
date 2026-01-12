```yaml
number: 5817
title: "fix(help): Display value terminator and delimiter in help"
type: pull_request
state: open
author: Aethelflaed
labels: []
assignees: []
draft: true
base: master
head: display-value-delimiter-and-terminator-in-help
created_at: 2024-11-14T15:39:35Z
updated_at: 2025-05-31T13:52:20Z
url: https://github.com/clap-rs/clap/pull/5817
synced_at: 2026-01-12T16:14:17Z
```

# fix(help): Display value terminator and delimiter in help

---

_@Aethelflaed_

Display value terminator and delimiter in the command usage and help.

I added explicitly `[value delimiter: ':']` and `[value terminator: ";"]` to the help message.

Closes https://github.com/clap-rs/clap/issues/5392 and https://github.com/clap-rs/clap/issues/4812.

Related to https://github.com/clap-rs/clap/issues/1052


---
