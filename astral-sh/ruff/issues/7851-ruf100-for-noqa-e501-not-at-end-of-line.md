```yaml
number: 7851
title: "RUF100 for noqa: E501 not at end of line"
type: issue
state: open
author: LefterisJP
labels:
  - bug
  - suppression
assignees: []
created_at: 2023-10-08T08:11:26Z
updated_at: 2023-12-07T18:58:00Z
url: https://github.com/astral-sh/ruff/issues/7851
synced_at: 2026-01-10T11:09:50Z
```

# RUF100 for noqa: E501 not at end of line

---

_Issue opened by @LefterisJP on 2023-10-08 08:11_

With the new ruff release `ruff==0.0.292` all unneeded noqa: E501 comments are detected! Yay!

But this seems to also be valid (or at least have been working until now), having the noqa: E501 in the middle of the comment and then have more comment text after it.

```python
# line length 99
averylongvariablenamethatyouprobablyshouldneveruse = 'avalue'  # noqa: E501  # ruff will try to autofix/remove cause by the end of the pragma the line is <99 but with this extra comment we made it go over, so once autofixed there is a line length violation
```

If you run `ruff --fix` on this it will autofix the noqa: E501 away thinking it's not needed since by the end of noqa: E501 line length is less than the limit. Ignoring the fact that the comment continues.

So we will end up with this:

```
# line length 99
averylongvariablenamethatyouprobablyshouldneveruse = 'avalue'  # ruff will try to autofix/remove cause by the end of the pragma the line is <99 but with this extra comment we made it go over, so once autofixed there is a line length violation
```

Which will then hit again with a line length violation and need manual fix/action.

---

_Comment by @charliermarsh on 2023-10-08 13:35_

Ahh very tricky! Thanks, this is a good call.

---

_Label `bug` added by @charliermarsh on 2023-10-08 13:35_

---

_Label `noqa` added by @charliermarsh on 2023-10-08 13:35_

---

_Comment by @charliermarsh on 2023-10-08 13:35_

I hope the change was overall a net positive :)

---

_Comment by @LefterisJP on 2023-10-08 15:02_

> I hope the change was overall a net positive :)

Oh yes ofc. Just had 1 file where we had 50 of those in a row and was annoying to fix by hand. But as you can see it's 2,5k lines PR that's linked :+1: 

Most of it autofixed!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-07 18:57_

---
