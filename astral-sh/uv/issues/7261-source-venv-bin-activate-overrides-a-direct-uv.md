```yaml
number: 7261
title: "`source venv/bin/activate` overrides a direct `uv` call"
type: issue
state: closed
author: jsonar-cpapke
labels:
  - question
assignees: []
created_at: 2024-09-10T17:22:33Z
updated_at: 2024-09-10T21:31:41Z
url: https://github.com/astral-sh/uv/issues/7261
synced_at: 2026-01-12T15:59:12Z
```

# `source venv/bin/activate` overrides a direct `uv` call

---

_@jsonar-cpapke_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I've run into an issue related to python venv environments. It seems that if I source one virtual environment, but run `uv` from a second virtual environment, the packages are actually installed in the first virtual environment instead of the one that contains `uv`. I've added a reproduction below.

```
python -m venv venv
python -m venv uv-venv

source venv/bin/activate
./uv-venv/bin/pip install uv

ls venv/lib/python3.12/site-packages
> pip                pip-24.2.dist-info

ls uv-venv/lib/python3.12/site-packages
> pip                pip-24.2.dist-info uv                 uv-0.4.8.dist-info

./uv-venv/bin/uv pip install requests

ls venv/lib/python3.12/site-packages
> certifi                            charset_normalizer-3.3.2.dist-info pip                                requests-2.32.3.dist-info
> certifi-2024.8.30.dist-info        idna                               pip-24.2.dist-info                 urllib3
> charset_normalizer                 idna-3.8.dist-info                 requests                           urllib3-2.2.2.dist-info

ls uv-venv/lib/python3.12/site-packages
> pip                pip-24.2.dist-info uv                 uv-0.4.8.dist-info
```


for comparison, here's the same flow but with pip
```
python -m venv venv
python -m venv pip-venv

source venv/bin/activate
./pip-venv/bin/pip install -U pip

ls venv/lib/python3.12/site-packages
> pip                pip-24.2.dist-info

ls pip-venv/lib/python3.12/site-packages
> pip                pip-24.2.dist-info

./pip-venv/bin/pip install requests

ls venv/lib/python3.12/site-packages
> pip                pip-24.2.dist-info

ls pip-venv/lib/python3.12/site-packages
> certifi                            charset_normalizer-3.3.2.dist-info pip                                requests-2.32.3.dist-info
> certifi-2024.8.30.dist-info        idna                               pip-24.2.dist-info                 urllib3
> charset_normalizer                 idna-3.8.dist-info                 requests                           urllib3-2.2.2.dist-info
```


---

_Comment by @charliermarsh on 2024-09-10 17:39_

I can see how it's confusing here, but ultimately this is by design. uv doesn't care what environment it's installed into -- it's just a static binary with no dependence on Python. Installing it into a virtual environment in the first place is mostly just a distribution mechanism to make it easier for folks to install.

---

_Label `question` added by @charliermarsh on 2024-09-10 17:39_

---

_Comment by @jsonar-cpapke on 2024-09-10 21:31_

Ah, I see, that makes sense. Thanks for the response!

---

_Closed by @jsonar-cpapke on 2024-09-10 21:31_

---
