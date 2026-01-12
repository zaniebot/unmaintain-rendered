```yaml
number: 11414
title: "Avoid flagging `__future__` annotations as required for non-evaluated type annotations"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/future
created_at: 2024-05-13T17:22:46Z
updated_at: 2024-05-23T21:30:02Z
url: https://github.com/astral-sh/ruff/pull/11414
synced_at: 2026-01-12T15:55:38Z
```

# Avoid flagging `__future__` annotations as required for non-evaluated type annotations

---

_@charliermarsh_

## Summary

If an annotation won't be evaluated at runtime, we don't need to flag `from __future__ import annotations` as required. This applies both to quoted annotations and annotations outside of runtime-evaluated positions, like:

```python
def main() -> None:
    a_list: list[str] | None = []
    a_list.append("hello")
```

Closes https://github.com/astral-sh/ruff/issues/11397.


---

_Label `bug` added by @charliermarsh on 2024-05-13 17:22_

---

_Renamed from "Avoid flagging __future__ annotations as required for non-evaluated type annotations" to "Avoid flagging `__future__` annotations as required for non-evaluated type annotations" by @charliermarsh on 2024-05-13 17:22_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-05-13 17:23_

---

_Review requested from @dhruvmanila by @charliermarsh on 2024-05-13 17:23_

---

_Comment by @AlexWaygood on 2024-05-13 17:26_

Hmm, I wouldn't do this for quoted annotations. I think it was definitely intentional in the original flake8 plugin for PEP-585 annotations in quotes to trigger this check, because otherwise UP037 won't automatically remove the quotes for you without the `__future__` annotations import. I like the idea of ignoring annotations inside functions, though.

---

_Comment by @github-actions[bot] on 2024-05-13 17:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -3 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/corporate/lib/stripe.py#L3017'>corporate/lib/stripe.py:3017:29:</a> FA102 Missing `from __future__ import annotations`, but uses PEP 604 union
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/corporate/lib/stripe.py#L3115'>corporate/lib/stripe.py:3115:48:</a> FA102 Missing `from __future__ import annotations`, but uses PEP 604 union
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/corporate/lib/stripe.py#L915'>corporate/lib/stripe.py:915:40:</a> FA102 Missing `from __future__ import annotations`, but uses PEP 604 union
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FA102 | 3 | 0 | 3 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -3 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/corporate/lib/stripe.py#L3017'>corporate/lib/stripe.py:3017:29:</a> FA102 Missing `from __future__ import annotations`, but uses PEP 604 union
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/corporate/lib/stripe.py#L3115'>corporate/lib/stripe.py:3115:48:</a> FA102 Missing `from __future__ import annotations`, but uses PEP 604 union
- <a href='https://github.com/zulip/zulip/blob/91bd6e2b12d219de268cc7ad84782b61561262b5/corporate/lib/stripe.py#L915'>corporate/lib/stripe.py:915:40:</a> FA102 Missing `from __future__ import annotations`, but uses PEP 604 union
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FA102 | 3 | 0 | 3 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-05-13 17:53_

The diagnostic and documentation aren't really correct or consistent with that use-case though, at least as-written. The messaging suggests you need this to avoid runtime errors. Almost feels like it needs an explicit message (e.g., you could remove quotes by adding this), but in that case, isn't it _always_ true that adding `from __future__ import annotations` would let you remove quotes? Regardless of the syntax you're using?

---

_Comment by @charliermarsh on 2024-05-13 17:59_

I'm now wondering if that should be a separate rule or part of FA100? Since the type _can_ be written more succinctly if you import `from __future__ import annotations`, at the very least by removing the quotes?

---

_Comment by @AlexWaygood on 2024-05-13 18:04_

> The messaging suggests you need this to avoid runtime errors.

Huh, that's not how I've ever interpreted that message. The message is "Missing `from __future__ import annotations`, but uses PEP 585 collection" -- I always interpreted it as "Missing `from __future__ import annotations`, but uses PEP 585 collection [which could be written more simply if you added this `__future__ import]".

The original flake8 plugin explicitly mentions pyupgrade in its README as one of the motivations for the plugin: https://github.com/TylerYep/flake8-future-annotations?tab=readme-ov-file#flake8-future-annotations.

And I don't think this rule is nearly adequate to prevent all runtime errors arising from using newer typing syntax on older versions, nor should it try to do that.

> isn't it _always_ true that adding `from __future__ import annotations` would let you remove quotes? Regardless of the syntax you're using?

Not necessarily... eg. removing quotes from this annotation would cause a syntax error:

```py
def foo() -> "Some kind of string":
    pass
```

---

_Comment by @AlexWaygood on 2024-05-13 18:05_

> I'm now wondering if that should be a separate rule or part of FA100? Since the type _can_ be written more succinctly if you import `from __future__ import annotations`, at the very least by removing the quotes?

IDK, because I've always thought of "enabling other tools to auto-upgrade your annotations" as being the _primary_ motivation of the rule 😄

---

_Comment by @charliermarsh on 2024-05-13 18:08_

Are you mixing up FA100 and FA102? Or am I mixing them up? FA100 is the rule that tells you "you can write this more succinctly if you import `from __future__ import annotations`." This PR does not modify FA100.

---

_Comment by @charliermarsh on 2024-05-13 18:10_

This rule modifies FA102, which tells you that you're using an unsupported type annotation that _will_ work if you add `from __future__ import annotations`.

---

_Comment by @AlexWaygood on 2024-05-13 18:20_

Argh, I should have checked more thoroughly. Yikes, and sorry :(

Yes, I didn't realise there was a distinction between `FA100` and `FA102`. In that case, we should indeed make this change, as long as we still emit `FA100` on all these snippets. (But maybe we should also update `FA100` so that it's not emitted for annotations inside functions, as well, since the `__future__` import is irrelevant in that context?)

It would be good if the rule summary at https://docs.astral.sh/ruff/rules/#flake8-future-annotations-fa made the distinction between the rules here a bit clearer. 

---

_Comment by @charliermarsh on 2024-05-13 18:21_

No need to apologize, I was also unsure. My guess is that we _don't_ emit `FA100` on the quoted ones. I'll have to check...

---

_Comment by @inoa-jboliveira on 2024-05-13 18:52_

LGTM! Thank you for such swift reply

---

_@dhruvmanila approved on 2024-05-20 06:05_

I think this makes sense.

---

_Comment by @charliermarsh on 2024-05-21 18:55_

Confirmed that we still emit `FA100` on these.

---

_Merged by @charliermarsh on 2024-05-21 18:57_

---

_Closed by @charliermarsh on 2024-05-21 18:57_

---

_Branch deleted on 2024-05-21 18:57_

---

_Comment by @AlexWaygood on 2024-05-21 19:01_

> Confirmed that we still emit `FA100` on these.

TYVM!

---

_Comment by @TylerYep on 2024-05-23 21:25_

> If an annotation won't be evaluated at runtime, we don't need to flag from __future__ import annotations as required.

@charliermarsh  This is only accurate in Python versions 3.10+ I believe:
```
Python 3.8.16 (default, Dec 20 2022, 18:52:29) 
[Clang 14.0.3 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> x: list[int]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'type' object is not subscriptable
>>> from __future__ import annotations
>>> x: list[int]
>>> 

```

```
Python 3.10.13 (main, Aug 24 2023, 22:36:46) [Clang 14.0.3 (clang-1403.0.22.14.1)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> x: list[int]
>>>
```

---

_Comment by @TylerYep on 2024-05-23 21:28_

But quoted ones will work regardless of the Python version:
```
Python 3.8.16 (default, Dec 20 2022, 18:52:29) 
[Clang 14.0.3 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> x: "list[int]"
>>> 
```

I didn't read the PR thoroughly, but wanted to make sure the lint rule is only changed for the quoted version but still flags non-quoted lowercase types without future annotations?

---

_Comment by @charliermarsh on 2024-05-23 21:29_

@TylerYep - the logic here only applies to annotated assignments within function bodies, which I believe works on all Python versions:

```python
def f():
    x: list[int] = 1

f()
```

---

_Comment by @TylerYep on 2024-05-23 21:30_

Ah, makes sense, thank you for clarifying!

---
