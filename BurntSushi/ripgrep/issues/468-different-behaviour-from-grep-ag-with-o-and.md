```yaml
number: 468
title: Different behaviour from grep/ag with -o and multiple matches
type: issue
state: closed
author: passcod
labels:
  - duplicate
assignees: []
created_at: 2017-04-29T01:24:31Z
updated_at: 2017-04-29T01:54:37Z
url: https://github.com/BurntSushi/ripgrep/issues/468
synced_at: 2026-01-12T16:13:22Z
```

# Different behaviour from grep/ag with -o and multiple matches

---

_@passcod_

With grep:

```
$ echo '50k + 1k + 11k + 37k + 18k' | grep -Po '[\d.]+'
50
1
11
37
18
```

With The Silver Searcher:

```
$ echo '50k + 1k + 11k + 37k + 18k' | ag -o '[\d.]+'
50
1
11
37
18
```

With ripgrep:

```
$ echo '50k + 1k + 11k + 37k + 18k' | rg -o '[\d.]+'
50
50
50
50
50
```

```
$ rg -V
ripgrep 0.5.1
```

---

_Comment by @BurntSushi on 2017-04-29 01:36_

I think this is a dupe of #451, which is fixed on master.

---

_Label `duplicate` added by @BurntSushi on 2017-04-29 01:36_

---

_Closed by @passcod on 2017-04-29 01:54_

---
