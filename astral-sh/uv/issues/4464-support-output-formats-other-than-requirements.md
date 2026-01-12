```yaml
number: 4464
title: Support output formats other than requirements.txt for uv pip compile
type: issue
state: open
author: frostming
labels:
  - wish
assignees: []
created_at: 2024-06-24T03:22:21Z
updated_at: 2024-06-24T12:53:24Z
url: https://github.com/astral-sh/uv/issues/4464
synced_at: 2026-01-12T15:58:50Z
```

# Support output formats other than requirements.txt for uv pip compile

---

_@frostming_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

It would be great if uv pip compile can produce a JSON format output with more information such as package dependencies, and origin URLs. As they are now not included in the requirements.txt

This will benefit other tools like PDM to integrate uv more easily.

I know [PEP 665](https://peps.python.org/pep-0665/) is still under discussion so I just report it here to see if you have any interest.

---

_Comment by @charliermarsh on 2024-06-24 11:05_

If PEP 665 wasn't around I would definitely be interested in this, but as-is it's hard to add another output format given that we'd need to support PEP 665 too.

---

_Label `wish` added by @charliermarsh on 2024-06-24 12:53_

---
