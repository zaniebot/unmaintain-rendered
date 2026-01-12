```yaml
number: 2851
title: uv pip install -r is not completely compatible with the original pip install -r - does not accept extra options
type: issue
state: closed
author: flyaroundme
labels:
  - compatibility
assignees: []
created_at: 2024-04-06T12:49:51Z
updated_at: 2024-04-06T14:27:50Z
url: https://github.com/astral-sh/uv/issues/2851
synced_at: 2026-01-12T15:58:41Z
```

# uv pip install -r is not completely compatible with the original pip install -r - does not accept extra options

---

_@flyaroundme_

I have a requirements.txt file which starts with extra options for `pip install`, like extra pypi index url and actually contains something like this:
```
--extra-index-url=https://<extra_pypi_index_url>
--trusted-host=github.com
-e .[development]
```

And the invocation of 
```
$ uv pip install -r requirements.txt
```

Leads to 

```
error: Unexpected '-', expected '-c', '-e', '-r' or the start of a requirement at requirements.txt:2:1
```

While the old `pip install -r` works perfectly fine


<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Label `compatibility` added by @AlexWaygood on 2024-04-06 12:51_

---

_Comment by @charliermarsh on 2024-04-06 13:58_

We don’t support trusted host. The rest of your requirements file will work fine.

---

_Comment by @charliermarsh on 2024-04-06 13:59_

You can track trusted host here: https://github.com/astral-sh/uv/issues/1339. But removing the second line of your file should lead to a successful run.

---

_Closed by @charliermarsh on 2024-04-06 13:59_

---

_Comment by @flyaroundme on 2024-04-06 14:27_

Thanks!

---
