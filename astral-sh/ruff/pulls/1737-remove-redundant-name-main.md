```yaml
number: 1737
title: "Remove redundant __name__ == \"__main__\""
type: pull_request
state: closed
author: mgeier
labels: []
assignees: []
base: main
head: patch-1
created_at: 2023-01-08T11:08:29Z
updated_at: 2023-01-08T21:07:35Z
url: https://github.com/astral-sh/ruff/pull/1737
synced_at: 2026-01-12T05:36:32Z
```

# Remove redundant __name__ == "__main__"

---

_Pull request opened by @mgeier on 2023-01-08 11:08_

The name of the module is `__main__` anyway.

---

_Comment by @andersk on 2023-01-08 11:14_

No, it’s `ruff.__main__`.

---

_Comment by @mgeier on 2023-01-08 12:16_

OK, fair enough.

I'd argue that it should still be removed because it is confusing, and nobody in their right mind will `import ruff.__main__` anyway.

I consider using `if __name__ == "__main__"` in `__main__.py` an antipattern, I'm wondering if there's a lint against that ...

---

_Comment by @Jackenmen on 2023-01-08 12:34_

> I consider using if __name__ == "__main__" in __main__.py an antipattern

I think it can be helpful if you ever want to play with anything you defined in it in REPL and don't want to start the whole application in the process. It doesn't apply to the case here (`ruff.__main__` doesn't define anything) but in general, I don't think it's an antipattern.

---

_Comment by @mgeier on 2023-01-08 12:54_

I'm only talking about `__main__.py`, I'm not talking about any old `*.py` file.

I consider it an antipattern in `__main__.py`, as I said. I'm not making any statement about files with different names and this PR doesn't suggest changes to any other files.

> `ruff.__main__` doesn't define anything

Exactly, and it shouldn't. No `__main__.py` ever should.

If it would define something, this would IMHO also be an antipattern.

---

_Comment by @charliermarsh on 2023-01-08 21:07_

I appreciate the argument, but I'm gonna leave this as-is. I don't think it's causing any harm, and it's a fairly [common pattern](https://cs.github.com/?scopeName=All+repos&scope=&q=path%3A__main__.py+if+__name__+%3D%3D+%22__main__%22). Here it is in [CPython](https://github.com/python/cpython/blob/9a68ff12c3e647a4f8dd935919ae296593770a6b/Tools/peg_generator/pegen/__main__.py#L184).

---

_Closed by @charliermarsh on 2023-01-08 21:07_

---
