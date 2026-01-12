```yaml
number: 1280
title: Pin actions in release workflow to version tags
type: pull_request
state: closed
author: silamon
labels: []
assignees: []
base: main
head: ty_pin_actions_to_version
created_at: 2025-09-29T17:12:20Z
updated_at: 2025-09-29T20:33:32Z
url: https://github.com/astral-sh/ty/pull/1280
synced_at: 2026-01-12T15:54:27Z
```

# Pin actions in release workflow to version tags

---

_@silamon_

<!--
Thank you for contributing to ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This workflow doesn't pin the Github Actions to a specific version using SHA tags. Now it does, just like the others, which will prevent weekly Dependabot PR's based on commit only. 

You can find the release SHA's here. 
https://github.com/actions/checkout/releases/tag/v5.0.0
https://github.com/actions/download-artifact/releases/tag/v5.0.0
https://github.com/actions/upload-artifact/releases/tag/v4.6.2

## Test Plan



---

_Comment by @MichaReiser on 2025-09-29 17:56_

Thank you. Unfortunately, this doesn't work because cargo dist doesn't support adding the `# version` comment (CC: @Gankra). But it would certainly be nice to have. 

---

_Closed by @MichaReiser on 2025-09-29 17:56_

---

_Comment by @zanieb on 2025-09-29 18:00_

Yes it does!

https://github.com/astral-sh/uv/blob/170ab1cd7fb795bdfa228162ace3328c7ccf0d12/dist-workspace.toml#L78-L82

---

_Comment by @MichaReiser on 2025-09-29 20:04_

It does support git commits (which we use in ty), but it doesn't copy over the version comment (unless the numbers of whitespaces matters)

---

_Comment by @zanieb on 2025-09-29 20:33_

Ah I see — I misread your message. Thanks for the correction :)

---
