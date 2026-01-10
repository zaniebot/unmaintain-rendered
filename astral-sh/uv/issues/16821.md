```yaml
number: 16821
title: "Documentation for the behavior of `source-exclude`"
type: issue
state: closed
author: himkt
labels:
  - documentation
assignees: []
created_at: 2025-11-23T09:46:52Z
updated_at: 2025-12-10T04:56:53Z
url: https://github.com/astral-sh/uv/issues/16821
synced_at: 2026-01-10T03:11:35Z
```

# Documentation for the behavior of `source-exclude`

---

_Issue opened by @himkt on 2025-11-23 09:46_

### Question

Hello uv developers, thank you so much for your all efforts to develop uv! I want to suggest one small suggestion for the documentation.

At first, I didn't understand the behaviors of `source-exclude` and `wheel-exclude` of uv build backend; specifying `wheel-exclude` only affects the contents of .whl but `source-exclude` affects both .tar.gz and .whl.

There are two options both for sdist and wheel (https://docs.astral.sh/uv/reference/settings/#build-backend_source-exclude and https://docs.astral.sh/uv/reference/settings/#build-backend_wheel-exclude), made me think that each option would work for sdist and wheel, respectively. But when I create a wheel, the setting in `source-exclude` also applied to .whl (which surprised me a bit).

After reading the documentation of [`uv build`](https://docs.astral.sh/uv/reference/cli/#uv-build), I noticed that **the wheel was made from the source distribution when I ran `uv build`** (not `uv build --sdist --wheel`) and I understand the reason of this behavior (so, I had to read the docs more carefully). But I also think it would take time to understand this behavior for users directly landing the settings documentation.

Given the journey, I have one suggestion; how about adding some explanation about `uv build` (=wheel will be created from sdist) in the documentation for `source-exclude`? (I think it would be sufficient to add a link to uv-build from https://docs.astral.sh/uv/reference/settings/#build-backend_source-exclude). Of course, I'd be happy to make a pull request if it would help.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @himkt on 2025-11-23 09:46_

---

_Renamed from "[RFC] documentation for the behavior of `source-exclude`" to "Documentation for the behavior of `source-exclude`" by @konstin on 2025-11-24 13:27_

---

_Label `question` removed by @konstin on 2025-11-24 13:28_

---

_Label `documentation` added by @konstin on 2025-11-24 13:28_

---

_Comment by @konstin on 2025-11-24 13:34_

Makes sense, it's explained in the guide but it's strange that it's missing in the reference docs: https://github.com/astral-sh/uv/pull/16832

---

_Closed by @zanieb on 2025-12-09 21:14_

---

_Comment by @himkt on 2025-12-10 04:56_

Thank you so much @konstin and @zanieb!

---
