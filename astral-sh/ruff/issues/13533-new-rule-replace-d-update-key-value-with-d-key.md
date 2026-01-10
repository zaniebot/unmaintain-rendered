---
number: 13533
title: "new rule - replace `d.update({'key': 'value'})` with `d['key'] = 'value'`"
type: issue
state: open
author: spaceby
labels:
  - rule
  - accepted
assignees: []
created_at: 2024-09-27T10:19:17Z
updated_at: 2025-11-10T09:32:56Z
url: https://github.com/astral-sh/ruff/issues/13533
synced_at: 2026-01-10T01:22:53Z
---

# new rule - replace `d.update({'key': 'value'})` with `d['key'] = 'value'`

---

_Issue opened by @spaceby on 2024-09-27 10:19_

I suggest adding a rule that detects this
```python
d = {}
d.update({'key': 'value'})
```

and replacing it
```python
d = {}
d['key'] = 'value'
```
As a result, the code becomes simpler and more understandable.

---

_Comment by @zanieb on 2024-09-27 13:23_

This make sense, but is just a style rule right? Is there a performance diff since you're skipping creating a dictionary?

---

_Label `rule` added by @zanieb on 2024-09-27 13:23_

---

_Comment by @ugochukwu-850 on 2024-09-27 13:50_

I like this issue, I am a new her btw [I'm also new to OSC],
But I did a little research and its variable , so I am not sure its safe to change based off only on aesthetics .
Basically at smaller and mid sized dict updates its ok to just assign but on larger updates creating a secondary dict as `update` does , would greatly increase the bulk update speed.

While its arguable , since the performance effects is variable maybe it should be optional.

---

_Comment by @AlexWaygood on 2024-09-29 15:29_

> This make sense, but is just a style rule right? Is there a performance diff since you're skipping creating a dictionary?

Even for small dictionaries, direct assignment is indeed quite a bit faster according to a very naive benchmark:

```
~/dev % python -m timeit -s "d = {}" "d.update({'key': 'value'})"            
5000000 loops, best of 5: 61.2 nsec per loop
~/dev % python -m timeit -s "d = {}" "d['key'] = 'value'"        
20000000 loops, best of 5: 12.2 nsec per loop
```

And this is what you'd expect, since the bytecode created for direct assignment is a lot simpler:

```pycon
~/dev/cpython % ./python.exe
Python 3.14.0a0 (heads/main:986a4e1b6fc, Sep 26 2024, 12:02:18) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> def x():
...     d = {}
...     d.update({'key': 'value'})
...     
>>> def y():
...     d = {}
...     d['key'] = 'value'
...     
>>> import dis
>>> dis.dis(x, adaptive=True)
  1           RESUME                   0

  2           BUILD_MAP                0
              STORE_FAST               0 (d)

  3           LOAD_FAST                0 (d)
              LOAD_ATTR                1 (update + NULL|self)
              LOAD_CONST               1 ('key')
              LOAD_CONST               2 ('value')
              BUILD_MAP                1
              CALL                     1
              POP_TOP
              RETURN_CONST             0 (None)
>>> dis.dis(y, adaptive=True)
  1           RESUME                   0

  2           BUILD_MAP                0
              STORE_FAST               0 (d)

  3           LOAD_CONST               1 ('value')
              LOAD_FAST                0 (d)
              LOAD_CONST               2 ('key')
              STORE_SUBSCR
              RETURN_CONST             0 (None)
```

But as I said, this benchmark is very naive:
- the keys and values being stored in the dictionary are all constants
- there's only a single item in the dictionary being passed to `.update()` when in most real-world uses of `dict.update` you'd generally pass in a dictionary that has multiple items.

@spaceby would your proposed rule only apply to dictionaries with a single item being passed to `.update`? Or do you also think it should suggest this?

```diff
-d.update({
-    "x": 1,
-    "y": 2,
-    "z": 3,
-})
+d["x"] = 1
+d["y"] = 2
+d["z"] = 3
```

---

_Label `needs-decision` added by @AlexWaygood on 2024-09-29 15:29_

---

_Label `needs-design` added by @AlexWaygood on 2024-09-29 15:29_

---

_Comment by @spaceby on 2024-09-29 16:49_

I think the rule should only apply to a dictionary with single item.
A rule similar in meaning to [B009](https://docs.astral.sh/ruff/rules/get-attr-with-constant/) - rewrite the code to something simpler.



---

_Comment by @AlexWaygood on 2024-09-29 16:53_

Makes sense... though I think we should explicitly state this in the docs for the rule, or we'll inevitably get a PR "improving" the rule so that it also covers dictionaries with multiple items 😄

---

_Label `needs-design` removed by @AlexWaygood on 2024-09-29 16:53_

---

_Comment by @autinerd on 2024-10-05 06:47_

For the update call with multiple entries, maybe the `|=` operator would be an idea? It seems to be slightly faster :thinking: 

```
❯ python -m timeit -s "d = {}" "d |= {'x': 1,'y': 2,'z': 3}"
2000000 loops, best of 5: 141 nsec per loop
❯ python -m timeit -s "d = {}" "d.update({'x': 1,'y': 2,'z': 3})"
2000000 loops, best of 5: 179 nsec per loop
```

And of course with single items, `|=` should also be replaced with directly assigning the value to the key.

---

_Comment by @AlexWaygood on 2024-10-05 10:46_

> For the update call with multiple entries, maybe the `|=` operator would be an idea? It seems to be slightly faster 🤔

Possibly, but I think that would be a different rule

---

_Comment by @MichaReiser on 2024-10-07 08:37_

> This make sense, but is just a style rule right? Is there a performance diff since you're skipping creating a dictionary?

I still think it's worthwhile to frame the motivation properly because, IMO, the performance difference seems too marginal to justify making it a rule.

---

_Comment by @tdulcet on 2024-10-07 09:59_

Maybe this would be a separate rule, but perhaps this rule could be generalized to avoid creating unnecessary intermediate representations. For example, these:
```py
l += ["value"]
l += ["value 1", "value 2"]
s |= {"value"}
s |= {"value 1", "value 2"}
d |= {"key": "value"}
d.update({"key": "value"})
# ect.
```
could potentially be rewritten as:
```py
l.append("value")
l.extend(("value 1", "value 2"))
s.add("value")
s.update(("value 1", "value 2"))
d["key"] = "value"
d["key"] = "value"
# ect.
```
This was first proposed in https://github.com/astral-sh/ruff/issues/8877#issuecomment-1835998576.

---

_Comment by @AlexWaygood on 2024-10-07 10:10_

> > This make sense, but is just a style rule right? Is there a performance diff since you're skipping creating a dictionary?
> 
> I still think it's worthwhile to frame the motivation properly because, IMO, the performance difference seems too marginal to justify making it a rule.

I think the performance difference is significant enough that it could easily make a difference in a hot loop somewhere... _but_ I agree that the motivation is primarily stylistic here, and that this is the kind of performance difference that could easily change as performance improvements are made to CPython itself. So I agree that this should be classified as a stylistic rule.

> Maybe this would be a separate rule, but perhaps this rule could be generalized to avoid creating unnecessary intermediate representations. For example, these:

Yes, this makes sense to me; all of these are essentially different manifestations of the same antipattern.

At this point I think I've seen enough support for this that I'd be happy to accept a PR implementing this rule (in the `RUF` category, I suppose, unless there have been any other linters to already implement this?).

---

_Label `needs-decision` removed by @AlexWaygood on 2024-10-07 10:10_

---

_Label `help wanted` added by @AlexWaygood on 2024-10-07 10:10_

---

_Label `accepted` added by @AlexWaygood on 2024-10-07 10:10_

---

_Referenced in [astral-sh/ruff#13706](../../astral-sh/ruff/issues/13706.md) on 2024-10-10 15:42_

---

_Referenced in [astral-sh/ruff#14564](../../astral-sh/ruff/pulls/14564.md) on 2024-11-24 05:05_

---

_Renamed from "new rule - replace d.update({'key': 'value'}) with d['key'] = 'value'" to "new rule - replace `d.update({'key': 'value'})` with `d['key'] = 'value'`" by @MichaReiser on 2024-11-25 08:30_

---

_Label `help wanted` removed by @MichaReiser on 2024-12-02 17:20_

---

_Referenced in [astral-sh/ruff#14831](../../astral-sh/ruff/pulls/14831.md) on 2024-12-08 00:40_

---

_Comment by @spaceby on 2025-11-10 09:32_

Any news?

---
