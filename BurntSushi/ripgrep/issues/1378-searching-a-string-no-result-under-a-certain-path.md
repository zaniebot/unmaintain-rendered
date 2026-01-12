```yaml
number: 1378
title: Searching a string no result under a certain path
type: issue
state: closed
author: randomwangran
labels: []
assignees: []
created_at: 2019-09-14T02:21:57Z
updated_at: 2019-09-14T02:34:53Z
url: https://github.com/BurntSushi/ripgrep/issues/1378
synced_at: 2026-01-12T16:13:23Z
```

# Searching a string no result under a certain path

---

_@randomwangran_

#### What version of ripgrep are you using?

ripgrep 11.0.1 (rev 344b423da5)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)


#### How did you install ripgrep?
I used cargo. Mingw64 shell

$ cargo build --release
$ ./target/release/rg --version

$ cargo version
cargo 1.36.0 (c4fcfb725 2019-05-15)

Shell: MINGW64.exe
C:\Program Files\Git\mingw64\bin

#### What operating system are you using ripgrep on?

OS Name	Microsoft Windows 7 Ultimate

Version	6.1.7601 Service Pack 1 Build 7601

cmd.exe

Microsoft Windows [Version 6.1.7601]


#### Describe your question, feature request, or bug.

I recently find a strange behavior using `rg` to search a string within a folder.

The folder structure is this (or [this minimal example](https://github.com/randomwangran/magit-playground/commit/16e4eb3d03ca9358f1ad1bbd2a30e42f4c4b4dc7)):

```
xx-org
|
|-- dummy-A
|-- dummy-A.org
|-- dummy-B
|-- dummy-B.org
|-- dummy-C
`-- dummy-C.org
```

In some of these dummy files, it contains a string: `:leila:`.


If I put `xx-org` somewhere in my file system, for example:


`e:\xx-org`


Then, open a `cmd.exe` and type: `rg :leila: e/xx-org`, I can get:

```
E:\xx-org>rg :leila: e:/xx-org

e:/xx-org\dummy-A.org
4::leila:

e:/xx-org\dummy-B.org
4::leila:

e:/xx-org\dummy-C.org
4::leila:


```

If xx-org is under `c:\xx-org`, positive result.


```
C:\>rg :leila: c:/xx-org

c:/xx-org\dummy-A.org
4::leila:

c:/xx-org\dummy-C.org
4::leila:

c:/xx-org\dummy-B.org
4::leila:
```

And again in `c:\User\xx-org`


```
C:\>rg :leila: c:/Users/xx-org

c:/Users/xx-org\dummy-A.org
4::leila:

c:/Users/xx-org\dummy-B.org
4::leila:

c:/Users/xx-org\dummy-C.org
4::leila:
```


However, if I put `xx-org` in the following path:

`c:\User\user\xx-org`

No result.

```
C:\>rg :leila: c:/Users/user/xx-org

C:\>

```

I was wondering what is the cause of such behavior? Can anyone help me
out of this issue?


Thanks,

Ran


#### If this is a bug, what are the steps to reproduce the behavior?

Not sure it is a bug, yet.

#### If this is a bug, what is the actual behavior?

[TBD]


#### If this is a bug, what is the expected behavior?

Show the same result in `c:\User\user\xx-org`




I also test the same searching using `grep`
`grep (GNU grep) 3.0` with the same shell on the same machine.


The results are expected in both `c:/Users/user/xx-org` and `c:/user/xx-org`

```
C:\Users\user>grep -nr :leila: c:/Users/user/xx-org
c:/Users/user/xx-org/dummy-A:4::leila:
c:/Users/user/xx-org/dummy-A.org:4::leila:
c:/Users/user/xx-org/dummy-B:4::leila:
c:/Users/user/xx-org/dummy-B.org:4::leila:
c:/Users/user/xx-org/dummy-C:4::leila:
c:/Users/user/xx-org/dummy-C.org:4::leila:

C:\Users\user>grep -nr :leila: c:/Users/xx-org
c:/Users/xx-org/dummy-A:4::leila:
c:/Users/xx-org/dummy-A.org:4::leila:
c:/Users/xx-org/dummy-B:4::leila:
c:/Users/xx-org/dummy-B.org:4::leila:
c:/Users/xx-org/dummy-C:4::leila:
c:/Users/xx-org/dummy-C.org:4::leila:
```




---

_Comment by @okdana on 2019-09-14 02:27_

Probably you have a `.ignore` or `.gitignore` in your home directory? The `--debug` output is usually helpful

---

_Comment by @randomwangran on 2019-09-14 02:34_

@okdana 

Thanks.

Problem fixed.

```diff
- *
- */
+# *
+# */
```

---

_Closed by @randomwangran on 2019-09-14 02:34_

---
