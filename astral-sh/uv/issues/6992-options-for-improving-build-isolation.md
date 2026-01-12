```yaml
number: 6992
title: Options for improving build isolation
type: issue
state: closed
author: dakinggg
labels:
  - question
assignees: []
created_at: 2024-09-04T01:59:23Z
updated_at: 2024-09-04T02:32:27Z
url: https://github.com/astral-sh/uv/issues/6992
synced_at: 2026-01-12T15:59:09Z
```

# Options for improving build isolation

---

_@dakinggg_

As I'm sure you know, build isolation is a real pain for packages with a requirement that packages are built with the same version of another package as will be used at runtime. This is very common in the pytorch ecosystem, and makes supporting multiple pytorch versions much more difficult. I'm sure there are many possible ways to improve the situation, but one thing I would like is to be able to specify in my pyproject.toml that one of my dependencies should be installed without build isolation. So I could make package A, that depends on package B, always install package B without build isolation. Wondering if something like this is a feature you would consider.

Apologies if there is an issue about this already (there are a lot of issues related to build isolation).

---

_Comment by @charliermarsh on 2024-09-04 02:09_

We do currently support disabling build isolation for individual packages with https://docs.astral.sh/uv/reference/settings/#no-build-isolation-package. Is that something like what you're imagining? We also have some docs here on the workflows required to get it working in projects: https://docs.astral.sh/uv/concepts/projects/#build-isolation

I'm interested in better solutions to this problem though -- not happy with what we have today. I'm kind of imagining something whereby you declare that packages that your package needs to include (e.g., you could say that package A needs PyTorch), and then we link those into the the build environment...

---

_Label `question` added by @charliermarsh on 2024-09-04 02:09_

---

_Comment by @dakinggg on 2024-09-04 02:23_

That does seem to be exactly what I described, thanks! Just to check, does that specification still work if I am using the `uv pip` interface?

And agreed on there must be better solutions...I suppose ideally my installation would understand somehow that both package A and package B rely on pytorch, and the same version of pytorch must be used for building B and running A, I would not need to preinstall pytorch in my environment, and it would just work with a simple `install` of package A.

---

_Comment by @charliermarsh on 2024-09-04 02:26_

Yup, `--no-build-isolation-package` is supported in the `uv pip` interface.

---

_Comment by @dakinggg on 2024-09-04 02:27_

Amazing! From my perspective, question is answered, feel free to close as you see fit.

---

_Closed by @charliermarsh on 2024-09-04 02:32_

---
