```yaml
number: 14585
title: PLC0206 should not flag if dict is being written
type: issue
state: open
author: silverwind
labels:
  - rule
assignees: []
created_at: 2024-11-25T15:07:05Z
updated_at: 2024-12-02T18:18:29Z
url: https://github.com/astral-sh/ruff/issues/14585
synced_at: 2026-01-10T11:09:56Z
```

# PLC0206 should not flag if dict is being written

---

_Issue opened by @silverwind on 2024-11-25 15:07_

This code is flagged for [PLC0206](https://docs.astral.sh/ruff/rules/dict-index-missing-items/) with v0.8.0:

```py
foo = {}
for key in foo.keys(): # PLC0206 Extracting value from dictionary without calling `.items()`
  if foo[key]:
    foo[key] = True
```

Arguable, changing `.keys()` to `.items()` here does not improve readability because one still needs to use `foo[key]` to write into the dict:

```py
foo = {}
for key, value in foo.items():
  if value:
    foo[key] = True
```

Suggestion would be to not trigger the rule when any write to the dict is present. Interestingly, the rule already does not flag when the `if` line is removed.

---

_Comment by @brody4hire on 2024-11-25 15:17_

What about using `.filter()` to only update items that match your criteria?

---

_Comment by @silverwind on 2024-11-25 15:26_

> What about using `.filter()` to only update items that match your criteria?

The code isn't really meant to be taken seriously and the actual code where I hit this was more complex. If you like a slightly more complex example, take this one, which is also flagged:

```py
foo = {}
for key in foo.keys():
  if foo[key] == "foo":
    foo[key] = "bar"
  elif foo[key] == "bar":
    foo[key] = "foo"
```

---

_Comment by @MichaReiser on 2024-11-25 15:58_

That makes sense. I first thought that this is the same as https://github.com/astral-sh/ruff/issues/13821 but it's not because #13821 also reads the value while iterating

---

_Label `rule` added by @MichaReiser on 2024-11-25 15:58_

---

_Comment by @dylwil3 on 2024-11-26 04:26_

> Interestingly, the rule already does not flag when the if is removed.

That's because the rule triggers the first time it sees `foo[key]` in a _load_ context - like in the if statement.

I'm actually kind of borderline on whether we should or shouldn't flag, especially because we already don't flag if `foo[key]` is _only_ used when someone assigns a new value to it (or deletes it).

Already in your second example, it starts to get a little verbose. I kind of find it more readable:

```python
for key,val in foo.items():
  if val == "foo":
    foo[key] = "bar"
  elif val == "bar":
    foo[key] = "foo"
```

One could imagine even more gymnastics:

```python
for key in foo:
   # <--- all sorts of complicated expressions with the value foo[key] --->
   if cond_now_holds:
       foo[key] = 10
```

It seems in the spirit of the rule to flag this.

But I also see your point. I'm on the fence.

(P.S. - ignore the linked PR below. Bad copy pasting ... too many tabs open 😅 )

---

_Comment by @MichaReiser on 2024-11-26 07:33_

Oh, @dylwil3, you're right. This is the same as #13821. That's what you get for replying late in the day :laughing: 

I then agree that the rule should flag this pattern because it helps remove some repetition. It's unfortunate that Python doesn't allow assigning to `*value = new_value` the same way as rust does

---

_Comment by @kpfleming on 2024-12-01 11:37_

I've got another one where PLC0206 is triggered but the loop is already 'expensive':

```python
class MyConfigParser(configparser.ConfigParser):
  def as_dict(self) -> Mapping[str, Any]:
    d = dict(self._sections)  # type: ignore[attr-defined]
    for k in d:
      d[k] = self._defaults | d[k]  # type: ignore[attr-defined]
      d[k].pop("__name__", None)
    return d
```

I've already changed the code to silence the warning, but since Python doesn't provide a *reference* to the value attached to 'k', only a copy, writing to that key and calling methods on it will always require indexing by the key.

---

_Comment by @MichaReiser on 2024-12-01 15:40_

Thanks. I consider flagging the read after any write a bug. We should fix that.

---

_Label `bug` added by @MichaReiser on 2024-12-01 15:40_

---

_Label `help wanted` added by @MichaReiser on 2024-12-01 15:41_

---

_Comment by @dylwil3 on 2024-12-01 18:41_

This still feels somewhat borderline to me. On the one hand, as in the previous examples, you can use the value in your reassignment:

```python
class MyConfigParser(configparser.ConfigParser):
  def as_dict(self) -> Mapping[str, Any]:
    d = dict(self._sections)  # type: ignore[attr-defined]
    for k,v in d.items():
      d[k] = self._defaults | v  # <--- can use v here
      d[k].pop("__name__", None)
    return d
```

You might worry that if you don't reassign but just use `pop` then it wouldn't revise the value, but as long as you don't reassign `v` in the for loop it will still point to the mutable value, e.g.:

```console
>>> d = {"a": {"b":1, "c":2}}
>>> for k,v in d.items():
...    v.pop("b")
...
1
>>> d
{'a': {'c': 2}}
```

I guess in this example you couldn't use `v`:

```python
for k in d:
    d[k] = new_object_not_depending_on_v
    d[k].pop()
```
but, of course, in that case you could have modified the new value for `d[k]` _before_ assigning it to `d[k]` and you wouldn't get a lint warning.

---

_Comment by @kpfleming on 2024-12-01 22:19_

Indeed, and that's exactly the change I made to the code to silence the warning. I don't really consider it to be an 'improvement' but it's also not harmful. The only reason I posted here is because this rule is intended to make the code more efficient, but in the situation where the value at the key is being written and/or other operations are being invoked on it, the efficiency gained is likely to be a tiny fraction of the overall time consumed by the loop.

The loop could also be written to use a temporary variable to hold the value of the 'default or v' expression, and then `pop` invoked on that, before writing to `d[k]`. I wouldn't consider this really an 'improvement' either, but this is all pretty subjective :-)

---

_Label `bug` removed by @MichaReiser on 2024-12-02 08:09_

---

_Label `help wanted` removed by @MichaReiser on 2024-12-02 08:09_

---
