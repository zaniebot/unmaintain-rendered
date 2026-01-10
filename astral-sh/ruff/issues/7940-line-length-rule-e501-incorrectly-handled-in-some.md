```yaml
number: 7940
title: Line length rule, E501, incorrectly handled in some circumstances
type: issue
state: closed
author: LasseGravesenSaxo
labels: []
assignees: []
created_at: 2023-10-13T11:27:09Z
updated_at: 2024-09-05T02:08:58Z
url: https://github.com/astral-sh/ruff/issues/7940
synced_at: 2026-01-10T11:09:50Z
```

# Line length rule, E501, incorrectly handled in some circumstances

---

_Issue opened by @LasseGravesenSaxo on 2023-10-13 11:27_

### `ruff` version and settings
`ruff` version:
```
ruff --version
ruff 0.0.292
```

`ruff` settings:
```
--select ALL --ignore D203,D212,T201,D100,D101,D103 --line-length=120
```

### Description
The line length rule, E501, doesn't seem to always be handled correctly. I'm not really sure about all the different cases, but below I demonstrate one case where `ruff` complains about `# noqa: E501` being applied to a line where it's unnecessary, i.e: ```RUF100 [*] Unused `noqa` directive (unused: `E501`)```, but `flake8` correctly handles it.

### Code

```python
from dataclasses import dataclass


@dataclass
class Context:
    really_long_variable_name_that_exceeds_limit_set_at_some_point_probably_during_cli_command_of_the_ruff_tool: bool


def main() -> None:
    # Correct, ruff doesn't complain if 'noqa: E501' is added.
    context = Context(really_long_variable_name_that_exceeds_limit_set_at_some_point_probably_during_cli_command_of_the_ruff_tool=True)  # noqa: E501

    # Correct, ruff doesn't complain if 'noqa: E501' is added.
    _ = context.really_long_variable_name_that_exceeds_limit_set_at_some_point_probably_during_cli_command_of_the_ruff_tool  # noqa: E501

    # Incorrect, ruff complains if 'noqa: E501' is added: 'RUF100 [*] Unused `noqa` directive (unused: `E501`)'
    Context(really_long_variable_name_that_exceeds_limit_set_at_some_point_probably_during_cli_command_of_the_ruff_tool=True)  # noqa: E501

```

### Result

To check the linting result, I ran 2 commands: One for `ruff` and one for `flake8`:

`ruff` command: `ruff check ruff_e501_check.py --select ALL --ignore D203,D212,T201,D100,D101,D103 --line-length=120 --isolated`

`flake8` command: `flake8 --max-line-length=120 ruff_e501_check.py`

`ruff` output:
```
> ruff check ruff_e501_check.py --select ALL --ignore D203,D212,T201,D100,D101,D103 --line-length=120 --isolated
ruff_e501_check.py:17:128: RUF100 [*] Unused `noqa` directive (unused: `E501`)
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```
`ruff` incorrectly reports an unused noqa issue directly on line 17.

`flake8` output:
```
> flake8 --max-line-length=120 ruff_e501_check.py

```
`flake8` correctly does not report an issue.

If I remove the `# noqa: E501` from line 17, and re-run the commands I get this:

`ruff` output:
```
> ruff check ruff_e501_check.py --select ALL --ignore D203,D212,T201,D100,D101,D103 --line-length=120 --isolated

```
`ruff` incorrectly does not report an issue.

`flake8` output:
```
> flake8 --max-line-length=120 ruff_e501_check.py
ruff_e501_check.py:17:121: E501 line too long (125 > 120 characters)
```
`flake8` correctly reports an issue on line 17.


---

_Comment by @charliermarsh on 2023-10-13 11:48_

I’ll take a look at this specific case when I’m back at my computer, but note that we have slightly different rules around when we flag E501 violations which are spelled out in the rule documentation. For example, we don’t consider a line to be too long if _only_ a pragma comment (like a noqa) exceeds the line limit.

---

_Comment by @LasseGravesenSaxo on 2023-10-13 11:58_

The problematic line by itself is 125 characters long. I also tried with an even longer variable name, that was 139 characters by itself, and that also showed the same problem.

Looking at the rules, it seems like this could be a case where the rules are applied differently in ruff than in flake8.

https://docs.astral.sh/ruff/rules/line-too-long/

> In the interest of pragmatism, this rule makes a few exceptions when determining whether a line is overlong. Namely, it:
> Ignores lines that consist of a single "word" (i.e., without any whitespace between its characters).

My specific case is this, with this amount of indentation at the beginning:

```python
            self.x = Context(
                a_really_long_variable_name_that_exceeds_the_set_limits=config.A_REALLY_LONG_VARIABLE_NAME_THAT_EXCEEDS_THE_SET_LIMITS,  # noqa: E501, RUF100
            )
```

I suppose that this is a "single word".

If this makes sense to you also, you can close this. I was just surprised about it.


---

_Comment by @harpener on 2023-10-13 14:14_

Hi guys, I might have a similar problem. RUF100 claims that E501 is unused, but it isn't true, Pycharm shows PEP8 violation and Black wants to format the same line from the moment I remove the noqa. Ruff does not correctly recognize E501 in my case. It is a simple class variable with a long URL in its string. The noqa for E501 actually prevents Black from thinking the line needs to be formatted, do you think it is related?

---

_Comment by @harpener on 2023-10-13 14:17_

Hmm, it seems it is precisely because Ruff does not include this behavior of Black. If I add `fmt: skip` for Black, Ruff correctly reports `E501` 🤔 

---

_Comment by @charliermarsh on 2023-10-14 18:52_

I'm not sure I fully follow the issue, but Ruff will avoid flagging E501 if your string ends in a long URL that starts _before_ the line length limit.

---

_Comment by @charliermarsh on 2023-10-14 18:53_

(Happy to keep discussing and answering questions as always, but gonna close as I believe this is all consistent with our intended behavior and docs.)

---

_Closed by @charliermarsh on 2023-10-14 18:53_

---

_Comment by @bastimeyer on 2023-10-18 13:16_

> I'm not sure I fully follow the issue, but Ruff will avoid flagging E501 **if your string ends in a long URL that starts _before_ the line length limit.**

There are some issues with the current implementation IMO (`ruff 0.1.0`). Just found this thread before deciding to open a new one.

We just got a PR on our project which unintentionally abused this lenient `E501` behavior of ruff. The code change was this:
```py
some_object.some_method_call(args, with, some, keywords=1, and=2, a=3, url="https://...")
```

Since the URL string started just before the max `line-length` mark, ruff didn't trigger `E501` and our linting check surprisingly passed without the (first-time) contributor noticing.

I believe the second point listed [in the docs](https://docs.astral.sh/ruff/rules/line-too-long/) should only be considered if there's no other sensible way to format the code. Comments should also be handled differently compared to strings. And there should be a config option for disabling it completely.

For comments, the current implementation seems fine, regardless of the comment style. Comments with URLs can be appended to any line if desired, and long URLs can be kept in block comments without having to split them up.

For strings however, the E501 ignore-logic does only make sense when the code can't be reformatted easily, e.g. a line with a single string token that contains a long URL, or a long URL in a multi-line string, etc.

In the case I just mentioned, this should definitely trigger `E501`, because the tokens/token-pairs of the individual args/keywords+values can be set on separate lines. It doesn't make sense to ignore the whole line just because the last keyword value is a string with a URL which barely fits into the max line length threshold.

Hope that makes sense. Thanks for your consideration.

---

_Comment by @jpmckinney on 2024-09-05 01:49_

I use ruff in individual projects with strict rules, but I have an overarching CI workflow that runs on all projects, and uses less strict rules (flake8, isort, etc.). That other workflow complains about E501 in cases where ruff does not (mainly, long URLs).

To workaround, I can write my URL strings like `"https:""//...."`. That way, ruff doesn't recognize the URL, and RUF100 will not be raised.

Edit: Actually, writing URLs like that causes reformatting... Instead I added the flake8-length plugin to that other workflow. However, it's more lenient that ruff (allows long strings, regardless of the presence of spaces).

---
