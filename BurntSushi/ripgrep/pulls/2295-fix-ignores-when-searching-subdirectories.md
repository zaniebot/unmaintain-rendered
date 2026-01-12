```yaml
number: 2295
title: Fix ignores when searching subdirectories
type: pull_request
state: closed
author: sengaya
labels:
  - rollup
assignees: []
base: master
head: fix-1757
created_at: 2022-08-29T20:39:28Z
updated_at: 2023-09-20T15:52:45Z
url: https://github.com/BurntSushi/ripgrep/pull/2295
synced_at: 2026-01-12T18:23:14Z
```

# Fix ignores when searching subdirectories

---

_@sengaya_

When searching subdirectories the path was not correctly build and
included duplicate parts. This fix will remove the duplicate part if
possible. It also contains a small optimization that will skip the
second for loop if there is already a match.

Fixes #1757

---

_Label `rollup` added by @BurntSushi on 2023-09-18 15:46_

---

_@BurntSushi reviewed on 2023-09-18 15:50_

Nice, thank you! This fixes quite a gnarly bug that I've seen a lot of folks run into for a while. Thank you.

I ended up fixing two problems with this patch though:

1. First, the optimization does not look correct to me. The issue is that, for some ignore files, the existence of any rule regardless of where it is in the directory tree will override any other rule from a different type of ignore file. For example, `.rgignore` has higher precedent than `.ignore`. So you need to let all of the rules be visited. In practice, I doubt this optimization matters too much anyway.
2. This used `Path::strip_prefix` instead of ripgrep's own `strip_prefix` routine which does things a little differently. This meant that giving ripgrep a path like `.////path` would still trigger the bug. But using ripgrep's `strip_prefix` routine avoids this issue.

Overall this still feels Not Right to me, but it fixes a bug and doesn't seem to cause any other regressions. Hopefully we can straighten out this logic in a better way in the future when I overhaul the `ignore` crate.

---

_Closed by @BurntSushi on 2023-09-20 15:52_

---
