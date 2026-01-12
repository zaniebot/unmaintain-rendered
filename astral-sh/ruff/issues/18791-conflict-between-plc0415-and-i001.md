```yaml
number: 18791
title: Conflict between PLC0415 and I001
type: issue
state: open
author: cdce8p
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-06-19T12:00:47Z
updated_at: 2025-07-24T19:27:47Z
url: https://github.com/astral-sh/ruff/issues/18791
synced_at: 2026-01-12T15:54:56Z
```

# Conflict between PLC0415 and I001

---

_@cdce8p_

### Summary

Disabling `PLC0415` inline with `noqa` triggers `I001` as soon as the line-length is exceeded. Wrapping it to the next line doesn't resolve it unfortunately.

```py
def ok():
    from custom_components_abc import x  # noqa: PLC0415

def bad():
    from custom_component_abcddddddddd import call_get_integration_frame  # noqa: PLC0415

def bad2():
    from custom_component_abcddddddddd import (  # noqa: PLC0415
        call_get_integration_frame
    )

```

```
L5 -> Import block is un-sorted or un-formatted
L8 -> Import block is un-sorted or un-formatted
```

https://play.ruff.rs/af23bcb0-ee1d-4a2e-82dd-f8655a94d041

### Version

ruff v0.12.0

---

_Comment by @ntBre on 2025-06-19 12:30_

I think I001 is just being picky about the missing trailing comma in your `bad2` example. In your playground link, if I apply the I001 code action, it adds the comma and both rules are satisfied.

However, it feels like a bug that applying I001 to `bad` breaks the noqa comment.

---

_Label `bug` added by @ntBre on 2025-06-19 12:31_

---

_Label `fixes` added by @ntBre on 2025-06-19 12:31_

---

_Comment by @ntBre on 2025-06-19 12:34_

It's a very different presentation, but I think this boils down to the same issue as https://github.com/astral-sh/ruff/issues/12179.

---

_Comment by @cdce8p on 2025-06-19 13:04_

> I think I001 is just being picky about the missing trailing comma in your `bad2` example. In your playground link, if I apply the I001 code action, it adds the comma and both rules are satisfied.

Missed that one. Yes indeed adding the comma resolve the issue.

> However, it feels like a bug that applying I001 to `bad` breaks the noqa comment.

> It's a very different presentation, but I think this boils down to the same issue as [#12179](https://github.com/astral-sh/ruff/issues/12179).

Yes, I think that is the real issue here. With `ruff check --fix` the `noqa` comment is removed which in turn triggers `PLC0415` again without really resolving the issue.

---

_Comment by @holtskinner on 2025-07-24 19:27_

> > I think I001 is just being picky about the missing trailing comma in your `bad2` example. In your playground link, if I apply the I001 code action, it adds the comma and both rules are satisfied.
> 
> Missed that one. Yes indeed adding the comma resolve the issue.
> 
> > However, it feels like a bug that applying I001 to `bad` breaks the noqa comment.
> 
> > It's a very different presentation, but I think this boils down to the same issue as [#12179](https://github.com/astral-sh/ruff/issues/12179).
> 
> Yes, I think that is the real issue here. With `ruff check --fix` the `noqa` comment is removed which in turn triggers `PLC0415` again without really resolving the issue.

I'm experiencing the same issue

---
