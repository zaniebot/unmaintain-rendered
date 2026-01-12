```yaml
number: 2832
title: "`--null-data` seems to inhibit some optimizations"
type: issue
state: open
author: BurntSushi
labels:
  - enhancement
assignees: []
created_at: 2024-06-05T12:45:44Z
updated_at: 2024-06-05T12:45:48Z
url: https://github.com/BurntSushi/ripgrep/issues/2832
synced_at: 2026-01-12T16:13:25Z
```

# `--null-data` seems to inhibit some optimizations

---

_@BurntSushi_

To reproduce, first create a large binary file:

```
dd status=progress if=/dev/zero count=10240000 bs=1024 > ./binary-test.bin ; echo 'ZQZQZQZQZQ' >> ./binary-test.bin ; dd if=/dev/zero count=10240000 bs=1024 status=progress >> ./binary-test.bin
```

And then try to search it with `--null-data`:

```
$ time rg --null-data --no-mmap ZQZQZQZQZQ binary-test.bin > out.rg

real    2:00.24
user    1:57.95
sys     2.246
maxmem  19 MB
faults  0
```

It should not take that long. A quick peak at `perf top` while the above process was running reveals it is doing a line-by-line search, probably because of some faulty reasoning about the line terminator in this case. Since most of the file is just line terminators, this spends a lot of time iterating over lines.

I found this as part of #2831.

---

_Label `enhancement` added by @BurntSushi on 2024-06-05 12:45_

---
