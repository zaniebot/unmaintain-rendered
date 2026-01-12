```yaml
number: 983
title: error parsing .gitignore from coreutils
type: issue
state: closed
author: ghost
labels:
  - duplicate
assignees: []
created_at: 2018-07-17T08:44:21Z
updated_at: 2018-07-17T10:50:21Z
url: https://github.com/BurntSushi/ripgrep/issues/983
synced_at: 2026-01-12T16:13:22Z
```

# error parsing .gitignore from coreutils

---

_@ghost_

#### What version of ripgrep are you using?

ripgrep 0.8.1
-SIMD -AVX


#### How did you install ripgrep?

I did install ripgrep through the standard repository from fedora 28

#### What operating system are you using ripgrep on?

fedora 28 x64

uname -r
4.17.2-200.fc28.x86_64


#### If this is a bug, what are the steps to reproduce the behavior?

I'm not 100% sure if this is a bug in ripgrep or in gnu coreutils utilisation of .gitignore files

anyway here is the reproducer:

```
git clone git://git.sv.gnu.org/coreutils
cd coreutils
rg foobar
./src/.gitignore: line 3: error parsing glob '\[': unclosed character class; missing ']'
tests/misc/numfmt.pl
649:     ['help-1', '--foobar',
```

#### If this is a bug, what is the expected behavior?

I'm not sure, is it expected to use globs in .gitignore files?

the corresponding line from the gitignore looks like this:

```
\[
```

should this be escaped?

Thanks for the awesome tool btw!

Feel free to close if this is more an issue with coreutils.

---

_Comment by @BurntSushi on 2018-07-17 10:50_

Thanks for the good bug report! I can reproduce this on `0.8.1`, but this appears to be fixed on master. I believe it was fixed in https://github.com/BurntSushi/ripgrep/commit/e2516ed0, which specifically supports escaping.

---

_Closed by @BurntSushi on 2018-07-17 10:50_

---

_Label `duplicate` added by @BurntSushi on 2018-07-17 10:50_

---
