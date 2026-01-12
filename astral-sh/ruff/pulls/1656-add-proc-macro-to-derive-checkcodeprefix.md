```yaml
number: 1656
title: "Add proc-macro to derive `CheckCodePrefix`"
type: pull_request
state: merged
author: messense
labels: []
assignees: []
merged: true
base: main
head: derive-check-code-prefix
created_at: 2023-01-05T06:17:00Z
updated_at: 2023-01-06T12:36:25Z
url: https://github.com/astral-sh/ruff/pull/1656
synced_at: 2026-01-12T05:36:32Z
```

# Add proc-macro to derive `CheckCodePrefix`

---

_Pull request opened by @messense on 2023-01-05 06:17_

IMO a derive macro is a natural way to generate new code, and it reduces the chance of merge conflicts.

---

_Comment by @charliermarsh on 2023-01-05 16:37_

Yeah this makes a lot of sense, thank you. My main critique of macros like this is that they hurt the IDE experience, since (e.g.) `CheckCodePrefix` will no longer autocomplete in the editor... But this does seem more natural, and it also solves some of the bootstrapping problems that we have today, whereby we can't reference check code prefixes before generating them, etc.

---

_Merged by @charliermarsh on 2023-01-05 16:39_

---

_Closed by @charliermarsh on 2023-01-05 16:39_

---

_Comment by @charliermarsh on 2023-01-05 16:39_

(Nice work.)

---

_Branch deleted on 2023-01-05 23:07_

---

_Comment by @not-my-profile on 2023-01-06 05:22_

>My main critique of macros like this is that they hurt the IDE experience, since (e.g.) CheckCodePrefix will no longer autocomplete in the editor...

Autocompletion works fine for me using rust-analyzer, which has [`rust-analyzer.procMacro.enable`](https://rust-analyzer.github.io/manual.html#rust-analyzer.procMacro.enable) enabled by default. Are you using another language server for rust? I highly recommend rust-analyzer.

---

_Comment by @messense on 2023-01-06 05:43_

I think intellij-rust also works fine with derive macros now: https://github.com/intellij-rust/intellij-rust/issues/6908

> `org.rust.macros.proc.derive` - enables expansion of [derive](https://doc.rust-lang.org/reference/procedural-macros.html#derive-macros) procedural macros. Enabled by default

---

_Comment by @charliermarsh on 2023-01-06 12:25_

I use IntelliJ, for me I still don't see expansion unfortunately:

<img width="758" alt="Screen Shot 2023-01-06 at 7 23 01 AM" src="https://user-images.githubusercontent.com/1309177/211012300-ea6c12e1-0977-4763-aae6-06a6e90d2d49.png">

<img width="235" alt="Screen Shot 2023-01-06 at 7 23 08 AM" src="https://user-images.githubusercontent.com/1309177/211012308-436b08c4-36a6-4580-b6df-43c0d0c9f089.png">


---

_Comment by @messense on 2023-01-06 12:30_

That's odd, it works for me in CLion, maybe you need to enable the `Expand macros` option

<img width="790" alt="image" src="https://user-images.githubusercontent.com/1556054/211012936-de5b4695-0bb7-4ddd-a77d-e6f43d77521b.png">

<img width="982" alt="image" src="https://user-images.githubusercontent.com/1556054/211012950-871c6775-2cc5-45f5-9bcc-1f241c947a42.png">


---

_Comment by @charliermarsh on 2023-01-06 12:36_

I think I had to enable some more stuff in "Experimental features" -- looks like it's working now! Amazing! Thanks both!

<img width="595" alt="Screen Shot 2023-01-06 at 7 36 01 AM" src="https://user-images.githubusercontent.com/1309177/211013818-8fea72cd-0b1c-4239-b311-1fcfadd829a0.png">


---
