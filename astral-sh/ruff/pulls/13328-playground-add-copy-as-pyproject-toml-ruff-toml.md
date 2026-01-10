```yaml
number: 13328
title: "Playground: Add Copy as pyproject.toml/ruff.toml and paste from TOML"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
assignees: []
merged: true
base: main
head: micha/playground-copy-paste-toml
created_at: 2024-09-11T18:48:12Z
updated_at: 2024-09-13T12:44:26Z
url: https://github.com/astral-sh/ruff/pull/13328
synced_at: 2026-01-10T21:38:32Z
```

# Playground: Add Copy as pyproject.toml/ruff.toml and paste from TOML

---

_Pull request opened by @MichaReiser on 2024-09-11 18:48_

## Summary

This PR extends our playground so that you can paste a `pyproject.toml` or `ruff.toml` configuration into the settings panel and 
it automatically converts it to the JSON configuration. 

This PR also adds a *Copy as pyproject.toml* and *Copy as ruff.toml* action that converts the JSON configuration to a TOML confguration. 

The motivation for extending the playground is that it makes it easier to try out configurations provided by users and to share configurations verified in the playground in a format that's familiar to users. 
I considered changing the settings a TOML file instead. However, monaco has no autocompletion or schema support for TOML files, which means that switching to TOML would lead to a worse overall editing experience. 

The pasting could be "nicer" because the implementation let's monaco handle the paste and then replaces the pasted content if it is a TOML setting. 
Users can see how the content change. I spent quiet a bit of time trying to figure out if there's a more official way of handling
pasting from the clip board in monaco and customizing the behavior by either overriding the standard paste operation or
adding a cutom paste action but was unsuccessful. That's why I ultimately went with the solution in this PR.

## Test Plan

https://github.com/user-attachments/assets/9d9db3c1-d80e-46bc-b65b-9e85b0d3e4df



---

_Label `playground` added by @MichaReiser on 2024-09-11 18:48_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-09-11 18:49_

---

_Merged by @MichaReiser on 2024-09-13 12:44_

---

_Closed by @MichaReiser on 2024-09-13 12:44_

---

_Branch deleted on 2024-09-13 12:44_

---
