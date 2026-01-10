```yaml
number: 14984
title: "`docstring-code-line-length = \"dynamic\"` not Working Correctly"
type: issue
state: closed
author: Bibo-Joshi
labels:
  - formatter
  - needs-info
assignees: []
created_at: 2024-12-15T14:44:52Z
updated_at: 2024-12-15T20:24:20Z
url: https://github.com/astral-sh/ruff/issues/14984
synced_at: 2026-01-10T11:09:56Z
```

# `docstring-code-line-length = "dynamic"` not Working Correctly

---

_Issue opened by @Bibo-Joshi on 2024-12-15 14:44_

Hey. I found another instance where `docstring-code-line-length = "dynamic"` is not corking correctly. I've already seen https://github.com/astral-sh/ruff/issues/13358, but the fix for that apparently is not sufficient for the following example:

```python
"""This is a module docstring


Example:
    .. code-block:: python

        SomeClass(long_param_name_1="long_param_value_1", a_very_long_param_name_2="long_param_value_2")
"""


class SampleClass:
    """This is a class docstring

    Example:
        .. code-block:: python

            SomeClass(
                long_param_name_1="long_param_value_1", a_very_long_param_name_2="long_param_value_2"
            )
    """
```

Output:

```bash
(.venv313) ~\PycharmProjects\chango git:[section-note]
ruff --version
ruff 0.8.3
(.venv313) ~\PycharmProjects\chango git:[section-note]
ruff format foo.py --line-length 99
1 file left unchanged
(.venv313) ~\PycharmProjects\chango git:[section-note]
ruff check foo.py --line-length 99
foo.py:7:100: E501 Line too long (104 > 99)
  |
5 |     .. code-block:: python
6 | 
7 |         SomeClass(long_param_name_1="long_param_value_1", a_very_long_param_name_2="long_param_value_2")
  |                                                                                                    ^^^^^ E501
8 | """
  |

foo.py:18:100: E501 Line too long (101 > 99)
   |
17 |             SomeClass(
18 |                 long_param_name_1="long_param_value_1", a_very_long_param_name_2="long_param_value_2"
   |                                                                                                    ^^ E501
19 |             )
20 |     """
   |

Found 2 errors.
```

Unfortunately, I don't know enough about rust to dig into this myself.

Thanks for the great tool, btw ❤ 

---

_Label `formatter` added by @MichaReiser on 2024-12-15 15:09_

---

_Label `needs-info` added by @MichaReiser on 2024-12-15 15:09_

---

_Comment by @MichaReiser on 2024-12-15 15:09_

Hi. 

Can you tell me more about what settings you're using (line lengths, preview mode?). 

---

_Comment by @Bibo-Joshi on 2024-12-15 15:59_

Ah, damn, I forgot the `--isolated` … But Apparently you can't set `docstring-code-format` via the command line, so I reduced my config to this minimal set:

```toml
[tool.ruff]
line-length = 99

[tool.ruff.lint]
select = ["E501"]

[tool.ruff.format]
docstring-code-format = true
```

Sorry, should have thought of that

---

_Comment by @MichaReiser on 2024-12-15 16:11_

No worries and thanks for the details. I can reproduce your problem when `format.preview = false`. This makes sense to me because the fix for #13358 hasn't been promoted to the stable style yet, because it can change the formatting of existing code. 

Can you try setting `tool.ruff.format.preview = true` but be careful to have a backup of your code (e.g. commit all your changes) because the preview style has a few experimental formattings that you might not like. Or you can try it in our [Playground](https://play.ruff.rs/1be035e8-cd1d-4944-9057-6deb4af46844) (here with `preview = true`). 

You can subscribe to #13371 to be notified when we promote the fix to stable. We plan to do so in early January.



---

_Comment by @Bibo-Joshi on 2024-12-15 20:24_

Awesome, thanks for the explanation! I'll close, then :)

---

_Closed by @Bibo-Joshi on 2024-12-15 20:24_

---
