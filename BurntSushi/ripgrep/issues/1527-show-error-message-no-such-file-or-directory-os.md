```yaml
number: 1527
title: "show error message ' No such file or directory (os error 2) '"
type: issue
state: closed
author: ryoppippi
labels:
  - invalid
assignees: []
created_at: 2020-03-21T05:19:56Z
updated_at: 2020-03-21T11:22:42Z
url: https://github.com/BurntSushi/ripgrep/issues/1527
synced_at: 2026-01-12T16:13:23Z
```

# show error message ' No such file or directory (os error 2) '

---

_@ryoppippi_

#### What version of ripgrep are you using?

ripgrep 12.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

installed with homebrew

#### What operating system are you using ripgrep on?

macOS 10.5
#### Describe your question, feature request, or bug.

When I search files like
```
rg --files --hidden --follow  --glob "!.git/*"        
```
It shows some unknown files like
```
 ./Library/ApplicationSupport/Microsoft Edge/SingletonCookie: No such file or directory (os error 2)  
```
I reinstalled ripgrep but it doesn't solve it
How can I solve this?
Is there any cache on my mac?



---

_Comment by @okdana on 2020-03-21 05:49_

It's reporting a broken symlink because you used `--follow`. If you just want to disable the error message you can use

```
--no-messages
    Suppress all error messages related to opening and reading files.
    Error messages related to the syntax of the pattern given are still
    shown.
```

---

_Comment by @ryoppippi on 2020-03-21 10:09_

Great! Thanks, it works!!

P.S.
I'll fix symbolic link...

---

_Closed by @ryoppippi on 2020-03-21 10:09_

---

_Label `invalid` added by @BurntSushi on 2020-03-21 11:22_

---
