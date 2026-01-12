```yaml
number: 711
title: Directory separators not normalised on Windows
type: issue
state: closed
author: mrmonday
labels:
  - duplicate
  - question
  - wontfix
assignees: []
created_at: 2017-12-11T10:23:11Z
updated_at: 2017-12-29T11:44:20Z
url: https://github.com/BurntSushi/ripgrep/issues/711
synced_at: 2026-01-12T16:13:22Z
```

# Directory separators not normalised on Windows

---

_@mrmonday_

Using either cmd or git bash on windows:
```
$ rg --version
ripgrep 0.6.0
-AVX -SIMD

$ find .
.
./a
./a/c
./a/c/b
$ cat a/c/b
aaaa
bbbb
cccc

$ rg aa a/
a/c\b
1:aaaa

```

Here the output mixed `/` and `\` as directory separators - they should be normalised for consistency.

---

_Comment by @BurntSushi on 2017-12-11 12:06_

This looks like a duplicate of #275. In particular, what should the paths be normalized to? Does `--path-separator` fix this issue for you?

---

_Label `duplicate` added by @BurntSushi on 2017-12-11 12:06_

---

_Label `question` added by @BurntSushi on 2017-12-11 12:06_

---

_Comment by @mrmonday on 2017-12-12 09:09_

That works, I'll set up an alias.

I thought it might be good to normalise by default, so I did a little investigation:

 * `grep` in git bash (find does similarly):
    ```
    $ grep -r aa 'a\'
    a\/c/b:aaaa
    $ grep -r aa 'a/'
    a/c/b:aaaa
    ```
 * `findstr` in cmd: I couldn't find a way to make it recurse, but it prints exactly as passed
 * `dir` in cmd: This always normalises to `\`
 * `Get-ChildItem a/ -Recurse` in powershell normalises to `\`

I'm no expert in Window's command line utilities, but my minor investigation reveals quite a mixed bag of results - there's no definitive "ripgrep should do this because it's what everything else does".

I'll close this, since `--path-separator` is a sufficient workaround. Thanks.

---

_Closed by @mrmonday on 2017-12-12 09:09_

---

_Label `wontfix` added by @BurntSushi on 2017-12-29 11:44_

---
