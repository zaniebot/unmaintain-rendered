```yaml
number: 12870
title: Comprehensions do not split as far as they can go, which preserves long line lengths
type: issue
state: open
author: rogersheu
labels:
  - formatter
  - style
  - needs-design
assignees: []
created_at: 2024-08-13T19:06:43Z
updated_at: 2025-07-09T17:36:55Z
url: https://github.com/astral-sh/ruff/issues/12870
synced_at: 2026-01-12T15:54:52Z
```

# Comprehensions do not split as far as they can go, which preserves long line lengths

---

_@rogersheu_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

**Keywords**
Line length, comprehension


**Example**
*Before `ruff --fix`*
```
really_really_really_long_dict_name = {"foo": "bar"}

dict_with_really_long_names = {really_really_really_long_key_name: really_really_really_long_key_value for really_really_really_long_key_name, really_really_really_long_key_value in really_really_really_long_dict_name.items()}
```


*After `ruff --fix`* (Both the "before ruff fix" and "manual attempt below" lead to this)

```
dict_with_really_long_names = {
    really_really_really_long_key_name: really_really_really_long_key_value
    for really_really_really_long_key_name, really_really_really_long_key_value in really_really_really_long_dict_name.items()
}
```

*Expected (or Proposed) after `ruff --fix`* (running ruff on this also leads back to the after case)
```
dict_with_really_long_names = {
    really_really_really_long_key_name: really_really_really_long_key_value
    for really_really_really_long_key_name, really_really_really_long_key_value 
    in really_really_really_long_dict_name.items()
}
```


That `for/in` line is 127 characters long, but it could come under the line length (default of 88, not set here) by splitting the `for/in` statements, which does not affect behavior in this case.


**Version/Command**
Using ruff v0.5.1 with pre-commit calling `ruff --fix`.


**Comparison with `black`**
Currently, this runs with the same behavior as `black`.
Another user brought up a similar suggestion here: https://github.com/psf/black/issues/3498

---

_Comment by @MichaReiser on 2024-08-14 06:58_

To clarify. Are you running `ruff format` or `ruff check --fix`? I  suspect it's the former because `E501` doesn't come with an autofix.

---

_Label `needs-info` added by @MichaReiser on 2024-08-14 06:58_

---

_Comment by @rogersheu on 2024-08-14 07:03_

@MichaReiser Thanks for asking! I'm running the following block in `.pre-commit-config.yaml`.

```yaml
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.5.1
    hooks:
      - id: ruff
        args: [ --fix ]
      - id: ruff-format
 ```

If I try to coerce it first by splitting the for and in statements, it squishes it back into one line.

I also just tried doing a bare `ruff format FPATH` and it exhibited the same behavior (springing back into a one-liner).

---

_Label `needs-info` removed by @MichaReiser on 2024-08-15 16:22_

---

_Label `formatter` added by @MichaReiser on 2024-08-15 16:23_

---

_Label `style` added by @MichaReiser on 2024-08-15 16:23_

---

_Comment by @MichaReiser on 2024-08-15 16:26_

Thanks.  Yeah, it is then the formatter that doesn't split the comprehension further. 

The main question here is what should happen when the comprehension does break over multiple lines:

```diff
 lcomp3 = [
         # This one is actually too long to fit in a single line.
         element.split("\n", 1)[0]
         yup
-        for element in collection.select_elements()
+        for element
+        in collection.select_elements()
         # right
         if element is not None
 ]
```

Because this does look worse. The desired result is probably that the `for ... in` should be kept together if possible


---

_Comment by @MichaReiser on 2024-08-15 16:31_

I think my biggest concern is that it hurts readability if there are multiple generators

```python
[
    [
        x 
        for x
        in bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
        for y 
        in xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxdxxxxxxxxxxxxxxxxxxxxxxxx
    ]
]
```

---

_Comment by @MichaReiser on 2024-08-15 16:34_

Thanks for linking to the relevant black issue. This is very helpful for both projects :)

---

_Comment by @MichaReiser on 2024-08-15 16:46_

There are surprisingly few differences in the [ecosystem check of a naive implementation](https://github.com/astral-sh/ruff/pull/12908#issuecomment-2291695892).

I'm not convinced that this looks better overall. It would require more refined rules. E.g. don't split if the left side is a simple expression like an identifier.  

---

_Label `needs-design` added by @MichaReiser on 2024-08-15 16:46_

---

_Comment by @rogersheu on 2024-08-16 02:05_

Thanks for giving this issue some thought @MichaReiser !

> The desired result is probably that the for ... in should be kept together if possible

I agree. If the `for ... in` can be kept to one line, it absolutely should. I'm purely thinking of cases where doing so would improve the soft adherence to the line length setting.

> There are surprisingly few differences in the https://github.com/astral-sh/ruff/pull/12908#issuecomment-2291695892.

> I'm not convinced that this looks better overall. It would require more refined rules. E.g. don't split if the left side is a simple expression like an identifier.

Interesting, thanks for doing that prototyping! Yeah, I assume most folks aren't using unnecessarily long variable or iterable variable names like I am (likely shoddy choices on my part), but the cases your implementation covers do seem to cover subset where the code has parentheses in the `for (...) in ...` and the parentheses get split out, perhaps unnecessarily.

For the prefect one, I think ideally, it could be the following, where trying to keep the `in` with the `for` led to the parentheses splitting out line-wise. (So the change was effective, but perhaps it could also compress the earlier lines.)

```
            for (block_schema, _, parent_block_schema_id) 
            in block_schemas_with_references
```

That might start needing some extra logic though (as you pointed out), and I lack the intuition in this codebase to know if that's even stylistically/philosophically reasonable. 

Additionally, in that fairly small diff, I didn't see any example of my original issue (where the `for ... in` is straight-up long without any parentheses and left all as one line), but I think you were testing out this other case so it makes sense.


Edit: By the way, I just saw your #12856 and corresponding Issues in black, and I'm glad the person who's already been thinking about parentheses/braces and looooooooooong variable name formatting is thinking about this!

---

_Comment by @MichaReiser on 2024-08-16 06:13_

> likely shoddy choices on my part), but the cases your implementation covers do seem to cover subset where the code has parentheses in the for (...) in ... and the parentheses get split out, perhaps unnecessarily.

I think the reason why the parentheses get split out is because the tuple contains a trailing comma. But I think the style could do better by trying to keep the closing `)` and the `in` keyword on the same line.

---

_Comment by @spaceone on 2025-07-09 17:14_

https://github.com/astral-sh/ruff/issues/19231 was a duplicate for this:

I am generally allowing a lower line length: 120 chars because the code base has function calls with many parameters.
On the other side I format certain list-comprehension code blocks for readability into multiple lines.

one example:
```diff
-    return [
-        idx * 8 + bit
-        for idx, value in enumerate(octets)
-        for bit in range(8)
-        if value & (1 << bit)
-    ]
+    return [idx * 8 + bit for idx, value in enumerate(octets) for bit in range(8) if value & (1 << bit)]
```

It would be nice if the formatter would allow a setting, that already multiline-splitted list/set/dict comprehensions are not joined into one single line again.
Maybe a line-length for comprehensions may differ from a general line length for function calls / the rest.

Basically that is the number one blocking issue for using the formatter at all.

---

_Comment by @MichaReiser on 2025-07-09 17:36_

This is also related to https://github.com/astral-sh/ruff/issues/11753

---
