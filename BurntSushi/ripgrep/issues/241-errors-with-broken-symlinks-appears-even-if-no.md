```yaml
number: 241
title: Errors with broken symlinks appears even if --no-messages is passed
type: issue
state: closed
author: ahmedelgabri
labels: []
assignees: []
created_at: 2016-11-20T19:13:18Z
updated_at: 2016-11-20T20:01:57Z
url: https://github.com/BurntSushi/ripgrep/issues/241
synced_at: 2026-01-12T18:23:11Z
```

# Errors with broken symlinks appears even if --no-messages is passed

---

_@ahmedelgabri_

If you have a broken symlink somewhere in the path that you are searching in you will get this error `No such file or directory (os error 2)` on macOS. Not sure how this should be handled though, for example it seems the `ag` ignores these errors.

```
$ tree
.
├── bar
│   └── brokensymlink -> ./moveme
└── moveme.broken

1 directory, 2 files

$ rg --files --hidden --follow --glob "\!.git/*"
./bar/brokensymlink: No such file or directory (os error 2)
moveme.broken
```
The problem that if you try to use `rg` with another tool that depends on the output, something like `fzf` it will break or you get some weird stuff.



---

_Comment by @BurntSushi on 2016-11-20 19:27_

I think this is intended behavior. You'll get a similar error if you don't have permission to read a file or directory, for example:

```
$ echo foo > test1
$ echo foo > test2
$ mkdir dir
$ echo foo > dir/test3
$ chmod 000 test2 dir
$ rg foo
./test2: Permission denied (os error 13)
test1
1:foo
./dir: Permission denied (os error 13)
```

This matches the behavior of `find` (for directories at least, `find` doesn't need to read files so it doesn't care if it can't access them):

```
$ find ./
./
./dir
find: ‘./dir’: Permission denied
./test2
./test1
```

To drive this point home, GNU grep behaves similarly:

```
$ grep -r foo
grep: dir: Permission denied
grep: test2: Permission denied
test1:foo
```

Given the above evidence, I think ripgrep's current behavior is probably right.

> The problem that if you try to use rg with another tool that depends on the output, something like fzf it will break or you get some weird stuff.

ripgrep writes all such error messages to stderr. This is common in CLI applications and it is easy to suppress such output by redirecting stderr to `/dev/null`:

```
$ rg foo 2> /dev/null
test1
1:foo
```

If redirection like this is inconvenient, then you can pass the `--no-messages` flag, which does the same thing it does in grep:

```
$ rg --no-messages foo
./test2: Permission denied (os error 13)
test1
1:foo
```

Ah ha! A bug! I'll push a fix shortly for that. But that `./test2` permission error shouldn't appear when `--no-messages` is set.


---

_Comment by @ahmedelgabri on 2016-11-20 19:34_

I think you are right & I agree with you, because honestly I like that it reports the errors back when you have a broken symlink & since there is a way to suppress this when needed then it's ok.

> Ah ha! A bug! I'll push a fix shortly for that. But that ./test2 permission error shouldn't appear when --no-messages is set.

Will use redirection for now till this is fixed then 😄 


---

_Closed by @ahmedelgabri on 2016-11-20 19:34_

---

_Reopened by @BurntSushi on 2016-11-20 20:00_

---

_Renamed from "Errors with broken symlinks" to "Errors with broken symlinks appears even if --no-messages is passed" by @BurntSushi on 2016-11-20 20:00_

---

_Comment by @BurntSushi on 2016-11-20 20:01_

@ahmedelgabri Great. Keeping this open to track the bug I found. :-)


---

_Closed by @BurntSushi on 2016-11-20 20:01_

---
