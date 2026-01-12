```yaml
number: 8566
title: formatter removes blank line after class definition
type: issue
state: closed
author: spaceone
labels:
  - formatter
  - style
assignees: []
created_at: 2023-11-08T16:05:43Z
updated_at: 2024-09-27T08:06:22Z
url: https://github.com/astral-sh/ruff/issues/8566
synced_at: 2026-01-12T15:54:48Z
```

# formatter removes blank line after class definition

---

_@spaceone_

```sh
$ black --diff foo.py 
All done! ✨ 🍰 ✨
1 file would be left unchanged.
$ ruff format --diff foo.py
--- foo.py
+++ foo.py
@@ -1,5 +1,4 @@
 class Foo:
-
     bar = 1
 
     def __init__(self):

$ cat foo.py
class Foo:

    bar = 1

    def __init__(self):
        pass

```

→ ruff should not remove the blank line

---

_Comment by @charliermarsh on 2023-11-08 16:06_

Do you know what Black version you're using? The behavior is identical for me in the playground: https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ACnAGxdAD2IimZxl1N_WlXnON2nzNX9HFvHyHbXdX0Ms6iKT6MFYYQATMUD1OePdwlX142nVYoknzNqjCZuF1C-2qRxjAe1b_PNzGhgl2Uikz5qLOibGl5-HYJNlgbh2oqOzRkM6DJB73sLqD6q1cogjgBuZBfc0jIxPgABiAGoAQAABTDLhbHEZ_sCAAAAAARZWg==

---

_Comment by @spaceone on 2023-11-08 16:12_

```
$ black --version
black, 22.10.0 (compiled: yes)
Python (CPython) 3.7.3

```


same for:
```python
class Foo:

    # bar

    def __init__(self):
        pass
```

Additionally I would like to have a flag in ruff which prevents that
```python
class Foo:

    def __init__(self):
        pass
```
get converted to:
```python
class Foo:
    def __init__(self):
        pass
```

---

_Comment by @charliermarsh on 2023-11-08 16:13_

Our current behavior is identical to Black's though in all of the above cases -- see the playground link I shared above.

---

_Comment by @spaceone on 2023-11-08 16:48_

OK, then see this as feature request to change it via a config option.

---

_Label `formatter` added by @zanieb on 2023-11-08 18:21_

---

_Label `style` added by @zanieb on 2023-11-08 18:21_

---

_Comment by @charliermarsh on 2023-11-09 01:48_

Realistically, I think we're unlikely to implement a setting for this given that it's so specific. We also enforce a blank line if you use a docstring for your classes which should mitigate this, but I'm gonna close for now in the interest of keeping the issue queue manageable.

---

_Closed by @charliermarsh on 2023-11-09 01:48_

---

_Comment by @Frost on 2023-11-15 09:59_

> ```
> $ black --version
> black, 22.10.0 (compiled: yes)
> Python (CPython) 3.7.3
> ```
> 
> same for:
> 
> ```python
> class Foo:
> 
>     # bar
> 
>     def __init__(self):
>         pass
> ```
> 
> Additionally I would like to have a flag in ruff which prevents that
> 
> ```python
> class Foo:
> 
>     def __init__(self):
>         pass
> ```
> 
> get converted to:
> 
> ```python
> class Foo:
>     def __init__(self):
>         pass
> ```

This behavior changed between `black` 22.x and 23.x. Black has a stable format that they only update yearly. This means that once per year, you can suddenly get massive diffs from black, _if you keep your dependencies updated_. 

---

_Comment by @spaceone on 2023-11-15 10:10_

This formatting is one of multiple reasons I don't use black - the maintainers also have strong opinions and you cannot argue with them. I have the hope that ruff is more user friendly and configurable.

---

_Comment by @Frost on 2023-11-15 12:39_

As long as ruff follows what black does, the same problem will exist. Soon it's 2024, and black will do some changes to their format again. There's a separate issue for tracking that here: #8678. 

---

_Comment by @jongracecox on 2024-03-15 18:04_

I'm using `black==24.2.0` and `ruff==0.3.2`, and finding they produce different results:

## Input file

```python
class MyClass:

    def __init__(self):
        pass
```

## black
```python
class MyClass:

    def __init__(self):
        pass
```

## ruff

```python
class MyClass:
    def __init__(self):
        pass
```

Something interesting to note is that black will leave the blank line after class statement if it is there, but **won't insert one** if it's not already there. This means if you take my input file and run black then ruff, only ruff will make changes to the file. However, if you run ruff then black, black will not change the file - it won't revert the ruff changes. It will accept the ruff formatting. This means there's a subtle difference in behavior that could be made more consistent between the two tools:

* Black honors the existing blank lines after the class statement
* Ruff always removes the blank line after the class statement

What's the stance on conformity with black? It would help me a lot if it was a true drop-in like-for-like.

_Side note: Thanks for an AMAZING project! I appreciate how complicated it is to manage everyones expectations._

---

_Comment by @MichaReiser on 2024-03-18 08:29_

Hi @jongracecox 

Thanks for the excellent write-up. Yes, Black 24 or newer allows an optional blank line at the start of a block. We intentionally didn't implement this design change (yet?) because it can lead to unintentionally blank lines at the top of blocks after moving or deleting code. 

> Currently, we are concerned that allowing blank lines at the start of a block leads https://github.com/astral-sh/ruff/issues/8893#issuecomment-1867259744. However, we will consider adopting Black's formatting at a later point with an improved heuristic. The style change is tracked in https://github.com/astral-sh/ruff/issues/9745.
> [source](https://github.com/astral-sh/ruff/blob/main/docs/formatter/black.md#blank-lines-at-the-start-of-a-block)

I'm glad that you pointed this out because it made me realize that the *known deviations* on our website don't mention this deviation yet because I only updated the `README.md` in the `ruff-python-formatter` crate but not the markdown file rendered on the website. I fixed this, and the deviations should be correctly reflected when we push the next version of our documentation.



> Side note: Thanks for an AMAZING project! I appreciate how complicated it is to manage everyones expectations.

Thank you

---

_Comment by @dody on 2024-09-27 07:53_

Using `black==24.8.0` and `ruff==0.6.8`. Ruff removing blank lines not only in classes but also in other blocks like `for` loop.

Any idea how to make ruff output equal to black ?

## black:
```python
class MyClass(object):

    def test(self):

        pass


for i in range(1):

    pass
```
## ruff:
```python
class MyClass(object):
    def test(self):
        pass


for i in range(1):
    pass
```

---

_Comment by @MichaReiser on 2024-09-27 08:06_

@dody see https://github.com/astral-sh/ruff/issues/9745

---
