```yaml
number: 1744
title: "Fail to read metadata from `.dist-info`: data did not match any variant of untagged enum DirectUrl"
type: issue
state: closed
author: messense
labels:
  - bug
assignees: []
created_at: 2024-02-20T09:59:47Z
updated_at: 2024-02-21T14:06:32Z
url: https://github.com/astral-sh/uv/issues/1744
synced_at: 2026-01-12T15:58:31Z
```

# Fail to read metadata from `.dist-info`: data did not match any variant of untagged enum DirectUrl

---

_@messense_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

```bash
$ uv pip freeze
error: Failed to read metadata: from /home/ubuntu/workspace/example/.venv/lib/python3.10/site-packages/qPython-2.0.0.dist-info
  Caused by: data did not match any variant of untagged enum DirectUrl
```

```bash
$ cat /home/ubuntu/workspace/example/.venv/lib/python3.10/site-packages/qPython-2.0.0.dist-info/direct_url.json
{"url": "git@github.example.com:example/qPython.git", "vcs_info": {"vcs": "git", "requested_revision": "7190a34911a789bd8e44f6b1986c8e84815b62e2", "commit_id": "7190a34911a789bd8e44f6b1986c8e84815b62e2"}}
```

https://github.com/astral-sh/uv/blob/d05cb8464a933a4b8e1ebe39988e0209da73ed19/crates/pypi-types/src/direct_url.rs#L32-L37

Probably because the `url` crate can't parse `git@github.example.com:example/qPython.git`: https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=3a089088700e500364057509872b61f9

> thread 'main' panicked at src/main.rs:4:72:
> called `Result::unwrap()` on an `Err` value: RelativeUrlWithoutBase

uv version: 0.1.5


---

_Comment by @konstin on 2024-02-20 11:29_

I've checked https://packaging.python.org/en/latest/specifications/direct-url-data-structure/ and PEP 610 neither defines what grammar the url field allows :/

What command did you use to install qPython?

---

_Comment by @messense on 2024-02-20 14:55_

It was installed by Poetry, I'm playing around with `uv` in a existing venv.

---

_Label `bug` added by @charliermarsh on 2024-02-20 14:57_

---

_Comment by @charliermarsh on 2024-02-20 15:08_

I can look at this (though mildly dreading it).

---

_Comment by @konstin on 2024-02-20 18:48_

I'd expect the poetry maintainers would be willing to fix this, i wonder if any other tools are doing something similar. It would be helpful if we could agree in https://packaging.python.org/en/latest/specifications/direct-url-data-structure/ that `url` contains a url as defined by https://url.spec.whatwg.org/.

---

_Comment by @sbidoul on 2024-02-20 19:11_

> It would be helpful if we could agree in https://packaging.python.org/en/latest/specifications/direct-url-data-structure/ that url contains a url as defined by https://url.spec.whatwg.org/.

Looks like an oversight in the spec, but that was the intent, yes.

---

_Comment by @messense on 2024-02-21 02:11_

Happy to report that `pip install` correctly uses `ssh://git@...` in `direct_url.json`.

---

_Comment by @messense on 2024-02-21 02:15_

> I'd expect the poetry maintainers would be willing to fix this

Seems like poetry is just writing out the source url specified in `pyproject.toml` to `direct_url.json`:
https://github.com/python-poetry/poetry/blob/cff4d7d51c9ac0f9a8b9ac91652b6dc20e63e203/src/poetry/installation/executor.py#L910-L922

I had something like 

```toml
[tool.poetry.dependencies]
qpython = { git = "git@github.example.com:example/qPython.git", rev = "7190a34911a789bd8e44f6b1986c8e84815b62e2" }
```

in `pyproject.toml` following https://python-poetry.org/docs/dependency-specification/#git-dependencies.

---

_Comment by @charliermarsh on 2024-02-21 03:14_

We should at least handle this gracefully though, I think...

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-21 03:21_

---

_Closed by @charliermarsh on 2024-02-21 14:06_

---
