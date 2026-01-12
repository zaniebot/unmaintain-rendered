```yaml
number: 1809
title: "`cargo install ripgrep` doesn't support pcre2"
type: issue
state: closed
author: c02y
labels: []
assignees: []
created_at: 2021-02-28T12:49:24Z
updated_at: 2021-02-28T13:03:04Z
url: https://github.com/BurntSushi/ripgrep/issues/1809
synced_at: 2026-01-12T16:13:24Z
```

# `cargo install ripgrep` doesn't support pcre2

---

_@c02y_

#### What version of ripgrep are you using?

Replace this text with the output of `rg --version`.
```
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```
#### How did you install ripgrep?

If you installed ripgrep with snap and are getting strange file permission or
file not found errors, then please do not file a bug. Instead, use one of the
Github binary releases.

`cargo install ripgrep`

#### What operating system are you using ripgrep on?

Replace this text with your operating system and version.

```
Linux 5.9.16-1-MANJARO #1 SMP PREEMPT Mon Dec 21 22:00:46 UTC 2020 x86_64 GNU/Linux
```
#### Describe your bug.

Give a high level description of the bug.

After I `cargo install ripgrep` and use `rg -P xxx`, it says `PCRE2 is not available in this build of ripgrep`, so how can I `cargo install ripgrep` with pcre2 supported?

#### What are the steps to reproduce the behavior?

If possible, please include both your search patterns and the corpus on which
you are searching. Unless the bug is very obvious, then it is unlikely that it
will be fixed if the ripgrep maintainers cannot reproduce it.

If the corpus is too big and you cannot decrease its size, file the bug anyway
and the ripgrep maintainers will help figure out next steps.

#### What is the actual behavior?

Show the command you ran and the actual output. Include the `--debug` flag in
your invocation of ripgrep.

If the output is large, put it in a gist: https://gist.github.com/

If the output is small, put it in code fences:

```
your
output
goes
here
```

#### What is the expected behavior?

What do you think ripgrep should have done?


---

_Comment by @BurntSushi on 2021-02-28 13:02_

[As documented in the README](https://github.com/BurntSushi/ripgrep#building), you need to pass `--features pcre2` to any commands that compile ripgrep from source in order to enable PCRE2.

---

_Closed by @BurntSushi on 2021-02-28 13:02_

---

_Locked by @ghost on 2021-02-28 13:03_

---
