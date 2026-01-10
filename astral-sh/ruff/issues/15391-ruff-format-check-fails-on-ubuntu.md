```yaml
number: 15391
title: ruff format --check Fails on Ubuntu
type: issue
state: closed
author: MelbourneDeveloper
labels:
  - formatter
  - needs-mre
assignees: []
created_at: 2025-01-09T22:29:15Z
updated_at: 2025-01-28T17:48:40Z
url: https://github.com/astral-sh/ruff/issues/15391
synced_at: 2026-01-10T11:09:56Z
```

# ruff format --check Fails on Ubuntu

---

_Issue opened by @MelbourneDeveloper on 2025-01-09 22:29_

I am working on macOS, and I use GitHub Actions. I run this in the pipeline:

```bash
ruff format --check .
```

It passes on Mac, but fails in Ubuntu. This is what I get:

> ruff format --check .
Would reformat: ai_coding_agent/tests/test_api_e2e.py
Would reformat: ai_coding_agent/tests/test_devagent.py
Would reformat: ai_coding_agent/tests/test_devagent_e2e.py
3 files would be reformatted, 34 files already formatted

Three files in particular are not formatted the same way. The rest are fine,. I would guess that this a line ending issue, but I've tried changing git attributes etc. and nothing seemed to make any difference. I've tried hard refreshing these files and also I tried running this on Ubuntu before running the formatting check:

```bash
find . -name "*.py" -type f -exec perl -pi -e 's/\r\n|\r/\n/g' {} +
```

Nothing seems to make any difference. 

I couldn't see any specific config for line endings in the documentation [here](https://docs.astral.sh/ruff/formatter/#ruff-format).

---

_Comment by @zanieb on 2025-01-09 22:40_

Can you share a reproducible example? i.e. host the file somewhere?

---

_Label `needs-mre` added by @MichaReiser on 2025-01-10 06:39_

---

_Label `formatter` added by @MichaReiser on 2025-01-10 07:13_

---

_Comment by @MelbourneDeveloper on 2025-01-16 23:32_

@zanieb unfortunately, I can't. However, I have been able to reproduce the problem locally on my Mac, and it's totally bizarre.

When I run this script:

```bash
#!/bin/bash

set -e

source venv/bin/activate

ruff format --check .
```

I get this:

> Would reformat: ai_coding_agent/tests/test_api_e2e.py
Would reformat: ai_coding_agent/tests/test_devagent.py
Would reformat: ai_coding_agent/tests/test_devagent_e2e.py
Would reformat: ai_coding_agent/tests/test_devagent_mocks.py
Would reformat: ai_coding_agent/tests/test_devagent_unit.py
5 files would be reformatted, 34 files already formatted

If I remove or delete the activation beforehand, the format check passes

```bash
#!/bin/bash

set -e

ruff format --check .
```

> 39 files already formatted

---

_Comment by @zanieb on 2025-01-17 00:27_

When you activate the environment are you getting a different version of Ruff? e.g., `which ruff` / `ruff version` in both examples? Are you using the same Python version?

---

_Comment by @MelbourneDeveloper on 2025-01-26 20:52_

Hmmmm...

Now that you mention it, I've ended up with two ways to get the ruff dependency and they could be interfering with each other.

This is the dev dependencies file in my test folder.

```
ruff==0.8.4
black==24.10.0
isort==5.13.2 
```

This is the section in the pyproject.toml:

```
[project.optional-dependencies]
dev = [
    "ruff>=0.1.9",
    "black>=23.12.1",
    "pytest>=7.4.3",
    "pytest-cov>=4.1.0",
    "pytest-asyncio>=0.23.2",
    "python-lsp-server[all]>=1.9.0",
    "python-lsp-ruff>=1.5.1",
    "pylsp-mypy>=0.6.7",
    "pyright>=1.1.335",
]
```

I need to work out which one I should be using and delete the other

---

_Comment by @zanieb on 2025-01-28 17:48_

That makes sense. I'm going to close since I think there's nothing actionable for us here.

---

_Closed by @zanieb on 2025-01-28 17:48_

---
