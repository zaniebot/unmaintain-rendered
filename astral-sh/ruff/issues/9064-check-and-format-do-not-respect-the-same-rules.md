```yaml
number: 9064
title: "`check` and `format` do not respect the same rules"
type: issue
state: closed
author: aaronmarkey
labels:
  - question
assignees: []
created_at: 2023-12-09T00:50:45Z
updated_at: 2024-06-18T06:30:00Z
url: https://github.com/astral-sh/ruff/issues/9064
synced_at: 2026-01-12T15:54:48Z
```

# `check` and `format` do not respect the same rules

---

_@aaronmarkey_

I've a simple reproducible issue: Ruff's `check` and `format` commands do not respect the same configuration. 

In some directory the two following files:

```python
# example.py
if True is True and True is False and False is True and 1 == 2 and 2 == 9 and 4 == 7 and 5 == True:
    print(1)


'''asdf'''

'adf'

a = 'asdf'
```

```toml
# ruff.toml
line-length=80
```

I would expect running `ruff check example.py` to have a number of issues related to line length and quote type. However, running that returns `0`. But, running `ruff format example.py` results in the following file: 

```python
# example.py
if (
    True is True
    and True is False
    and False is True
    and 1 == 2
    and 2 == 9
    and 4 == 7
    and 5 == True
):
    print(1)


"""asdf"""

"adf"

a = "asdf"

```


So my issue: Is it expected that ruff's formatter and checker don't follow one another? If so, why are there shared configs for both at top level which both are documented as following if only the formatter follows them?

---

_Comment by @zanieb on 2023-12-09 00:57_

Hi! We do not expect the linter to reimplement the behavior of the formatter. We intend for `ruff check` to highlight formatter violations eventually (see #8232), but until then you'll either need to check formatting separately _or_ make use of lint rules that check formatting.

For example, `E501` checks line length so if you select that you'll see a violation as you expect:

```
❯ ruff check example.py --select E501
example.py:1:89: E501 Line too long (99 > 88)
Found 1 error.
```

And the `flake8-quote` rules check for quote usage:

```
❯ ruff check example.py --select Q
example.py:5:1: Q001 [*] Single quote multiline found but double quotes preferred
example.py:7:1: Q000 [*] Single quotes found but double quotes preferred
example.py:9:5: Q000 [*] Single quotes found but double quotes preferred
Found 3 errors.
[*] 3 fixable with the `--fix` option.
```

The linter and formatter work very differently, so it's challenging for us to provide the same style of diagnostics. We do not expect to encourage usage of lint rules that overlap with the formatter in the long run.

---

_Label `question` added by @zanieb on 2023-12-09 00:57_

---

_Comment by @aaronmarkey on 2023-12-09 01:02_

So in order to lint a project to it's fullest extent, I need to chain hundreds of `--select`s? 

---

_Comment by @zanieb on 2023-12-09 01:05_

@aaronmarkey You can select rules in your `pyproject.toml` file — it is very common for projects to choose a subset of the rules since we implement so many different plugins. You can also select `ALL` then ignore rules that you do not want, however we don't generally suggest that. We do not include rules that overlap with the formatter by default, so if you want to have formatter violations in `check` you will need to enable those until we implement #8232.

---

_Comment by @aaronmarkey on 2023-12-09 01:12_

Is there a set of rules I can supply `lint.select` which will match whatever the `format` command uses? 

---

_Comment by @zanieb on 2023-12-09 01:23_

No, the `format` command is an entirely different implementation than the linter and it is not feasible to display line by line violations as done in the linter. The background on our goals in [the announcement blog post](https://astral.sh/blog/the-ruff-formatter) or the discussion in #8232 may be useful.

Perhaps https://docs.astral.sh/ruff/formatter/#conflicting-lint-rules would be helpful if you want lint rules that overlap with formatting. As I said though, we'd highly recommend disabling formatting related lint rules and instead using `ruff format --check`.

---

_Comment by @tylerlaprade on 2023-12-14 16:40_

@aaronmarkey, the linter and formatter serve entirely different purposes. Lint is for detecting possible bugs, while format is to standardize whitespace so humans don't have to think about it. I'm not sure which of those purposes you're seeking to achieve, but if you want to run a script that tells you if you have any formatting "violations" (things which would be automatically reformatted), then you can run `ruff format --check` or `ruff format --diff`.

---

_Comment by @charliermarsh on 2023-12-15 00:44_

Gonna close based on the explanations above.

---

_Closed by @charliermarsh on 2023-12-15 00:44_

---

_Comment by @Gatsby-Lee on 2024-06-18 06:29_

I understand the goal of the design, but inconvenient. 

---
