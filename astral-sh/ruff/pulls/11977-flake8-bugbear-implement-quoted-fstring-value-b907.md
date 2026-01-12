```yaml
number: 11977
title: "[`flake8-bugbear`] Implement `quoted-fstring-value` (`B907`)"
type: pull_request
state: open
author: alexcdennis
labels:
  - rule
  - preview
assignees: []
base: main
head: quoted-fstring-value
created_at: 2024-06-21T21:54:14Z
updated_at: 2024-06-28T20:47:45Z
url: https://github.com/astral-sh/ruff/pull/11977
synced_at: 2026-01-12T15:55:40Z
```

# [`flake8-bugbear`] Implement `quoted-fstring-value` (`B907`)

---

_@alexcdennis_

## Summary

Implements rule B907 `quoted-fstring-value`.

Tracking Issue: #3758 

From the [bugbear documentation](https://github.com/PyCQA/flake8-bugbear/tree/18abb9fdec66430d36010ffcee2d7dc54c802d3a?tab=readme-ov-file#opinionated-warnings)
> B907: Consider replacing f"'{foo}'" with f"{foo!r}" which is both easier to read and will escape quotes inside foo if that would appear. The check tries to filter out any format specs that are invalid together with !r. If you're using other conversion flags then e.g. f"'{foo!a}'" can be replaced with f"{ascii(foo)!r}". Not currently implemented for python<3.8 or str.format() calls.

## Changes from original

The original rule in bugbear was marked as optional/opinionated (https://github.com/PyCQA/flake8-bugbear/pull/333) because there were several complaints about limitations (https://github.com/PyCQA/flake8-bugbear/issues/329, https://github.com/PyCQA/flake8-bugbear/issues/331, https://github.com/PyCQA/flake8-bugbear/issues/332, https://github.com/astral-sh/ruff/issues/1893#issuecomment-1383132618).

The cause of all these limitations was that the suggested fix (to always add `!r`) was too opinionated and could changed the semantics of the output.

As such, this implementation diverges from the original by changing the framing in the documentation and error messages to remove all reference to the problematic suggested fix. Instead, it opts for a more nuanced approach which requires the user to consider their own fix on a case-by-case basis, while also providing an opt out (the user may add a noop to the code).

## Test Plan

`cargo nextest run`

## Sources

Implementation inspired by [original bugbear implementation](https://github.com/PyCQA/flake8-bugbear/blob/18abb9fdec66430d36010ffcee2d7dc54c802d3a/bugbear.py#L1419)
Test cases include all test cases from the [original bugbear test file](https://github.com/PyCQA/flake8-bugbear/blob/18abb9fdec66430d36010ffcee2d7dc54c802d3a/tests/b907.py), plus extras

## Related/References

### ruff
#1893
https://github.com/astral-sh/ruff/issues/2954#issuecomment-1483594606

### bugbear
https://github.com/PyCQA/flake8-bugbear/issues/319


---

_Label `rule` added by @charliermarsh on 2024-06-23 16:53_

---

_Label `preview` added by @charliermarsh on 2024-06-23 16:53_

---

_Comment by @charliermarsh on 2024-06-26 00:41_

It seems like the check in bugbear suffered from a lot of false positives (and hence was moved to the opinionated section). Do you know how many of those are present in the implementation here? What are the limitations of the rule?

---

_Comment by @alexcdennis on 2024-06-27 01:10_

This rule has all of the same limitations as the original, since it is the same algorithm.

All of the limitations that I have seen suggested fall under the same issue: this rule may suggests a fix (adding `!r`) that changes the meaning of the output.
(1) If the value is not a string, adding `!r` changes the output (https://github.com/PyCQA/flake8-bugbear/issues/329, https://github.com/astral-sh/ruff/issues/1893#issuecomment-1383132618)
(2) If the value is a string where there is a different meaning between double quotes and single quotes, then `!r` can change this meaning (eg an SQL query https://github.com/PyCQA/flake8-bugbear/issues/331) (since `!r` considers double and single quotes to be interchangeable and will substitute one for the other at unexpected times)

Under the current framing (that adding `!r` is the correct fix), neither of these limitations can be overcome.
(1) Detecting whether or not the value is a string requires type inference
(2) This is no static rule that can understand the intent of the programmer for the arbitrary semantics of the string output

However, the real cause of both of these issues is the original framing that the proposed fix (adding `!r`) is correct in all situations. Thus, in order to overcome these limitations, we need to change this framing by removing that proposed fix altogether. (It is not clear to me that there is a fix that is correct in all situations)

For this reason, I originally tried to word the message as a suggestion for the user to consider if this is a problem on a case-by-case basis instead of as something that the user must fix. I think this still requires more rewording in order to make this clearer.

edit: rewritten in order to hopefully improve clarity

---

_Comment by @alexcdennis on 2024-06-28 19:13_

I have changed the framing around the rule by changing the suggested fix. I believe that this overcomes all previously suggested limitations, as discussed above.

Summary:
Instead of the original suggested fix of "always add `!r`", the new suggested fix is "consider on a case-by-case basis; if you want to signal that this is intended behavior, add a noop"

Open questions:
Is the new documentation/error message too wordy/nuanced/confusing?
If so, is there a way to reword it to make it clearer?

---
