---
number: 14359
title: "New logging rule: logging conditional on verbosity"
type: issue
state: open
author: sbrugman
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-11-15T11:37:57Z
updated_at: 2024-11-16T21:58:55Z
url: https://github.com/astral-sh/ruff/issues/14359
synced_at: 2026-01-07T13:12:16-06:00
---

# New logging rule: logging conditional on verbosity

---

_Issue opened by @sbrugman on 2024-11-15 11:37_

The following anti-pattern occurs when users are not aware of how to adjust the stdlib logging visibility:

```python
if verbosity > 1:
    logger.info("...")
```

Logging occurs often, and expands the code rapidly and making it harder to parse.
instead, they should use the log level, something along the lines of:

```python
level = logging.INFO if verbosity > 1 else logging.ERROR
logging.basicConfig(level=level)
```

and users can change the log level of the message:

```python
logger.debug("...")
```

Example: https://github.com/Azure/azure-sdk-for-python/blob/main/sdk/ai/azure-ai-generative/azure/ai/generative/index/_tasks/update_pinecone.py#L183

---

_Comment by @MichaReiser on 2024-11-16 21:58_

It's unclear how we woudl detect this reliably because `verbosity > 1` might be a check unrelated to the log level. 



---

_Label `rule` added by @MichaReiser on 2024-11-16 21:58_

---

_Label `needs-decision` added by @MichaReiser on 2024-11-16 21:58_

---
