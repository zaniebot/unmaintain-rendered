```yaml
number: 2304
title: "uv pip install sometimes results in `error: Cannot uninstall package; RECORD file not found at ...`"
type: issue
state: closed
author: mkleinbort-ic
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2024-03-08T19:01:27Z
updated_at: 2024-03-14T01:08:13Z
url: https://github.com/astral-sh/uv/issues/2304
synced_at: 2026-01-12T15:58:37Z
```

# uv pip install sometimes results in `error: Cannot uninstall package; RECORD file not found at ...`

---

_@mkleinbort-ic_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

When running 
```python -m uv pip install --system --prerelease=allow -r pyproject.toml```

I am hitting the error:

```
error: Cannot uninstall package; 
RECORD file not found at: C:\Users\{USER}\AppData\Local\Programs\Python\Python310\Lib\site-packages\catboost-1.2.2.dist-info\RECORD
```

Which seems to be a common error in other installation systems (ie. not `uv` specific):

https://stackoverflow.com/questions/68886239/cannot-uninstall-numpy-1-21-2-record-file-not-found

That said, if `uv` has any mitigation for this - that'd be great. 

Using uv 0.1.16 on Windows 11



---

_Renamed from "uv pip install sometimes results in `error: Cannot uninstall package; RECORD file not found at`" to "uv pip install sometimes results in `error: Cannot uninstall package; RECORD file not found at ...`" by @mkleinbort-ic on 2024-03-08 19:01_

---

_Comment by @charliermarsh on 2024-03-08 19:10_

That's interesting, can you share more on the dependencies included to trigger this? If we can repro reliably, we can probably fix it.

---

_Label `needs-mre` added by @charliermarsh on 2024-03-08 19:10_

---

_Comment by @mkleinbort-ic on 2024-03-08 19:14_

Not sure what caused it, It may be that I killed a process mid-way through a pip install (I'm cautiously moving from pip to uv these days).

I tried pip uninstalling catboost (which succeeded) but the issue remained when re running `uv pip install`

The solution was to navigate to

C://Users/{USER}/AppData/Local/Programs/Python/Python310/Lib/site-packages/

and `rm -r catboost-1.2.2.dist-info/` which was empty 



---

_Comment by @mkleinbort-ic on 2024-03-08 19:15_

Ah, now the issue with catboost is solved, but I hit the same issue with matplotlib

error: Cannot uninstall package; RECORD file not found at: C:\Users\mkleinbort\AppData\Local\Programs\Python\Python310\Lib\site-packages\matplotlib-3.8.2.dist-info\RECORD

(The matplotlib-3.8.2.dist-info/ folder is empty - even without running pip uninstall)


---

_Comment by @mkleinbort-ic on 2024-03-08 19:27_

Ok, mostly managed to reproduce the issue

1) When running uv pip install to install a newer version of a package
2) and I hit the error described here: https://github.com/astral-sh/uv/issues/1491#issuecomment-1983615072
3) When I re-run the uv pip install I hit the error described here on the package on which the error (2) was hit
4) So I go and manually delete the {package}-dist-info/ 
5) now that package installation succeeds, but I hit the https://github.com/astral-sh/uv/issues/1491#issuecomment-1983615072 somewhere else and it continues so 

I hope this will succeed once for all the packages and then become a quicker way to install new versions than pip

---

_Comment by @mkleinbort-ic on 2024-03-08 19:36_

Ok, just FYI - it did work after 42 runs and re-runs of uv pip install 

But it was worth it - now my "update dependencies" workflow has gone down from 4 minutes (via pip) to basically nothing

---

_Comment by @charliermarsh on 2024-03-08 19:55_

Ah interesting, so you're on Windows and you're hitting that even with the latest version. Hmm...

---

_Comment by @charliermarsh on 2024-03-09 14:49_

Ah okay, sorry, I misunderstood -- so you're hitting https://github.com/astral-sh/uv/issues/1491#issuecomment-1983615072 which is causing there to be "incomplete" packages in the site-packages, got it.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-13 16:04_

---

_Label `bug` added by @charliermarsh on 2024-03-13 16:04_

---

_Closed by @charliermarsh on 2024-03-14 01:08_

---
