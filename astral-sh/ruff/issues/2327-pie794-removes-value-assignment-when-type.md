---
number: 2327
title: "PIE794: Removes value assignment when type annotation is on the line above"
type: issue
state: closed
author: Jeremiah-England
labels: []
assignees: []
created_at: 2023-01-29T22:21:52Z
updated_at: 2023-02-01T02:01:22Z
url: https://github.com/astral-sh/ruff/issues/2327
synced_at: 2026-01-10T01:22:40Z
---

# PIE794: Removes value assignment when type annotation is on the line above

---

_Issue opened by @Jeremiah-England on 2023-01-29 22:21_

To avoid long lines for my variable declarations, sometimes I put the type hint for the variable on the line above. This avoids it getting reformatted into three lines by Black. A silly example:

```python
class Hello:
    world: str
    world = "Something with enough characters that I want to put the type on a separate line above to avoid ugly parenthesis."
```

<blockquote>
(Reformatted with Black because my line was too long when I put the type in it.)

```python
class Hello:
    world: str = (
        "Something with enough characters that I want to put the type on a separate line above to avoid ugly parenthesis."
    )
```
</blockquote>

When I run ruff with this config, the second line gets removed:

```toml
[tool.ruff]
select = ["PIE794"]
fix = true
```

Leaving just this behind...

```python
class Hello:
    world: str
```

At first I thought this could be updated to not count type type annotations as a a variable "definition". But given [the context][0] of the rule ("...can occur in large ORM model definitions"), I am not sure that would work. I think FastAPI uses dataclass-like classes for ORM stuff. And those just declare the type annotations.

```python
# A Pydantic model
class User(BaseModel):
    id: int
    name: str
    joined: date
```

The solution I am thinking of right now is not allowing two declarations of the same "type", where the types are assigning a value or just adding a type annotation. That would catch duplicate _actual_ definitions in dataclass-like things and regular classes, while allowing me to do my formatting trick.

But the parenthesis really are not that bad, and maybe that solution is a lot of work. So this is a very tentative feature request.

[0]: https://github.com/sbdchd/flake8-pie#pie794-dupe-class-field-definitions

Edit: Oh, I was running this on version 0.0.237.

---

_Comment by @charliermarsh on 2023-01-31 21:35_

I hate saying no to things, especially when the proposal is as clearly laid out as this one... but I think this would add more complexity than I'd like, to accommodate a pretty specific use-case (no offense intended -- I can totally see why you prefer the above, I just don't see it as a super common pattern).


---

_Closed by @charliermarsh on 2023-01-31 21:35_

---

_Comment by @Jeremiah-England on 2023-02-01 00:46_

Makes sense! Thanks for taking a look. I'm sure Charliest Marsh would have made the same decision! :smile:

---

_Comment by @charliermarsh on 2023-02-01 02:01_

Lmaooo no Charliest Marsh would maintain a custom build that solved this problem specifically for you

---
