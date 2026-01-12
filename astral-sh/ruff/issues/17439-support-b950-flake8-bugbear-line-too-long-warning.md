```yaml
number: 17439
title: Support B950 flake8-bugbear line too long warning
type: issue
state: open
author: daviewales
labels:
  - rule
  - needs-design
assignees: []
created_at: 2025-04-17T01:30:21Z
updated_at: 2025-07-28T14:21:31Z
url: https://github.com/astral-sh/ruff/issues/17439
synced_at: 2026-01-12T15:54:55Z
```

# Support B950 flake8-bugbear line too long warning

---

_@daviewales_

### Summary

There was some discussion of this when `flake8-bugbear` was implemented (https://github.com/astral-sh/ruff/issues/389#issuecomment-1383237964). However, so far as I can tell, `B950` was not ultimately included.

I believe there are advantages to supporting `B950`, rather than using `E501` with a line length of 96:

- You keep your line length setting at 88, which means that auto-formatters will aim to keep lines below 88 characters (or whatever your preferred line length is).
- The warning says 'Line too long (100 > 88)', rather than 'Line too long (100 > 96)', which informs the user that the preferred line length is 88, not 96.
- You only trigger warnings when the line length is over 96.

The outcome is that _most_ of your codebase sticks below 88 characters, but in the rare occasion that your auto-formatter can't reduce the line length below 88 characters, and the line is between 88 and 96 characters, no warning will be raised.

Just because the police won't stop you for driving 85 in an 80 zone doesn't mean we should change all the speed limits to 85.

See the description of the rule from [flake8-bugbear](https://pypi.org/project/flake8-bugbear/):

> B950: Line too long. This is a pragmatic equivalent of pycodestyle’s E501: it considers “max-line-length” but only triggers when the value has been exceeded by more than 10%. noqa and type: ignore comments are ignored. You will no longer be forced to reformat code due to the closing parenthesis being one character too far to satisfy the linter. At the same time, if you do significantly violate the line length, you will receive a message that states what the actual limit is. This is inspired by Raymond Hettinger’s [“Beyond PEP 8” talk](https://www.youtube.com/watch?v=wf-BqAjZb8M) and highway patrol not stopping you if you drive < 5mph too fast. Disable E501 to avoid duplicate warnings. Like E501, this error ignores long shebangs on the first line and urls or paths that are on their own line:
>
> ``` python
> #! long shebang ignored
>
> # https://some-super-long-domain-name.com/with/some/very/long/paths
> url = (
>     "https://some-super-long-domain-name.com/with/some/very/long/paths"
> )
> ```

---

_Comment by @ntBre on 2025-04-17 14:49_

I think I understand the difference between the two rules and can see your point. The main idea I see in the other thread is setting `line-length` to 97, but we now also have [max-line-length](https://docs.astral.sh/ruff/settings/#lint_pycodestyle_max-line-length), which used in combination with `line-length` sounds to me like what you're after. Here's the example from the docs:

```toml
# The formatter wraps lines at a length of 88.
line-length = 88

[pycodestyle]
# E501 reports lines that exceed the length of 100.
max-line-length = 100
```

Is that still different from B950?

---

_Comment by @daviewales on 2025-04-22 02:52_

Thanks. Functionally that's the same.
However, the warning message is different.

With B950 and `line-length = 88`, the warning message for lines longer than 96 is:

    Line too long (100 > 88)

With E501, `line-length = 88` and `max-line-length = 96`, the warning message for lines longer than 96 is:

    Line too long (100 > 96)

The difference is that when a warning is raised, the first informs the user that the standard line length is 88 characters, while the second informs the user that the standard line length is 96 characters.

i.e. B950 keeps the speed limit at 88, but does not trigger a violation until you exceed 96.
E501 raises the speed limit to 96, but installs a cruise control in your car which tries to keep your speed below 88.

I think there's still value in B950.

---

_Label `rule` added by @MichaReiser on 2025-04-22 06:22_

---

_Comment by @MichaReiser on 2025-04-22 06:40_

Introducing `B950` as another rule isn't an option because we want to avoid having rules with overlapping responsibilities (that, when configured differently, may even result in conflicting diagnostics). 

I think my preferred solution here would be to adjust the message if a user configured both `max-line-length` and `line-length` and to include both lengths in the message. Any suggestions on how we could rephrase the message?

The alternatives are a new option but that just feels overkill. Or we wait for our new diagnostic system that allows attaching notes. The diagnostic could then explain that the global line-length limit is 88 and the max-line-length is 96.

---

_Comment by @daviewales on 2025-05-02 03:28_

What about the following?

    Line too long (100 > 96 > 88)

This includes the violating line length, the `max-line-length` and the target `line-length`.

A longer version of the message might say:

    Line too long (100 > max-line-length=96 > line-length=88)

---

_Label `help wanted` added by @MichaReiser on 2025-05-02 07:26_

---

_Comment by @Skylion007 on 2025-05-10 17:42_

Another question is how we suppress too long line B950 errors when using max-line-length?

---

_Label `help wanted` removed by @MichaReiser on 2025-05-12 07:31_

---

_Label `needs-design` added by @MichaReiser on 2025-05-12 07:31_

---

_Comment by @Dreamsorcerer on 2025-07-28 14:21_

B950 also ignores `pragma: no cover` comments, but I'm guessing that's just an oversight in ruff as it appears to ignore noqa/type:ignore comments (which I don't think the original E501 does).

There's a list of test cases in https://github.com/PyCQA/flake8-bugbear/blob/main/tests/eval_files/b950.py

---
