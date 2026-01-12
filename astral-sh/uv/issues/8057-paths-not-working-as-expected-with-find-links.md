```yaml
number: 8057
title: "Paths not working as expected with `--find-links`"
type: issue
state: closed
author: xenago
labels:
  - bug
assignees: []
created_at: 2024-10-09T20:12:20Z
updated_at: 2024-10-09T23:05:52Z
url: https://github.com/astral-sh/uv/issues/8057
synced_at: 2026-01-12T15:59:19Z
```

# Paths not working as expected with `--find-links`

---

_@xenago_

Hello,

The behaviour of `--find-links` seems to have changed, possibly with #7912 (shipped in version `0.4.19`). In version `0.4.18`, the following line works to install a `.whl` stored in a local directory e.g. `Sentiment Analysis/en_core_web_sm/en_core_web_sm-3.7.1-py3-none-any.whl`:

`uv pip install --find-links="Sentiment Analysis/en_core_web_sm" en_core_web_sm`

![image](https://github.com/user-attachments/assets/e7fce811-0aea-49f7-8aad-3c197b13b4ae)

However, in versions >`0.4.18`, the following error is printed:

`error: invalid value 'Sentiment' for '--find-links <FIND_LINKS>': relative URL without a base`

![image](https://github.com/user-attachments/assets/0b36b2eb-493c-4500-b107-468b3327bf6e)

It looks like it is treating the file path as a URL and also splitting it on the space character?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-09 21:01_

---

_Label `bug` added by @charliermarsh on 2024-10-09 22:39_

---

_Comment by @charliermarsh on 2024-10-09 22:39_

Thanks, that's a bug on my part.

---

_Closed by @charliermarsh on 2024-10-09 23:05_

---

_Closed by @charliermarsh on 2024-10-09 23:05_

---
