```yaml
number: 20218
title: "Django rule suggestion, spot misuse of DRF `fields = [\"__all__\"]`"
type: issue
state: open
author: Avasam
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-09-03T20:32:47Z
updated_at: 2025-09-16T16:10:58Z
url: https://github.com/astral-sh/ruff/issues/20218
synced_at: 2026-01-12T15:54:57Z
```

# Django rule suggestion, spot misuse of DRF `fields = ["__all__"]`

---

_@Avasam_

### Summary

Sorry for the less descriptive issue summary, I'm in a rush but didn't wanna forget about this.

We just had a contributor do the following in a Django Rest Framework Serializer, and I missed it in review. Seems like a common possible error that can be caught statically. So I figure I'd suggest it.

<img width="482" height="269" alt="Image" src="https://github.com/user-attachments/assets/c655ad2d-564c-4677-a108-464b3ce4c49d" />

It should be `fields = "__all__"` instead of `fields = ["__all__"]` or `fields = ("__all__",)`

---

_Comment by @ntBre on 2025-09-04 12:58_

Thanks! Should this just be `"__all__"` instead of a list? That's what I found from some searching, but I'm not too familiar with Django.

---

_Label `rule` added by @ntBre on 2025-09-04 12:58_

---

_Label `needs-decision` added by @ntBre on 2025-09-04 12:58_

---

_Comment by @Avasam on 2025-09-04 14:27_

Hi. A bit less in a rush this morning. You are correct. I've updated the issue. I also realize this is technically using `rest_framework.serializer.ModelSerializer`, not part of the core django, but it is such a popular plugin.

I understand if this is out of scope, then I'd wait for something like AST-based custom rules (relates to https://github.com/astral-sh/ruff/issues/283). I wanted to at least write it down.

---

_Renamed from "Django rule suggestion, spot misuse of `fields = ["__all__"]`" to "Django rule suggestion, spot misuse of DRF `fields = ["__all__"]`" by @Avasam on 2025-09-04 14:28_

---

_Comment by @MichaReiser on 2025-09-16 07:35_

Ideally, this would be something that a type checker would catch but it would require understanding that `Meta` needs to have a very specific shape when declared in a class inheriting from `ModelSerializer`

---

_Comment by @Avasam on 2025-09-16 16:07_

> Ideally, this would be something that a type checker would catch

Good ol' "str is a valid Sequence[str]" issue 😢
Still, good point though.
`fields` could be typed as `tuple[str,...] | list[str] | Literal["__all__"]` given this tends to be static, and only run once (being a classvar), I don't think anyone would really mind having to convert a Sequence to a List in special cases.

> but it would require understanding that `Meta` needs to have a very specific shape when declared in a class inheriting from `ModelSerializer`

Actually yeah, that might be more of an issue, I don't recall the typing system's limitations there off the top of my head. I'll try to play around with it and may bring that up with https://github.com/typeddjango/djangorestframework-stubs

At the very least it's something the mypy plugin may be able to handle.




---
