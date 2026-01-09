---
number: 8482
title: Better documentation of unsafe fixes
type: issue
state: closed
author: hauntsaninja
labels:
  - documentation
assignees: []
created_at: 2023-11-03T22:34:33Z
updated_at: 2024-08-20T10:55:27Z
url: https://github.com/astral-sh/ruff/issues/8482
synced_at: 2026-01-07T13:12:15-06:00
---

# Better documentation of unsafe fixes

---

_Issue opened by @hauntsaninja on 2023-11-03 22:34_

It would be nice if it was easier to figure out what checks have unsafe fixes, e.g. maybe there could be a different icon next to the rules in https://docs.astral.sh/ruff/rules/

It would also be nice if the per-rule documentation also mentioned why the fix is unsafe. E.g. for https://docs.astral.sh/ruff/rules/unnecessary-comprehension/ I'm left guessing, is it because ruff will apply the fix even when `dict` is bound to something else (edit: maybe because cases where we unpack we now lose errors)?

(For some user context: on a lot of our code, we only run ruff for autofixes and do not report errors for unfixable lints. Autofixing is a killer feature)

---

_Comment by @charliermarsh on 2023-11-03 23:51_

Yeah that's a nice idea. We've started doing this in some places via a "Known problems" section. I'll try to get the remaining rules covered next week.

(If helpful, you can also set `unsafe-fixes = true` in your configuration file to get the same behavior as before we shipped the applicability settings -- but I assume you're asking because you want to know which rules you should be fixing vs. not :))

---

_Label `documentation` added by @charliermarsh on 2023-11-03 23:51_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-03 23:51_

---

_Comment by @Avasam on 2023-11-04 19:55_

There's some overlap in the ideas from my comment in https://github.com/astral-sh/ruff/issues/8075#issuecomment-1783877479 so I'll copy here as well:

> On the rule page itself, I think you could have:
> "Fix is always available" / "Fix is sometimes available" / (no fix available)
> and separately mention that "Some fixes are marked [unsafe](https://docs.astral.sh/ruff/linter/#fix-safety) by default"
> 
> I don't think it's necessary to mention that "All fixes are unsafe". But at a user I wouldn't mind being a touch more exact. As long as it is show that I need to run `--unsafe-fixes` to get all the available fixes.
> 
> ---
> 
> As for the "all rules" page.
> You could differentiate between "sometimes autofixable" and "always autofixable" with 🔨 or 🔧, it's visually just 🛠️ but with a tool missing. I personally quite like that.
> 
> As fox fix safety, I'm less certain of the best way to display that / convey the information concisely. I think that adding a column should be fine.
> The first thing that comes to mind is ⚠️, but it looks out of place/scary even when grayed out: 
> ![image](https://github.com/astral-sh/ruff/assets/1350584/130736c3-d225-4c7b-bb66-bc940967a8ac)
> Other suggestions may include ⁉, 🚦, or just annotating the autofix with `*`, or even get fancy with it with a set-width column (or a 0-width element translated to the right position):
> ![image](https://github.com/astral-sh/ruff/assets/1350584/b07547c7-781f-4a32-b491-99cf37e475d2)

---

_Referenced in [astral-sh/ruff#8868](../../astral-sh/ruff/issues/8868.md) on 2023-11-28 14:39_

---

_Comment by @zanieb on 2023-11-28 18:42_

Please let us know if there's a rule with an unsafe fix that is not explained in the documentation, we can start tracking adding documentation at least.

---

_Comment by @hauntsaninja on 2023-11-29 05:20_

Rule with unsafe fix that caused me to open this issue was C416. I'll post on here as I notice others :-)

---

_Comment by @sh-at-cs on 2023-11-29 09:18_

Why the fix is unsafe is not explained for [`UP007`](https://docs.astral.sh/ruff/rules/non-pep604-annotation/), as mentioned in #8868.

---

_Comment by @charliermarsh on 2023-11-30 01:34_

(Adding docs for those rules now.)

---

_Comment by @charliermarsh on 2023-11-30 01:52_

Done in #8918.

---

_Comment by @hauntsaninja on 2023-12-17 22:08_

A few more unsafe fixes that could use docs:
- [x] UP028
- [x] PERF102
- [x] PT014
- [x] PYI025
- [x] PLR6201


---

_Comment by @charliermarsh on 2023-12-18 03:37_

I think UP028 _might_ be safe except in very rare circumstances. I'm trying to come up with an example in which the modification could lead to a breaking change.

---

_Referenced in [astral-sh/ruff#9306](../../astral-sh/ruff/pulls/9306.md) on 2024-01-02 00:38_

---

_Referenced in [astral-sh/ruff#9351](../../astral-sh/ruff/pulls/9351.md) on 2024-01-02 00:39_

---

_Comment by @notatallshaw-gts on 2024-01-02 18:46_

> I think UP028 _might_ be safe except in very rare circumstances. I'm trying to come up with an example in which the modification could lead to a breaking change.

Sure, when the generator is being sent values (and discarding them) but it's yielding values from an iterator it is an unsafe fix. 

E.g. this code works fine:

```python
def foo():
    for x in range(10):
        yield x

gen_foo = foo()
next(gen_foo)
gen_foo.send(10)
```

But UP028 fixes is to this:

```python
def foo():
    yield from range(10)

gen_foo = foo()
next(gen_foo)
gen_foo.send(10)
```

Which throws the exception:

```
Traceback (most recent call last):
  File "yield_example.py", line 7, in <module>
    gen_foo.send(10)
  File "yield_example.py", line 2, in foo
    yield from range(10)
AttributeError: 'range_iterator' object has no attribute 'send'
```

Now, there probably should be some rule about sending to a generator that doesn't read the values. But I'm sure someone has some code somewhere that does this.

---

_Comment by @charliermarsh on 2024-01-02 18:50_

@notatallshaw-gts - thank you!

---

_Referenced in [astral-sh/ruff#9364](../../astral-sh/ruff/pulls/9364.md) on 2024-01-02 19:27_

---

_Referenced in [astral-sh/ruff#9677](../../astral-sh/ruff/pulls/9677.md) on 2024-01-29 17:24_

---

_Referenced in [astral-sh/ruff#9678](../../astral-sh/ruff/pulls/9678.md) on 2024-01-29 17:26_

---

_Referenced in [astral-sh/ruff#9679](../../astral-sh/ruff/pulls/9679.md) on 2024-01-29 17:30_

---

_Comment by @charliermarsh on 2024-01-29 17:31_

Okay, all of the enumerated rules in the issue have either been documented or are being prompted to safe in v0.2.0. Going forward, I'm also ensuring that all unsafe fixes are included in the rule documentation for new fixes and rules.

---

_Closed by @charliermarsh on 2024-01-29 17:31_

---

_Comment by @sh-at-cs on 2024-08-20 10:55_

I don't know if it's fine to comment here or if I should open a new issue, but [SIM117's docs](https://docs.astral.sh/ruff/rules/multiple-with-statements/) don't have a "Fix safety" section, even though the fix for it seems to only get activated when enabling unsafe fixes. So it would be good if such a section was added.

---
