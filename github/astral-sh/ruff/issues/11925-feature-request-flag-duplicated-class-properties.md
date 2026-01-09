---
number: 11925
title: "feature request: flag duplicated class properties"
type: issue
state: closed
author: newresu
labels:
  - rule
assignees: []
created_at: 2024-06-18T13:26:37Z
updated_at: 2024-06-25T23:45:32Z
url: https://github.com/astral-sh/ruff/issues/11925
synced_at: 2026-01-07T13:12:15-06:00
---

# feature request: flag duplicated class properties

---

_Issue opened by @newresu on 2024-06-18 13:26_

I don't think there is a rule flagging this common mistake?

```python
class ThisOne:
    def __init__(self):
      self.hello="hi"
      self.hello="bye"   # <-- flag repeated property
```

---

_Label `rule` added by @AlexWaygood on 2024-06-18 13:57_

---

_Comment by @MichaReiser on 2024-06-19 06:23_

@AlexWaygood how common is it to reassign self variables in the constructor? Like, is this something that's frowned upon

```python
class Test:
	def __init__(self, arg):
		self.name = "hi"
		if arg:
			self.name += " more"
```

Either way, I think Ruff needs a control flow sensitive analysis to avoid too many false positives, or it would flag:

```python
class Test:
	def __init__(self, arg):
		if arg:
			self.message = "hy"
		else:
			self.message = "bye"
		
```

which should remain valid.

---

_Comment by @AlexWaygood on 2024-06-19 08:35_

> @AlexWaygood how common is it to reassign self variables in the constructor? Like, is this something that's frowned upon

It's pretty common. I wouldn't say that that's frowned upon. If we were to accept this, we'd need a way to distinguish between "pointless" assignments that are immediately overriden, like OP's example, and assignments like your example there which seem fine to me.

---

_Comment by @newresu on 2024-06-19 14:55_

Also is it possible to flag error here:

```
class ThisOne:
    hello: int 
   def __init__(self):
      self.hello="hi" # error, not an int
```

?

---

_Comment by @MichaReiser on 2024-06-19 15:05_

> Also is it possible to flag error here:

This is something that we can flag once we have type checking support.

---

_Closed by @newresu on 2024-06-25 23:45_

---
