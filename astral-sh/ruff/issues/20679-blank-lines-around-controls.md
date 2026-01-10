---
number: 20679
title: blank lines around controls
type: issue
state: open
author: SmikeForYou
labels:
  - formatter
  - style
assignees: []
created_at: 2025-10-02T10:45:36Z
updated_at: 2025-10-09T12:04:13Z
url: https://github.com/astral-sh/ruff/issues/20679
synced_at: 2026-01-10T01:23:01Z
---

# blank lines around controls

---

_Issue opened by @SmikeForYou on 2025-10-02 10:45_

Hi. First of all want to thank you on such a powerfull formatting and lining tool as `ruff`. I have one suggestion for formatting improvement wich will make codestyle much readable. 

**Summary**

To add support for blank lines around control blocks(if/with/for/while/try) in formatting options.

_Unformatted_

```python
def unformatted():
    if True:
        print(1234)
    while False:
        print(123)
    for _ in range(1):
        print(1234)
    try:
        pass
    except:
        print(123)
    finally:
        print(123)
```



_Formatted_

```python
def unformatted():
    if True:
        print(1234)

    while False:
        print(123)

    for _ in range(1):
        print(1234)

    try:
        pass
    except:
        print(123)
    finally:
        print(123)
```

---

_Label `formatter` added by @AlexWaygood on 2025-10-02 11:03_

---

_Label `style` added by @MichaReiser on 2025-10-02 11:22_

---

_Comment by @MichaReiser on 2025-10-02 11:27_

Thanks for this suggestion. What about nested controls?

```py
if True:
	if False: ...
```


or 

```

```py
if True:

	if False
```

IMO, this is too opinionated but I'm interested in other opinions.

---

_Comment by @SmikeForYou on 2025-10-02 12:59_

Nice point. For my current project i wrote python script for this kind of formatting and it formats only top level of controls blocks without diving into nested blocks. But its really interesting to see the opinion of community

---

_Comment by @SmikeForYou on 2025-10-02 13:03_

But basicly i think that better approach is not to add blank line before first nested block(same behaviour as for first line after `def`):
_Unformatted:_
```python
def unformatted():
    if True:
        if True:
            print(1234)
        for _ in (1,):
            print(123)
    while False:
        if True:
            print(123)
    for _ in range(1):
        print(1234)
    try:
        pass
    except:
        print(123)
    finally:
        print(123)
```

_Formatted_:
```python
def formatted():
    if True:
        if True:
            print(1234)

        for _ in (1,):
            print(123)

    while False:
        if True:
            print(123)

    for _ in range(1):
        print(1234)

    try:
        pass
    except:
        print(123)
    finally:
        print(123)

```

---

_Comment by @SmikeForYou on 2025-10-09 11:34_

@MichaReiser hey. How do you think, we can implement?

---

_Comment by @MichaReiser on 2025-10-09 12:04_

This will require broader community interest to be implemented in the formatter. 

---
