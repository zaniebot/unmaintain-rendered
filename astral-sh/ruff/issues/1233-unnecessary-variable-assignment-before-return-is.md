```yaml
number: 1233
title: "\"Unnecessary variable assignment before `return`\" is too eager"
type: issue
state: closed
author: DanCardin
labels:
  - question
assignees: []
created_at: 2022-12-14T03:55:44Z
updated_at: 2022-12-20T00:48:28Z
url: https://github.com/astral-sh/ruff/issues/1233
synced_at: 2026-01-12T15:54:41Z
```

# "Unnecessary variable assignment before `return`" is too eager

---

_@DanCardin_

Some minimal example:
```python
def foo():
    bar = 4
    x()
    return bar
```

`x()` arbitrary function call can have side-effects. This is a dirt simple example, but it's not hard to imagine sequences of statements which would mutate `bar`'s value between the assignment and the return, even if there aren't direct references to `bar` itself (which seems to be the current heuristic).

Thus, it seems to me to be too eager to flag this, so long as there are potentially side-effectful operations between the assignment and the `return`. and given that most operations can be side-effectful, really i guess this just means, I'm suggesting it should mostly be checking for sequential statements like so:

```
x = 4
return x
```

---

_Comment by @charliermarsh on 2022-12-15 00:39_

I'm torn on this! Almost any statement can be side-effectful -- even something like `a = b + c` could actually involve operator overloading under-the-hood and thus make use of `bar`. So, I guess the suggestion is that we make this check much more conservative, and only flag when there are no expressions between the assignment and the return?


---

_Label `question` added by @charliermarsh on 2022-12-15 00:39_

---

_Comment by @charliermarsh on 2022-12-16 04:14_

Closing for now.

---

_Closed by @charliermarsh on 2022-12-16 04:14_

---

_Comment by @DanCardin on 2022-12-16 17:23_

That was the suggestion yes. I can't decide i agree with myself

Something like:

```
def bad(a={"a": 1}):
    a['a'] += 1
    return a

def foo():
    bar = bad()
    bad()
    return bar
```
you can't really reorder the statements without changing the behavior.

And while this example seems contrived, the real code that triggered this was SQLAlchemy models being "hydrated" through some associated function call equivalent to `bad`, without referencing the equivalent of `bar`.

With that being said, this is perhaps edge-casey enough that one just puts a noqa on it and calls it a day (which is what i did).

---

_Comment by @charliermarsh on 2022-12-20 00:48_

@DanCardin - FYI, I ended up changing this in https://github.com/charliermarsh/ruff/pull/1294 since it got filed again, which I took as a sign that it was too aggressive :)

---
