```yaml
number: 2108
title: improve documentation for replacement group syntax
type: issue
state: closed
author: Antiever
labels:
  - doc
assignees: []
created_at: 2021-12-20T20:22:06Z
updated_at: 2023-07-08T22:53:09Z
url: https://github.com/BurntSushi/ripgrep/issues/2108
synced_at: 2026-01-12T16:13:24Z
```

# improve documentation for replacement group syntax

---

_@Antiever_

#### What version of ripgrep are you using?

ripgrep 13.0.0 

#### How did you install ripgrep?

Github binary release

#### What operating system are you using ripgrep on?

Windows 10 Pro 21H2 

#### Describe your bug.

Replace group strange behavior if not separated from text

#### What are the steps to reproduce the behavior?

```
>echo link="https:\\site.com" | rg "(.*)(https.*?)(\")" -or "$1localhost$3"
>"
```
however
```
>echo link="https:\\site.com" | rg "(.*)(https.*?)(\")" -or "$1 localhost$3"
>link=" localhost"
```
Workaround for now is changing regex to insert "" between $1 and text

#### What is the expected behavior?
```
>echo link="https:\\site.com" | rg "(.*)(https.*?)(\")" -or "$1localhost$3"
>link="localhost"
```


---

_Label `doc` added by @BurntSushi on 2021-12-21 13:38_

---

_Comment by @BurntSushi on 2021-12-21 13:40_

The behavior here is correct, but it turns out that the docs don't explain how to work around it. You need to use `${1}localhost$3`.

The full docs for the replacement string are here: https://docs.rs/regex/latest/regex/struct.Captures.html#method.expand

More of that should be transplanted into ripgrep's `--help` output, particularly the parts about braces.

---

_Comment by @Antiever on 2021-12-21 14:16_

Yes, my bad, Thx!

---

_Closed by @Antiever on 2021-12-21 14:16_

---

_Comment by @BurntSushi on 2021-12-21 14:45_

This is a documentation bug. Let's leave it open. :-)

---

_Reopened by @BurntSushi on 2021-12-21 14:45_

---

_Renamed from "Replace group bug" to "improve documentation for replacement group syntax" by @BurntSushi on 2021-12-21 14:45_

---

_Closed by @BurntSushi on 2023-07-08 22:53_

---
