---
number: 1931
title: uv freezes vcs subdirectory requirements in non-roundtrippable format
type: issue
state: closed
author: mqudsi
labels:
  - bug
assignees: []
created_at: 2024-02-23T18:53:44Z
updated_at: 2024-02-24T17:50:21Z
url: https://github.com/astral-sh/uv/issues/1931
synced_at: 2026-01-10T01:23:09Z
---

# uv freezes vcs subdirectory requirements in non-roundtrippable format

---

_Issue opened by @mqudsi on 2024-02-23 18:53_

_Apologies in advance if I'm missing something as I'm primarily a low-level (rust + others) dev and speaking from a position of little production Python experience here._

Installing a package via the `vcs+https://` protocol and specifying both a revision and a subdirectory results in a subsequent `uv pip freeze` output that seems to include the entry in a human-readable rather than machine-parseable format. If piped to `requirements.txt` then fed back into either `uv` or `pip` (`[uv] pip install -r requirements.txt`), this always fails to install as it does not understand the syntax.

Reproduction:

```
uv pip install insightface@git+https://github.com/deepinsight/insightface.git#subdirectory=python-package@
01a34cd94f7b0f4a3f6c84ce4b988668ad7be329
```

Resulting in 

```
> uv pip freeze | tee requirements.txt | grep insightface
insightface==0.7.3 (from git+https://github.com/deepinsight/insightface.git@01a34cd94f7b0f4a3f6c84ce4b988668ad7be329#subdirectory=python-package)
```

Attempting to install the generated `requirements.txt` with `pip` results in the following error:

```
ERROR: Invalid requirement: 'insightface==0.7.3 (from git+https://github.com/deepinsight/insightface.git@01a34cd94f7b0f4a3f6c84ce4b988668ad7be329#subdirectory=python-package)' (from line 15 of requirements.txt)
Hint: It looks like a path. File 'insightface==0.7.3 (from git+https://github.com/deepinsight/insightface.git@01a34cd94f7b0f4a3f6c84ce4b988668ad7be329#subdirectory=python-package)' does not exist.
```

while attempting to install with `uv` also fails:

```
error: Couldn't parse requirement in `requirements.txt` at position 248
  Caused by: Trailing `(from git+https://github.com/deepinsight/insightface.git@01a34cd94f7b0f4a3f6c84ce4b988668ad7be329#subdirectory=python-package)` is not allowed
insightface==0.7.3 (from git+https://github.com/deepinsight/insightface.git@01a34cd94f7b0f4a3f6c84ce4b988668ad7be329#subdirectory=python-package)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
````

whereas it seems that a `requirements.txt` entry in the following format is supported by both `pip` and `uv pip`:

```
insightface@git+https://github.com/deepinsight/insightface.git@01a34cd94f7b0f4a3f6c84ce4b988668ad7be329#subdirectory=python-package
```

so presumably this is what `uv pip freeze` should emit?


-----

Running `uv` v0.1.10 and `pip` v23.3.1

---

_Comment by @charliermarsh on 2024-02-23 18:56_

👍 This is a bug. There's another issue for it somewhere...

---

_Label `bug` added by @charliermarsh on 2024-02-23 18:56_

---

_Comment by @charliermarsh on 2024-02-23 18:58_

Can't find it, so we'll just track this here.

---

_Comment by @mqudsi on 2024-02-23 18:59_

Thanks for confirming I wasn't holding it wrong!
(fwiw I wasn't able to find anything with a quick search before creating this issue either.)

---

_Comment by @charliermarsh on 2024-02-23 19:01_

I swear it's somewhere :joy: But it might be buried.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-23 19:35_

---

_Referenced in [astral-sh/uv#1936](../../astral-sh/uv/pulls/1936.md) on 2024-02-23 19:43_

---

_Closed by @charliermarsh on 2024-02-23 20:01_

---

_Comment by @mqudsi on 2024-02-24 16:45_

I was going to submit a PR for this myself, but tbh would not have figured out the test harness changes you implemented in #1936. Thanks for the fix!

---

_Comment by @charliermarsh on 2024-02-24 17:50_

No prob, thanks for reporting @mqudsi!

---

_Referenced in [astral-sh/uv#2677](../../astral-sh/uv/issues/2677.md) on 2024-03-26 21:23_

---
