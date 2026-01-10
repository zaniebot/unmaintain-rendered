---
number: 16715
title: Add .env to .gitignore and optionally create requirements.txt in uv init
type: issue
state: closed
author: aryanj10
labels:
  - enhancement
assignees: []
created_at: 2025-11-13T03:27:52Z
updated_at: 2025-11-28T17:27:28Z
url: https://github.com/astral-sh/uv/issues/16715
synced_at: 2026-01-10T01:26:09Z
---

# Add .env to .gitignore and optionally create requirements.txt in uv init

---

_Issue opened by @aryanj10 on 2025-11-13 03:27_

### Summary

Hi! I’d love to suggest two small DX improvements for `uv init`:

### 1. Add `.env` to the default `.gitignore`
Many Python projects use a `.env` file (e.g., python-dotenv, django-environ).  
Accidentally committing `.env` is common, so having it ignored by default feels safe and low-risk.

### 2. Optional creation of `requirements.txt`
Some teams still rely on `requirements.txt` for CI/Docker workflows.  
`uv export` is great, but having an empty file created on `uv init` (or behind a flag like `uv init --requirements`) would be nice.

### Questions
- Would maintainers be open to adding `.env` to the init template?
- For `requirements.txt`, is there interest in:
  - adding a flag, or
  - keeping the current philosophy and skipping this?

### Example

_No response_

---

_Label `enhancement` added by @aryanj10 on 2025-11-13 03:27_

---

_Comment by @Zander-1024 on 2025-11-16 13:02_

you can use `uv add -r requirements.txt`

---

_Comment by @zanieb on 2025-11-16 17:54_

I don't think we should add `.env` to the `.gitignore`, I'd prefer our default ignore from `uv init` to just include files that are specifically relevant to Python and always present.

I don't think we should generate a `requirements.txt`, I don't really understand your workflow around that? It doesn't make sense to me for us to create an empty file and it'd be moving backwards from our intended project workflows.

---

_Closed by @charliermarsh on 2025-11-28 17:27_

---

_Reopened by @charliermarsh on 2025-11-28 17:27_

---

_Closed by @charliermarsh on 2025-11-28 17:27_

---
