```yaml
number: 1315
title: Multiple issues configuring ripgrep
type: issue
state: closed
author: mcandre
labels: []
assignees: []
created_at: 2019-06-30T16:42:55Z
updated_at: 2019-07-01T13:08:41Z
url: https://github.com/BurntSushi/ripgrep/issues/1315
synced_at: 2026-01-12T16:13:23Z
```

# Multiple issues configuring ripgrep

---

_@mcandre_

#### What version of ripgrep are you using?

```console
$ rg --version
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

apt-get

#### What operating system are you using ripgrep on?

Ubuntu 19.04 Disco Dingo

#### Describe your question, feature request, or bug.

* `$HOME/.ripgreprc` appears to be ignored on my machine.
* Configuration guide does not provide sufficient examples for excluding file and folder patterns from search results.
* Configuration guide is difficult to navigate to.

---

_Comment by @BurntSushi on 2019-07-01 13:08_

> $HOME/.ripgreprc appears to be ignored on my machine

Consider reading the docs carefully. The existence of that file is insufficient: https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file

> Configuration guide does not provide sufficient examples for excluding file and folder patterns from search results.

There are examples in the guide, so I don't know what you're talking about. See: https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#manual-filtering-globs

> Configuration guide is difficult to navigate to.

Again, I don't know what you're talking about.

I appreciate that you took the time to file an issue, but this is largely not actionable because you didn't really explain anything.

---

_Closed by @BurntSushi on 2019-07-01 13:08_

---
