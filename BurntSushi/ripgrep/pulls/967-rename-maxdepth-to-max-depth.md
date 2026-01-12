```yaml
number: 967
title: Rename --maxdepth to --max-depth
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: dana/maxdepth
created_at: 2018-06-26T00:02:33Z
updated_at: 2018-06-26T00:22:10Z
url: https://github.com/BurntSushi/ripgrep/pull/967
synced_at: 2026-01-12T18:23:13Z
```

# Rename --maxdepth to --max-depth

---

_@okdana_

The name of the `--maxdepth` option is inconsistent with all of the other `--max-*` options, and aside from being aesthetically displeasing it affects the ordering in the help output and completion results. I assume the wording `maxdepth` comes from `find`, and that makes sense on its own i guess, but... it just feels out of place to me here.

This PR changes the canonical name of the option to `max-depth`, leaving `maxdepth` as a hidden alias for backwards compatibility.

(I realise this is petty; feel free to just close if i'm being *too* ridiculous...)

---

_@BurntSushi approved on 2018-06-26 00:05_

Hah. I like it. You're right about its provenance; I wanted to match `find`. But you're also right about everything else too. :+1:

---

_Merged by @BurntSushi on 2018-06-26 00:22_

---

_Closed by @BurntSushi on 2018-06-26 00:22_

---
