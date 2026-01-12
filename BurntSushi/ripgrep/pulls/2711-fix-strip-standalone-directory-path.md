```yaml
number: 2711
title: "Fix: strip standalone \".\" directory path"
type: pull_request
state: closed
author: taeruh
labels:
  - rollup
assignees: []
base: master
head: master
created_at: 2024-01-11T02:50:58Z
updated_at: 2025-09-20T01:08:23Z
url: https://github.com/BurntSushi/ripgrep/pull/2711
synced_at: 2026-01-12T18:23:14Z
```

# Fix: strip standalone "." directory path

---

_@taeruh_

The problem is that in Ignore::matched, the perfix "./" is stripped from files. In Ignore::matched_ignore, the directory path is then stripped from file paths. Now imagine we have a hidden file "./.foo" and pass "." as the search path. "./.foo" gets first stripped to ".foo" and then it would have been stripped to "foo".

I am not completely sure though whether this is the best way to fix it. One could also check earlier whether the directory path is "." and then change it to "./". I don't know which is the best way regarding performance; the current fix is just at the place where it breaks. What do you think?

The implications of this problem (at least the ones I noticed) are that when grepping (including hidden files and passing `.` as search directory) in a subdirectory `a/b` of `a`, and `b` has a hidden file `.foo`, which is ignored in a ignore file in `a`, the contents of `.foo` are shown.
I put in https://github.com/taeruh/debug_file_paths a minimal debugging example to show that.

Issue #829 is similar, but I am not sure about how much it is related.

---

_Label `rollup` added by @BurntSushi on 2025-07-11 17:03_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
