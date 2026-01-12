```yaml
number: 2737
title: uv venv references already existing environment instead of creating new
type: issue
state: closed
author: ssslakter
labels:
  - needs-mre
assignees: []
created_at: 2024-03-30T17:03:16Z
updated_at: 2024-04-03T11:56:20Z
url: https://github.com/astral-sh/uv/issues/2737
synced_at: 2026-01-12T15:58:40Z
```

# uv venv references already existing environment instead of creating new

---

_@ssslakter_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
I've just started using uv, but I couldn't find any information about this behavior, so don't know if it's expected, but it wasn't to me originally.
First, I've installed uv with my conda installation on WSL. Then I ran
```sh
cd my-repo
uv venv
source .venv/bin/activate
uv pip install -r requirements.txt
``` 
But then I've realized that I have some packages installed in my environment that are not in requirements and not supposed to be installed, then I've deactivated venv to get back to base conda and saw with `conda list` that **everything** I've installed into venv was installed into my base environment!😭
I inspected the .venv folder and found that python binary and activation scripts were linking to the base python from conda.

Well, I guess it makes sense that uv didn't create a separate environment, if it doesn't depend on python, but if you have to create actual python venv by yourself, what's the purpose of `uv venv` command?

---

_Comment by @charliermarsh on 2024-03-30 17:17_

What version on uv are you on?

I'm having a little trouble following the issue. Are you saying that `uv pip install -r requirements.txt` didn't install into `.venv`, it installed into your Conda environment?

---

_Comment by @ssslakter on 2024-03-30 17:29_

I have uv 0.1.26
Yes, I guess so, if I get it correctly `uv venv` somehow referenced my global environment and when I installed requirements inside `.venv` they ended up there as well, but I think I might have also messed up something. I'll try to reproduce it with other env and write here later

---

_Comment by @charliermarsh on 2024-03-30 17:43_

Ok, we recently changed the logic to ensure that if you have a virtualenv _and_ a Conda environment enabled, we favor the virtual environment, so this _should_ be working as you originally expected.

---

_Label `needs-mre` added by @charliermarsh on 2024-03-31 17:52_

---

_Comment by @ssslakter on 2024-04-01 12:18_

Oh, I found what was the problem, I accidentally used `pip install` instead of `uv pip install` so it was my mistake apparently.
I expected it to work like python venv which also installs pip, so you can interchangeably use pip and other package managers. Now I see how it is different.
I've also found an interesting thing when I tried to install pip with uv, basically you need to reactivate your environment to use it correctly.
```sh
uv venv
source .venv/bin/activate
uv pip install pip
pip list # this is not pip installed by uv
deactivate
source .venv/bin/activate
pip list # now this pip shows the same packages as uv pip list
```

---

_Comment by @charliermarsh on 2024-04-01 14:39_

Ah yes, you can run `uv venv --seed` if you want to include the "seed packages" (i.e., `pip`). Otherwise, `uv` excludes `pip` by default, since you already have `uv` installed and `uv` can handle installation for you.

---

_Closed by @charliermarsh on 2024-04-01 14:39_

---

_Comment by @ssslakter on 2024-04-01 14:47_

Thank you! It is much more clear now

---

_Comment by @charliermarsh on 2024-04-01 14:50_

Thanks for following up :)

---

_Comment by @ssslakter on 2024-04-03 11:56_

Hi @charliermarsh! 

I was able to reproduce multiple times a similar problem where uv can't find itself in a new environment. Is this expected?
```sh
python -m venv venv
source venv/bin/activate
pip install uv
uv venv
source activate .venv/bin/activate
uv --help
# outputs uv: command not found
```

---
