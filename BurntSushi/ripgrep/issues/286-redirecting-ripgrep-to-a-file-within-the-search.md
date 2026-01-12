```yaml
number: 286
title: Redirecting ripgrep to a file within the search directory will cause rg to also search that file
type: issue
state: closed
author: Ophirr33
labels:
  - bug
assignees: []
created_at: 2016-12-22T19:15:21Z
updated_at: 2017-01-09T21:12:10Z
url: https://github.com/BurntSushi/ripgrep/issues/286
synced_at: 2026-01-12T16:13:21Z
```

# Redirecting ripgrep to a file within the search directory will cause rg to also search that file

---

_@Ophirr33_

This leads to ripgrep searching its own output, taking a lot of time and eating up disk space. Here's a minimal example:
```
mkdir rg-test && cd rg-test
echo "hello" > found.txt
rg -e "hello" > tmp.txt
rg -e "hello" > tmp2.txt"
rg -e "hello" > tmp3.txt"
```

Then, running `rg -e "hello"` will output the following:
```
found.txt
1:hello

tmp.txt
1:found.txt:hello
2:tmp.txt:found.txt:hello

tmp2.txt
1:found.txt:hello
2:tmp.txt:found.txt:hello
3:tmp.txt:tmp.txt:found.txt:hello
4:tmp2.txt:found.txt:hello
5:tmp2.txt:tmp.txt:found.txt:hello
6:tmp2.txt:tmp.txt:tmp.txt:found.txt:hello 

tmp3.txt
1:found.txt:hello
2:tmp.txt:found.txt:hello
3:tmp.txt:tmp.txt:found.txt:hello
4:tmp2.txt:found.txt:hello
5:tmp2.txt:tmp.txt:found.txt:hello
6:tmp2.txt:tmp.txt:tmp.txt:found.txt:hello
7:tmp2.txt:tmp2.txt:found.txt:hello
8:tmp2.txt:tmp2.txt:tmp.txt:found.txt:hello
9:tmp2.txt:tmp2.txt:tmp.txt:tmp.txt:found.txt:hello
10:tmp3.txt:found.txt:hello
11:tmp3.txt:tmp.txt:found.txt:hello
12:tmp3.txt:tmp.txt:tmp.txt:found.txt:hello
13:tmp3.txt:tmp2.txt:found.txt:hello
14:tmp3.txt:tmp2.txt:tmp.txt:found.txt:hello
15:tmp3.txt:tmp2.txt:tmp.txt:tmp.txt:found.txt:hello
16:tmp3.txt:tmp2.txt:tmp2.txt:found.txt:hello
17:tmp3.txt:tmp2.txt:tmp2.txt:tmp.txt:found.txt:hello
18:tmp3.txt:tmp2.txt:tmp2.txt:tmp.txt:tmp.txt:found.txt:hello
```

When doing the same with grep instead, we get the expected output (and switching hello with yellow)

```
found2.txt
1:yellow

temp.txt
1:./found2.txt:yellow

temp2.txt
1:./found2.txt:yellow
2:./temp.txt:./found2.txt:yellow

temp3.txt
1:./found2.txt:yellow
2:./temp.txt:./found2.txt:yellow
3:./temp2.txt:./found2.txt:yellow
4:./temp2.txt:./temp.txt:./found2.txt:yellow
```

The more searches that ripgrep reports, the bigger the file blow up is (I did this on a ripgrep search that had 17,000 hits...). Currently, this can be worked around just by piping to a file outside the directory being searched, or piping into a pager instead.

---

_Comment by @BurntSushi on 2016-12-23 11:46_

I think we can probably fix this by getting the file descriptor of where stdout is being redirected and then make sure we don't search any file with that same descriptor.

---

_Label `bug` added by @BurntSushi on 2016-12-23 11:46_

---

_Comment by @retep998 on 2017-01-02 17:59_

On Windows you'd solve this by getting the ID of the file. Call `GetFileInformationByHandle` and examine the `dwVolumeSerialNumber` `nFileIndexHigh` and `nFileIndexLow` fields.

---

_Comment by @BurntSushi on 2017-01-08 17:26_

@retep998 There's some subtle additional details. [From MSDN](https://msdn.microsoft.com/en-us/library/windows/desktop/aa363788(v=vs.85).aspx):

> The identifier that is stored in the nFileIndexHigh and nFileIndexLow members is called the file ID. Support for file IDs is file system-specific. File IDs are not guaranteed to be unique over time, because file systems are free to reuse them. In some cases, the file ID for a file can change over time.

AFAIK, the only way to get a guarantee that the file index numbers aren't reused is if you keep the file handle open. Which I guess is fine in this case, since we only need to keep one handle (stdout) open for the lifetime of the process.

---

_Comment by @BurntSushi on 2017-01-08 17:27_

I guess this applies similarly on Linux too. You can't rely on *just* the inode number, since your directory might span multiple mount points.

---

_Comment by @BurntSushi on 2017-01-08 18:26_

There's enough subtlety here that I'm going to push this logic into a new independent crate, and then use that inside of *both* ripgrep and `walkdir`. `walkdir` does expose `is_same_file`, but the API operates on paths, and we'd ideally not need to stat the `stdout` file every single time.

---

_Closed by @BurntSushi on 2017-01-09 21:12_

---
