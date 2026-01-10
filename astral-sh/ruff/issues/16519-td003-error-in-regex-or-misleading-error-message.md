---
number: 16519
title: TD003 error in regex or misleading error message
type: issue
state: open
author: JimMajor
labels:
  - documentation
  - rule
  - help wanted
assignees: []
created_at: 2025-03-05T14:02:30Z
updated_at: 2025-10-03T15:59:34Z
url: https://github.com/astral-sh/ruff/issues/16519
synced_at: 2026-01-10T01:22:57Z
---

# TD003 error in regex or misleading error message

---

_Issue opened by @JimMajor on 2025-03-05 14:02_

### Summary

Rule TD0003 ('Missing issue link for this TODO') allows for 4 different forms off issue link, however only explicit link and  `#123` works anywhere in the TODO comment, `123` and `FOOBAR-1234` have to be put alone in the separate line.
1. While it may be reasonable for digits alone version I think it would be much more convenient to search for FOOBAR-1234 form in the whole TODO comment.
2. This difference in behavior is not stated anywhere clearly, nor in the error message neither in ruff docs (it can be only inferred form the examples) which is very confusing for the user.

### Version

0.9.3

---

_Renamed from "TD0003 error in regex or misleading error message" to "TD003 error in regex or misleading error message" by @MichaReiser on 2025-03-05 14:17_

---

_Comment by @MichaReiser on 2025-03-05 14:20_

Looking at the code, the distinction between same and own line comments seems very intentional

https://github.com/astral-sh/ruff/blob/79a2c7eaa28daed1de520f35119ec0625b007188/crates/ruff_linter/src/rules/flake8_todos/rules/todos.rs#L233-L248

@dylwil3 I see you're name pop up when annotating the code. Do you know more?

I do agree, that it isn't clear from the documentation which pattern are allowed on the same or have to be on a separate line.

---

_Label `documentation` added by @MichaReiser on 2025-03-05 14:21_

---

_Comment by @dylwil3 on 2025-03-06 14:18_

I agree this is confusing!

The change in `TD003` from #15519 was the minimal necessary to support the VSCode issue link formats. 

But I think it would be reasonable to support links on the same line as the TODO in general. This was suggested in #9986, but that was closed under the assumption that supporting VSCode formats would cover the general case.

So I propose we make the rule less rigid and simultaneously improve the documentation.

---

_Label `rule` added by @dylwil3 on 2025-03-06 14:18_

---

_Label `help wanted` added by @dylwil3 on 2025-03-06 14:18_

---

_Comment by @MattTheCuber on 2025-05-23 14:24_

To add to this, both the [third and fourth examples](https://docs.astral.sh/ruff/rules/missing-todo-link/#example) in the documentation raise the warning:

```py
# TODO(charlie): https://github.com/astral-sh/ruff/issues/3870
# this comment has an issue link

# TODO(charlie): #003 this comment has a 3-digit issue code
# with leading character `#`
```

---

_Comment by @dylwil3 on 2025-10-02 15:36_

> To add to this, both the [third and fourth examples](https://docs.astral.sh/ruff/rules/missing-todo-link/#example) in the documentation raise the warning:
> 
> ```
> # TODO(charlie): https://github.com/astral-sh/ruff/issues/3870
> # this comment has an issue link
> 
> # TODO(charlie): #003 this comment has a 3-digit issue code
> # with leading character `#`
>```

Can you share a way to reproduce that behavior? I don't see any diagnostics in the playground for this: https://play.ruff.rs/339852bf-dcf3-45b0-af34-4c3b4ad5aefe

---

_Comment by @MattTheCuber on 2025-10-03 11:53_

> Can you share a way to reproduce that behavior? I don't see any diagnostics in the playground for this: https://play.ruff.rs/339852bf-dcf3-45b0-af34-4c3b4ad5aefe

After further testing, this seems to only occur in the VS Code extension (`ruff check` works fine).

I am using version 2025.26.0 of the Ruff extension with a test project of 2 files:

pyproject.toml

```toml
[tool.ruff.lint]
select = ["TD003"]
```

test.py

```py
# TODO(charlie): https://github.com/astral-sh/ruff/issues/3870
# this comment has an issue link

# TODO(charlie): #003 this comment has a 3-digit issue code
# with leading character `#`
```

Here is the problems tab in VS Code:

<img width="623" height="124" alt="Image" src="https://github.com/user-attachments/assets/0bb9e9df-ff53-41a5-8243-f608586ca357" />

Here is the Ruff warning popup:

<img width="649" height="175" alt="Image" src="https://github.com/user-attachments/assets/91b9cf4f-ee44-4af7-89eb-149022d5daf3" />

---

_Comment by @dylwil3 on 2025-10-03 12:41_

I cannot reproduce this with VSCode either. Perhaps your VSCode is picking up an older Ruff binary from somewhere? If you look at the Output console in the "Ruff" channel it should tell you which Ruff it found and what version it is.

<img width="1278" height="427" alt="Image" src="https://github.com/user-attachments/assets/f1af01db-e928-4540-a222-1b3e2fb5c5a5" />

If that doesn't work could you open a new issue so we can discuss further?

---

_Comment by @MattTheCuber on 2025-10-03 15:59_

That resolved my problem, thanks! Ruff was using a very old install from my user `.local` pip environment instead of the virtual environment in the project I had open... `deactivate` then `pip uninstall ruff` and a reload fixed my problem.

---
