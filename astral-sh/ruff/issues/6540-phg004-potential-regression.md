```yaml
number: 6540
title: "PHG004: Potential regression"
type: issue
state: open
author: AA-Turner
labels:
  - needs-decision
assignees: []
created_at: 2023-08-13T19:56:06Z
updated_at: 2023-12-26T20:37:50Z
url: https://github.com/astral-sh/ruff/issues/6540
synced_at: 2026-01-12T15:54:46Z
```

# PHG004: Potential regression

---

_@AA-Turner_

Recently on updating Ruff, I started getting ``PGH004`` errors for a paragraph comment describing ``#noqa`` handling [^1]. I've attached a minimal reproducer below; the errors started appearing in Ruff 0.0.281 and a warning which previously appeared is now absent in Ruff 0.0.284.

My suggested remedy would be to only match a ``noqa`` comment if there are only whitespace tokens between the comment marker and the ``noqa`` token.

A

```python
# A sentence talking about using #noqa in prose.
# A new sentence talking about using #noqa.
# A third sentence talking about using #noqa
```

```console
$ pip install "ruff==0.0.280" --quiet
$ ruff check pgh004.py --isolated --select PGH004
$ pip install "ruff==0.0.281" --quiet
$ ruff check pgh004.py --isolated --select PGH004
warning: Invalid `# noqa` directive on pgh004.py:2: expected `:` followed by a comma-separated list of codes (e.g., `# noqa: F401, F841`).
pgh004.py:1:34: PGH004 Use specific rule codes when using `noqa`
pgh004.py:3:40: PGH004 Use specific rule codes when using `noqa`
Found 2 errors.
$ pip install "ruff==0.0.284" --quiet
$ ruff check pgh004.py --isolated --select PGH004
pgh004.py:1:34: PGH004 Use specific rule codes when using `noqa`
pgh004.py:3:40: PGH004 Use specific rule codes when using `noqa`
Found 2 errors.
```

[^1]: The specific pararaph is:

            # There is no point in having #noqa on literal blocks because
            # they cannot contain references.  Recognizing it would just
            # completely prevent escaping the #noqa.  Outside of literal
            # blocks, one can always write \#noqa.


---

_Comment by @charliermarsh on 2023-08-13 20:38_

> My suggested remedy would be to only match a `noqa` comment if there are only whitespace tokens between the comment marker and the `noqa` token.

Do you mind expanding on this suggestion? What would you consider examples where we should and shouldn't match on the token?

---

_Comment by @AA-Turner on 2023-08-13 20:41_

I suppose I was envisioning something similar to ``r'(?i)# *noqa:(.*)'``, but clearly this would look different in terms of tokens.

A common occurance is to have both a mypy ignore and a noqa comment on the same line, but I have always seen each prefixed with a ``'#'``.

A

---

_Comment by @charliermarsh on 2023-08-13 20:45_

I think my suggestion would be to just remove the `#` prefix from the prose. Does it add much? Making the parser more lax can actually lead to a more confusing experience -- e.g., the user writes `#noqa`, and it doesn't work, and they're not sure why. (Even if the argument is that they should be inserting a space between in such cases, we may _still_ want to warn them as such.) 

---

_Comment by @AA-Turner on 2023-08-13 20:50_

Ruff currently recognises ``#noqa`` (sans space) as a noqa comment, as the text above is erroring -- if adding a space were mandatory that'd be fine for me!

> Does it add much?

Here, yes, as it is an extremely succinct way to denote explicitly that the relevant code is parsing text exactly of that form.

Another possibility would be if the comment is after a logical newline with no other token, don't parse it as a noqa comment -- e.g. here the text was in a paragraph, not in-line after an expression/statement/etc. 

A

---

_Comment by @charliermarsh on 2023-08-14 18:20_

Ah yeah, that _was_ an intentional change, as I don't think it _should_ require a space. `# type: ignore` doesn't require a space, neither does `# nosec`, and I think requiring a space can make for a confusing user experience. Adding that requirement would fix the issue in this _specific_ case in a way that I'd consider coincidental, because it wouldn't really fix the more general problem of wanting to use `# noqa` as a literal in docstring prose.

---

_Label `needs-decision` added by @charliermarsh on 2023-08-14 18:20_

---

_Comment by @danieleades on 2023-12-26 20:37_

it's a reasonably common convention to surround code examples with backticks, as in-

```python
# A sentence talking about using `#noqa` in prose.
# A new sentence talking about using `#noqa`.
# A third sentence talking about using `#noqa`
```

treating backticks as escape characters is likely to be a fairly significant change in behaviour though, if it's applied across the board

---
