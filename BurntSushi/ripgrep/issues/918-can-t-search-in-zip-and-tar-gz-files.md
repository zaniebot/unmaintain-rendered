```yaml
number: 918
title: "Can't search in ZIP and tar.gz files"
type: issue
state: closed
author: kenorb
labels:
  - bug
  - question
assignees: []
created_at: 2018-05-11T23:40:15Z
updated_at: 2018-07-22T14:59:15Z
url: https://github.com/BurntSushi/ripgrep/issues/918
synced_at: 2026-01-12T16:13:22Z
```

# Can't search in ZIP and tar.gz files

---

_@kenorb_

#### What version of ripgrep are you using?

```
$ rg --version
ripgrep 0.8.1
-SIMD -AVX
```

#### What operating system are you using ripgrep on?

```
$ macosver 
ProductBuildVersion 17C2120
ProductCopyright 1983-2017 Apple Inc.
ProductName Mac OS X
ProductUserVisibleVersion 10.13.2
ProductVersion 10.13.2
```

#### If this is a bug, what are the steps to reproduce the behavior?

```
$ echo test > test
$ zip test.zip test
  adding: test (stored 0%)
$ tar zcvf test.tar.gz test
a test
$ gzip test
$ rg -z test
./test.gz
1:test
 $ rg -zl test
test.gz
```

#### What is the actual behavior?

No `.zip` or `.tar.gz` are searched.

#### If this is a bug, what is the expected behavior?

Should search in `.zip` and `.tar.gz` files.

---

_Comment by @BurntSushi on 2018-05-12 00:14_

The documentation states:

```
Search in compressed files. Currently gz, bz2, xz, and lzma files are
supported. This option expects the decompression binaries to be available in
your PATH.
```

So this is expected behavior. ripgrep does not advertise support for searching through tar archives.

---

_Label `invalid` added by @BurntSushi on 2018-05-12 00:14_

---

_Closed by @BurntSushi on 2018-05-12 00:14_

---

_Comment by @kenorb on 2018-05-12 00:24_

I thought `--search-zip` arg is self-explanatory. And also thought `.tar.gz` is included as part of `.gz`.

And at the end, `.tar` is just plain binary file, so this works:
```
$ rg -a test test.tar.gz
1?(?Ztest.tar??A
```
but this not:
```
$ rg -z -a test test.tar.gz
```

So if this is as expected, can we keep it open as part of the feature request?

---

_Comment by @BurntSushi on 2018-05-12 00:34_

That's because tar archives are explicitly skipped:

https://github.com/BurntSushi/ripgrep/blob/64317bda9f497d66bbeffa71ae6328601167a5bd/src/decompressor.rs#L92-L95

I admit, it does seem like `rg -z -a test test.tar.gz` should work.

---

_Reopened by @BurntSushi on 2018-05-12 00:34_

---

_Label `bug` added by @BurntSushi on 2018-06-22 10:47_

---

_Label `question` added by @BurntSushi on 2018-06-22 10:47_

---

_Label `invalid` removed by @BurntSushi on 2018-06-22 10:47_

---

_Closed by @BurntSushi on 2018-07-22 14:59_

---
