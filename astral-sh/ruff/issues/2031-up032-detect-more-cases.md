```yaml
number: 2031
title: "UP032: detect more cases"
type: issue
state: open
author: spaceone
labels:
  - fixes
assignees: []
created_at: 2023-01-20T15:49:40Z
updated_at: 2025-03-16T02:10:52Z
url: https://github.com/astral-sh/ruff/issues/2031
synced_at: 2026-01-10T11:09:44Z
```

# UP032: detect more cases

---

_Issue opened by @spaceone on 2023-01-20 15:49_

UP032 could detect more cases when `.format()` is used and suggest to use a fstring.

```
'Magic wand: {}'.format(bag['wand'])
'Magic wand: {}'.format(bag["wand"])
'Magic wand: {}'.format('bag')
'Magic wand: {}'.format(bag)
```
(only the last is detected).

A fixer for all cases is of course not trivial: https://peps.python.org/pep-0701/

---

_Label `autofix` added by @charliermarsh on 2023-01-20 16:58_

---

_Comment by @link2xt on 2023-02-08 12:13_

Also multiline `.format()` is not detected:
```
'Magic wand: {}'.format(
  bag
)
```

---

_Comment by @charliermarsh on 2023-02-08 15:40_

IIRC we only flag what we can confidently fix. We could instead flag _all_ usages of `.format`, even those we can't fix right now...

---

_Comment by @Avasam on 2024-08-17 21:12_

I found a special case today that I think should be safe to fix:
```py
"  reading {filename}".format(**locals())
```
I don't see why fixing to `f"  reading {filename}"` shouldn't be safe as `filename` should necessarily be in the local scope (otherwise the code would already fail anyway)


---

_Comment by @Avasam on 2024-08-20 21:04_

Here's another fun case I found today in setuptools again:
```py
    '%(foo)s:%(bar)s' % vars(x)
    # can be either
    '{x.foo}:{x.bar}'.format(x=x)
    f'{x.foo}:{x.bar}'

    # original example I found:
    def __str__(self):
        return '%(username)s:%(password)s' % vars(self)
```

Which also makes me realize that for UP031:
```py
    '%(foo)s:%(bar)s' % x
    # can be
    '{foo}:{bar}'.format(**x)
```

---

_Comment by @tdulcet on 2025-01-12 16:27_

UP031 does not flag this, although I thought it was supposed to detect everything after #11229:
```py
'├─%s─' % (len(l) * '─')
```
After manually updating it use `str.format`, it is also not detected by UP032:
```py
'├─{}─'.format(len(l) * '─')
```
The issue seems to be the string multiplication, as removing that causes them to be detected by both rules respectively as expected. This of course should have no effect on UP031, as an autofix would not need to embed the string and thus change the quote style like it would with UP032.


---

_Comment by @inoa-jboliveira on 2025-03-16 02:10_

> IIRC we only flag what we can confidently fix. We could instead flag _all_ usages of `.format`, even those we can't fix right now...

Yes please. I don't believe there is a single format op that cannot be expressed as f-string. Specially with python 3.12 changes regarding quotes and backslashes.

So at least give us the warnings even if no fix exists. That is the purpose of a checker. IMO not showing a warn for `"{}".format("a")` should be considered a bug.

---
