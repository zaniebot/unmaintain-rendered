```yaml
number: 498
title: Use uppercase for required argument names
type: pull_request
state: merged
author: ericbn
labels: []
assignees: []
merged: true
base: master
head: uppercase
created_at: 2017-06-01T23:08:45Z
updated_at: 2017-06-02T01:20:13Z
url: https://github.com/BurntSushi/ripgrep/pull/498
synced_at: 2026-01-12T18:23:13Z
```

# Use uppercase for required argument names

---

_@ericbn_

This reverts a couple of changes introduced in 4c78ca8

@BurntSushi, not sure if you want to merge this. Just trying to revert some changes I introduced, plus keeping the `PATTERN` argument consistently uppercased, so error messages can look like

```
error: The following required arguments were not provided:
    <PATTERN>
```

---

_Merged by @BurntSushi on 2017-06-02 00:41_

---

_Closed by @BurntSushi on 2017-06-02 00:41_

---

_Comment by @BurntSushi on 2017-06-02 00:41_

LGTM. Thanks. :-) Consistency is good!

---

_Branch deleted on 2017-06-02 01:20_

---
