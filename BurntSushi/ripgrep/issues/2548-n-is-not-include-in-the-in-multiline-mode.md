```yaml
number: 2548
title: "/n is not include in the \".\" in multiline mode "
type: issue
state: closed
author: hawktang
labels:
  - invalid
assignees: []
created_at: 2023-07-05T02:18:02Z
updated_at: 2023-07-05T03:05:30Z
url: https://github.com/BurntSushi/ripgrep/issues/2548
synced_at: 2026-01-12T16:13:24Z
```

# /n is not include in the "." in multiline mode 

---

_@hawktang_

#### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)
#### How did you install ripgrep?
brew

#### What operating system are you using ripgrep on?

MacOS 13.3

#### Describe your bug.

/n is not include in the "." in multiline mode 

#### What are the steps to reproduce the behavior?

success\t.*?dept:ssq(.|\n)*?(?=\n\d{4}-\d{2}-\d{2})

need to be used instead of 

success\t.*?dept:ssq.*?(?=\n\d{4}-\d{2}-\d{2})

#### What is the actual behavior?

\n was ignored when use . pattern  in multiline mode


```
rg -U --pcre2 "success\t.*?dept:proc(.|\n)*?(?=\n\d{4}-\d{2}-\d{2})" logs > proc.log

```

#### What is the expected behavior?

\n should include when use . pattern  in multiline mode

```
rg -U --pcre2 "success\t.*?dept:proc.*?(?=\n\d{4}-\d{2}-\d{2})" logs > proc.log
```

---

_Comment by @hawktang on 2023-07-05 02:31_

Sorry, we have --multiline-dotall

and maybe always enable this is not a good idea.

---

_Closed by @hawktang on 2023-07-05 02:31_

---

_Comment by @BurntSushi on 2023-07-05 03:05_

You can also use `(?s:.)`. This is indeed intended behavior.

---

_Label `invalid` added by @BurntSushi on 2023-07-05 03:05_

---
