```yaml
number: 22598
title: ruff server applies exclude rules from parent directory configuration
type: issue
state: open
author: iggyfisk
labels: []
assignees: []
created_at: 2026-01-15T13:47:03Z
updated_at: 2026-01-15T13:47:03Z
url: https://github.com/astral-sh/ruff/issues/22598
synced_at: 2026-01-15T14:51:09Z
```

# ruff server applies exclude rules from parent directory configuration

---

_@iggyfisk_

### Summary

Minimal reproduction is available at https://github.com/iggyfisk/ruff-native-server-ignore-issue-reproduction

I have a monorepo with several Python packages that share code and ruff configuration, and an isolated package that doesn't share any code with the rest of the repo, and has its own ruff configuration. This is set up as a VSCode workspace where the repo root is one folder, and the isolated package is another folder. 

Because the ruff configuration is different for the isolated package than the rest of the repo, I want to exclude that package from ruff when running from the root, using `extend-exclude`. And then to run ruff for the isolated package I can run it from within that directory, and it uses the Python virtual environment and ruff config that's specific for the isolated package. 

All of this works perfectly from the command line, and in VSCode it works perfectly for everything except the isolated package. When editing the isolated package in VSCode, ruff is completely disabled. I can fix this and enable ruff in my isolated package by either:

* Turn off the native language server by setting `"ruff.nativeServer": "off"` in my `.code-workspace` file. Which I'd rather not since ruff-lsp is deprecated.
* Remove the isolated package from `extend-exclude` in the root `pyproject.toml`. Which I'd rather not since then ruff would try to lint/format the isolated package with the wrong rules when running from the command line, unless I specify specific exclude rules as arguments (and remember to do that everywhere in CI, git hooks etc)
* Manually specify `include` in the root `pyproject.toml`. Which I'd rather not since I might forget to add new packages as they appear.



### Version

ruff 0.14.11, charliermarsh.ruff  2026.34.0, VSCode 1.108.1

---
