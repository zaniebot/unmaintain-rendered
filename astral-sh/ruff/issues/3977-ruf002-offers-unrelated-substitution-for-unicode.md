```yaml
number: 3977
title: RUF002 offers unrelated substitution for unicode quotation mark
type: issue
state: open
author: syntaxaire
labels:
  - fixes
assignees: []
created_at: 2023-04-14T19:10:27Z
updated_at: 2025-06-11T21:25:51Z
url: https://github.com/astral-sh/ruff/issues/3977
synced_at: 2026-01-10T11:09:46Z
```

# RUF002 offers unrelated substitution for unicode quotation mark

---

_Issue opened by @syntaxaire on 2023-04-14 19:10_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
With ruff 0.0.261, using the "RUF" rule, the following docstring:
```python
"""All the account’s items"""
```
produces the following error:

```
RUF002 [*] Docstring contains ambiguous unicode character `’` (did you mean ```?)
```

The Unicode character in the comment is U+2019 RIGHT SINGLE QUOTATION MARK which is drawn from upper right to lower left. The suggested substitution is the backtick character which is drawn from upper left to lower right - this character is not semantically related to the Unicode character.

This suggestion is automatically applied when `--fix` is specified, producing:
```python
"""All the account`s items"""
```

Is this to avoid messing with docstring quotes by not substituting the single quote character `'`? In that case I would suggest not making this Unicode character autofixable at all, rather than substituting an unrelated character.

---

_Comment by @charliermarsh on 2023-04-14 19:25_

Hm, yeah, that's an interesting substitution. We actually didn't create the substitution table -- it's taken from the same source as VS Code, and is intended to match VS Code's own list of confusing characters + their replacements. It looks like VS Code offers the same suggestion:

![Screen Shot 2023-04-14 at 3 24 50 PM](https://user-images.githubusercontent.com/1309177/232137713-75fc65b9-5152-4fd5-a024-7396b74921d6.png)

That's not to say that the table can't be improved, only to clarify that we didn't apply any of our own discretion in generating the list. We could consider changing that replacement to instead suggest `'` though?


---

_Label `question` added by @charliermarsh on 2023-04-14 19:25_

---

_Comment by @syntaxaire on 2023-04-14 19:35_

`'` has syntactic meaning, so it would not be able to be a naive fix.
```python
'All the account’s items.'
```
works as a docstring, although a dodgy one... Substituting `'` would make semantic sense but would cause a syntax error if autofixed.

---

_Comment by @syntaxaire on 2023-04-16 17:33_

I would make 2 suggestions for this issue:
1. Leave the existing behaviour but remove autofix for this.

or,

2. Substitute `'` for `’`, automatically adding an escape character if in a single quoted string.

For # 2, would it be possible to construct an anti-example where this suggestion could still change the meaning of code? For example, constructing a string and passing it to eval?

---

_Comment by @sbdchd on 2023-04-17 00:03_

I think auto fix should probably be disabled for this sort of thing since the underlying ambiguous characters might be in a i18n related string or some documentation (even if it's not a doc comment, e.g., Django column help text) that is intentional

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:16_

---

_Label `autofix` added by @charliermarsh on 2023-07-10 01:16_

---

_Comment by @flying-sheep on 2023-07-14 08:17_

Does anyone here see `'` and `’` on their screen without being able to visually distinguish them?

If not: why is `’` included in the table at all?

On a different note, it hurts my typographic sensibilities to see that specific substitution being made lol. I always see when people using grave accents instead of typographically correct quotes, and I always wince.

---

_Comment by @bodograumann on 2023-11-12 13:18_

What is being replaced originally comes from [Unicode directly](https://www.unicode.org/Public/security/14.0.0/confusables.txt), with only some minor overrides from vscode.
I think the main intention of this feature is to prevent malicious code.

That a grave accent is recommended here is an upstream bug imho. My PR contains some background information as well: https://github.com/hediet/vscode-unicode-data/pull/1

The discussion whether replacing with `'` is a good idea, because it might break code in context of a single-quoted string should probably be had, but I am also with you @flying-sheep , that `'` and `’` are distinct enough to be allowed.
In the mean time you can allow it explicitely in your project:

```toml
[tool.ruff]
allowed-confusables = [
    "’",  # Cf. https://github.com/astral-sh/ruff/issues/3977
]
```

---

_Comment by @kureta on 2024-01-28 16:48_

I have a similar problem. I get `RUF002 Docstring contains ambiguous `ı` (LATIN SMALL LETTER DOTLESS I). Did you mean `i` (LATIN SMALL LETTER I)?` but "ı" and "i" are separate letters in the Turkish alphabet.

Couldn't be sure if I should open a new issue. Please let me know if so.

---

_Comment by @charliermarsh on 2024-01-28 19:28_

@kureta - I think that's following the intent of the rule, since the goal is to identify confusable Unicode characters in otherwise-ASCII words. I'd suggest adding it to the `allowed-confusables` list if it makes sense in your project! https://docs.astral.sh/ruff/settings/#allowed-confusables

---

_Comment by @kureta on 2024-01-29 15:05_

@charliermarsh thanks, i didn't know that option. 

---

_Comment by @rirze on 2025-06-11 21:25_

Just ran into this while copy-pasting some documentation into docstrings. 

Is there any way we could specify a translation table for our projects? 

---
