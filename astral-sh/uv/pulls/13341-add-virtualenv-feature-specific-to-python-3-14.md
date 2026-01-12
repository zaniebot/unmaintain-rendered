```yaml
number: 13341
title: Add virtualenv feature specific to Python 3.14
type: pull_request
state: closed
author: Rogdham
labels: []
assignees: []
base: main
head: venv-314
created_at: 2025-05-08T06:13:46Z
updated_at: 2025-10-07T18:31:23Z
url: https://github.com/astral-sh/uv/pull/13341
synced_at: 2026-01-12T16:10:39Z
```

# Add virtualenv feature specific to Python 3.14

---

_@Rogdham_

## Summary

Python 3.14, which is now in [feature freeze](https://peps.python.org/pep-0745/), introduced a [change](https://github.com/python/cpython/issues/119535) (note: the name has been renamed in the second PR of this ticket) in the `venv` module.

More precisely, the interpreter is available in the `$PATH` once the venv is activated with yet another name.

This change is only for non-`nt` operating systems, and only for Python version 3.14 specifically.

I'm no rust developer, so I may have made mistakes when porting the code to rust. Please review accordingly!

_I use a vague commit message to avoid spoiling the fun 🐇🥚_

## Test Plan

I ran `uv venv` under a linux machine and checked the content of the `.venv/bin` directory:
- With Python 3.13, it was as before the PR
- With Python 3.14b1, a new symlink is created

---

_Marked ready for review by @Rogdham on 2025-05-08 06:23_

---

_Comment by @zanieb on 2025-05-08 14:16_

Thanks for contributing. I'm actually not sure I want to support this, I think it's more confusing than it's worth :/

---

_Comment by @Rogdham on 2025-05-08 17:08_

No worries, I understand. You can read in the issues and PRs on CPython discussions about whether it would cause issues for nothing. I believe that in the end, having the name starting with a non-ascii letter (especially not with a `p`) means that it will not get shown when users try to autocomplete `python`.

It landed in `venv` module, so I thought you may be interested to have it in your crate as well, and this is why I created the PR. I personally think that is the kind of little things that can bring joy when discovered, which I believe was the intent on CPython.

But at the end of the day, the maintenance cost falls on you, so no hard feelings if you don't want to get this merged!

---

_Comment by @flying-sheep on 2025-09-09 08:53_

I don’t think anyone will be confused by this harmless joke, but I’ve underestimating peoples’ capacity to be confused in the past.

---

_Comment by @Rogdham on 2025-10-07 18:31_

Python 3.14 is officially released, time to close the PR I guess. As I said, no hard feelings!

Enjoy the new Python version everyone! :partying_face: 

---

_Closed by @Rogdham on 2025-10-07 18:31_

---
