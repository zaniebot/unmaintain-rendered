```yaml
number: 4789
title: "Implement `perflint`"
type: issue
state: open
author: 93578237
labels:
  - plugin
  - accepted
assignees: []
created_at: 2023-06-01T16:45:31Z
updated_at: 2023-09-15T01:15:53Z
url: https://github.com/astral-sh/ruff/issues/4789
synced_at: 2026-01-12T15:54:44Z
```

# Implement `perflint`

---

_@93578237_

https://github.com/tonybaloney/perflint - Python Linter for performance anti patterns.

- [x] PERF101: Unnecessary list() on already iterable type (unnecessary-list-cast)
- [x] PERF102: Incorrect iterator method for dictionary (incorrect-dictionary-iterator)
- [ ] PERF201: Loop invariant statement (loop-invariant-statement)
- [ ] PERF202: Global name usage in a loop (loop-global-usage)
- [x] PERF203: Try..except blocks have a significant overhead. Avoid using them inside a loop (loop-try-except-usage)
- [ ] PERF204: Looped slicing of bytes objects is inefficient. Use a memoryview() instead (memoryview-over-bytes)
- [ ] PERF205:  Importing the "%s" name directly is more efficient in this loop. (dotted-import-in-loop)
- [ ] PERF301: Use tuple instead of list for a non-mutated sequence. (use-tuple-over-list)
- [x] PERF401: Use a list comprehension instead of a for-loop (use-list-comprehension)
- [x] PERF402: Use a list copy instead of a for-loop (use-list-copy)
- [x] PERF403: Use a dictionary comprehension instead of a for-loop (use-dict-comprehension)

---

_Label `plugin` added by @charliermarsh on 2023-06-01 19:39_

---

_Comment by @93578237 on 2023-06-07 13:12_

Also rules from https://github.com/tonybaloney/anti-patterns
like https://github.com/tonybaloney/anti-patterns/pull/4 and https://github.com/tonybaloney/anti-patterns/pull/5

---

_Comment by @qdegraaf on 2023-06-07 21:22_

If that's a separate plugin it's best to make a different issue for that. As for perflint, I will wait until the first PR is merged before looking at further rules.

---

_Comment by @qdegraaf on 2023-06-15 11:26_

Rules checklist:

EDIT: Added to Issue body

---

_Comment by @evanrittenhouse on 2023-06-15 12:49_

@qdegraaf I can pick up some of these - are there any that you're particularly interested in? Don't want to overstep 

---

_Comment by @charliermarsh on 2023-06-15 13:27_

Thanks @qdegraaf - I went ahead and added that to the issue body :)

---

_Comment by @qdegraaf on 2023-06-15 14:09_

> @qdegraaf I can pick up some of these - are there any that you're particularly interested in? Don't want to overstep

Awesome! I've got an almost complete PERF101. Otherwise haven't picked up anything yet. I'll open a draft PR for PERF101 and otherwise take your pick! Feel free to ping me on the Discord if you want to collaborate or check availability.

---

_Comment by @evanrittenhouse on 2023-06-17 15:59_

I can grab PERF203 and PERF301 to start.

---

_Comment by @qdegraaf on 2023-06-22 11:34_

I'll start looking at 401 and 402 

EDIT: Picked up 403 in a separate PR as well. I can leave the 2X and 3X to you if you like @evanrittenhouse 

---

_Comment by @evanrittenhouse on 2023-06-23 14:40_

@qdegraaf You can grab 3X as well! I'll do 2X, if that works for you?

---

_Comment by @93578237 on 2023-06-23 14:50_

> If that's a separate plugin it's best to make a different issue for that.

Rules from perflint are derived from [tonybaloney/anti-patterns](https://github.com/tonybaloney/anti-patterns).
These rules ( https://github.com/tonybaloney/anti-patterns/pull/4 and https://github.com/tonybaloney/anti-patterns/pull/5) are not yet implemented in perflint (https://github.com/tonybaloney/perflint/issues/31)

So these 2 rules (join with generator and regex with IGNORECASE) should be part of perflint or RUF rules?

---

_Comment by @qdegraaf on 2023-06-23 14:57_

> > If that's a separate plugin it's best to make a different issue for that.
> 
> Rules from perflint are derived from [tonybaloney/anti-patterns](https://github.com/tonybaloney/anti-patterns). These rules ( [tonybaloney/anti-patterns#4](https://github.com/tonybaloney/anti-patterns/pull/4) and [tonybaloney/anti-patterns#5](https://github.com/tonybaloney/anti-patterns/pull/5)) are not yet implemented in perflint ([tonybaloney/perflint#31](https://github.com/tonybaloney/perflint/issues/31))
> 
> So these 2 rules (join with generator and regex with IGNORECASE) should be part of perflint or RUF rules?

@charliermarsh what's the protocol for this. I think it makes functional sense to add it to `perflint` but not sure if Ruff is ok with expanding upon upstream plugin scope for direct ports.

> @qdegraaf You can grab 3X as well! I'll do 2X, if that works for you?

Cool, works for me! If I get round to them at some point in the near future I'll make sure to announce it here. Feel free to change your mind in the meantime if you find time before me.

---

_Label `accepted` added by @charliermarsh on 2023-07-10 01:26_

---

_Comment by @flying-sheep on 2023-07-14 10:01_

PERF203 should only trigger when there’s no loop flow control (`break`, `continue`) in the try-catch statement. E.g. I don’t think there’s a better way to write the following:

```py
for repo in ['R-patched', 'cran', 'bioc']:
    try:
        pkg_cache = self._fetch_cache(repo, pkg)
        break
    except HTTPError:
        pass
else:
    return None
```

---
