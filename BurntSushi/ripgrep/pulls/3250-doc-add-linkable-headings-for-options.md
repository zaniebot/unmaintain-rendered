```yaml
number: 3250
title: "doc: add linkable headings for options"
type: pull_request
state: open
author: kanata9819
labels: []
assignees: []
base: master
head: add-linkable-headings
created_at: 2025-12-24T13:55:54Z
updated_at: 2025-12-24T13:55:54Z
url: https://github.com/BurntSushi/ripgrep/pull/3250
synced_at: 2026-01-12T18:23:15Z
```

# doc: add linkable headings for options

---

_@kanata9819_

This adds linkable headings for the option list in the generated man page.
Most options already work, but `--hidden` uses `.` as its short form (`-.`),
which prevents some man-to-HTML renderers from generating a stable anchor.
This change prefers the long option as the tag when the short option is not
ASCII alphanumeric, while preserving the usual short/long ordering for normal flags.

This addresses the request in #3243 to make option headings linkable.


---
