```yaml
number: 3918
title: Consider publishing visitor trait, or the ast crate as a whole
type: issue
state: closed
author: sondrelg
labels: []
assignees: []
created_at: 2023-04-08T08:37:55Z
updated_at: 2023-05-08T22:24:58Z
url: https://github.com/astral-sh/ruff/issues/3918
synced_at: 2026-01-10T11:09:46Z
```

# Consider publishing visitor trait, or the ast crate as a whole

---

_Issue opened by @sondrelg on 2023-04-08 08:37_

Hi!

I just wanted to ask a quick question: 

I'm writing a little pre-commit hook to reformat some Python code, and I found the visitor trait in Ruff's `ruff_python_ast` crate extremely valuable. 

Right now I've just copied the whole thing into my little app.

Does the trait exist in some form in a published crate somewhere, and if not, would you consider publishing it in the future? Seems like something many would benefit from 🙂 

---

_Comment by @not-my-profile on 2023-04-08 19:00_

I think it would make sense to upstream the trait in question to RustPython.

---

_Comment by @youknowone on 2023-04-20 14:38_

Hello from RustPython. If anyone interested in, RustPython is open to take patch to add `trait Visitor` to RustPython.
`rustpython-ast` looks like appropriate place.

---

_Comment by @thejcannon on 2023-04-27 17:36_

@sondrelg check out https://github.com/RustPython/RustPython/pull/4930

---

_Comment by @youknowone on 2023-04-28 07:03_

If anyone use rustpython-ast or rustpython-parser, please take a look if splitting parser from the main repository will be helpful or not https://github.com/RustPython/RustPython/issues/4941

---

_Comment by @sondrelg on 2023-04-28 19:28_

My motivation is I need to be able to do the equivalent `ast.parse(...)` and python and implement a `NodeVisitor`, so pulling in the parser as a component seems like it might be ideal for that. My use-case probably isn't super important, but that's at least one datapoint for you 🙂 

---

_Comment by @youknowone on 2023-05-07 16:38_

@sondrelg could you check if this covers your use case? https://github.com/RustPython/Parser/pull/12

---

_Comment by @sondrelg on 2023-05-07 17:16_

Yes that seems to cover everything :heart: Here's the plugin for reference: https://github.com/snok/printf-log-formatter/blob/main/src/gen_visitor.rs


---

_Comment by @charliermarsh on 2023-05-08 22:24_

Awesome. Going to close since it looks like this will be covered from the RustPython side :heart:

---

_Closed by @charliermarsh on 2023-05-08 22:24_

---
