```yaml
number: 1379
title: "--smart-case ignores .ignore file when using path argument"
type: issue
state: closed
author: blueyed
labels:
  - duplicate
assignees: []
created_at: 2019-09-15T14:34:39Z
updated_at: 2019-09-16T13:09:09Z
url: https://github.com/BurntSushi/ripgrep/issues/1379
synced_at: 2026-01-12T16:13:23Z
```

# --smart-case ignores .ignore file when using path argument

---

_@blueyed_

Using `--smart-case` does not ignore a file specified in `.ignore`.

`.ignore` includes:
```
test/functional/fixtures/bigfile.txt
test/functional/fixtures/bigfile_oneline.txt
```

```
% /usr/bin/rg scroll -l test/functional --smart-case | grep oneline
test/functional/fixtures/bigfile_oneline.txt
```

```
% /usr/bin/rg scroll -l test/functional | grep oneline
```

The whole .ignore file:
```
src/nvim/po/*.po
luacov.report*
!local.mk
log-*

test/functional/fixtures/bigfile.txt
test/functional/fixtures/bigfile_oneline.txt

# https://github.com/BurntSushi/ripgrep/issues/1349
!/build
/build/**
!/build/src
!/build/src/nvim
!/build/src/nvim/auto
!/build/src/nvim/auto/**/*.c
```

This might be due to the file itself (via https://github.com/neovim/neovim/tree/b9d035a39cfcd61eab2633f55e6064460f557c69/test/functional/fixtures), I can provide more info if it is not reproducible for you.

ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)


---

_Comment by @blueyed on 2019-09-15 14:37_

It only happens when specifying the dir as an arg.
With just `/usr/bin/rg scroll -l` it makes no difference if `--smart-case` is used (it is always ignored then as expected).

---

_Renamed from "--smart-case ignores .ignore file?!" to "--smart-case ignores .ignore file when using path argument" by @blueyed on 2019-09-15 14:37_

---

_Comment by @BurntSushi on 2019-09-16 02:41_

I don't see what smart case has to do with this. The output you've shown is invariant with respect to whether smart case is enabled or not.

This looks like a duplicate of a big where ignore rules aren't always handled correctly when a directory is specified as an argument. But I'm on mobile, so I'm not searching for it right now.

---

_Comment by @blueyed on 2019-09-16 12:54_

Oh, sorry, edited the text - with `/usr/bin/rg scroll -l test/functional --smart-case | grep oneline` the output is empty, i.e. it is ignored there.

---

_Comment by @BurntSushi on 2019-09-16 13:08_

The `--smart-case` behavior is easily explained. The file contains the text `SCROLL`, so `rg scroll` won't match it but `rg scroll --smart-case` will. This is expected and correct.

Otherwise, this looks like a duplicate of #829.

---

_Closed by @BurntSushi on 2019-09-16 13:08_

---

_Label `bug` added by @BurntSushi on 2019-09-16 13:08_

---

_Label `bug` removed by @BurntSushi on 2019-09-16 13:09_

---

_Label `duplicate` added by @BurntSushi on 2019-09-16 13:09_

---
