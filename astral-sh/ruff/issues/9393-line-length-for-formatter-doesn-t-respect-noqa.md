```yaml
number: 9393
title: "line-length for formatter doesn't respect noqa: E501 comment"
type: issue
state: closed
author: mmohaveri
labels:
  - question
assignees: []
created_at: 2024-01-04T19:17:09Z
updated_at: 2024-01-09T03:14:45Z
url: https://github.com/astral-sh/ruff/issues/9393
synced_at: 2026-01-12T15:54:49Z
```

# line-length for formatter doesn't respect noqa: E501 comment

---

_@mmohaveri_

I'm running ruff's formatter with default config (ruff v0.1.11), and it's trying to 
fix my E501 violations even when the line has a `# noqa: E501`.

I'm expecting it to respect the E501 ignore comment and do not re-format and add-line-break to lines that exceed the line length limit (cause E501 error).

Not respecting the E501 ignore in formatter will effectively make it useless in linter because as soon as you run the formatter it will break all the long lines and effectivly make the `# noqa: E501` useless.

---

_Comment by @charliermarsh on 2024-01-04 19:21_

You might be looking for `# fmt: skip` at the end of the line?

---

_Comment by @mmohaveri on 2024-01-04 21:03_

I did not know about `# fmt: skip`, but not only it does not work (I just tested it), but even if it worked it would disable all formatting on the line, what I want it to only disable the line length rule.

Also understand that linting and formatting are separate things, but I think some lint rules that are related to formatting (like E501) should be considered when formatting.

---

_Comment by @charliermarsh on 2024-01-04 21:06_

Can you provide an example of what you’re trying to accomplish? I don’t quite understand.

(The skip comment must be placed at the end of the _statement_, not the end of the line.)

---

_Comment by @mmohaveri on 2024-01-04 21:31_

Sure, I tend to write function definitions in a single line, even if they surpass the line length limit.

To achieve this I add a `# noqa: E501` at the end of the line:

```python
def some_function_with_a_very_long_and_descriptive_name(arg_1: str, arg_2: int) -> tuple[SomeType | SomeOtherType, str | None]:  # noqa: E501
    pass
```

Based on your comment I should also add a `# fmt: skip` at the end as well to let formatter know to skip it, I tried following combinations:

```python
def some_function_with_a_very_long_and_descriptive_name_1(arg_1: str, arg_2: int) -> tuple[SomeType | SomeOtherType, str | None]:  # noqa: E501 # fmt: skip
    pass
    
def some_function_with_a_very_long_and_descriptive_name_2(arg_1: str, arg_2: int) -> tuple[SomeType | SomeOtherType, str | None]:   # fmt: skip # noqa: E501
    pass

def some_function_with_a_very_long_and_descriptive_name_3(arg_1: str, arg_2: int) -> tuple[SomeType | SomeOtherType, str | None]:  # fmt: skip
    pass
```

The first two does not work (you can see the result bellow), and the last one that works will cause a lint error (as it still requires an `#noqa: E501`):

```python
def some_function_with_a_very_long_and_descriptive_name_1(
    arg_1: str, arg_2: int
) -> tuple[SomeType | SomeOtherType, str | None]:  #  noqa: E501 # fmt: skip
    pass

def some_function_with_a_very_long_and_descriptive_name_2(
    arg_1: str, arg_2: int
) -> tuple[SomeType | SomeOtherType, str | None]: # fmt: skip #  noqa: E501
    pass

def some_function_with_a_very_long_and_descriptive_name_3(arg_1: str, arg_2: int) -> tuple[SomeType | SomeOtherType, str | None]: # fmt: skip
    pass
```

Now, if I run `ruff --fix` after the running `ruff format`, even the last option won't work anymore as `--fix` tries to fix the line length issue and it will result in:

```python
# fmt: skip
def some_function_with_a_very_long_and_descriptive_name_3(
    arg_1: str, arg_2: int
) -> tuple[SomeType | SomeOtherType, str | None]:
    pass
```

Further more, even if I disable the E501 rule and only rely on formatter for line length, `fmt: skip` is too broad. For example, imagine the following code:

```python
def some_function_with_a_very_long_and_descriptive_name(arg_1: str, arg_2   :    int ) -> tuple[   SomeType | SomeOtherType, str | None]:
    pass
```

what I want is to be able to skip the line length formatting for this line, while still applying the whitespace formatting. But if I apply the `# fmt: skip` at the end it will disable all formattings all together, hence leaving the extra whitespaces in the code.

So it will become:

```python
def some_function_with_a_very_long_and_descriptive_name(arg_1: str, arg_2   :    int ) -> tuple[   SomeType | SomeOtherType, str | None]:  # fmt: skip
    pass
```

while the desired outcome is something like:

```python
def some_function_with_a_very_long_and_descriptive_name(arg_1: str, arg_2: int ) -> tuple[   SomeType | SomeOtherType, str | None]:  # fmt: skip-line-length
    pass
```

---

_Comment by @charliermarsh on 2024-01-05 00:20_

Thanks, that's really helpful!

That Ruff is ignoring the skip in `# fmt: skip # noqa: E501` is a limitation that I just lifted in https://github.com/astral-sh/ruff/pull/9395 (arguably a bug).

To be honest, I think it's unlikely that we modify the formatter to respect `# noqa: E501` such that it formats the line but doesn't wrap. I do see why you want that per above, but I don't think it would outweigh the complexity budget of maintaining such a behavior.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-05 00:20_

---

_Comment by @mmohaveri on 2024-01-05 08:16_

If allowing individual switches for individual formatting rules is too complicated to implement, can we at least have the ability to set a separate line length limit for formatter (separate from the linter's line length limit)?

This way I'll be able to set the formatter's line length limit to something large (say 1000 characters) and effectively offload the line length formatting to the linter.


---

_Comment by @charliermarsh on 2024-01-05 12:33_

I believe that actually _is_ supported, if you use the setting under `pycodestyle` to overwrite the linter's enforced line length:

```toml
[tool.ruff]
# Use a line length of 100 by default (e.g., in the formatter).
line-length = 100

[tool.ruff.lint.pycodestyle]
# But override the linter's enforced line length to 10.
max-line-length = 10
```

---

_Comment by @mmohaveri on 2024-01-06 12:54_

It works and it doesn't :laughing: 

My current config is:

```toml
[tool.ruff]
line-length = 320

[tool.ruff.lint.pycodestyle]
max-line-length = 120
```

now when running `ruff format && ruff lint --fix` I'll get a lint error this line:

```python
    def _some_really_really_really_really_really_long_function_name) -> tuple[SomeClassOne | SomeOtherClassTwo, str | None]:  # noqa: E501
    ...
```

The error I get is: `E501 Line too long (194 > 120)` which means for some reason it's ignoring the noqa comment.

The same thing happens if I run `ruff lint`.

I'm not sure why, but I'm not getting this error in my IDE (I'm using ruff-lsp 0.0.48), I'm assuming its an new issue in a newer version of ruff (newer than the one used by ruff-lsp 0.0.48).

I think it's a separate issue in ruff linter! If you think so as well, I'll open a separate issue for it.


---

_Comment by @charliermarsh on 2024-01-06 15:22_

Hmm, I'm not able to reproduce this. For me running with the above configuration yields no errors on:

```python
def _some_really_really_really_really_really_long_function_name() -> tuple[SomeClassOne | SomeOtherClassTwo, str | None]:  # noqa: E501
    ...
```

Given:

```toml
[tool.ruff]
line-length = 320

[tool.ruff.lint.pycodestyle]
max-line-length = 120
```

Running `ruff format .` gives me `1 file left unchanged`, and `ruff check --select E` gives me no errors. If I remove the `# noqa: E501`, then I get `bar.py:1:121: E501 Line too long (121 > 120)`.


---

_Label `question` added by @charliermarsh on 2024-01-06 15:22_

---

_Comment by @charliermarsh on 2024-01-09 03:14_

Closing for now as we support the mixed `# noqa # fmt: skip` syntax in the next release. (Happy to reopen or try again if you're able to repro the above.)

---

_Closed by @charliermarsh on 2024-01-09 03:14_

---
