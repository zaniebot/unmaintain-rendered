```yaml
number: 988
title: "ignore: fix has_any_ignore_rules for explicit ignores"
type: pull_request
state: merged
author: phiresky
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2018-07-21T10:41:25Z
updated_at: 2018-07-21T17:26:55Z
url: https://github.com/BurntSushi/ripgrep/pull/988
synced_at: 2026-01-12T18:23:13Z
```

# ignore: fix has_any_ignore_rules for explicit ignores

---

_@phiresky_

When building a ignore::WalkBuilder by disabling all standard filters
and adding a custom global ignore file, the ignore file is not used. Example:

    let mut walker = ignore::WalkBuilder::new(dir);
    walker.standard_filters(false);
    walker.add_ignore(myfile);

This makes it impossible to use the ignore crate to walk a directory
with only custom ignore files. Very similar to issue #800 (fixed in
b71a110).

---

_@BurntSushi approved on 2018-07-21 17:26_

Nice find and fix!

---

_Merged by @BurntSushi on 2018-07-21 17:26_

---

_Closed by @BurntSushi on 2018-07-21 17:26_

---
