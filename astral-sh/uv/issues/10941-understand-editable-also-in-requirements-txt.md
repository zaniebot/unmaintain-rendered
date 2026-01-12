```yaml
number: 10941
title: "Understand `--editable=.` also in `requirements.txt`"
type: issue
state: closed
author: amotl
labels:
  - bug
  - compatibility
  - needs-decision
assignees: []
created_at: 2025-01-24T18:09:59Z
updated_at: 2025-01-25T20:30:32Z
url: https://github.com/astral-sh/uv/issues/10941
synced_at: 2026-01-12T16:00:24Z
```

# Understand `--editable=.` also in `requirements.txt`

---

_@amotl_

Hi. Thanks a stack for conceiving `uv`. It is a true game changer in so [many](https://github.com/kennethreitz/responder/issues/578) [scenarios](https://github.com/crate/crate/pull/17277). 💯 

### Summary

`requirements.txt` files can reference dependencies of the local package at hand inside the working tree by using different option syntax like `-e .`, `--editable .`, or `--editable=.`, iirc.

We are using the latter variant throughout our projects [^1], but it trips `uv`:

```
$ uv pip install -r docs/requirements.txt
error: Couldn't parse requirement in `docs/requirements.txt` at position 122
  Caused by: Expected package name starting with an alphanumeric character, found `=`
=.
^
```

Maybe this little report can help to make `uv` more universal in edge case situations like ours. It is certainly not a blocking issue for us, as we can easily update the offending line to use `-e .` instead of `--editable=.`.

### References
- https://github.com/astral-sh/uv/issues/10417
- https://github.com/astral-sh/uv/issues/1589
- https://github.com/astral-sh/uv/issues/1827
- https://github.com/astral-sh/uv/issues/1499

[^1]: Example: https://github.com/crate/crate-docs-theme/blob/0.37.2/docs/requirements.txt


### Platform

Darwin 22.6.0 x86_64

### Version

uv 0.5.23 (Homebrew 2025-01-23)

### Python version

Python 3.13.1

---

_Label `bug` added by @amotl on 2025-01-24 18:09_

---

_Comment by @zanieb on 2025-01-24 18:11_

Thanks for the clear report. I'm of the opinion that we shouldn't support this, but am curious what others think.

---

_Label `compatibility` added by @zanieb on 2025-01-24 18:11_

---

_Label `needs-decision` added by @zanieb on 2025-01-24 18:11_

---

_Comment by @charliermarsh on 2025-01-24 23:57_

I'm sort of fine with it, since it seems pretty trivial.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-25 00:58_

---

_Comment by @charliermarsh on 2025-01-25 02:35_

Oh interesting, we already support this for most values (but not `--editable`), that seems like an oversight.

---

_Closed by @charliermarsh on 2025-01-25 02:55_

---

_Comment by @amotl on 2025-01-25 20:30_

That was fast. Thank you for improving per GH-10954 so quickly.

---
