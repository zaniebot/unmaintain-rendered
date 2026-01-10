```yaml
number: 7414
title: "Formatter: wrap comments (`E501`)"
type: issue
state: open
author: kdeldycke
labels:
  - formatter
  - wish
assignees: []
created_at: 2023-09-15T18:13:51Z
updated_at: 2025-05-02T21:53:54Z
url: https://github.com/astral-sh/ruff/issues/7414
synced_at: 2026-01-10T11:09:49Z
```

# Formatter: wrap comments (`E501`)

---

_Issue opened by @kdeldycke on 2023-09-15 18:13_

Playing with the beta formatter, I was curious why it did not wrap long comments.

See the following `long.py` file:
```python
def test():
    # This is a very very very very very very very very very very very very very very very very very long comment.
    pass
```

Running the latest `ruff` version, the file is left unchanged, while I expected the long comment line to be wrapped:
```shell-session
$ ruff --version
ruff 0.0.289

$ ruff format ./long.py
warning: `ruff format` is a work-in-progress, subject to change at any time, and intended only for experimentation.
1 file left unchanged
```

I then realized that [Black itself doesn't format comments](https://github.com/psf/black/issues/181#issuecomment-385326100). Even in its last iteration:
```shell-session
$ black --version
black, 23.9.1 (compiled: yes)
Python (CPython) 3.11.5

$ black --preview ./long.py
All done! ✨ 🍰 ✨
1 file left unchanged.
```

Wrapping comments would be a nice quality of life improvement to auto-fix a number of `E501` rule violations.

---

_Comment by @kdeldycke on 2023-09-15 18:16_

Possibly related issues:
- https://github.com/astral-sh/ruff/issues/7361
- https://github.com/astral-sh/ruff/issues/5630
- https://github.com/astral-sh/ruff/issues/3894
- https://github.com/astral-sh/ruff/issues/1904#issuecomment-1671613520
- https://github.com/astral-sh/ruff/issues/4595
- https://github.com/astral-sh/ruff/issues/6269
- https://github.com/astral-sh/ruff/issues/7400
- https://github.com/astral-sh/ruff/issues/1904
- https://github.com/astral-sh/ruff/issues/2402

---

_Comment by @zanieb on 2023-09-15 18:17_

Hey @kdeldycke — I think there are some fair concerns raised in the Black issue but thanks for starting the discussion!

Out of curiosity, would docstring formatting resolve most of your needs here?

---

_Comment by @kdeldycke on 2023-09-15 18:28_

In the mean time, `autopep8` is supporting comment wrapping. You can perform this task and this only with the following one-liner:
```shell-session
$ autopep8 --in-place --max-line-length 88 --select E501 --aggressive ./long.py
```
```shell-session
$ cat ./long.py
```
```python
def test():
    # This is a very very very very very very very very very very very very
    # very very very very very long comment.
    pass
```
```shell-session
$ autopep8 --version
autopep8 2.0.4 (pycodestyle: 2.11.0)
```

---

_Comment by @kdeldycke on 2023-09-15 18:38_

> Hey @kdeldycke — I think there are some fair concerns raised in the Black issue but thanks for starting the discussion!

Oh yes. I'm just starting to understand the complexity of the issue. It's hard to generalize comment autoformat. Especially because of the line-dependency of `# pragma` kind of comments. I haven't looked into ruff code so I have no idea how hard and how feasible is it to handle these two situations.

> Out of curiosity, would docstring formatting resolve most of your needs here?

Oh you mean moving this long comment into a docstring? On paper yes. And yes, I guess when comments become that long, it is time to consider moving them into your function's docstring.

I still have the case in which I need long comments: to not have them appears in the auto-generated Sphinx documentation. I have some places in my code where I need long explanation of implementation details, but don't want to distract the user of my library with these implementation details in the top-level documentation.

---

_Renamed from "Formatter: wrap comments" to "Formatter: wrap comments (`E501`)" by @kdeldycke on 2023-09-15 18:44_

---

_Label `formatter` added by @charliermarsh on 2023-09-18 16:31_

---

_Label `wish` added by @charliermarsh on 2023-09-18 16:31_

---

_Comment by @ngnpope on 2024-03-12 10:13_

I've just come across this again and wanted to throw in something for consideration if this gets implemented...

Some directives in Sphinx, e.g. [`autodata`](https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html#directive-autodata), have a special comment notation to indicate that the comment should be used for documentation, e.g.

```python
#: A generic type variable.
T = TypeVar("T")
```

It would be nice to preserve the correct comment marker when wrapping, i.e. the following:

```python
#: This is a very very very very very very very very very very very very very very very very very long comment.
MAGIC_NUMBER = 123
```

...after formatting would produce:

```python
#: This is a very very very very very very very very very very very very very very very
#: very very long comment.
MAGIC_NUMBER = 123
```

...and not this:

```python
#: This is a very very very very very very very very very very very very very very very
# very very long comment.
MAGIC_NUMBER = 123
```

Another thing for consideration might be what happens with comment tags, as defined in `lint.task-tags`. How should these wrap?

Like this?

```python
# TODO: This is a very very very very very very very very very very very very very very
# very very very long comment.
```

Or like this? _(Which I personally like...)_

```python
# TODO: This is a very very very very very very very very very very very very very very
#       very very very long comment.
```

---

_Comment by @skyfenton on 2025-04-03 22:18_

+1 on this, using yet another VS Code extension (Rewrap) to handle this doesn't always fix up paragraphs correctly :(

---

_Comment by @real-or-random on 2025-04-04 06:36_

I think if this is ever implemented, it should have a separate configurable line length because [PEP8](https://peps.python.org/pep-0008/#maximum-line-length) suggests different line lengths for code and comments/docstrings.

---

_Comment by @rudy-lath-vizio on 2025-04-22 15:38_

+1 This would be a godsent feature

---

_Comment by @swirle13 on 2025-05-02 21:29_

> I think if this is ever implemented, it should have a separate configurable line length because [PEP8](https://peps.python.org/pep-0008/#maximum-line-length) suggests different line lengths for code and comments/docstrings.

Found this issue due to looking into exactly this, using Ruff. I'm wrapping up defining a team/project coding standards doc and used as much of PEP8 and Google Python style guide. I'm using as many tools to automate the enforcement of our coding standards, but I couldn't find a way to check/force comment line lengths separately from code. I was hoping to enforce with Ruff a max line length for any code to 88 chars and autoformat comments that exceed 72 lines, per PEP8.

---

_Comment by @Avasam on 2025-05-02 21:53_

On the subject of autopep8, I'll share this list I made comparing parity with it: https://github.com/astral-sh/ruff/discussions/9057#discussioncomment-7850584 (of course `E501` isn't checked)

I think it would be fair for an `E501` autofix to always be marked as `unsafe` (since it really depends on human intentions).
And to always ignore lines with special comments that Ruff cares to identify (`noqa:`, `type:`, `ruff:`, `isort:`, etc.)

For more prior art, `clang-format` can also be looked at.

---
