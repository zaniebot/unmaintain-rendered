```yaml
number: 2716
title: "`0.1.25` Failed to deserialize cache entry on `pip install -U`"
type: issue
state: closed
author: CoderJoshDK
labels:
  - duplicate
assignees: []
created_at: 2024-03-28T18:22:56Z
updated_at: 2024-03-28T18:28:46Z
url: https://github.com/astral-sh/uv/issues/2716
synced_at: 2026-01-12T15:58:40Z
```

# `0.1.25` Failed to deserialize cache entry on `pip install -U`

---

_@CoderJoshDK_

When running `uv pip install -r requirements.txt -U`, I get the following error:
```
error: Failed to download and build: django-allauth==0.61.1
  Caused by: Failed to deserialize cache entry
  Caused by: expected version to start with a number, but no leading ASCII digits were found
```

<details>
<summary>requirements.txt</summary>

```txt
django
django-allauth
djangorestframework
drf-spectacular
django-filter
django-tree-queries
django-money
psycopg2-binary
markdown
```

</details>

This problem is only present in version `0.1.25`. I downgraded to `0.1.24` (`curl --proto '=https' --tlsv1.2 -LsSf https://github.com/astral-sh/uv/releases/download/0.1.24/uv-installer.sh | sh`) and tested the same thing, and no errors.

<details>
<summary>Verbose error output</summary>

```
INFO Found a virtualenv through VIRTUAL_ENV at: /Users/path.../.venv
DEBUG Cached interpreter info for Python 3.12.2, skipping probing: .venv/bin/python
DEBUG Using Python 3.12.2 environment at .venv/bin/python
DEBUG Using registry request timeout of 300s
DEBUG Solving with target Python version 3.12.2
DEBUG Adding direct dependency: django*
DEBUG Adding direct dependency: django-allauth*
DEBUG Adding direct dependency: djangorestframework*
DEBUG Adding direct dependency: drf-spectacular*
DEBUG Adding direct dependency: django-filter*
DEBUG Adding direct dependency: django-tree-queries*
DEBUG Adding direct dependency: django-money*
DEBUG Adding direct dependency: psycopg2-binary*
DEBUG Adding direct dependency: markdown*
DEBUG Found fresh response for: https://pypi.org/simple/django-allauth/
DEBUG Found fresh response for: https://pypi.org/simple/django/
DEBUG Found fresh response for: https://pypi.org/simple/django-filter/
DEBUG Found fresh response for: https://pypi.org/simple/django-tree-queries/
DEBUG Found fresh response for: https://pypi.org/simple/django-money/
DEBUG Found fresh response for: https://pypi.org/simple/djangorestframework/
DEBUG Found fresh response for: https://pypi.org/simple/drf-spectacular/
DEBUG Found fresh response for: https://pypi.org/simple/markdown/
DEBUG Found fresh response for: https://pypi.org/simple/psycopg2-binary/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/72/70/77c010fb24d7bebb6042354543f20f75cc7934944165d1a90091459e3d0b/django-allauth-0.61.1.tar.gz
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9c/5b/eed82065c5d938b17c4b7304ab5ebe762c7a5a7eaa8a10ab35541580d79a/Django-5.0.3-py3-none-any.whl.metadata
WARN Broken fresh cache entry (for payload) at /Users/.../Library/Caches/uv/wheels-v0/pypi/django/django-5.0.3-py3-none-any.msgpack, removing: Cache deserialization failed
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7b/7d/ee51b0fd69425035a2efc101b2252cd08eab525c12db92fbea298823cc9f/django_filter-24.2-py3-none-any.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/7b/7d/ee51b0fd69425035a2efc101b2252cd08eab525c12db92fbea298823cc9f/django_filter-24.2-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d9/8a/7d3ac58a2f4f4c4d71f9d600831da38fd9b38fdb7e46753e57163bdcbd0f/django_tree_queries-0.17.0-py3-none-any.whl.metadata
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/d9/8a/7d3ac58a2f4f4c4d71f9d600831da38fd9b38fdb7e46753e57163bdcbd0f/django_tree_queries-0.17.0-py3-none-any.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/d9/8a/7d3ac58a2f4f4c4d71f9d600831da38fd9b38fdb7e46753e57163bdcbd0f/django_tree_queries-0.17.0-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/04/68/632ae0dd4343bd569441233a187b5c4fecd2e263cae8faa0e07539d24db2/django_money-3.4.1-py3-none-any.whl.metadata
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/04/68/632ae0dd4343bd569441233a187b5c4fecd2e263cae8faa0e07539d24db2/django_money-3.4.1-py3-none-any.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/04/68/632ae0dd4343bd569441233a187b5c4fecd2e263cae8faa0e07539d24db2/django_money-3.4.1-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/c0/7e/8c45ea7f85dd5d52ceddbacc6f56ecaca21ecbfc0e8c34c95618a14d5082/djangorestframework-3.15.1-py3-none-any.whl.metadata
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/c0/7e/8c45ea7f85dd5d52ceddbacc6f56ecaca21ecbfc0e8c34c95618a14d5082/djangorestframework-3.15.1-py3-none-any.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/c0/7e/8c45ea7f85dd5d52ceddbacc6f56ecaca21ecbfc0e8c34c95618a14d5082/djangorestframework-3.15.1-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/4b/13/c2b24ed6757b83c9263fc8d4e563b375d6da22bd54a2d22e8898273a2ef9/drf_spectacular-0.27.1-py3-none-any.whl.metadata
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/4b/13/c2b24ed6757b83c9263fc8d4e563b375d6da22bd54a2d22e8898273a2ef9/drf_spectacular-0.27.1-py3-none-any.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/4b/13/c2b24ed6757b83c9263fc8d4e563b375d6da22bd54a2d22e8898273a2ef9/drf_spectacular-0.27.1-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/fc/b3/0c0c994fe49cd661084f8d5dc06562af53818cc0abefaca35bdc894577c3/Markdown-3.6-py3-none-any.whl.metadata
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/fc/b3/0c0c994fe49cd661084f8d5dc06562af53818cc0abefaca35bdc894577c3/Markdown-3.6-py3-none-any.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/fc/b3/0c0c994fe49cd661084f8d5dc06562af53818cc0abefaca35bdc894577c3/Markdown-3.6-py3-none-any.whl.metadata
error: Failed to download and build: django-allauth==0.61.1
  Caused by: Failed to deserialize cache entry
  Caused by: expected version to start with a number, but no leading ASCII digits were found
```

</details>

---

_Comment by @zanieb on 2024-03-28 18:26_

Thanks for raising, this is a duplicate of https://github.com/astral-sh/uv/issues/2711 and the fix is on its way out.

---

_Label `duplicate` added by @zanieb on 2024-03-28 18:26_

---

_Comment by @charliermarsh on 2024-03-28 18:26_

You can run `uv cache clean` in the meantime if you want.

---

_Closed by @charliermarsh on 2024-03-28 18:26_

---

_Comment by @CoderJoshDK on 2024-03-28 18:28_

So sorry, I tried to look for if this was a duplicate and couldn't find it. 
Thank you for the quick response

---

_Comment by @charliermarsh on 2024-03-28 18:28_

No prob.

---
