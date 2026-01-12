```yaml
number: 3608
title: "Rewrite `os.environ.get` as `os.getenv`"
type: issue
state: open
author: janosh
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-03-19T15:36:04Z
updated_at: 2025-08-07T21:30:56Z
url: https://github.com/astral-sh/ruff/issues/3608
synced_at: 2026-01-12T15:54:43Z
```

# Rewrite `os.environ.get` as `os.getenv`

---

_@janosh_

**Bad**

```py
os.environ.get(secret)
os.environ.get(secret, "fallback")
```

**Good**

```py
os.getenv(secret)
os.getenv(secret, "fallback")
```

---

_Label `rule` added by @charliermarsh on 2023-03-19 15:52_

---

_Comment by @dhruvmanila on 2023-03-20 08:47_

I'm a bit hesitant of this change as in the CPython implementation (https://github.com/python/cpython/blob/5e6661bce968173fa45b74fa2111098645ff609c/Lib/os.py#L779-L783), they both are equivalent and there's no set recommendation to use one or the other.

However, there is a difference between `os.environ[key]` and `os.getenv(key)` where the former will raise a `KeyError` if the key doesn't exist in the environment.

Another thing to note is that in the documentation, there's this note regarding **updating an environment variable:**

> Note Calling [putenv()](https://docs.python.org/3/library/os.html#os.putenv) directly does not change [os.environ](https://docs.python.org/3/library/os.html#os.environ), so it’s better to modify [os.environ](https://docs.python.org/3/library/os.html#os.environ). 
>
> _Ref_: https://docs.python.org/3/library/os.html#os.environ



---

_Comment by @janosh on 2023-03-20 14:49_

Just a suggestion. Feel free to close or put this in one of the opinionated rule sets.

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:29_

---

_Comment by @Tatsh on 2023-12-17 09:16_

I would actually recommend the opposite. putenv does not update os.environ. getenv caches os.environ at import time which can happen at the very first import in any given module.

>Note Calling putenv() directly does not change os.environ, so it’s better to modify os.environ.


>Note that since getenv() uses os.environ, the mapping of getenv() is similarly also captured on import, and the function may not reflect future environment changes.

I was searching if there was a rule to error when getenv/putenv are encountered to replace with os.environ. That would be a welcome addition and sadly I don't know enough Rust.

---

_Comment by @Vir2S on 2024-11-22 08:46_

> I'm a bit hesitant of this change as in the CPython implementation (https://github.com/python/cpython/blob/5e6661bce968173fa45b74fa2111098645ff609c/Lib/os.py#L779-L783), they both are equivalent and there's no set recommendation to use one or the other.
> 
> However, there is a difference between `os.environ[key]` and `os.getenv(key)` where the former will raise a `KeyError` if the key doesn't exist in the environment.
> 
> Another thing to note is that in the documentation, there's this note regarding **updating an environment variable:**
> 
> > Note Calling [putenv()](https://docs.python.org/3/library/os.html#os.putenv) directly does not change [os.environ](https://docs.python.org/3/library/os.html#os.environ), so it’s better to modify [os.environ](https://docs.python.org/3/library/os.html#os.environ).
> > _Ref_: https://docs.python.org/3/library/os.html#os.environ

They are not totally equivalent)

---

_Comment by @TrevinAvery on 2025-03-22 01:37_

> I would actually recommend the opposite. putenv does not update os.environ. getenv caches os.environ at import time which can happen at the very first import in any given module.
> 
> > Note Calling putenv() directly does not change os.environ, so it’s better to modify os.environ.
> 
> > Note that since getenv() uses os.environ, the mapping of getenv() is similarly also captured on import, and the function may not reflect future environment changes.
> 
> I was searching if there was a rule to error when getenv/putenv are encountered to replace with os.environ. That would be a welcome addition and sadly I don't know enough Rust.

I fully agree with this idea. I want my team's code base is using a single path to modify the environment. My current use case: I was just trying to find where an environment variable was being loaded and searched for `os.environ`. To my surprise, I didn't find it. In this situation, I didn't know the name of the environment variable, so this was my only option. I like knowing that I can see all entries with a simple search.

I would recommend that `os.getenv` be replaced by `os.environ.get`, as suggested above, because it would be more "pythonic", and likely have less confusion. Why not just directly access a dictionary that is mutable, allowing the expected behavior when an update is made? This seems much easier to understand.

Ultimately, I am in favor of enabling people to have their preferences, and thus I suggest we have a rule that allows a preference for either one.

---

_Comment by @jhbuhrman on 2025-07-04 15:49_

> [...]
> Ultimately, I am in favor of enabling people to have their preferences, and thus I suggest we have a rule that allows a preference for either one.

Although in general this is a humble line of thinking, I guess the argument in https://github.com/astral-sh/ruff/issues/3608#issuecomment-1859082473 counts much heavier, so I'm quite in favor to ban the use of `os.{put,get}env()` completely.

---

_Comment by @sergiitk on 2025-08-07 21:25_

While Python documentation doesn't have a recommendation for the prefered way to get environment variables, it's pretty clear about modifying them:

> Assignments to items in [`os.environ`](https://docs.python.org/3.13/library/os.html#os.environ) are automatically translated into corresponding calls to [`putenv()`](https://docs.python.org/3.13/library/os.html#os.putenv); however, calls to [`putenv()`](https://docs.python.org/3.13/library/os.html#os.putenv) don’t update [`os.environ`](https://docs.python.org/3.13/library/os.html#os.environ), **so it is actually preferable to assign to items of [`os.environ`](https://docs.python.org/3.13/library/os.html#os.environ)**.
> — https://docs.python.org/3.13/library/os.html#os.putenv

And [`os.unsetenv()`](https://docs.python.org/3.13/library/os.html#os.unsetenv) has the same note on preferring `os.environ` for deleting env vars.
Note that updates to `os.environ` are automatically forwarded to [`os.environb`](https://docs.python.org/3.13/library/os.html#os.environb).

IMO it makes sense to prefer `os.environ.get()` over `os.getenv()` to be consistent with the recommended way to modify the env vars. For example, one might incorrectly chose to use suboptimal `os.putenv()` just because they saw `os.getenv()` elsewhere in the same file, while if we used `os.environ.get()` they're more likely to default to the prefered `os.environ.put()`.

---
