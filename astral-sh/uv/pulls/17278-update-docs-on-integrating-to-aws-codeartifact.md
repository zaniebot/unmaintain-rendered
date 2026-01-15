```yaml
number: 17278
title: Update docs on integrating to AWS CodeArtifact with currently working version
type: pull_request
state: open
author: jaltgen
labels: []
assignees: []
draft: true
base: main
head: main
created_at: 2025-12-31T10:40:53Z
updated_at: 2026-01-15T20:21:37Z
url: https://github.com/astral-sh/uv/pull/17278
synced_at: 2026-01-15T20:53:27Z
```

# Update docs on integrating to AWS CodeArtifact with currently working version

---

_@jaltgen_

## Summary

Upon recently using uv in CICD in CodeBuild and wanting to use a private pypi repo in AWS CodeArtifact, I noticed the documented method with the given env vars was producing a 401 "Unauthorized" error when connecting to the repository. After debugging and referencing the [AWS docs](https://docs.aws.amazon.com/codeartifact/latest/ug/python-configure-pip.html#python-configure-without-pip) I tried passing the token via the URL and it worked just fine. 

Decided to include this working method and improve the code snippets to be easy to copy over if someone else needs this.

Also, first contribution, happy to amend and take on-board recommendations.

## Test Plan

Implemented the code snippet in my CICD pipeline and successfully authenticated to my private pypi-compatible repo.


---

_Comment by @zanieb on 2026-01-03 13:42_

I think we need to understand more about why it wasn't working for you before we change anything here.

---

_Converted to draft by @zanieb on 2026-01-15 20:21_

---
