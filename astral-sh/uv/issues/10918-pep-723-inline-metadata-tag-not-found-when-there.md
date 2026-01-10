```yaml
number: 10918
title: "PEP 723 inline metadata tag not found when there's trailing whitespace"
type: issue
state: open
author: snat-s
labels:
  - error messages
assignees: []
created_at: 2025-01-23T22:27:55Z
updated_at: 2025-01-24T20:14:30Z
url: https://github.com/astral-sh/uv/issues/10918
synced_at: 2026-01-10T04:27:58Z
```

# PEP 723 inline metadata tag not found when there's trailing whitespace

---

_Issue opened by @snat-s on 2025-01-23 22:27_

### Summary

i have an issue with PEP 723, when you have a blank space at the end of the `# ///` `uv` doesn't properly parse the closing tag. it tells you that there is an error:

```
error: An opening tag (`# /// script`) was found without a closing tag (`# ///`). Ensure that every line between the opening and closing tags (including empty lines) starts with a leading `#`.
```

all the script had was an extra blank space at the end, it can be a bit annoying if this happens.

```
# /// script
# requires-python = ">=3.11"
# dependencies = [
#   "requests-oauthlib",
#   "requests"
#   "rich",
# ]
# /// 
```
but not that it is simply that there is an extra blank space at the end.

i'm not sure if it already solved but just letting you know!

### Platform

Linux 6.12.9-200.fc41.x86_64 x86_64 GNU/Linux

### Version

uv 0.5.1

### Python version

Python 3.11.10

---

_Label `bug` added by @snat-s on 2025-01-23 22:27_

---

_Comment by @zanieb on 2025-01-23 22:32_

I think we "must" do this per the specification (https://peps.python.org/pep-0723/#specification)


---

_Comment by @zanieb on 2025-01-23 22:32_

cc @ofek perhaps that's an oversight?

---

_Comment by @snat-s on 2025-01-23 22:42_

maybe an error would help? like to see that you had an extra space and you have to fix it might help

---

_Comment by @ofek on 2025-01-23 22:48_

I think using an exact match is preferable but I don't have a strong preference. What do you think?

---

_Comment by @zanieb on 2025-01-23 22:51_

We could definitely hint on a similar ending tag to what we're looking for.

I think trailing whitespace is _fairly_ reasonable to allow, but does complicate things. I don't have strong feelings.

---

_Renamed from "issues with PEP 723 – Inline script metadata" to "PEP 723 inline metadata tag not found when there's trailing whitespace" by @zanieb on 2025-01-23 23:04_

---

_Label `needs-decision` added by @zanieb on 2025-01-23 23:05_

---

_Comment by @charliermarsh on 2025-01-24 14:44_

I suppose I'm fine to allow trailing whitespace.

---

_Comment by @notatallshaw on 2025-01-24 16:05_

While I think it would be a UX improvement, a trailing whitespace would not be matched by the reference implementation: https://peps.python.org/pep-0723/#reference-implementation

Someone implementing themselves would find the reference implementation doesn't work _some_ of the time even though it works when they use other tools, and they would need to figure out why.

That is to say, if trailing whitespace is allowed, I would like the PEP (or just spec page? I don't know how these kind of corrections flow to which documents) updated, including the reference implementation (which would then make clear what type of whitespaces would be allowed (vertical tabs?)).

---

_Comment by @charliermarsh on 2025-01-24 16:34_

Hmm yeah. I think you're right. We should continue to reject this. We can add a better error message if anyone is motivated to do so, though.

---

_Label `bug` removed by @charliermarsh on 2025-01-24 16:36_

---

_Label `needs-decision` removed by @charliermarsh on 2025-01-24 16:36_

---

_Label `error messages` added by @charliermarsh on 2025-01-24 16:36_

---

_Comment by @hugovk on 2025-01-24 20:14_

> That is to say, if trailing whitespace is allowed, I would like the PEP (or just spec page? I don't know how these kind of corrections flow to which documents) updated, including the reference implementation (which would then make clear what type of whitespaces would be allowed (vertical tabs?)).

PEP editor hat:

Just the spec. The PEP has a banner saying:

> This PEP is a historical document. The up-to-date, canonical spec, [Inline script metadata](https://packaging.python.org/en/latest/specifications/inline-script-metadata/#inline-script-metadata), is maintained on the [PyPA specs page](https://packaging.python.org/en/latest/specifications/).
> 
> See the [PyPA specification update process](https://www.pypa.io/en/latest/specifications/#handling-fixes-and-other-minor-updates) for how to propose changes. 

---
