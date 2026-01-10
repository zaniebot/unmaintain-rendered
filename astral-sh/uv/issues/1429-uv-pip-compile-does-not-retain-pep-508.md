```yaml
number: 1429
title: "`uv pip compile` Does not retain PEP-508 environment markers in output"
type: issue
state: closed
author: CoolCat467
labels:
  - bug
  - compatibility
assignees: []
created_at: 2024-02-16T04:52:57Z
updated_at: 2024-06-25T20:56:00Z
url: https://github.com/astral-sh/uv/issues/1429
synced_at: 2026-01-10T05:31:36Z
```

# `uv pip compile` Does not retain PEP-508 environment markers in output

---

_Issue opened by @CoolCat467 on 2024-02-16 04:52_

`uv pip compile` does not respect [PEP-508 Environment Markers](https://peps.python.org/pep-0508/#environment-markers) (such as `; implementation_name ==`).

```console
> uv --version
uv 0.1.11
```

For example, we have `test-requirements.in`:
```
black; implementation_name == "cpython"
```
and running
```console
> uv pip compile test-requirements.in -o test-requirements.txt
```

yields `black==24.2.0` instead of `black==24.2.0 ; implementation_name == "cpython"`

---

_Label `bug` added by @zanieb on 2024-02-16 04:55_

---

_Label `compatibility` added by @zanieb on 2024-02-16 04:55_

---

_Comment by @charliermarsh on 2024-02-16 04:56_

I'm surprised that `pip-compile` retains those markers, but seems like we should too.

---

_Comment by @jakkdl on 2024-02-16 11:04_

If removing `; python_version` markers depending on the `--python-version` flag is intended, then maybe the same thing should happen here and you have a flag for specifying platform that makes use of those markers and can be used to generate multiple requirements.txt files depending on version+implementation

---

_Renamed from "`uv pip compile` does not respect `; implementation_name ==` specifiers" to "`uv pip compile` Does Not Respect PEP-508 Environment Markers" by @CoolCat467 on 2024-02-27 00:21_

---

_Comment by @CoolCat467 on 2024-02-27 00:25_

This is currently a blocker for https://github.com/python-trio/trio/pull/2958

---

_Comment by @charliermarsh on 2024-02-27 00:57_

I'm open to including them but I don't know that it's a "correct" requirement. E.g., in the Trio example, it's not really "safe" to install from that file with a non-CPython implementation, because it could be _missing_ dependencies that are required for PyPy or any other implementation. The locked requirements are only guaranteed to be correct on the same Python platform as that which generated the lock file.

---

_Comment by @A5rocks on 2024-02-27 01:33_

I don't think the markers are propagated either, though I haven't checked against pip-compile. We use `pip install -r <locked file>` so missing dependencies are fine (we don't have hashes because requirement files vary per platform, and since lockfiles are more anti-bitrot than for security).

I think that copying over the marker to the output, even if not necessarily "correct," is a step forward towards platform independent lockfiles. (is there an issue for that? I know there's a section in the readme)

---

_Comment by @charliermarsh on 2024-02-27 01:45_

@A5rocks - There's a standardized proposal for multi-platform (but not platform-independent) installer files (to replace `requirements.txt` for this purpose), we've been participating in the discussion.

---

_Renamed from "`uv pip compile` Does Not Respect PEP-508 Environment Markers" to "`uv pip compile` Does not retain PEP-508 environment markers in output" by @charliermarsh on 2024-03-07 15:57_

---

_Comment by @sondr3 on 2024-03-20 09:33_

Could markers like `platform_system` be included at the very least in the beginning? We keep running into issues where developers are updating dependencies on their Windows machines that crash in CI when using `uv` that works with `pip-compile`. We've need to add `pywin32==306 ; platform_system=='Windows'` to our `pyproject.toml`	(using `rye`) which works with `pip-compile` but not `uv`.

---

_Comment by @Franky1 on 2024-03-23 20:35_

I have this in my `requirements.txt` file 

```txt
my-lib@https://github.com/path-to-my-lib-wheel.whl; platform_system=="Windows" and python_version=="3.10"
```

and `uv pip install --upgrade --requirement requirements.txt` fails with 

```log
error: Couldn't parse requirement in `requirements.txt` at position 180
  Caused by: Missing space before ';', the end of the URL is ambiguous
```


---

_Comment by @charliermarsh on 2024-03-23 21:26_

The grammar is technically ambiguous for some URLs so we require a space before the `;` when it follows a URL. pip also accepts this (though does not always require it). So in this case:

```
my-lib@https://github.com/path-to-my-lib-wheel.whl ; platform_system=="Windows" and python_version=="3.10"
```

---

_Comment by @Franky1 on 2024-03-23 21:38_

> so we require a space before the `;` when it follows a URL

Thanks for the quick reply, that was indeed the solution, tried it and it works now :heart:

---

_Comment by @adminy on 2024-06-05 12:13_

Also pip-compile does not support the `--trusted-host` argument or the `--config=pyproject.toml` argument.

When you run pip compile it does not put `--index-url` or `--trusted-host` in its output, generating the requirements.txt without indication of the source. 

---

_Closed by @charliermarsh on 2024-06-25 20:56_

---

_Closed by @charliermarsh on 2024-06-25 20:56_

---
