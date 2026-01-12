```yaml
number: 12361
title: Add Installation Option Using HTTPie
type: pull_request
state: closed
author: nenas1ya
labels: []
assignees: []
base: main
head: patch-1
created_at: 2025-03-21T10:51:58Z
updated_at: 2025-05-04T16:57:50Z
url: https://github.com/astral-sh/uv/pull/12361
synced_at: 2026-01-12T16:10:14Z
```

# Add Installation Option Using HTTPie

---

_@nenas1ya_

Added installation option using httpie

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This pull request introduces an additional installation option for the uv project using httpie. The purpose of this change is to provide users with an alternative method to install the package, enhancing accessibility and usability. By including httpie, users can leverage its user-friendly command-line interface for making HTTP requests, which can simplify the installation process for those who prefer it over traditional methods.

---

_Comment by @zanieb on 2025-03-21 13:29_

Thanks for taking the time to contribute. The bar for new installation methods is quite high. Isn't `httpie` for testing APIs? It seems like a weird fit here.

---

_Comment by @nenas1ya on 2025-03-22 16:38_

@zanieb, Thanks for the reply. First of all - yes, it is used for testing, but it can be as multitasking as a curl.

---

_Comment by @charliermarsh on 2025-05-04 16:57_

Per @zanieb's comment, I think we're going to omit this for now, but thank you for contributing.

---

_Closed by @charliermarsh on 2025-05-04 16:57_

---
