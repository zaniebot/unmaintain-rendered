```yaml
number: 4134
title: Ruff fails to exclude a file, tries to parse it, and bails
type: issue
state: closed
author: flying-sheep
labels:
  - question
assignees: []
created_at: 2023-04-27T15:22:13Z
updated_at: 2023-04-28T10:01:04Z
url: https://github.com/astral-sh/ruff/issues/4134
synced_at: 2026-01-10T11:09:47Z
```

# Ruff fails to exclude a file, tries to parse it, and bails

---

_Issue opened by @flying-sheep on 2023-04-27 15:22_

I have a git repository. Since it’s a cookiecutter template, it contains a directory called `{{cookiecutter.project_name}}`[sic]. I try to run Ruff on the other files in the repo like this:

```console
$ ruff "--extend-exclude=\\{\\{cookiecutter.project_name\\}\\}/**" --force-exclude -v .
[2023-04-27][17:17:32][ruff::resolver][DEBUG] Ignored path via `exclude`: "/Users/phil/Dev/cookiecutter-scverse/.git"
error: TOML parse error at line 50, column 1
   |
50 | {% if cookiecutter.project_name.lower().replace('-', '_') != cookiecutter.package_name -%}
   | ^
invalid key
```

Ruff shouldn’t try to parse that TOML file, I’m excluding it!

---

_Comment by @flying-sheep on 2023-04-27 15:59_

I circumvented it by using pre-commit’s `files` key, but I guess for others this would still be an issue.

---

_Comment by @charliermarsh on 2023-04-27 17:55_

Hmm yeah, that does look like a bug. I thought I'd fixed this in #3588, just confirming that you're on a recent release?

---

_Label `question` added by @charliermarsh on 2023-04-27 17:56_

---

_Comment by @charliermarsh on 2023-04-27 18:57_

The linked PR suggests v0.0.250 -- mind checking if something like v0.0.260 or newer resolves this?

---

_Comment by @flying-sheep on 2023-04-28 10:01_

Huh, did use a freshly installed command line Ruff to verify (without actually checking if it was the newest version), but now I can’t reproduce it.

Apologies!

---

_Closed by @flying-sheep on 2023-04-28 10:01_

---
