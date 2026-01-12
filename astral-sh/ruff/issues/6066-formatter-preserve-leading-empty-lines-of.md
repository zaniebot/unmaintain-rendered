```yaml
number: 6066
title: "Formatter: Preserve leading empty lines of indented classes and functions"
type: issue
state: closed
author: konstin
labels:
  - formatter
assignees: []
created_at: 2023-07-25T11:08:19Z
updated_at: 2023-08-01T15:31:03Z
url: https://github.com/astral-sh/ruff/issues/6066
synced_at: 2026-01-12T15:54:45Z
```

# Formatter: Preserve leading empty lines of indented classes and functions

---

_@konstin_

While the behavior for empty lines before top level classes and functions looks correct, indented classes and function can gain or lose their leading newlines

black:
```python
def f():
    ...
    # comment

    class Meta:
        pass

    if True:

        def register_type():
            pass
```

ours:
```python
def f():
    ...

    # comment

    class Meta:
        pass

    if True:
        def register_type():
            pass
```

---

_Label `formatter` added by @konstin on 2023-07-25 11:08_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-31 01:14_

---

_Comment by @charliermarsh on 2023-07-31 01:15_

I'm working on this.

---

_Comment by @charliermarsh on 2023-07-31 02:03_

I've noticed a few other incompatibilities after looking back at my old implementation:


1. The first entry _does_ need a separator if it's a class or function within a nested
   body that _isn't_ a class or function. Also, Black allows one blank line between a
   class and its docstring (the same does _not_ apply to functions).
2. If a function or class has a leading comment, then a blank line, we don't want to
   add one before the comment. The enforcement around
   `is_last_function_or_class_definition || is_current_function_or_class_definition`
   should be relative to the function or class definition, not its leading comments.
3. We're not inserting newlines after imports.
4. Black requires one newline after a class docstring.

(2) will cause problems with the current approach, I think. As an example, given:

```python
x = 1
# comment

def f():
    pass
```

Black wants:

```python
x = 1
# comment


def f():
    pass
```

Whereas we do:

```python
x = 1


# comment

def f():
    pass
```

That means the code that handles newlines-after-comments needs to now be aware of the thing it's commenting, the level of nesting, etc.


---

_Comment by @charliermarsh on 2023-07-31 02:04_

Perhaps the "easiest" way to get this to work would be to make `# comment` a trailing comment on the previous node, not a leading comment on the function, _if_ there's at least one blank line between the comment and the function?

---

_Comment by @charliermarsh on 2023-07-31 03:22_

(I will own all of those deviations.)

---

_Comment by @MichaReiser on 2023-07-31 07:58_

> (I will own all of those deviations.)

Are you looking for input or are these comment's meant as documentation?

---

_Comment by @charliermarsh on 2023-07-31 12:35_

The deviations are just meant as documentation, not expecting any input! Unless you have a quick reaction to the idea I suggested in ‘Perhaps the "easiest" way to get this to work…’, but I haven’t spent time validating that yet.

---

_Comment by @MichaReiser on 2023-07-31 12:42_

> Perhaps the "easiest" way to get this to work would be to make # comment a trailing comment on the previous node, not a leading comment on the function, if there's at least one blank line between the comment and the function?

I think that could work well. Not sure if implementing custom placement is necessarily easy. 

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-07-31 15:49_

---

_Closed by @charliermarsh on 2023-08-01 15:31_

---
