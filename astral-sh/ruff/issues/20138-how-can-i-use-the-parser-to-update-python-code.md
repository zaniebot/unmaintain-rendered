```yaml
number: 20138
title: How can I use the parser to update python code ? (insert type annotations, reformat)
type: issue
state: closed
author: LeGEC
labels:
  - question
assignees: []
created_at: 2025-08-28T15:26:19Z
updated_at: 2025-09-15T14:38:25Z
url: https://github.com/astral-sh/ruff/issues/20138
synced_at: 2026-01-12T15:54:57Z
```

# How can I use the parser to update python code ? (insert type annotations, reformat)

---

_@LeGEC_

### Question

**Context:**
My intention is to write a script to update the code of some python modules.

(for illustration: we currently have a workflow where, when updating fields in our database, we have to manually update both a schema file _and_ the model classes in some python modules; I want to write a utility which updates the existing python classes based on the list of columns in the schema file)

I have a poc, written in python, which sort of works, using the standard python `ast` module.
One limitation of `ast`, though, it that it does not preserve comments in the generated ast for a python module, which makes it cumbersome for my use case.

**My goal:**
So I am looking for another library, which would allow me to parse, update and reformat a python module.

Ideally, this library should be usable directly in python.

**My question:**
Is the python parser used in ruff published as a separate python module ?


### Version

_No response_

---

_Label `question` added by @LeGEC on 2025-08-28 15:26_

---

_Comment by @ntBre on 2025-08-28 15:33_

We don't currently have a Python API for any of our crates (or actually publish the Rust crates to crates.io). I think [LibCST](https://github.com/Instagram/LibCST) might be helpful, though. We use it for some of our fixes.

@amyreese might also have some suggestions

---

_Comment by @amyreese on 2025-08-28 18:37_

I second the recommendation for using LibCST. It has a parser written in Rust, so it's currently the fastest full CST usable from Python, and it includes its own ["codemod" mechanisms](https://libcst.readthedocs.io/en/latest/codemods_tutorial.html) for scripting transformations across entire repositories. The Python API is quite nice, with [good documentation](https://libcst.readthedocs.io/en/latest/) and an [online playground](https://libcst.me/) if you want to explore the CST for a bit of code.


---

_Closed by @MichaReiser on 2025-09-15 14:38_

---
