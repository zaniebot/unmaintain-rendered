```yaml
number: 2778
title: "feat: stray quote keep quote"
type: pull_request
state: closed
author: fatelei
labels: []
assignees: []
base: main
head: issue-2551
created_at: 2024-04-02T15:48:46Z
updated_at: 2024-04-23T18:37:19Z
url: https://github.com/astral-sh/uv/pull/2778
synced_at: 2026-01-12T16:05:13Z
```

# feat: stray quote keep quote

---

_@fatelei_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

stray quote keep quote for python >= "3.7", fix #2551 

## Test Plan

rust playground: https://gist.github.com/rust-play/eebce903f361ab4b58d5c7c98f4a3537


---

_Comment by @fatelei on 2024-04-02 15:49_

when i add test:

```
LenientVersionSpecifiers::from_str(">=\"3.6\"").unwrap().into();
```

there is a error: expected version to start with a number

---

_Comment by @konstin on 2024-04-03 09:06_

PEP 508, the standard for the requirements, is a bit tricky here: The main version specifier must not have quotes, while the version specifiers in the markers must have quotes.

This is a correct requirement: `numpy >=1.19; python_version >= "3.7"`
This one is incorrect, we want to fix it up: `numpy ">=1.19"; python_version >= "3.7"`
But this one is also incorrect, the markers need quotes: `numpy >=1.19; python_version >= 3.7`

So when we see `numpy ">=1.19"; python_version >= "3.7"`, we only want to remove the first set of quotes, not the second set of quotes.

---

_Assigned to @konstin by @zanieb on 2024-04-09 14:50_

---

_Comment by @ibraheemdev on 2024-04-23 15:28_

I opened up https://github.com/astral-sh/uv/pull/3214, which should address this. Thanks @fatelei for your initial work on this.

---

_Closed by @zanieb on 2024-04-23 18:37_

---
