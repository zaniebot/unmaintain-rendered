---
number: 11569
title: Formatter changes order of comments.
type: issue
state: closed
author: randolf-scholz
labels:
  - bug
  - formatter
assignees: []
created_at: 2024-05-27T14:58:49Z
updated_at: 2024-05-31T12:06:19Z
url: https://github.com/astral-sh/ruff/issues/11569
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter changes order of comments.

---

_Issue opened by @randolf-scholz on 2024-05-27 14:58_

```python
class Foo:
    # comment 1
    @abstractmethod
    def foo(self) -> None: ...
    @abstractmethod
    def bar(self) -> None: ...
    # comment 2

    # comment 3
    def baz(self) -> None:
        return None
    # comment 4
```

becomes

```python
class Foo:
    # comment 1
    @abstractmethod
    def foo(self) -> None: ...
    @abstractmethod
    def bar(self) -> None: ...

    # comment 3
    # comment 2
    def baz(self) -> None:
        return None

    # comment 4
```

[[ruff-playground]](https://play.ruff.rs/90543207-c186-4bb6-b5f6-a658d65c31c6)

1. Comments 2 and 3 get swapped.
2. An extra space is inserted after `def bar`. Since these are stubs, no space should be inserted. (for instance, comment 1/2 can be  `# region`/`#endregion`). But this is just my opinion.

---

_Label `bug` added by @MichaReiser on 2024-05-27 15:00_

---

_Label `formatter` added by @MichaReiser on 2024-05-27 15:00_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-05-27 15:00_

---

_Comment by @randolf-scholz on 2024-05-27 15:19_

Another example, where the comment actually moves inside another function def.

```python
class Foo:
    @abstractmethod
    def foo(self) -> None: ...
    # comment 1

    def baz(self) -> None:
        return None
```

becomes

```python
class Foo:
    @abstractmethod
    def foo(self) -> None: ...

    def baz(self) -> None:
    # comment 1
        return None
```

https://play.ruff.rs/973d0af8-e586-4fc0-9aec-28fecf0dc047


---

_Comment by @MichaReiser on 2024-05-31 08:28_

You're good at finding comment reordering bugs and thank you so much at taking the time to report them! 

Uhh, the second one is really bad. Let me have a look. 

What's interesting is that all comments are associated with the right method. So this is a rendering bug and not a placement bug (logic where we figure out where a comment is most likely to belong). 

https://play.ruff.rs/5e5ae359-b017-4d82-ac47-2c77f254f161

---

_Comment by @MichaReiser on 2024-05-31 08:33_

To make it worse, this is not even related to classes nor does it require decorators. https://play.ruff.rs/2bcde006-72e6-409f-9fa9-659ef78d7944

---

_Comment by @randolf-scholz on 2024-05-31 08:48_

I would suspect this is somehow related to special logic for formatting stub implementations https://github.com/astral-sh/ruff/issues/8357

---

_Comment by @MichaReiser on 2024-05-31 08:56_

Yeah, that's a great observation. The issue is that, for stubs, we don't write extra empty lines between the comment and the next function, which drips over the formatting.

---

_Referenced in [astral-sh/ruff#11632](../../astral-sh/ruff/pulls/11632.md) on 2024-05-31 09:17_

---

_Comment by @MichaReiser on 2024-05-31 09:20_

@randolf-scholz thanks for debugging this together. The fix turned out to be not that bad :)

---

_Closed by @MichaReiser on 2024-05-31 12:06_

---
