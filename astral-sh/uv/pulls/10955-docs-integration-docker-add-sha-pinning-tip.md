```yaml
number: 10955
title: "[docs/integration/docker] add sha pinning tip"
type: pull_request
state: merged
author: ryan-ph
labels:
  - documentation
assignees: []
merged: true
base: main
head: ryan-ph/guides/integration/docker/tip-pin-sha
created_at: 2025-01-25T06:40:13Z
updated_at: 2025-01-27T18:29:24Z
url: https://github.com/astral-sh/uv/pull/10955
synced_at: 2026-01-12T16:09:36Z
```

# [docs/integration/docker] add sha pinning tip

---

_@ryan-ph_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

As requested in https://github.com/astral-sh/uv/issues/6565, this adds a tip discussing the ability to pin the image to a specific SHA digest and why it may be useful.

## Test Plan

<!-- How was it tested? -->

Start serving the documentation locally

```shell
uvx --with-requirements docs/requirements.txt -- mkdocs serve -f mkdocs.public.yml
```

Then navigate to http://127.0.0.1:8000/uv/guides/integration/docker/  to see the tool tip being rendered properly

---

_@zanieb reviewed on 2025-01-25 16:31_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:121 on 2025-01-25 16:31_

This only is usable with the Docker image, not the installer, right?

Should we move this up to below just the Dockerfile example and show _how_ to pin to a SHA?

---

_@ryan-ph reviewed on 2025-01-27 06:29_

---

_Review comment by @ryan-ph on `docs/guides/integration/docker.md`:121 on 2025-01-27 06:29_

Yea, that's good point. Moved the tooltip and added a codeblock with the sha for `0.5.24`: https://github.com/astral-sh/uv/compare/731a317dfa5c2fde9b9588c7c417d63592e405fe..4c0b29ed03871fb998a79c4e1f923dd829207780

---

_Review requested from @zanieb by @ryan-ph on 2025-01-27 06:29_

---

_@zanieb approved on 2025-01-27 18:27_

---

_@zanieb reviewed on 2025-01-27 18:28_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:121 on 2025-01-27 18:28_

```suggestion
    # e.g., using a hash from a previous release
```

---

_@zanieb reviewed on 2025-01-27 18:28_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:121 on 2025-01-27 18:28_

Unfortunately we'll auto-update 0.5.24 -> 0.5.25 on the next release but don't have a way to bump the SHA

---

_Label `documentation` added by @zanieb on 2025-01-27 18:28_

---

_Merged by @zanieb on 2025-01-27 18:29_

---

_Closed by @zanieb on 2025-01-27 18:29_

---
