```yaml
number: 7368
title: "Formatter undocumented deviation: comment on function arguments forces wrapping"
type: issue
state: open
author: andersk
labels:
  - formatter
assignees: []
created_at: 2023-09-13T20:57:43Z
updated_at: 2025-01-10T07:19:35Z
url: https://github.com/astral-sh/ruff/issues/7368
synced_at: 2026-01-10T11:09:49Z
```

# Formatter undocumented deviation: comment on function arguments forces wrapping

---

_Issue opened by @andersk on 2023-09-13 20:57_

Input, unchanged under Black:

```python
reasonably_long_function_name_to_force_three_line_formatting(
    a, b, c, d, e, f  # comment
)
```

Ruff:

```python
reasonably_long_function_name_to_force_three_line_formatting(
    a,
    b,
    c,
    d,
    e,
    f,  # comment
)
```

---

_Comment by @zanieb on 2023-09-13 21:16_

Thanks for the report!

We'll need to hear from someone else on the team, but I believe the idea here is that comments should either be regarding:

- All of the arguments and consequently present on a line before them
- A single argument and consequently present on the line before it or trailing on the same line

So I prefer this formatting because it encourages better comment placement.

---

_Label `formatter` added by @zanieb on 2023-09-13 21:16_

---

_Comment by @andersk on 2023-09-13 21:24_

In the [actual code](https://github.com/zulip/zulip/blob/28597365da030d88c137adfc78156ae0b9c59cd4/zerver/lib/cache.py#L726-L728) that inspired this report, the comment is a `type: ignore` pragma and must be on the same line, so Ruff and mypy would together require duplicating the comment:

```diff
             return self.cached_function(
-                *args, **kwargs  # type: ignore[arg-type] # might be unhashable
+                *args,  # type: ignore[arg-type] # might be unhashable
+                **kwargs,  # type: ignore[arg-type] # might be unhashable
             )
```

which is not the end of the world but seems suboptimal.

---

_Comment by @charliermarsh on 2023-09-13 21:27_

Thanks for testing the formatter @andersk!

---

_Comment by @zanieb on 2023-09-13 22:00_

Ah thanks for the additional context! I believe we have special handling for pragma comments in some places. In this case I remember discussing that it would be good to suggest _narrower_ use of the pragmas as it's rare that you want it to apply to all of the arguments but I'm not sure the formatter should force you to duplicate like this (or break pragmas in general).

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-14 06:12_

---

_Label `needs-decision` added by @MichaReiser on 2023-09-14 06:12_

---

_Comment by @MichaReiser on 2023-09-14 06:23_

The underlying technical reason for the splitting is that Ruff associates the comment with the `kwargs` argument and it splits the whole function to ensure the comment stays close to the `kwargs` argument (or we  may run into issues where comments wander from one node to the next the more code is collapsed). 

We could improve our formatting to test if the last end of line comment is on the same line as the first argument (indicating that it uses the `a(\n\tb, c, d # arg\n)` layout, attach it as a dangling comment to arguments and render the comment right after the inner `arguments `group`. 

The alternative is that you write this as:

```python
             return self.cached_function(*args, **kwargs)  # type: ignore[arg-type] # might be unhashable
```

Ruff allows this formatting because pragma comments don't count toward the line width.

Regarding pragma comments. You can read more about the [decision the reasoning in this discussion](https://github.com/astral-sh/ruff/discussions/6670#discussion-5533063). The main challenge with pragma comments is that there's a tension between:

1. Guaranteeing that the pragma comment doesn't move at all cost. This means that only limited formatting rules can be applied to attributed code (which is what Black does for type ignore)
2. Format the code as usual, at the cost that pragma comments may move

I wanted to avoid 1. because it defeats the purpose of a formatter: Having consistent code formatting is the primary goal. We instead change the definition of pragma comments so that they don't count toward the line width do avoid the annoying case where you've written some code, add a pragma comment, and now the whole code re-flows (including your pragma comment) because it now exceeds the line width. You then have to move the comment and, if you're unlucky, it will trigger another reformat... which is very annoying. 



---

_Removed from milestone `Formatter: Beta` by @MichaReiser on 2023-09-27 15:56_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-09-27 15:56_

---

_Label `needs-decision` removed by @MichaReiser on 2023-09-27 15:57_

---

_Comment by @charliermarsh on 2023-09-27 15:57_

We're going to explore treating these as dangling and implementing special formatting. It's a known deviation, and not blocking for Beta, but we want to at least _try_ to improve it and see how it goes.


---

_Comment by @henribru on 2023-10-24 20:21_

Is this the same issue that's making the `type: ignore` comment follow `None` here instead of the full expression? Ran into this a few times in our codebase. Though I ran into the version the OP mentions a lot more, it caused 64 Ruff errors from misplaced noqa comments after formatting.

https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AKmAOxdAD2IimZxl1N_WlbvK5V9KEd0suDTtKdXyW-KHSefan1a-gnTsIBd_i7g2CHARB-DRfppQ8G8Ear_soAoOgGdRLZ8Od6lFlSonZzv7GuFtvxwFXtmT_90wnYxJXLNuTD2c3g0ZVM_jFV9YDTyvP-o8lPe_pQJx7Knkl8vNaac-zdyi5hcdcUS0mHilW73yIeIYoDw_Ba2rFZPXGwljEA0EoCbazw2DY8EA4lPVkDEeu1Q8h11L3Xu1JSHDRN-bbFlEPdq-jLPkQrDK3eCovkt9GT45eDhUVFZk5RoyZvJuX5xvRE3ZSQVFGGgMRFAAHmJjfhkn-EfAAGIAqcFAABf8gGdscRn-wIAAAAABFla

https://play.ruff.rs/62d5edc9-46dd-46b8-89bd-bc3f835a7d24?secondary=Format

---

_Comment by @MichaReiser on 2023-10-24 23:40_

@henribru What you're seeing is related to Ruff handling [trailing end of line comments](https://docs.astral.sh/ruff/formatter/black/#trailing-end-of-line-comments) and [pragma comments](https://docs.astral.sh/ruff/formatter/black/#pragma-comments-are-ignored-when-computing-line-width) differently.

Pragma comments are challenging to get right, especially when migrating between the formatters. I expect them to be less of a problem when you only use ruff (or black, to the matter)

---

_Comment by @henribru on 2023-10-25 05:23_

> @henribru What you're seeing is related to Ruff handling [trailing end of line comments](https://docs.astral.sh/ruff/formatter/black/#trailing-end-of-line-comments) and [pragma comments](https://docs.astral.sh/ruff/formatter/black/#pragma-comments-are-ignored-when-computing-line-width) differently.
> 
> Pragma comments are challenging to get right, especially when migrating between the formatters. I expect them to be less of a problem when you only use ruff (or black, to the matter)

That makes more sense, but the descriptions there arguably don't cover my case, since the first one says "This deviation only impacts unformatted code, in that Ruff's output should not deviate for code that has already been formatted by Black." and the second talks about Ruff _not_ moving pragma comments. Perhaps those descriptions need to be expanded a bit.

---

_Comment by @kdebrab on 2023-10-28 12:07_

I encountered the same issue with `noqa` pragma comments. E.g., following code is left as is by black, whereas ruff wraps the arguments:
```python
def read_csv_file(
    sensor_fpath, start, end, header, timezone, convention  # noqa: ARG001
):
    pass
```
One could indeed argue that wrapping encourages *narrower* use of the pragma, but then it would be consequent to apply it also for other cases, like, e.g., the following example (which is left as is by both black and ruff):
```python
def read_csv_file(sensor_fpath, start, end, header, timezone):  # noqa: ARG001
    pass
```
In my view, wrapping is undesired in both cases. Encouraging "narrower" use of pragma comments makes sense and could e.g. become a lint rule, but I don't think it should determine the wrapping behaviour of the formatter.

FYI, this is the only deviation I encountered (twice) when migrating four projects from black to ruff.

---

_Comment by @henribru on 2023-11-02 06:55_

> We're going to explore treating these as dangling and implementing special formatting. It's a known deviation, and not blocking for Beta, but we want to at least _try_ to improve it and see how it goes.
> 

Could it be added to the documented known deviations in the meantime? Or is that only intended for deviations that aren't expected to change?

---

_Comment by @MichaReiser on 2023-11-03 14:57_

We could implement a similar logic to https://github.com/astral-sh/ruff/pull/8431. Except that the `arguments` formatting "steals" the trailing end of line comments from the last argument. 

https://github.com/astral-sh/ruff/blob/f4c7bff36b0cb8470e87aa597c2e35f92414f2b2/crates/ruff_python_formatter/src/other/arguments.rs#L106-L107

* Extract the last argument's end of line comments
* Give the group a unique id (`.with_group_id(...)`, generate the id with `f.group_id`)
* Format the comments with `if_group_breaks(&trailing_comments(end_of_line_comments))` after `all_arguments` (inside of the group). This ensures that the comment sticks with the last argument if the group expands
* Format the comments with `if_group_fits(&trailing_comments(end_of_line_comments)).with_group_id(Some(group_id))` where `group_id` is the unique id created above. Formatting the comments outside of the group ensures that a trailing comment does not expand the group. 

Note: 
* It is necessary to manually mark the comments as formatted before formatting the arguments, then mark the comments as unformatted again each time before calling `trailing_comments`. 
* We should avoid creating the `if_group_fits` and `if_group_breaks` IR elements if no trailing comments exist.


One issue I see come up is that call formatting, arrays, etc would be different from parameter formatting:

```python
def func(
	a, b, c, d  # a
): ...
```

We may need to make the same change for parameters if we want to be consistent.


---

_Comment by @fruch on 2024-01-11 08:13_

I think it's something I'm hitting now

I was using the lint for some time, and add lots of noqa comments,
now I've introduced the formatter and getting those type of changes, which break the linter, cause command is in the wrong place:
```diff
-    def compare_table_data(self, expected_table_data: list[dict[str, str]], table_name: str | None = None,  # noqa: PLR0913
-                           node: ScyllaNode = None, ignore_order: bool = True, consistent_read: bool = True,
-                           table_data: list[dict[str, str]] | None = None, **kwargs) -> DeepDiff:
+    def compare_table_data(
+        self,
+        expected_table_data: list[dict[str, str]],
+        table_name: str | None = None,  # noqa: PLR0913
+        node: ScyllaNode = None,
+        ignore_order: bool = True,
+        consistent_read: bool = True,
+        table_data: list[dict[str, str]] | None = None,
+        **kwargs,
+    ) -> DeepDiff:
```

I fixed it with the linter `ruff --fix --add-no-qa`, but now I have hundred of leftover comments 


---

_Comment by @MichaReiser on 2024-01-11 08:23_

@fruch this sounds related but is slightly different because the comment isn't at the end of the arguments line (it's after a specific argument). Unfortunately, it's challenging for the formatter to know to which node a suppression comment belongs without running the linter, which would be extremely slow. 

Can you try using the `RUF100` rule. It should point out unused noqa comments and has a fix to remove them

```bash
ruff  check --select=RUF100 <path>
```

And apply the fixes with



```bash
ruff  check --fix --select=RUF100 <path>
```

---

_Comment by @kurt-rhee on 2025-01-10 01:25_

I find this interesting because adding a comment is the only way that I can figure out how to force ruff to allow me to put function arguments on separate lines.  Is there a better way to do something like that?

```py
if __name__ == "__main__":
    commission_inverter(
        source_data=InverterDataSource.CEC,
        inverter_name="string",  # comment
    )
```

---

_Comment by @MichaReiser on 2025-01-10 07:19_

Adding a trailing comma forces Ruff to split the arguments on their own line. so this should be enough

```py
if __name__ == "__main__":
    commission_inverter(
        source_data=InverterDataSource.CEC,
        inverter_name="string",
    )
```

---
