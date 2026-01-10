```yaml
number: 2452
title: "Suggestion: mark fixable checks in the default output?"
type: issue
state: closed
author: henryiii
labels:
  - cli
assignees: []
created_at: 2023-02-01T18:24:46Z
updated_at: 2023-02-03T21:26:08Z
url: https://github.com/astral-sh/ruff/issues/2452
synced_at: 2026-01-10T11:09:45Z
```

# Suggestion: mark fixable checks in the default output?

---

_Issue opened by @henryiii on 2023-02-01 18:24_

One thing IMO that would really be nice would be Ruby Rubocop's feature of marking changes that support autofix. For example, on the dummy Ruby file:

```rb
# frozen_string_literal: true

def abs(x)
  if x > 0
    x
  else
    -x
  end
end
```

Rubocop produces this:

```
Inspecting 1 file
C

Offenses:

tmp.rb:3:9: C: Naming/MethodParameterName: Method parameter must be at least 3 characters long.
def abs(x)
        ^
tmp.rb:4:6: C: [Correctable] Style/NumericPredicate: Use x.positive? instead of x > 0.
  if x > 0
     ^^^^^

1 file inspected, 2 offenses detected, 1 offense auto-correctable
```

Yeah, the default is a little verbose, but notice the `[Correctable]` marker. That's fantastic, as it gives you an idea of what adding the fix (`-A` in Rubocop) flag will do, and what it won't. I miss not having some little mark or something in Ruff's output that lets me know what fixing will do and what it won't.

This is different than looking up each issue and checking to see if it's fixable or not, and it's a bit more helpful than just the summary of how many would be fixable, which is already there. I don't think it needs to be spelled out, but a little marker might be useful (like `[*]`, then `[*]` could be placed by the total number of fixable values, to help a user realize what it is there for.

Maybe the target is to try to mimic flake8's output as closely as possible, and that's okay, but just thought I'd point it out.

---

_Label `cli` added by @charliermarsh on 2023-02-01 18:26_

---

_Comment by @charliermarsh on 2023-02-01 18:27_

Yeah this is a nice suggestion (and an improvement over the `X potentially fixable` note we include in the output today). Would love feedback from others on what the right format could look like.

---

_Comment by @charliermarsh on 2023-02-01 19:39_

A few options...

```console
resources/test/fixtures/pyupgrade/UP035.py:47:14: F811 Redefinition of unused `Counter` from line 33
resources/test/fixtures/pyupgrade/UP035.py:50:15: F401 `a.b` imported but unused
resources/test/fixtures/pyupgrade/future_annotations.py:8:5: F401 `models.Nut` imported but unused
resources/test/fixtures/pyupgrade/future_annotations.py:26:19: F821 Undefined name `Bar` [*]
...
[*] 1 potentially fixable...
```

```console
resources/test/fixtures/pyupgrade/UP035.py:47:14: F811 Redefinition of unused `Counter` from line 33
resources/test/fixtures/pyupgrade/UP035.py:50:15: F401 `a.b` imported but unused
resources/test/fixtures/pyupgrade/future_annotations.py:8:5: F401 `models.Nut` imported but unused
[*] resources/test/fixtures/pyupgrade/future_annotations.py:26:19: F821 Undefined name `Bar`
...
[*] 1 potentially fixable...
```

I suspect that the former is more likely to be backwards-compatible for anyone relying on the format, and it avoids an offset in the filename.


---

_Comment by @henryiii on 2023-02-01 19:54_

Or:

```
resources/test/fixtures/pyupgrade/UP035.py:47:14: F811 Redefinition of unused `Counter` from line 33
resources/test/fixtures/pyupgrade/UP035.py:50:15: F401 `a.b` imported but unused
resources/test/fixtures/pyupgrade/future_annotations.py:8:5: F401 `models.Nut` imported but unused
resources/test/fixtures/pyupgrade/future_annotations.py:26:19: F821 [*] Undefined name `Bar`
```

Or

```
resources/test/fixtures/pyupgrade/UP035.py:47:14: F811 Redefinition of unused `Counter` from line 33
resources/test/fixtures/pyupgrade/UP035.py:50:15: F401 `a.b` imported but unused
resources/test/fixtures/pyupgrade/future_annotations.py:8:5: F401 `models.Nut` imported but unused
resources/test/fixtures/pyupgrade/future_annotations.py:26:19: [*] F821 Undefined name `Bar`
```

I like it at the end or in the middle (either example above), I don't like it at the beginning, makes the paths harder to read & quickly jump to.

Hey, how do you autofix an undefined name? :P

---

_Comment by @charliermarsh on 2023-02-01 20:07_

> Hey, how do you autofix an undefined name? :P

Haha oops, I was just adding stars randomly to real output :)

---

_Comment by @akx on 2023-02-02 12:26_

Related to #2149 :) 

---

_Comment by @charliermarsh on 2023-02-02 17:21_

I'm gonna go with:

```
resources/test/fixtures/pyupgrade/UP035.py:47:14: F811 Redefinition of unused `Counter` from line 33
resources/test/fixtures/pyupgrade/UP035.py:50:15: F401 `a.b` imported but unused
resources/test/fixtures/pyupgrade/future_annotations.py:8:5: F401 `models.Nut` imported but unused
resources/test/fixtures/pyupgrade/future_annotations.py:26:19: F821 [*] Undefined name `Bar`
```

It's least likely to break any existing clients as it retains the {file}:{row}:{column}:{code}:{message} structure (if we call `[*]` part of the message), visually clear, etc.


---

_Closed by @charliermarsh on 2023-02-03 21:26_

---
