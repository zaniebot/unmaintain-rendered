```yaml
number: 12541
title: Check supported plugins for new codes
type: issue
state: closed
author: nkakouros
labels:
  - question
  - needs-info
assignees: []
created_at: 2024-07-27T08:19:22Z
updated_at: 2024-09-09T12:03:44Z
url: https://github.com/astral-sh/ruff/issues/12541
synced_at: 2026-01-10T11:09:54Z
```

# Check supported plugins for new codes

---

_Issue opened by @nkakouros on 2024-07-27 08:19_

It would be nice to have a CI job report new, unsupported codes for the plugins (or tools) that ruff already has some support for. There could be a blacklist of codes that were purposefully omitted in ruff. 

As it stands now, users have to request support for new codes. Reading in the docs that e.g. `flake8-builtins` rules are supported may also give the wrong impression that ruff and the plugin are on par. Searching in the issue tracker may not return a relevant issue. The user then has to visit the flake8 extension's page and compare the ruleset with ruff's rules.

Since the list of supported plugins and tools is known, extracting the codes should be straightforward.

Would there be interest in such a CI job? 

---

_Comment by @MichaReiser on 2024-07-27 16:40_

Hi @nkakouros 

Thanks for the suggestion. Can you tell me more about what the CI job would do and how it would work? What does it do when it detects a missing rule? 

---

_Comment by @charliermarsh on 2024-07-27 17:07_

It's a reasonable request but I have no idea how we'd extract them reliably.

---

_Label `question` added by @charliermarsh on 2024-07-27 21:34_

---

_Label `needs-info` added by @MichaReiser on 2024-09-09 12:03_

---

_Closed by @MichaReiser on 2024-09-09 12:03_

---
