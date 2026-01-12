```yaml
number: 728
title: "Documentation updates (path operand, &c.)"
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: dana/document-path-override
created_at: 2018-01-01T22:58:26Z
updated_at: 2018-01-11T13:05:53Z
url: https://github.com/BurntSushi/ripgrep/pull/728
synced_at: 2026-01-12T18:23:13Z
```

# Documentation updates (path operand, &c.)

---

_@okdana_

Fixes #725 (i guess?)

* I capitalised the place-holders `options` and `path` to make them consistent with `PATTERN` &al.

* I added documentation of the positional arguments to the man page

* I added a note to both the man page and the usage help to clarify that an explicit `PATH` overrides glob/ignore rules. This could possibly be clearer i guess — a glob/ignore rule is only overridden if the path as given on the command line contains a segment that actually matches it — but it didn't seem worth writing a whole paragraph about

* I'm not sure when this became an issue, but when i regenerated the man page all of the long option names were broken — Pandoc had converted `--foo` into `–foo` (with an en dash). I updated `convert-to-man` to disable 'smart typography', which seems to have fixed it for Pandoc 2.0.6; hopefully it doesn't cause issues for contributors on older versions

---

_Comment by @BurntSushi on 2018-01-01 23:12_

RE ci failures: My bet is on the capitalization of `path`.

---

_Comment by @okdana on 2018-01-01 23:16_

lol, oops. Should have done a more thorough test than just `rg --help` i guess.

Pushed a fix, it seems better now.

---

_Merged by @BurntSushi on 2018-01-11 13:05_

---

_Closed by @BurntSushi on 2018-01-11 13:05_

---
