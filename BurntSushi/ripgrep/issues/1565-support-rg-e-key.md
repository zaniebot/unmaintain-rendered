```yaml
number: 1565
title: "support rg -e \"*key*\""
type: issue
state: closed
author: laoshaw
labels:
  - wontfix
assignees: []
created_at: 2020-04-30T14:57:16Z
updated_at: 2020-04-30T15:06:31Z
url: https://github.com/BurntSushi/ripgrep/issues/1565
synced_at: 2026-01-12T16:13:23Z
```

# support rg -e "*key*"

---

_@laoshaw_

when I do `grep -E "*key*ord"` or `grep -E "*key*" it works fine

when I do `rg -e "*key*"`: `*key*: No such file or directory (os error 2)`

`rg -e '*ard*ist'` I got `regex parse error`

is this a missing feature for rg?

`rg -P "*key*"` leads to "PCRE2 is not available in this build of ripgrep"

---

_Label `wontfix` added by @BurntSushi on 2020-04-30 15:03_

---

_Comment by @BurntSushi on 2020-04-30 15:06_

This issue is hard for me to understand. In particular, I do not understand why you removed the issue template.

As far as I can tell, you're requesting that `rg '*' some-file` should work. Where `*` is inferred to apply to an empty sub-expression, which in turn matches anything. In other words, it's strictly superfluous. There should be no difference between the regexes `*key*ord` and `key*ord`.

ripgrep's regex engine does not permit applying a repetition operator to a non-existent expression. You left this out of your issue:

```
$ rg "*key*"
regex parse error:
    *key*
    ^
error: repetition operator missing expression
```

PCRE2 has the same restriction:

```
$ rg "*key*" -P
PCRE2: error compiling pattern at offset 0: quantifier does not follow a repeatable item
```

This is working as intended.

---

_Closed by @BurntSushi on 2020-04-30 15:06_

---
