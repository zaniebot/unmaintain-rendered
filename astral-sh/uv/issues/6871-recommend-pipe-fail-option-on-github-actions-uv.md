---
number: 6871
title: Recommend pipe fail option on GitHub actions uv install? 
type: issue
state: open
author: notatallshaw
labels:
  - documentation
assignees: []
created_at: 2024-08-30T14:05:01Z
updated_at: 2024-09-03T13:59:21Z
url: https://github.com/astral-sh/uv/issues/6871
synced_at: 2026-01-10T01:24:07Z
---

# Recommend pipe fail option on GitHub actions uv install? 

---

_Issue opened by @notatallshaw on 2024-08-30 14:05_

I'd be happy to open a PR to update the docs, but it wasn't clear to me just yet if this *should* be recommended.

I set up a github workflow in my company's local github enterprise install with the following step taken from the [docs](https://docs.astral.sh/uv/guides/integration/github/#installation):

```yaml
 - name: Set up uv
   # Install latest uv version using the installer
   run: curl -LsSf https://astral.sh/uv/install.sh | sh
```

However, what I found was if the curl command fails this step can still succeed, this can happen because of either curl not being available, some network issue, or something else. I ended up replacing this step with:

```yaml
  - name: Set up uv
    shell: bash
    run: | # Install latest uv version using the installer
      set -o pipefail
      curl -LsSf https://astral.sh/uv/install.sh | sh
```

So if the curl command fails it produces an error.

Do you think the docs should be updated in general? Or do you think this is enough of an edge case not to make the instructions overly complicated? 


---

_Comment by @pplmx on 2024-08-30 15:56_

I also encountered this issue while building from the `Dockerfile`. When running the command `RUN curl -LsSf https://astral.sh/uv/install.sh | sh`, it failed. However, the next build skips this layer.

---

_Label `documentation` added by @zanieb on 2024-09-03 13:58_

---

_Comment by @zanieb on 2024-09-03 13:59_

Yeah we should set pipefail, PR welcome.

@pplmx I'd highly recommend copying from our Docker image instead of using `curl` to install (this is covered in the docs too)

---
