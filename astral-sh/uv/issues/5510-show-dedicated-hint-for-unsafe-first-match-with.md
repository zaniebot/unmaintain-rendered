```yaml
number: 5510
title: "Show dedicated hint for `--unsafe-first-match` with PyTorch index"
type: issue
state: closed
author: charliermarsh
labels:
  - error messages
assignees: []
created_at: 2024-07-27T18:41:00Z
updated_at: 2024-09-23T19:02:21Z
url: https://github.com/astral-sh/uv/issues/5510
synced_at: 2026-01-10T04:45:09Z
```

# Show dedicated hint for `--unsafe-first-match` with PyTorch index

---

_Issue opened by @charliermarsh on 2024-07-27 18:41_

This is so common that we should special-case it for a list of packages (like `requests`): https://github.com/astral-sh/rye/issues/1282#issuecomment-2254213458

---

_Label `error messages` added by @charliermarsh on 2024-07-27 18:41_

---

_Comment by @charliermarsh on 2024-09-22 18:37_

@zanieb -- For this one, do you think it's sufficient to show this if: we fail to resolve (can't find a valid version), and the failing package came from the first index, i.e., there are more indexes remaining? Is that too noisy?

---

_Comment by @zanieb on 2024-09-22 19:50_

Yeah, I think the minimal idea is that if we have a `NoVersions` incompatibility, there are multiple indexes, and you're using `first-match` we should hint. This is so common that I don't mind it being noisy to start. I think we should implement this ASAP, I started looking into it but need to figure out what index information is available to the reporter and what we have to thread

I have a few improvements in mind but am not sure what information is available, e.g.:

- Confirm that we didn't check all of the indexes for the package (i.e., as you suggested)
- Confirm if the package is available on a subsequent index
- Confirm if there is a version in the range available on a subsequent index

I think with these we could provide really helpful messages.

---

_Comment by @charliermarsh on 2024-09-22 20:28_

> Confirm if the package is available on a subsequent index

This is the hard one, because with `--first-match`, we don't even check subsequent indexes. So we'd either have to (1) start checking subsequent indexes, or (2) make new network requests in the error reporter.

---

_Comment by @zanieb on 2024-09-22 23:01_

I figured that might be the case, but we can try to solve that later. 

I think it'd be okay to have a low-priority pool checking the subsequent indexes in the background then finish the checks if needed for a no-solution error? 🤷‍♀️ 

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-23 14:24_

---

_Closed by @charliermarsh on 2024-09-23 19:02_

---

_Closed by @charliermarsh on 2024-09-23 19:02_

---
