```yaml
number: 11357
title: Strangeness with implicit string concatenation, f-strings, and rules F541, RUF027
type: issue
state: closed
author: dougthor42
labels:
  - question
assignees: []
created_at: 2024-05-10T03:15:48Z
updated_at: 2024-05-10T14:48:07Z
url: https://github.com/astral-sh/ruff/issues/11357
synced_at: 2026-01-12T15:54:51Z
```

# Strangeness with implicit string concatenation, f-strings, and rules F541, RUF027

---

_@dougthor42_

## Summary

Seems like [f-string-missing-placeholders (F541)](https://docs.astral.sh/ruff/rules/f-string-missing-placeholders/) and [missing-f-string-syntax (RUF027)](https://docs.astral.sh/ruff/rules/missing-f-string-syntax/) don't play nicely with implicit string concatenation.

```console
$ ruff --version
ruff 0.4.4
$ python --version
Python 3.8.13
```

## Examples:

Commands used:

```console
$ ruff check --select F541 --preview --isolated ruff_bug.py
$ ruff check --select RUF027 --preview --isolated ruff_bug.py
```

<table>
<tr>
<th>Code</th>
<th>Expected F541</th>
<th>Actual F541</th>
<th>Expected RUF027</th>
<th>Actual RUF027</th>
</tr>

<tr>
<td>

```python
# ruff_bug.py
a = "hello"
print(
    f"some text here"
    f" bar {a}."
    f" {a}"
)
```

</td>
<td>Error on line 4</td>
<td><code>All checks passed!</code></td>
<td><code>All checks passed!</code></td>
<td><code>All checks passed!</code></td>
</tr>

<tr>
<td>

```python
# ruff_bug.py
a = "hello"
print(
    f"some text here"
    f" bar {a}."
    " {a}"
)
```

</td>
<td>Error on line 4</td>
<td><code>All checks passed!</code></td>
<td>Error on line 6</td>
<td><code>All checks passed!</code></td>
</tr>

<tr>
<td>

```python
# ruff_bug.py
a = "hello"
print(
    "some text here"
    f" bar {a}."
    " {a}"
)
```

</td>
<td><code>All checks passed!</code></td>
<td><code>All checks passed!</code></td>
<td>Error on line 6</td>
<td><code>All checks passed!</code></td>
</tr>

<tr>
<td>

```python
# ruff_bug.py
a = "hello"
print(
    f"some text here"
    " bar {a}."
    " {a}"
)
```

</td>
<td>Error on line 4</td>
<td>Error on line 4</td>
<td>Error on line 5 and 6</td>
<td><code>All checks passed!</code></td>
</tr>

<tr>
<td>

```python
# ruff_bug.py
a = "hello"
print(
    "some text here"
    " bar {a}."
    f" {a}"
)
```

</td>
<td><code>All checks passed!</code></td>
<td><code>All checks passed!</code></td>
<td>Error on line 5</td>
<td><code>All checks passed!</code></td>
</tr>
</table>

---

_Comment by @dhruvmanila on 2024-05-10 05:17_

Thank you providing such a detailed set of examples along with the diagnosis!

Regarding `F541`, we decided to maintain this behavior where the `F541` rule is only flagged when _all_ of the f-strings in an implicit string concatenation doesn't have any placeholders. Refer to https://github.com/astral-sh/ruff/issues/10885 for more details.

I've pasted all of the examples in the playground (https://play.ruff.rs/0de4d4a3-bbb3-44bd-b163-2778dba84f64) and I'm seeing the expected behavior for `RUF027` for examples 2, 3, 4, and 5. 

And, I'm seeing the same behavior on the CLI as well:
```console
$ ruff check --select RUF027 --preview --isolated src/RUF027.py --output-format=concise
src/RUF027.py:14:5: RUF027 Possible f-string without an `f` prefix
src/RUF027.py:22:5: RUF027 Possible f-string without an `f` prefix
src/RUF027.py:29:5: RUF027 Possible f-string without an `f` prefix
src/RUF027.py:30:5: RUF027 Possible f-string without an `f` prefix
src/RUF027.py:37:5: RUF027 Possible f-string without an `f` prefix
Found 5 errors.
No fixes available (5 hidden fixes can be enabled with the `--unsafe-fixes` option).
```

I'm not sure why you're seeing different behavior for `RUF027`. Can you re-check?

---

_Label `question` added by @dhruvmanila on 2024-05-10 05:17_

---

_Comment by @dougthor42 on 2024-05-10 14:30_

### F541

Ah ok, I didn't realize F541 was expected behavior.:+1:. I plan on making a PR to update the docs https://docs.astral.sh/ruff/rules/f-string-missing-placeholders/ to include something like your comment. Perhaps:

> **Note regarding implicit string concatenation:** In order to maintain compatibilty with the source pyflakes rule, this rule is only flagged when _all_ of the f-strings in an implicit string concatenation don't have any placeholders. For example:
>
> <python code showing example>
>
> Please see Issue #10885 for more details.


### RUF027

Strange! Yeah I'll double check.

...

Oh son of a b*. My CLI arg was set to RUF02**9** :facepalm: 

Sorry about that! It looks like everything is WAI so I'll close this. Thanks for double checking!

---

_Closed by @dougthor42 on 2024-05-10 14:30_

---

_Comment by @dhruvmanila on 2024-05-10 14:36_

No worries, thanks for confirming!

> Ah ok, I didn't realize F541 was expected behavior.:+1:. I plan on making a PR to update the docs [docs.astral.sh/ruff/rules/f-string-missing-placeholders](https://docs.astral.sh/ruff/rules/f-string-missing-placeholders/) to include something like your comment. Perhaps:

Yeah, I think that would be useful

---
