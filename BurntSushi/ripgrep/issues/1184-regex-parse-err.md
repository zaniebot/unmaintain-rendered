```yaml
number: 1184
title: regex parse err
type: issue
state: closed
author: danyill
labels: []
assignees: []
created_at: 2019-02-01T07:46:27Z
updated_at: 2019-02-01T18:37:31Z
url: https://github.com/BurntSushi/ripgrep/issues/1184
synced_at: 2026-01-12T16:13:23Z
```

# regex parse err

---

_@danyill_

#### What version of ripgrep are you using?

```
20:35 $ rg --version
ripgrep 0.9.0
-SIMD -AVX
```

#### How did you install ripgrep?

`apt install ripgrep`

#### What operating system are you using ripgrep on?

```
20:36 $ uname -a
Linux mulhollandd-XPS-13-9360 4.18.8-041808-generic #201809150431 SMP Sat Sep 15 08:33:36 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux

20:37 $ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 18.10
Release:        18.10
```
#### If this is a bug, what are the steps to reproduce the behavior?

If possible, please include both your search patterns and the corpus on which
you are searching. Unless the bug is very obvious, then it is unlikely that it
will be fixed if the ripgrep maintainers cannot reproduce it.

If the corpus is too big and you cannot decrease its size, file the bug anyway
and the ripgrep maintainers will help figure out next steps.

#### If this is a bug, what is the actual behavior?

[Debug output](https://gist.github.com/danyill/5658de419142f42b98daf0a9580f0bc1)
[Corpus](https://gist.github.com/danyill/306cd8d9cf987797df78129b2216acb1)

#### If this is a bug, what is the expected behavior?

What do you think ripgrep should have done?

Not produced a regex error...

---

_Comment by @danyill on 2019-02-01 07:52_

I think this is my fault for using `-f` when there is no need for it, but I might not expect failure, after all is the filename not a legitimate `PATTERNFILE`?

---

_Comment by @BurntSushi on 2019-02-01 10:25_

Sorry, but this bug report doesn't make sense to me. Please describe what you're trying to do. The behavior of ripgrep looks correct to me. Please consider reading the documentation for the `-f` flag.

---

_Comment by @danyill on 2019-02-01 18:37_

Thanks for responding, I think you're quite right The cold light of day makes it clearer what `PATTERNFILE` is.

---

_Closed by @danyill on 2019-02-01 18:37_

---
