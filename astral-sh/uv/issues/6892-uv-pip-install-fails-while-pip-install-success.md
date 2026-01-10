```yaml
number: 6892
title: uv pip install fails while pip install success
type: issue
state: closed
author: rakeshksr
labels:
  - question
assignees: []
created_at: 2024-08-31T06:01:00Z
updated_at: 2024-09-15T18:30:46Z
url: https://github.com/astral-sh/uv/issues/6892
synced_at: 2026-01-10T04:45:10Z
```

# uv pip install fails while pip install success

---

_Issue opened by @rakeshksr on 2024-08-31 06:01_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
`uv pip install unstructured[pdf]` fails with following error
```
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: pycrypto==2.6.1
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit code: 1
```
`pip install unstructured[pdf]` success without error.

After some debugging found that issue caused by two packages `unstructured-inference` and `pdfminer.six`

### Steps to Reproduce:
1. `uv venv`
2. `.venv\Scripts\activate`
3. `uv pip install pdfminer.six unstructured-inference`

It's strange that installing packages individually( `uv pip install pdfminer.six; uv pip install unstructured-inference`) working fine.


**uv platform:** Windows 11
**uv version:** uv 0.4.1 (823f23e22 2024-08-30)


---

_Label `question` added by @charliermarsh on 2024-09-01 15:56_

---

_Comment by @charliermarsh on 2024-09-01 16:07_

Have you seen this behavior repeatedly?

---

_Comment by @rakeshksr on 2024-09-03 11:15_

Yes, I have tested in different windows machines.

---

_Comment by @dhofstetter on 2024-09-06 15:21_

can confirm that with uv version `0.4.6` something has changed, so that uv now wants to build pycrypto wheel (and considers it as being an explicit dependency!) but I have no deps requiring pycrypto.

Is there a way to figure out why uv wants to install a certain package?

---

_Comment by @charliermarsh on 2024-09-06 15:28_

Can you share the output with `--verbose`? That will tell us exactly why it's being included.

---

_Comment by @dhofstetter on 2024-09-06 15:34_

hmm good to know, I got the dependency that requires pycrypto now

And there it is indeed a direct dependency. Im just uncertain, why this dependency `jose` is within my project.

Thanks for your quick response :) 

---

_Comment by @charliermarsh on 2024-09-08 18:19_

No prob!

---

_Closed by @charliermarsh on 2024-09-08 18:19_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-08 18:19_

---

_Comment by @rakeshksr on 2024-09-13 12:33_

@charliermarsh issue still persists with `uv 0.4.9 (77d278f68 2024-09-10)`

After some investigation, I have determined that the root cause lies in the conflicting versions of `pdfplumber` installed. When using the command `uv pip install pdfminer.six unstructured-inference`, an older version of `pdfplumber (v0.5.3)` is installed, which has a dependency on `pycrypto`. This results in the pycrypto dependency being added to the project, dependency tree `unstructured-inference -> layoutparser -> pdfplumber-0.5.3 -> pycrypto`. 

However, when using the command `uv pip install pdfminer.six; uv pip install unstructured-inference`, the latest version of `pdfplumber (v0.11.4)` is installed, which does not have a dependency on `pycrypto`. Therefore, the pycrypto dependency is not added to the project in this case, dependency tree `unstructured-inference -> layoutparser -> pdfplumber-0.11.4`. 


---

_Comment by @charliermarsh on 2024-09-13 13:20_

I think this is all reasonable behavior... The difference is that if you run `uv pip install pdfminer.six`, then `uv pip install unstructured-inference`, we end up _downgrading_ `pdfminer.six`:

```
 - pdfminer-six==20240706
 + pdfminer-six==20231228
```

Whereas if you run `uv pip install pdfminer.six unstructured-inference`, we prefer using a more recent `pdfminer.six` and a less recent transitive dependency (`pdfplumber`). Both are completely valid resolutions.

If you specifically want to avoid using an old `pdfplumber`, you should add a constraint for `pdfplumber>0.6` or similar. You need to give more information to the resolver if you want and are expecting a specific outcome like that.


---

_Comment by @rakeshksr on 2024-09-15 18:30_

@charliermarsh  thanks for information.

---
