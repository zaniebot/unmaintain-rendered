---
number: 1872
title: uv does not handle --find-link for pytorch, 403 Forbidden
type: issue
state: closed
author: KacpiW
labels:
  - question
assignees: []
created_at: 2024-02-22T15:07:27Z
updated_at: 2024-02-23T09:21:10Z
url: https://github.com/astral-sh/uv/issues/1872
synced_at: 2026-01-07T13:12:16-06:00
---

# uv does not handle --find-link for pytorch, 403 Forbidden

---

_Issue opened by @KacpiW on 2024-02-22 15:07_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I've encountered an issue with pytorch installation from requirements.txt file. My dependency file looks like that:

```
boto3==1.19.12
cryptography==38.0.4
pandas==1.5.2
scikit-learn==1.2.0
snowflake-connector-python==2.9.0
spacy[cuda110]==3.6.1
scispacy==0.5.3
bs4==0.0.1
tqdm==4.66.1
transformers==4.36.2
torch==2.0.1
torchvision==0.15.2
typing_extensions==4.5.0
-f https://download.pytorch.org/whl/cu110.html
en_ner_bc5cdr_md @ https://s3-us-west-2.amazonaws.com/ai2-s2-scispacy/releases/v0.5.3/en_ner_bc5cdr_md-0.5.3.tar.gz
fsspec[http]==2023.10.0
datasets==2.16.1
ipykernel==6.28.0
pendulum==3.0.0
```

I'm running installation with `uv pip install --no-cache -r requirements.txt` and my response message looks as follows:

```
error: Failed to read `--find-links` URL: https://download.pytorch.org/whl/cu110.html Caused by: HTTP status client error (403 Forbidden) for url (https://download.pytorch.org/whl/cu110.html)
```

uv 0.1.6

---

_Comment by @charliermarsh on 2024-02-22 16:39_

I think `pip` is silently failing there... You can try changing `-f https://download.pytorch.org/whl/cu110.html` to anything else, like `-f https://download.pytorch.org/whl/foo.html`, and you get the same behavior from `pip`, I think?

I think you want `-f https://download.pytorch.org/whl`.

---

_Label `question` added by @charliermarsh on 2024-02-22 16:39_

---

_Closed by @charliermarsh on 2024-02-23 01:30_

---

_Comment by @KacpiW on 2024-02-23 09:21_

Adding `-f https://download.pytorch.org/whl` and modifying `spacy[cuda110]==3.6.1` to `spacy[cupy-cuda110]==3.6.1` like it is in `https://download.pytorch.org/whl` helped 😄

---
