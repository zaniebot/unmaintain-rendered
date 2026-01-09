---
number: 8426
title: Request for changing python versions more easily
type: issue
state: open
author: brenorb
labels: []
assignees: []
created_at: 2024-10-21T20:31:13Z
updated_at: 2025-01-26T22:03:55Z
url: https://github.com/astral-sh/uv/issues/8426
synced_at: 2026-01-07T13:12:17-06:00
---

# Request for changing python versions more easily

---

_Issue opened by @brenorb on 2024-10-21 20:31_

I just started trying uv, so I started

```
uv init
uv add pandas
...
uv add transformers
```

Then I got an error. The project initialized using `python 3.13` and `transformers` couldn't support that (` error: the configured Python interpreter version (3.13) is newer than PyO3's maximum supported version (3.12)`). 
So I tried to no avail:

```
uv python install 3.9
uv python uninstall 3.13
uv python pin 3.9
uv venv --python 3.9
```

I also manually changed `.python-version` from `3.13` to `3.9`, but it didn't work as well. I was always getting the same error: 
```
error: The requested Python version `3.9` is incompatible with the project `requires-python` value of `>=3.13`.
```

Now I know that I can use `uv init --python 3.9`, but then I would need to erase everything and start again. I know I could also use `uv run --python 3.9 example.py` but I don't write this all the time.

I eventually could solve the issue manually changing `pyproject.toml` from `requires-python = ">=3.13"` to `requires-python = ">=3.9"`, but it could be done in the command line. I'm suggesting to add a command to change the `requires-python` value without needing to manually rewrite the file, and maybe having some link between `.python-version` and the `pyproject.toml`.

If there is a better way, I'd love to know about it.

I'm using uv 0.4.25 (Homebrew 2024-10-21)


<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @zanieb on 2024-10-21 22:23_

Sorry you ran into problems here. I'm not sure what our best option is for changing `requires-python`. It doesn't quite seem worth a dedicated command.

---

_Comment by @inikolaev on 2024-10-24 18:19_

That would be really great to have support out-of-the-box. It's often a painful process with many package managers such as `pipenv` and `poetry` and worse it's not usually very well documented.

What I often want is to migrate my application from one Python version to another and minimise the amount of packages I have to upgrade to the minimum. But most package managers eagerly upgrade all dependencies if you approach this in a naive way. With `pipenv` what I happen to do is update Python version in `Pipfile` and then "upgrade" some package to trick `pipenv` to regenerate the lock file.

---

_Comment by @ChrisAichinger on 2025-01-26 06:49_

I ran into the same issue: new to `uv`, needed to change Python version due to some incompatible libraries.

My main issue was the the solution was not discoverable: I didn't find any documentation about how to upgrade the Python version in an existing project. I was happy to finally stumble across this Github issue.

My solution:
```bash
rm -rf .python-version .venv
mv pyproject.toml pyproject.old
uv init --python 3.11
uv add ...  // add all libraries from pyproject.old
```

So maybe `uv` doesn't need a separate command for it, but better documentation would be fantastic.

---

_Comment by @brenorb on 2025-01-26 22:03_

Or maybe just link `.python-version` file somehow with `requires-python` value inside  `pyproject.toml`.

---
