```yaml
number: 17284
title: Clarify requirements file format in docs
type: pull_request
state: merged
author: svlandeg
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/requirements
created_at: 2026-01-02T13:20:05Z
updated_at: 2026-01-06T09:51:39Z
url: https://github.com/astral-sh/uv/pull/17284
synced_at: 2026-01-10T05:49:14Z
```

# Clarify requirements file format in docs

---

_Pull request opened by @svlandeg on 2026-01-02 13:20_

## Summary

The docs for `requirements.in` currently wrongly refer to `requirements.txt` in the first sentence:
> ### Using requirements.in
>
> It is also common to use a lightweight **requirements.txt** format to declare the dependencies for the project. Each requirement is defined on its own line. Commonly, this file is called requirements.in to distinguish it from requirements.txt which is used for the locked dependencies.

The occurrence in bold should be **requirements.in** instead.


---

_@svlandeg reviewed on 2026-01-02 13:23_

---

_Review comment by @svlandeg on `docs/pip/dependencies.md`:41 on 2026-01-02 13:23_

Or alternatively, drop the extension alltogether from the first sentence:
```suggestion
It is also common to use a lightweight `requirements` format to declare the dependencies for the
```

---

_@woodruffw requested changes on 2026-01-02 15:27_

I think this is actually intentional -- `requirements.txt` refers to both the conventional filename *and* the name of the format itself, so what the docs are saying is that there's a file named `requirements.in` that's in `requirements.txt` format.



---

_Comment by @svlandeg on 2026-01-02 15:44_

> I think this is actually intentional -- `requirements.txt` refers to both the conventional filename _and_ the name of the format itself, so what the docs are saying is that there's a file named `requirements.in` that's in `requirements.txt` format.

I can see how it could be intended to read like that, but when I read this paragraph the first time that's definitely not how it parsed for me. In this part of the docs, there has also been no prior explanation about the "requirements.txt format". 

Maybe the text could still be updated somehow to avoid confusion. Maybe something like 

> It is also common to use a lightweight `requirements.txt`-like format

Anyway, feel free to close if you want to keep as-is, ofc!

---

_Comment by @woodruffw on 2026-01-02 15:53_

Yeah, I completely agree it's confusing 😅 -- maybe we could link to pip's documentation for this, since I think that's the canonical source for the format:

https://pip.pypa.io/en/stable/reference/requirements-file-format/

Also, I apologize -- I thought the actual name of the format was `requirements.txt`, but pip calls it the "requirements file format" or "requirements file". My bad!

I'll make a suggestion on top of your PR for that.

---

_@woodruffw reviewed on 2026-01-02 15:56_

---

_Review comment by @woodruffw on `docs/pip/dependencies.md`:41 on 2026-01-02 15:56_

@svlandeg maybe this?

```suggestion
It is also common to use a lightweight [requirements file format](https://pip.pypa.io/en/stable/reference/requirements-file-format/) to declare the dependencies for the
```

I just checked and we have a similar link in the export docs 🙂 

---

_Comment by @svlandeg on 2026-01-02 16:11_

Yep, makes sense, definitely less confusing and more informative with the link. Thanks!

---

_Renamed from "Fix extension of requirements file in docs" to "Clarify requirements file format in docs" by @svlandeg on 2026-01-02 16:14_

---

_@woodruffw approved on 2026-01-02 16:40_

Thanks @svlandeg!

---

_Merged by @woodruffw on 2026-01-02 16:40_

---

_Closed by @woodruffw on 2026-01-02 16:40_

---

_Branch deleted on 2026-01-02 17:23_

---

_Label `documentation` added by @konstin on 2026-01-06 09:51_

---
