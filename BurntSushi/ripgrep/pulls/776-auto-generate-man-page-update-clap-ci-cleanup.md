```yaml
number: 776
title: auto generate man page, update clap, CI cleanup
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/misc-improvements
created_at: 2018-02-06T16:43:53Z
updated_at: 2018-02-06T17:08:04Z
url: https://github.com/BurntSushi/ripgrep/pull/776
synced_at: 2026-01-12T18:23:13Z
```

# auto generate man page, update clap, CI cleanup

---

_@BurntSushi_

This PR improves the quality of life for ripgrep maintainers and includes a tweak to the `-M/--max-columns` flag to interpret `0` as if the flag were omitted.

Quality of life improvements:

* ripgrep's man page is now automatically generated on every build, assuming you have `asciidoc` installed. :tada:
* Clean up the CI scripts. There was a lot of cruft and incorrect comments.
* Update clap to `2.29.4` and remove the previous work-around for permitting self-overriding flags and use official clap support for it instead.

---

_Merged by @BurntSushi on 2018-02-06 17:08_

---

_Closed by @BurntSushi on 2018-02-06 17:08_

---

_Branch deleted on 2018-02-06 17:08_

---
