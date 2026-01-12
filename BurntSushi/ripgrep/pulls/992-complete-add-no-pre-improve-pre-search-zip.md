```yaml
number: 992
title: "complete: add --no-pre, improve --pre/--search-zip exclusivity"
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: dana/complete-no-pre
created_at: 2018-07-24T00:28:35Z
updated_at: 2018-07-24T20:28:48Z
url: https://github.com/BurntSushi/ripgrep/pull/992
synced_at: 2026-01-12T18:23:13Z
```

# complete: add --no-pre, improve --pre/--search-zip exclusivity

---

_@okdana_

Complete `--no-pre` — and then break pre/zip options apart since not all of them should be exclusive.

(The CI script doesn't account for completion specs that are marked with a `!`, which is why the tests didn't break when `--no-pre` was added. I could change that, but i think my concern at the time was that those 'negatory' options wouldn't always show up in the usage help. idk, probably not a big deal either way)

(Edit for posterity: I realised that i explained the issue backwards before: The script didn't complain about it being added because it only checks the 'top-level' options in the usage help (the ones that start a new section). It ignores the ones that are in the body of a help section. The reason it doesn't complain about them being added *afterwards* is what i explained above.)

---

_@BurntSushi approved on 2018-07-24 11:21_

Ah thanks! I will figure out the syntax some day. :-)

---

_Merged by @BurntSushi on 2018-07-24 11:21_

---

_Closed by @BurntSushi on 2018-07-24 11:21_

---
