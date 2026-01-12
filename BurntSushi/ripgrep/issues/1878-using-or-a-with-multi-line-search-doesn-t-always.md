```yaml
number: 1878
title: "using '^' (or '\\A') with multi-line search doesn't always produce expected result"
type: issue
state: closed
author: BurntSushi
labels: []
assignees: []
created_at: 2021-05-29T11:27:39Z
updated_at: 2021-05-29T11:38:01Z
url: https://github.com/BurntSushi/ripgrep/issues/1878
synced_at: 2026-01-12T16:13:24Z
```

# using '^' (or '\A') with multi-line search doesn't always produce expected result

---

_@BurntSushi_

Initially found on HN: https://news.ycombinator.com/item?id=27324265

Specifically, while this output is correct (since `^` is set to be in regex multi-line mode always):

```
$ printf 'a\nbaz\nabc\n' | rg -U '^b'
baz
```

It should be the case that using `(?-m)^b` or `\Ab` would _not_ print `baz` as a match. But that's not the case here:

```
$ printf 'a\nbaz\nabc\n' | rg -U '(?-m)^b'
baz
$ printf 'a\nbaz\nabc\n' | rg -U '\Ab'
baz
```

The issue here is that in this case, ripgrep isn't memory mapping the input. In that case, ripgrep tries to be "smart" and not actually read the entire contents on to the heap if it knows the pattern can't match through a line terminator. But in this case, we can't quite make that assumption since anchors can match line terminators as look-around.

---

_Closed by @BurntSushi on 2021-05-29 11:38_

---
