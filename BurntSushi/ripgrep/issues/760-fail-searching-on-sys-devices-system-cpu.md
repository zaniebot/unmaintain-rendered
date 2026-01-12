```yaml
number: 760
title: "Fail searching on /sys/devices/system/cpu/vulnerabilities/*"
type: issue
state: closed
author: drrlvn
labels:
  - bug
assignees: []
created_at: 2018-01-25T12:27:19Z
updated_at: 2018-02-01T00:20:49Z
url: https://github.com/BurntSushi/ripgrep/issues/760
synced_at: 2026-01-12T16:13:22Z
```

# Fail searching on /sys/devices/system/cpu/vulnerabilities/*

---

_@drrlvn_

These files were recently added on Linux but it seems they cannot be `mmap()`ed. I'm getting the following error:

```
$ rg . /sys/devices/system/cpu/vulnerabilities/*
No such device (os error 19)
No such device (os error 19)
No such device (os error 19)
```
Disabling `mmap()` works:
```
$ rg --no-mmap . /sys/devices/system/cpu/vulnerabilities/*
/sys/devices/system/cpu/vulnerabilities/spectre_v1
Vulnerable

/sys/devices/system/cpu/vulnerabilities/spectre_v2
Mitigation: Full generic retpoline

/sys/devices/system/cpu/vulnerabilities/meltdown
Mitigation: PTI
```

Weirdly, this works (running on the directory instead of all the files, omitting `*`):
```
$ rg . /sys/devices/system/cpu/vulnerabilities/
/sys/devices/system/cpu/vulnerabilities/spectre_v1
Vulnerable

/sys/devices/system/cpu/vulnerabilities/spectre_v2
Mitigation: Full generic retpoline

/sys/devices/system/cpu/vulnerabilities/meltdown
Mitigation: PTI
```

---

_Comment by @BurntSushi on 2018-01-25 12:37_

Thanks for the report! This is definitely because of memory mapping. The reason why you're seeing inconsistencies is because memory mapping is enabled by default using heuristics. e.g., If you search a directory, it's always disabled. But if you search a small number of single files, then it's generally enabled.

There is indeed a special case where memory mapping these sorts of files doesn't work. Currently, [ripgrep detects this by asking the file size](https://github.com/BurntSushi/ripgrep/blob/35f802166d1c5788e8c833f398d13b9c9e8cb360/src/worker.rs#L285-L292). If the size is 0, then it always uses standard `read` calls instead of memory maps. Interestingly, it looks like these files do not report a size of `0`:

```
$ stat /sys/devices/system/cpu/vulnerabilities/meltdown 
  File: /sys/devices/system/cpu/vulnerabilities/meltdown
  Size: 4096            Blocks: 0          IO Block: 4096   regular file
Device: 13h/19d Inode: 57          Links: 1
Access: (0444/-r--r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2018-01-25 07:29:47.883246649 -0500
Modify: 2018-01-25 07:29:47.883246649 -0500
Change: 2018-01-25 07:29:47.883246649 -0500
 Birth: -
```

I confess that my file size trick was not informed by what the OS purports to guarantee, but rather, it was informed by observed behavior. It looks like it doesn't work in this case.

I suppose the fix to this is to look at the error returned by opening a memory map, and if it's a "no such device" error, then perhaps we should fallback to normal `read` calls.

---

_Label `bug` added by @BurntSushi on 2018-01-25 12:37_

---

_Closed by @BurntSushi on 2018-02-01 00:20_

---
