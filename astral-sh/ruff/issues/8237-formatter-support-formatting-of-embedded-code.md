```yaml
number: 8237
title: "☂️ Formatter: Support formatting of embedded code"
type: issue
state: open
author: MichaReiser
labels:
  - formatter
  - wish
assignees: []
created_at: 2023-10-26T02:56:06Z
updated_at: 2025-11-03T15:26:30Z
url: https://github.com/astral-sh/ruff/issues/8237
synced_at: 2026-01-12T15:54:47Z
```

# ☂️ Formatter: Support formatting of embedded code

---

_@MichaReiser_

Support formatting Python code embedded in other languages like:

* Markdown
* reStructuredText
* HTML
* ... 

The goal of this issue is not that we implement support for all these languages but to build up the infrastructure to run ruff (at least the formatter) on files that contain embedded python code and format it. Ideally, the infrastructure would, in the future, allow us to support arbitrary nesting: 

* Format SQL in Python
* Format Markdown in Python
* ... 

Prettier and JetBrains code formatter do an excellent job at this.

Related: 
* https://github.com/astral-sh/ruff/issues/7146
* https://github.com/astral-sh/ruff/issues/3792

---

_Label `formatter` added by @MichaReiser on 2023-10-26 02:56_

---

_Label `wish` added by @MichaReiser on 2023-10-26 02:56_

---

_Renamed from "☂️ Format Python code embedded in other languages" to "☂️ Formatter: Support formatting of embedded code" by @MichaReiser on 2023-10-26 02:58_

---

_Comment by @dhruvmanila on 2023-10-26 03:10_

> The goal of this issue is not that we implement support for all these languages but to build up the infrastructure to run ruff (at least the formatter) on files that contain embedded python code and format it.

I think we should keep the linter in mind while designing the infrastructure as, and I'm not 100% sure but it's mainly my intuition from working on the Notebook support, it's more than likely than we get the formatter support for _free_. Free in the sense that it'll require considerably less effort on the formatter side once the infrastructure is in place.

---

_Comment by @MichaReiser on 2023-10-26 03:14_

I agree, but I wanted to keep this issue scoped. The linter and formatter likely have similar requirements and need similar infrastructure, but with slight nuances. 

---

_Comment by @ddelange on 2023-10-26 06:41_

potential duplicate of https://github.com/astral-sh/ruff/issues/3792

---

_Comment by @henryiii on 2023-10-26 16:25_

https://github.com/adamchainz/blacken-docs does this with black and markdown, ReST, and LaTeX. You can see the block types supported there. (python, pycon, etc). Even better, maybe  the block types could be configurable, say setting `md-blocks = ["python", "ipython", "{code-cell} ipython3"]` would allow you to run on python, ipython, and executable Python (see https://jupyterbook.org/en/stable/file-types/myst-notebooks.html). 

---

_Comment by @KelSolaar on 2024-01-29 04:44_

Hello,

I would be keen to see doctests formatting, e.g.:

```python
def least_square_mapping_MoorePenrose(
    y: ArrayLike, x: ArrayLike
) -> NDArrayFloat:
    """
    Compute the *least-squares* mapping from dependent variable :math:`y` to
    independent variable :math:`x` using *Moore-Penrose* inverse.

    Parameters
    ----------
    y
        Dependent and already known :math:`y` variable.
    x
        Independent :math:`x` variable(s) values corresponding with :math:`y`
        variable.

    Returns
    -------
    :class:`numpy.ndarray`
        *Least-squares* mapping.

    References
    ----------
    :cite:`Finlayson2015`

    Examples
    --------
    >>> prng = np.random.RandomState(2)
    >>> y = prng.random_sample((24, 3))
    >>> x = y + (    prng.random_sample(    (24, 3)) - 0.5) * 0.5
    >>> least_square_mapping_MoorePenrose(y, x)  # doctest: +ELLIPSIS
    array([[ 1.0526376...,  0.1378078..., -0.2276339...],
           [ 0.0739584...,  1.0293994..., -0.1060115...],
           [ 0.0572550..., -0.2052633...,  1.1015194...]])
    """

    y = np.atleast_2d(y)
    x = np.atleast_2d(x)

    return np.dot(np.transpose(x), np.linalg.pinv(np.transpose(y)))
```

*Ruff* currently does not format anything inside `>>>` and `...` in docstrings. I managed to drop *Black* and replace it with *Ruff* but I still depend on *adamchainz/blacken-docs*.

Cheers,

Thomas

---

_Comment by @MichaReiser on 2024-01-29 07:25_

Hey @KelSolaar 

Docstring code block formatting is supported but off by default. You can enable it in your settings using [`format.docstring-code-format = true`](https://docs.astral.sh/ruff/settings/#format-docstring-code-format)

---

_Comment by @KelSolaar on 2024-01-29 07:42_

Hi @MichaReiser,

I have it enabled but it does not seem to format the above.

Cheers,

Thomas

---

_Comment by @MichaReiser on 2024-01-29 08:22_

@KelSolaar 

The issue is that 

```python
array([[ 1.0526376...,  0.1378078..., -0.2276339...],
           [ 0.0739584...,  1.0293994..., -0.1060115...],
           [ 0.0572550..., -0.2052633...,  1.1015194...]])
```

is not valid python syntax (because of the `...`). The formatter can only format examples that are valid python.

---

_Comment by @KelSolaar on 2024-01-29 08:30_

Right I see! So this particular code output is used as a [doctest](https://docs.python.org/3/library/doctest.html) where any whitespace counts so it should NOT be formatted, ever. Ruff formatter should ignore those outputs fully which is what `adamchainz/blacken-docs` does. Hope it does make sense!

---

_Comment by @KelSolaar on 2024-01-29 18:36_

For that specific case, Ruff should only consider docstring lines starting with either `>>>` or `...`. There might be some subtleties and I would check the doctests parser to confirm but it is the main idea.

---

_Comment by @henryiii on 2024-01-29 19:46_

That example should use the [`pycon` lexer](https://pygments.org/docs/lexers/#pygments.lexers.python.PythonConsoleLexer) (similar to how "console" is used for console input with `$`/`#`). Anything in the `python` language should be valid Python and formatted, so I think Ruff is doing the right thing trying to format it (and failing). Supporting pycon and formatting the "python" parts would be nice, though! (Note that `pycon` is supported elsewhere, including in markdown like here on GitHub)

If no language is given (such code with a 4-space indent), I think it should be treated as whatever the default is (which IIRC might be Python).

---

_Comment by @ddelange on 2024-01-29 20:06_

corresponding [google-style](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/) doctest:

```py
def least_square_mapping_MoorePenrose(
    y: ArrayLike, x: ArrayLike
) -> NDArrayFloat:
    """
    Compute the *least-squares* mapping from dependent variable :math:`y` to
    independent variable :math:`x` using *Moore-Penrose* inverse.

    Examples:
        >>> prng = np.random.RandomState(2)
        >>> y = prng.random_sample((24, 3))
        >>> x = y + (    prng.random_sample(    (24, 3)) - 0.5) * 0.5
        >>> least_square_mapping_MoorePenrose(y, x)  # doctest: +ELLIPSIS
        array([[ 1.0526376...,  0.1378078..., -0.2276339...],
               [ 0.0739584...,  1.0293994..., -0.1060115...],
               [ 0.0572550..., -0.2052633...,  1.1015194...]])
    """

    y = np.atleast_2d(y)
    x = np.atleast_2d(x)

    return np.dot(np.transpose(x), np.linalg.pinv(np.transpose(y)))
```

---

_Comment by @aneeshusa on 2024-03-04 23:10_

+1 to this - beyond `blacken-docs`, `shed` which I'm currently switching from has this feature too!

---

_Comment by @paddyroddy on 2025-11-03 15:26_

> potential duplicate of [#3792](https://github.com/astral-sh/ruff/issues/3792)

I would say it was. I made that issue for this same desired feature.

---
