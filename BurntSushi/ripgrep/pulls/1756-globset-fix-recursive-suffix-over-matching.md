```yaml
number: 1756
title: "globset: fix recursive suffix over matching"
type: pull_request
state: closed
author: zertosh
labels:
  - rollup
assignees: []
base: master
head: fix_recursive_suffix
created_at: 2020-12-08T05:03:20Z
updated_at: 2021-07-10T10:20:37Z
url: https://github.com/BurntSushi/ripgrep/pull/1756
synced_at: 2026-01-12T18:23:14Z
```

# globset: fix recursive suffix over matching

---

_@zertosh_

The syntax documentation correctly states how recursive suffixes should
behave:

> `**` recursively matches directories [..] `foo/**` matches `foo/a` and
> `foo/a/b`, but not `foo`.

Previously `**` would actually match `foo`. This patch makes it so that
it doesn't. Also adds support to the "prefix" strategic matcher so that
it handles common cases like `foo/**`.


---

_@BurntSushi approved on 2021-05-31 00:20_

Makes sense to me, thank you!

---

_Label `rollup` added by @BurntSushi on 2021-05-31 00:22_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---

_Branch deleted on 2021-06-01 01:55_

---

_Comment by @passcod on 2021-07-10 08:13_

Even though I agree this is a fix as per documentation, perhaps it should have been a semver bump? It changed behaviour (and broke my tests)...

---

_Comment by @BurntSushi on 2021-07-10 10:20_

Perhaps. But in this case, this is bringing the implementation in line with documented behavior.

---
