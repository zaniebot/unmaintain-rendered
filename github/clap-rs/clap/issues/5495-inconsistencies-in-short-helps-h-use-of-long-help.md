---
number: 5495
title: "Inconsistencies in short helps (`-h`) use of long help"
type: issue
state: open
author: epage
labels:
  - S-waiting-on-decision
  - A-help
  - M-minor-incompat
assignees: []
created_at: 2024-05-14T19:40:57Z
updated_at: 2024-05-14T19:41:13Z
url: https://github.com/clap-rs/clap/issues/5495
synced_at: 2026-01-07T13:12:20-06:00
---

# Inconsistencies in short helps (`-h`) use of long help

---

_Issue opened by @epage on 2024-05-14 19:40_

From #5491

| location | short | long | notes |
|-------------|----------|--------|----------|
| before help | `before_help` | `before_long_help` or `before_help` | added in #2008, changed in #2174 |
| command about | `about` | `long_about` or `about` | changed in #1612 |
| arg help | `help` or `long_help` | `long_help` or `help` | |
| subcommand about |  `about` or `long_about` | `about` or `long_about` | changed in #2558 |
| after help | `after_help` | `after_long_help` or `after_help` | added in #2008, changed in #2174 |


The inconsistency in "command about"'s short and "subcommand about" is suspicious

Likely the fallback to long help isn't worth it and dropping it could reduce some overhead.  We'd still need the long help to short help fallback.

---

_Label `S-waiting-on-decision` added by @epage on 2024-05-14 19:41_

---

_Label `A-help` added by @epage on 2024-05-14 19:41_

---

_Label `M-minor-incompat` added by @epage on 2024-05-14 19:41_

---
