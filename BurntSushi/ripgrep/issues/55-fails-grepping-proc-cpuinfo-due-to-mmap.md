```yaml
number: 55
title: Fails grepping /proc/cpuinfo due to mmap
type: issue
state: closed
author: lespea
labels: []
assignees: []
created_at: 2016-09-24T06:37:31Z
updated_at: 2016-09-25T00:45:10Z
url: https://github.com/BurntSushi/ripgrep/issues/55
synced_at: 2026-01-12T18:23:11Z
```

# Fails grepping /proc/cpuinfo due to mmap

---

_@lespea_

Not sure if this is expected behavior or not.  Using 0.1.17.

```
rg -i mhz /proc/cpuinfo --debug
DEBUG:rg::args: will try to use memory maps
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'm',
        'h',
        'z'
    ],
    casei: true
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(MHZ), Complete(mHZ), Complete(MhZ), Complete(mhZ), Complete(MHz), Complete(mHz), Complete(Mhz), Complete(mhz)], limit_size: 250, limit_class: 10 }
FAIL: 1
```

```
rg -i mhz /proc/cpuinfo --no-mmap
8:cpu MHz       : 1284.832
35:cpu MHz      : 1216.351
62:cpu MHz      : 1200.036
89:cpu MHz      : 1199.835
116:cpu MHz     : 1200.238
143:cpu MHz     : 1199.835
170:cpu MHz     : 1205.474
197:cpu MHz     : 1228.033
224:cpu MHz     : 1267.712
251:cpu MHz     : 1199.835
278:cpu MHz     : 1199.835
305:cpu MHz     : 1209.301
```

```
grep -i mhz /proc/cpuinfo
cpu MHz     : 1199.835
cpu MHz     : 1199.835
cpu MHz     : 1199.835
cpu MHz     : 1202.252
cpu MHz     : 1199.633
cpu MHz     : 1199.835
cpu MHz     : 1219.573
cpu MHz     : 1200.036
cpu MHz     : 1199.633
cpu MHz     : 1213.128
cpu MHz     : 1199.835
cpu MHz     : 1217.358
```


---

_Comment by @BurntSushi on 2016-09-25 00:41_

This is an interesting one. `/proc/cpuinfo` advertises itself as a regular empty file, but it appears that it cannot be memory mapped. `rg` will try to use memory maps for single file searches, but since `/proc/cpuinfo` is an empty file, it reports no matches.

If you pass `--no-mmap` to `rg`, then searching works.

(`ag` has the same failure mode.)


---

_Comment by @BurntSushi on 2016-09-25 00:43_

Duh. I know. If a file has size `0`, then just fall back to regular `read` calls.


---

_Closed by @BurntSushi on 2016-09-25 00:45_

---
