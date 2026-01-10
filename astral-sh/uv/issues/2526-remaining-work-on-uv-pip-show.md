---
number: 2526
title: "Remaining work on `uv pip show`"
type: issue
state: open
author: danielhollas
labels:
  - compatibility
assignees: []
created_at: 2024-03-18T21:50:39Z
updated_at: 2025-08-16T22:08:45Z
url: https://github.com/astral-sh/uv/issues/2526
synced_at: 2026-01-10T01:23:18Z
---

# Remaining work on `uv pip show`

---

_Issue opened by @danielhollas on 2024-03-18 21:50_

This is a follow-up to #1594 to track the remaining work to reach pip compatibility (ordered by importance)

- [x] `Required-by` field (extremely useful)
- [x] `Editable project location`
- [ ] Other fields that are less important imo: Summary, Home-page, Author, Author-email, License

Current example uv output (for an editable package)

```
Name: aiidalab-qe
Version: 24.4.0a2
Location: /home/hollas/atmospec/aiidalab-qe/.venv/lib/python3.12/site-packages
Requires: aiida-core, aiida-pseudo, aiida-quantumespresso, aiidalab-widgets-base, filelock, importlib-resources, jinja2
```

Compare to pip output

```
Name: aiidalab_qe
Version: 24.4.0a2
Summary: Package for the AiiDAlab QE app
Home-page: https://github.com/aiidalab/aiidalab-qe
Author: Carl Simon Adorf, Aliaksandr Yakutovich, Marnik Bercx, Jusong Yu
Author-email: aiidalab@materialscloud.org
License: MIT
Location: /home/hollas/atmospec/aiidalab-qe/.venv/lib/python3.12/site-packages
Editable project location: /home/hollas/atmospec/aiidalab-qe
Requires: aiida-core, aiida-pseudo, aiida-quantumespresso, aiidalab-widgets-base, filelock, importlib-resources, Jinja2
Required-by: 
```

Thank you so much @ChannyClaus for the current implementation! :clap: 

---

_Label `compatibility` added by @zanieb on 2024-03-18 22:16_

---

_Comment by @eth3lbert on 2024-03-20 09:38_

I'd like to take this issue! Before I start working on it, I have a question about output compatibility with pip.

Currently, our output warns about each missing package on a separate line. Additionally, the input packages are being sorted and deduplicated. In contrast, pip warns about missing packages in a single, combined string and preserves the original order of input packages (without deduplication).

---

_Comment by @ChannyClaus on 2024-03-20 12:52_

> I'd like to take this issue! Before I start working on it, I have a question about output compatibility with pip.
> 
> Currently, our output warns about each missing package on a separate line. Additionally, the input packages are being sorted and deduplicated. In contrast, pip warns about missing packages in a single, combined string and preserves the original order of input packages (without deduplication).

which version of pip are you using? the one i'm using (23.2.1, also tried on 24.0) seems to be sorting the input packages:
```
$  pip show blah asdf blah
WARNING: Package(s) not found: asdf, blah, blah
```
keeping the de-duplication + single-line warning for multiple missing packages seem like better UX to me but i'm not a maintainer :)


---

_Comment by @eth3lbert on 2024-03-21 00:13_

Ah, you're right. The missing packages section is indeed sorted, only the response details are not.

pip 24.0:
```console
$ pip show six blah asdf blah python-dateutil six
WARNING: Package(s) not found: asdf, blah, blah
Name: six
Version: 1.16.0
Summary: Python 2 and 3 compatibility utilities
Home-page: https://github.com/benjaminp/six
Author: Benjamin Peterson
Author-email: benjamin@python.org
License: MIT
Location: /Users/user/uv/.venv/lib/python3.12/site-packages
Requires: 
Required-by: python-dateutil
---
Name: python-dateutil
Version: 2.9.0.post0
Summary: Extensions to the standard Python datetime module
Home-page: https://github.com/dateutil/dateutil
Author: Gustavo Niemeyer
Author-email: gustavo@niemeyer.net
License: Dual License
Location: /Users/user/uv/.venv/lib/python3.12/site-packages
Requires: six
Required-by: 
---
Name: six
Version: 1.16.0
Summary: Python 2 and 3 compatibility utilities
Home-page: https://github.com/benjaminp/six
Author: Benjamin Peterson
Author-email: benjamin@python.org
License: MIT
Location:  /Users/user/uv/.venv/lib/python3.12/site-packages
Requires: 
Required-by: python-dateutil
```

---

_Comment by @zanieb on 2024-03-21 02:02_

I don't think matching `pip`'s output is important here, I'd lean towards retaining the current ux.

---

_Comment by @Andrei-Aksionov on 2024-03-21 12:54_

> I don't think matching pip's output is important here, I'd lean towards retaining the current ux.

Current implementation doesn't show `Required-by` info. And this is the only reason why I use `pip show`.

---

_Referenced in [astral-sh/uv#2589](../../astral-sh/uv/pulls/2589.md) on 2024-03-21 14:03_

---

_Comment by @mbway on 2024-04-05 19:21_

uv is also missing support for `show --files` which shows the path of each file belonging to the package under a `Files` heading

---

_Comment by @itsuresh248 on 2024-12-12 11:55_

`poetry show` displays verbose **dependencies** and **required by** information which lists version requirements.

```
poetry show pendulum

name        : pendulum
version     : 1.4.2
description : Python datetimes made easy

dependencies
 - python-dateutil >=2.6.1
 - tzlocal >=1.4
 - pytzdata >=2017.2.2

required by
 - calendar >=1.4.0
```

It will be good to display similar information.

---

_Referenced in [astral-sh/uv#10962](../../astral-sh/uv/issues/10962.md) on 2025-01-25 17:53_

---

_Comment by @Flimm on 2025-08-16 22:03_

If I run `pip show django` in my virtual environment, it outputs the fields `Name`, `Version`, `Summary`, `Home-page`, `Author`, `Author-email`, `License`, `Location`, `Requires` and `Required-by`. **If I add the verbose flag `-v` to the command, these additional fields are printed to stdout**: `Matadata-Version`, `Installer`, `Classifiers`, `Entry-points`, `Project-URLs`. I presume the exact list fields depends on the metadata that is included with the examined package.

By contrast, `uv pip show -v django` does not show these additional fields, instead, it shows additional debugging messages, but not additional fields. Even `uv pip show django` does not show all the fields that `pip show django` shows. This difference in behaviour between `pip show -v <package>` and `uv pip show -v <package>` is not documented in [the compatibility page](https://docs.astral.sh/uv/pip/compatibility/).

<details><summary>Output of `pip show` and `uv pip show` commands</summary>

```
$ uv venv --seed
$ uv init
$ uv add django
$ uv run -- pip show django
Name: Django
Version: 5.2.5
Summary: A high-level Python web framework that encourages rapid development and clean, pragmatic design.
Home-page: https://www.djangoproject.com/
Author: 
Author-email: Django Software Foundation <foundation@djangoproject.com>
License: BSD-3-Clause
Location: /home/flimm/bla/.venv/lib/python3.12/site-packages
Requires: asgiref, sqlparse
Required-by:

$ uv run -- pip show -v django
Name: Django
Version: 5.2.5
Summary: A high-level Python web framework that encourages rapid development and clean, pragmatic design.
Home-page: https://www.djangoproject.com/
Author: 
Author-email: Django Software Foundation <foundation@djangoproject.com>
License: BSD-3-Clause
Location: /home/flimm/bla/.venv/lib/python3.12/site-packages
Requires: asgiref, sqlparse
Required-by: 
Metadata-Version: 2.4
Installer: uv
Classifiers:
  Development Status :: 5 - Production/Stable
  Environment :: Web Environment
  Framework :: Django
  Intended Audience :: Developers
  License :: OSI Approved :: BSD License
  Operating System :: OS Independent
  Programming Language :: Python
  Programming Language :: Python :: 3
  Programming Language :: Python :: 3 :: Only
  Programming Language :: Python :: 3.10
  Programming Language :: Python :: 3.11
  Programming Language :: Python :: 3.12
  Programming Language :: Python :: 3.13
  Topic :: Internet :: WWW/HTTP
  Topic :: Internet :: WWW/HTTP :: Dynamic Content
  Topic :: Internet :: WWW/HTTP :: WSGI
  Topic :: Software Development :: Libraries :: Application Frameworks
  Topic :: Software Development :: Libraries :: Python Modules
Entry-points:
  [console_scripts]
  django-admin = django.core.management:execute_from_command_line
Project-URLs:
  Homepage, https://www.djangoproject.com/
  Documentation, https://docs.djangoproject.com/
  Release notes, https://docs.djangoproject.com/en/stable/releases/
  Funding, https://www.djangoproject.com/fundraising/
  Source, https://github.com/django/django
  Tracker, https://code.djangoproject.com/

$ uv pip show django
Name: django
Version: 5.2.5
Location: /home/song/bla/.venv/lib/python3.12/site-packages
Requires: asgiref, sqlparse
Required-by:

$ uv pip show -v django
DEBUG uv 0.8.11
DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.12.11-linux-x86_64-gnu` at `/home/flimm/bla/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.11 environment at: .venv
Name: django
Version: 5.2.5
Location: /home/flimm/bla/.venv/lib/python3.12/site-packages
Requires: asgiref, sqlparse
Required-by:
```

</details>

---

_Comment by @Flimm on 2025-08-16 22:06_

Here's another difference between `pip show <package` and `uv pip show <package>`. The way that case is handled for the package name is different. The former prints `Name: Django` and the latter `Name: django`, for example.

```
$ uv pip show django | grep Name
Name: django
$ uv run -- pip show django | grep Name
Name: Django
```

---

_Comment by @charliermarsh on 2025-08-16 22:08_

Those names are [equivalent](https://packaging.python.org/en/latest/specifications/name-normalization/) from a packaging perspective; we always show the normalized name.

Contributions always welcome to expand the output here, it just hasn't been prioritized since it doesn't come up much.


---

_Referenced in [astral-sh/uv#15516](../../astral-sh/uv/issues/15516.md) on 2025-08-25 16:43_

---
