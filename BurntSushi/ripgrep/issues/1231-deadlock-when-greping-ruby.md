```yaml
number: 1231
title: Deadlock when greping ruby
type: issue
state: closed
author: graywolf
labels: []
assignees: []
created_at: 2019-03-29T21:32:08Z
updated_at: 2019-03-29T21:40:23Z
url: https://github.com/BurntSushi/ripgrep/issues/1231
synced_at: 2026-01-12T16:13:23Z
```

# Deadlock when greping ruby

---

_@graywolf_

#### What version of ripgrep are you using?

```
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`pacman -S ripgrep`

#### What operating system are you using ripgrep on?

Archlinux, current

#### Describe your question, feature request, or bug.

Deadlock when greping ruby sources.

#### If this is a bug, what are the steps to reproduce the behavior?

```
$ cd /tmp
$ git clone https://github.com/ruby/ruby
$ time rg foo
...
^C

real	0m34.282s
user	0m1.871s
sys	0m3.203s
```

For comparison

```
$ ag foo
...
real	0m0.315s
user	0m0.375s
sys	0m0.369s
```

#### If this is a bug, what is the actual behavior?

rg does not finishe (I've waiting up to almost 3 minutes in one of the tries)

#### If this is a bug, what is the expected behavior?

To finish in finite time.


---

_Comment by @BurntSushi on 2019-03-29 21:40_

This was fixed on master a while back. On mobile but it was a bug in transcoding one of the utf16 files in the ruby repo.

---

_Closed by @BurntSushi on 2019-03-29 21:40_

---
