---
number: 6256
title: "Implement `valid-magic-values` setting for rule PLR2004"
type: issue
state: closed
author: dzh-ad
labels:
  - needs-info
assignees: []
created_at: 2023-08-01T21:22:35Z
updated_at: 2023-12-04T22:20:47Z
url: https://github.com/astral-sh/ruff/issues/6256
synced_at: 2026-01-07T13:12:15-06:00
---

# Implement `valid-magic-values` setting for rule PLR2004

---

_Issue opened by @dzh-ad on 2023-08-01 21:22_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Pylinter has a nice configuration that ignores warnings for specified values, [valid-magic-values](https://pylint.readthedocs.io/en/latest/user_guide/configuration/all-options.html#magic-value-options)
```
[tool.pylint.magic-value]
valid-magic-values = [0, -1, 1, "", "__main__"]
```

Ruff does not current have an equivalent configuration. The closest one seems to be [allow-magic-value-types](https://beta.ruff.rs/docs/settings/#pylint-allow-magic-value-types), which would ignore any variables of a specific type.

---

_Comment by @charliermarsh on 2023-08-01 21:33_

This was an intentional divergence, if I recall correctly, since we couldn't find any motivating use-cases for parameterizing on specific values, the thinking being that if there's a value that should be allowed that's specific to the codebase, it should probably be encoded as a variable; and if a value should be allowed but isn't specific to the codebase, it should probably just be allowed by default (like `0`).

What's the use-case? What value were you looking to allow?


---

_Label `waiting-on-author` added by @charliermarsh on 2023-08-01 21:33_

---

_Comment by @dzh-ad on 2023-08-02 17:05_

Ah I see. Thank you for your quick reply!

Our project deals with 2D and 3D arrays quite a bit, so I was hoping to ignore the warning for `2` and `3` as well (for the axis, checking if the number of dimension is correct etc). 

Another place where it might be helpful is performing a certain action for a given http response status codes (ie. `200` for success)

---

_Comment by @dzh-ad on 2023-08-02 17:19_

One more example for the use of `2`, the simple line of 

`if len(header_elements) != 2:`

would be flagged, and editing to the following looks kind of funny
```
expected_header_elements = 2
if len(header_elements) != expected_header_elements:
```
In-line ignore also works, but it would make our life a lot easier if we can add other common (and easily understood) values to the ignore list for our project.

---

_Comment by @zanieb on 2023-08-02 17:22_

I would generally recommend using status codes as constants e.g. https://github.com/encode/starlette/blob/a8b8856ce393a82ab4a714131085dbc4d658e34d/starlette/status.py#L96 which tends to be clearer anyway.

Regarding other small integers, I'm not sure what the best course is. Why is `expected_header_elements` bad? That seems like it follows the spirit of the rule. That number should probably be a constant with a comment explaining why it is that length.


---

_Comment by @charliermarsh on 2023-08-04 01:40_

Yeah I think many of these adhere to the spirit of the rule, and I'm hesitant to make this configurable. We could consider adding 2 and 3 to the allow list though since they are commonly used as dimensions. Wdyt @zanieb?

---

_Comment by @dzh-ad on 2023-08-04 21:21_

Woo the starlette looks like a cool package! Thanks @zanieb!
You comments make sense. I don't have a strong preference on if we add 2 and 3 into the allow list now.

---

_Closed by @dzh-ad on 2023-08-04 21:21_

---

_Comment by @jonyscathe on 2023-12-04 05:12_

Was there any resolution on adding 2 and 3 to the list?

Even the ruff example of this rule violates the rule by using 2 as a magic number... https://docs.astral.sh/ruff/rules/magic-value-comparison/

---

_Comment by @tjkuson on 2023-12-04 09:39_

I think `2` is allowed as a magic value (I linted the example in the documentation, and Ruff only complained about `100`).

---

_Comment by @charliermarsh on 2023-12-04 15:16_

@tjkuson - I _think_ it's that magic values are only linted in comparisons, and not arbitrary expressions.

---

_Comment by @jonyscathe on 2023-12-04 22:20_

Oh yes sorry. I had missed the magic numbers can only not be used in comparisons but are fine to use elsewhere according to this rule.
I guess that makes it less of an issue that 2 and 3 are not exempt as using those values as indexes in 2D, 3D arrays etc is still fine to do.

---

_Referenced in [franneck94/Vscode-Python-Config#5](../../franneck94/Vscode-Python-Config/issues/5.md) on 2024-02-04 07:59_

---

_Referenced in [astral-sh/ruff#20468](../../astral-sh/ruff/issues/20468.md) on 2025-09-18 14:15_

---
