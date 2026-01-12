```yaml
number: 8244
title: uv as a pipx replacement
type: issue
state: closed
author: powercoconola
labels:
  - question
assignees: []
created_at: 2024-10-16T07:08:36Z
updated_at: 2024-10-16T19:57:01Z
url: https://github.com/astral-sh/uv/issues/8244
synced_at: 2026-01-12T15:59:22Z
```

# uv as a pipx replacement

---

_@powercoconola_

Hello, I have a specific usecase for `uv`. Right now I deploy many custom packages to many PCs using `pipx`. 

I use a simple script to install the packages in bulk using `python -m pipx install <package-name> --index-url <url>`. With `pipx`, this automatically installs them into separate venvs in pipx/venvs/ and name them after the package-name, which is perfect behavior. I can then call their .exes and they are in isolated environments.
In `uv`, when using `uv pip install <package-name> --index-url <url>`, this puts them all together, unisolated in a .venv folder. Is there a setting to automatically place them in separate venvs in a central location like `pipx` does with pipx/venvs/? 

I also use a simple script to then update my packages periodically with `python -m pipx upgrade <package-name> --index-url <url>`. Is there a way to then have `uv` do the same?

I did read about environment variables from the documentation that looked promising, but I'm unsure how to implement that.

I am hoping to replace `pipx` because `uv` is so much faster when installing/upgrading many packages. `uv` is very promising. Any help would be appreciated. Thank you to the developers.

---

_Comment by @Ravencentric on 2024-10-16 09:30_

I think you're looking for [`uv tool`](https://docs.astral.sh/uv/concepts/tools/)

---

_Comment by @charliermarsh on 2024-10-16 12:11_

That's right -- `uv tool` or `uvx` is what you're looking for!

---

_Closed by @charliermarsh on 2024-10-16 12:11_

---

_Label `question` added by @charliermarsh on 2024-10-16 12:12_

---
