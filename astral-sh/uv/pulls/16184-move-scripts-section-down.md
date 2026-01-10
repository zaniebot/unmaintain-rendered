```yaml
number: 16184
title: "Move 'Scripts' section down"
type: pull_request
state: closed
author: mrexodia
labels: []
assignees: []
base: main
head: patch-1
created_at: 2025-10-08T12:38:04Z
updated_at: 2025-10-09T15:47:23Z
url: https://github.com/astral-sh/uv/pull/16184
synced_at: 2026-01-10T06:36:15Z
```

# Move 'Scripts' section down

---

_Pull request opened by @mrexodia on 2025-10-08 12:38_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Opening this as more of a discussion. A friend of mine (who is not super familiar with programming) started using `uv` and for his project he is using inline metadata in every script, presumably because this feature is mentioned in the documentation first.

Personally I do not think you should use this, except in niche use cases (like downloading a single script from the internet). Mainly because VSCode does not support it, whereas a regular project's `.venv` is automatically recognized. I would propose moving the discussion about projects earlier in the documentation, to indicate that this is the recommended path in most use cases.

## Test Plan

Not relevant.


---

_Comment by @zanieb on 2025-10-08 13:26_

The current ordering makes sense for covering topics in increasing complexity. We're hoping widespread editor support for inline metadata isn't far off!

---

_Comment by @mrexodia on 2025-10-08 13:52_

> The current ordering makes sense for covering topics in increasing complexity. 

I understand the idea, but I do not think 'increasing complexity' is the right sort order here. In my experience, users do not read the documentation back to back, they read until they find what they _think_ are looking for and then close it. The best practice should be first, the rest might as well not exist.

> We're hoping widespread editor support for inline metadata isn't far off!

At the present moment this is not supported. The current situation already led to a concrete user doing the following:

1. Run `uv add --script foo.py requests` because it is mentioned first in the documentation
2. Open the folder in VS Code
3. Complain that the dependencies are not detected correctly
4. Create their own venv or doing some other blind workarounds until the editor works, completely defeating the point of using `uv` in the first place

I honestly have no idea how VSCode will ever support `uv`here (since there is no mention of `uv` in the script and there is no project), but I digress...

---

_Comment by @zanieb on 2025-10-08 14:22_

> I honestly have no idea how VSCode will ever support uvhere (since there is no mention of uv in the script and there is no project), but I digress...

The metadata format in the script is a Python standard that was approved in Jan 2024 https://peps.python.org/pep-0723/

Editor support _is_ coming — we're working with the VS Code team.

---

_Comment by @mrexodia on 2025-10-08 15:22_

Yeah that's great, but I don't think it changes anything for current beginner users who are having a real problem _today_.

---

_Comment by @zanieb on 2025-10-09 14:11_

I appreciate the feedback, but we're going to keep this as-is for now.

---

_Closed by @zanieb on 2025-10-09 14:11_

---

_Comment by @mrexodia on 2025-10-09 14:56_

So the recommendation for new users is to use inline metadata for all their scripts? Because if not this can be clarified in a different way…

---

_Comment by @zanieb on 2025-10-09 15:47_

Yes, I think inline metadata is the current best practice for scripts. I think we'll just continue to invest in improving the editor experience then document that.

---
