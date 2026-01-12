```yaml
number: 1461
title: "Implement `flake8-implicit-str-concat`"
type: issue
state: closed
author: pradyunsg
labels:
  - rule
assignees: []
created_at: 2022-12-30T03:04:46Z
updated_at: 2022-12-30T09:38:10Z
url: https://github.com/astral-sh/ruff/issues/1461
synced_at: 2026-01-12T15:54:41Z
```

# Implement `flake8-implicit-str-concat`

---

_@pradyunsg_

👋🏽 

- [ ] [error code TBD] implicitly concatenated string literals on one line
- [ ] [error code TBD] implicitly concatenated string literals over continuation line
- [ ] [error code TBD] explicitly concatenated string should be implicitly concatenated

I was looking at integrating `ruff` into pip's linting pipeline, and noticed that the specific flake8 plugins that we use aren't implemented already. This specific one is useful to flag when black reformats strings to implicitly concat by reflowing lines.


---

_Comment by @charliermarsh on 2022-12-30 03:14_

Hey, really cool to see you interested in Ruff! I've read a lot of your comments on the Python forums 🙈 

Happy to implement this, looks relatively straightforward assuming there are no relevant / subtle differences in the tokenization.

---

_Label `rule` added by @charliermarsh on 2022-12-30 03:15_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-30 03:15_

---

_Comment by @charliermarsh on 2022-12-30 03:18_

Yeah I can almost certainly ship this tomorrow if not tonight. (Probably not tonight, gotta log off soon.)

---

_Closed by @charliermarsh on 2022-12-30 04:00_

---

_Comment by @charliermarsh on 2022-12-30 04:01_

Ok, I was just kidding, I implemented it in #1463, so it'll go out tonight in the end-of-day release.

---

_Comment by @charliermarsh on 2022-12-30 04:02_

I used the same codes as the originating plugin (`ISC001`, `ISC002`, and `ISC003`). That plugin doesn't seem to have any tests, so I wrote a small suite, but would appreciate any feedback on bugs or incompatibilities.

---

_Comment by @pradyunsg on 2022-12-30 09:36_

Wow, I wasn't expecting this to be closed by the time I woke up.

> Ok, I was just kidding, I implemented it in #1463, so it'll go out tonight in the end-of-day release.

No kidding, I really appreciate your quick response here!

> I've read a lot of your comments on the Python forums 🙈

🙈

> Hey, really cool to see you interested in Ruff!

You've built a really cool thing, standing on the shoulders of really cool projects (am thinking of Rust _and_ RustPython here). :)

I'm excited to see the slowest linter in our setup get replaced with something that nearly finishes in the time Python takes to startup!

> That plugin doesn't seem to have any tests, so I wrote a small suite, but would appreciate any feedback on bugs or incompatibilities.

Definitely! Taking a look at the PR, that looks about right. I'll give it a drive after my work day, and let you know how that goes! :)


---

_Comment by @pradyunsg on 2022-12-30 09:38_

TIL expedient doesn't mean what I thought it did.

---
