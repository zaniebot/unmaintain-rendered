```yaml
number: 1125
title: Autofix F541 (f-string without any placeholders)?
type: issue
state: closed
author: keul
labels:
  - fixes
assignees: []
created_at: 2022-12-07T16:01:19Z
updated_at: 2023-01-03T03:28:33Z
url: https://github.com/astral-sh/ruff/issues/1125
synced_at: 2026-01-10T12:05:23Z
```

# Autofix F541 (f-string without any placeholders)?

---

_Issue opened by @keul on 2022-12-07 16:01_

I think that a string like:

```python
f"%H:%M"
```

Which reports:

```
F541 f-string without any placeholders
```

Can be automatically fixed by the ``--fix`` option without any side effect.

---

_Label `enhancement` added by @charliermarsh on 2022-12-07 16:11_

---

_Comment by @harupy on 2022-12-09 11:46_

@charliermarsh Can I work on this?

---

_Comment by @charliermarsh on 2022-12-09 15:14_

@harupy - Of course, go for it!

It might be easiest to go through LibCST since there are a lot of cases to handle and f-strings are kind of an oddity (by the time you get the AST, the parser has already combined some of the substrings).


---

_Assigned to @harupy by @charliermarsh on 2022-12-09 15:14_

---

_Comment by @harupy on 2022-12-10 01:00_

@charliermarsh Just removing `f` doesn't work?

---

_Comment by @harupy on 2022-12-10 01:26_

I see, just removing the top `f` doesn't work in this case:

```python
x = (
    f"a"
    f"b"
    "c"
)
```

---

_Comment by @charliermarsh on 2022-12-10 01:33_

Yeah exactly. Implicit string concatenations get transformed by the parser. (If any segment has an f prefix, the whole thing becomes a JoinedStr; but consecutive non-f segments get collapsed.)

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:15_

---

_Label `autofix` added by @charliermarsh on 2022-12-31 18:15_

---

_Comment by @niccolomineo on 2023-01-01 19:23_

Hi there, I am getting this false positive:

```
f"{myfunc():.1f}"
```

---

_Comment by @charliermarsh on 2023-01-01 19:41_

@niccolomineo - This was fixed in v0.0.205! Let me know if you continue to see it but I just tested it myself.

---

_Comment by @niccolomineo on 2023-01-01 19:45_

@charliermarsh the problem seems to be with VSCode's Ruff, actually.

Anyways, thank you for the ground you're covering with this module, it's nothing short of amazing!

---

_Comment by @charliermarsh on 2023-01-01 21:27_

Yeah the bundled version might have that error. I can cut a new pre-release today! (And thank you, that means a lot, truly!)

---

_Comment by @charliermarsh on 2023-01-01 22:40_

I cut a new pre-release. If you switch to the Ruff extension pre-release, you should get stuff v0.0.206 by default!

---

_Comment by @harupy on 2023-01-02 03:35_

@charliermarsh I'm implementing autofix for F541 and want to hear your thoughts on this approach. Let's say we have an implicitly-concatenated string that contains both f-strings and plain strings:

```python
(
  f""
  ""
  rf""
)
```

We can locate each f-string token and f-prefix position using RustPython's tokenizer. Once we locate them, we can raise F541 on **each token**. To fix them, we can apply `Fix::deletion` on each f-prefix position. Here's a demo:


https://user-images.githubusercontent.com/17039389/210193377-b1426cd1-8f9f-42af-b25b-a5cea5ab5bd3.mov






---

_Comment by @marscher on 2023-01-02 09:48_

good job! Thank you! :+1: 

---

_Comment by @harupy on 2023-01-02 10:08_

We can extend the approach above to find useless f-strings in an implicitly-concatenated `JoinedStr` containing `FormattedValue`s.

```python
(
  f"abc"  # <- we want to locate this
  f"{x}"
)
```

1. Tokenize the string into tokens.
2. The ones that don't contain `FormattedValue` are useless f-strings.

However, this may lead to bad performance because we need to tokenize all f-strings.


https://user-images.githubusercontent.com/17039389/210224488-433faa3e-8b6e-4d3d-842b-7e1df8cfcf4c.mov

Code: https://github.com/harupy/ruff/commit/f33539d5cabbb23cbaa211fdf363270ba5fa5544

---

_Comment by @charliermarsh on 2023-01-02 18:33_

@harupy - I think these approaches make sense! Maybe we start by only enforcing this in the first case? So that we don't have to tokenize every f-string (only those that don't contain any placeholders)?

_Arguably_, this is actually desirable:

```py
(
  f"abc"
  f"{x}"
)
```

Since with this version, you have different indentation levels for each string, you have to keep track of which parts have a prefix and which don't, etc.:

```py
(
  "abc"
  f"{x}"
)
```


---

_Closed by @charliermarsh on 2023-01-03 03:28_

---
