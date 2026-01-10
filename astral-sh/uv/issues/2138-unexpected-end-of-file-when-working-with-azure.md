---
number: 2138
title: unexpected end of file when working with Azure Artifacts
type: issue
state: open
author: janbernloehr
labels:
  - network
assignees: []
created_at: 2024-03-03T16:13:02Z
updated_at: 2024-07-09T09:15:10Z
url: https://github.com/astral-sh/uv/issues/2138
synced_at: 2026-01-10T01:23:13Z
---

# unexpected end of file when working with Azure Artifacts

---

_Issue opened by @janbernloehr on 2024-03-03 16:13_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

We are hosting our own pypi repo using Azure Artifacts. When compiling a larger requirements file ~ 500 reqs, `uv` chokes at different points when trying to download wheels

```
           uv_client::cached_client::fresh_request url="https://pkgs.dev.azure.com/path/to/our/pypi/download/cryptography/40.0.2/cryptography-40.0.2-cp36-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.whl#sha256=0dcca15d3a19a66e63662dc8d30f8036b07be851a8680eda92d079868f106288"
error: Failed to download: grpcio==1.62.0
  Caused by: The wheel grpcio-1.62.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl is not a valid zip file
  Caused by: an upstream reader returned an error: request or response body error: error reading a body from connection: unexpected end of file
  Caused by: request or response body error: error reading a body from connection: unexpected end of file
  Caused by: error reading a body from connection: unexpected end of file
  Caused by: unexpected end of file
```

repeating this fails at other points

`pip compile` works fine however, probably it is retrying in that case.

- The uv version used 0.1.13
- The uv command `uv pip compile req.txt.in --no-emit-index-url --no-emit-find-links --output-file req.txt -v --index-url https://usr:pat@pkgs.dev.azure.com/path/to/our/pypi/simple/ --extra-index-url https://usr:pat@pkgs.dev.azure.com/path/to/some/other/pypi/simple/`

---

_Label `registry` added by @konstin on 2024-03-03 19:12_

---

_Label `compatibility` added by @konstin on 2024-03-03 19:12_

---

_Comment by @charliermarsh on 2024-03-04 14:45_

We may need to setup our own Azure Artifacts index in order to reproduce this (unless your index is public).

---

_Referenced in [astral-sh/uv#1502](../../astral-sh/uv/issues/1502.md) on 2024-03-08 17:56_

---

_Comment by @charliermarsh on 2024-03-08 20:19_

I set up an Azure Artifacts instance but haven't been able to repro yet.

---

_Comment by @iflare3g on 2024-03-09 17:36_

https://github.com/astral-sh/uv/issues/1502#issuecomment-1976714554

this is my command, if you need I can run it with verbose mode and giving you more logs.
I've tried with the latest version of uv

---

_Comment by @iflare3g on 2024-03-27 10:52_

@charliermarsh could I kindly ask you if there are updates about it ?
we're waiting for this fix like football teams wish winning the Champion's League 🙏 

tnx in advance!

---

_Comment by @charliermarsh on 2024-03-27 14:49_

The core issue is I need to be able to reproduce. I'd love to fix it... I can try again today.

---

_Comment by @nsphung on 2024-03-27 16:33_

@charliermarsh this is working for me now. Probably thanks to https://github.com/astral-sh/uv/pull/2083 

The private lib which was only in the Azure Artifacts works for me now with uv `0.1.24`

But now, I'm running into a shadowed dependencies between Pypi and my private repo on Azure Artifacts:

```
Audited 171 packages in 14ms
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of python-json-logger==2.0.7 and you require python-json-logger==2.0.7, we can conclude that the requirements are
      unsatisfiable.
```

---

_Comment by @iflare3g on 2024-03-27 17:52_

```
DEBUG Cached interpreter info for Python 3.11.1, skipping probing: /Users/iflare3g/.venvs/emergency-api/bin/python
DEBUG Using Python 3.11.1 interpreter at /Users/iflare3g/.venvs/emergency-api/bin/python for builds
DEBUG Using registry request timeout of 300s
DEBUG Solving with target Python version 3.11.1
DEBUG Adding direct dependency: celery<6
DEBUG Adding direct dependency: boto3>=1.21.25
DEBUG Adding direct dependency: django-admin-autocomplete-filter==0.7.1
DEBUG Adding direct dependency: django-constance>=2.8.0
DEBUG Adding direct dependency: django-cors-headers>=3.11.0
DEBUG Adding direct dependency: django-environ==0.10.0
DEBUG Adding direct dependency: django-extensions>=3.1.5
DEBUG Adding direct dependency: django-filter>=21.1
DEBUG Adding direct dependency: django-import-export>=2.7.1
DEBUG Adding direct dependency: django-storages>=1.12.3
DEBUG Adding direct dependency: django-xworkflows>=1.0.0
DEBUG Adding direct dependency: djangorestframework>=3.13.1
DEBUG Adding direct dependency: drf-oidc-auth>=2.0.0
DEBUG Adding direct dependency: drf-spectacular>=0.22.0
DEBUG Adding direct dependency: gunicorn==20.1.0
DEBUG Adding direct dependency: psycopg2-binary>=2.9.3
DEBUG Adding direct dependency: pytz*
DEBUG Adding direct dependency: redis>=4.2.0
DEBUG Adding direct dependency: requests>=2.27.1
DEBUG Adding direct dependency: rich>=12.0.1
DEBUG Adding direct dependency: sentry-sdk>=1.11
DEBUG Adding direct dependency: toml>=0.10.2
DEBUG Adding direct dependency: watchtower>=3.0.0
DEBUG Adding direct dependency: django-crashlog>=1.2.0
DEBUG Adding direct dependency: certifi>=2022.12.7
DEBUG Adding direct dependency: lxml>=4.9.1
DEBUG Adding direct dependency: authlib>=1.1.0
DEBUG Adding direct dependency: django-permissions-policy>=4.17.0
DEBUG Adding direct dependency: wfp-oidc-client>=5.0
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/celery/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/celery/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/boto3/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/boto3/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-admin-autocomplete-filter/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-admin-autocomplete-filter/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-cors-headers/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-cors-headers/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-extensions/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-extensions/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-filter/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-filter/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-import-export/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-import-export/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-constance/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-constance/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-storages/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-storages/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-xworkflows/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-xworkflows/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-environ/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-environ/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/djangorestframework/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/djangorestframework/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/drf-oidc-auth/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/drf-oidc-auth/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/drf-spectacular/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/drf-spectacular/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/gunicorn/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/gunicorn/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/pytz/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/pytz/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/psycopg2-binary/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/psycopg2-binary/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/requests/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/requests/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/rich/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/rich/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/watchtower/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/watchtower/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/redis/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/redis/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-crashlog/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-crashlog/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/certifi/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/certifi/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/sentry-sdk/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/sentry-sdk/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/lxml/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/lxml/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/toml/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/toml/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-permissions-policy/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-permissions-policy/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/authlib/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/authlib/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/wfp-oidc-client/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/wfp-oidc-client/
DEBUG Found fresh response for: https://pypi.org/simple/django-crashlog/
DEBUG Found fresh response for: https://pypi.org/simple/django-admin-autocomplete-filter/
DEBUG Found fresh response for: https://pypi.org/simple/django-filter/
DEBUG Found fresh response for: https://pypi.org/simple/toml/
DEBUG Found fresh response for: https://pypi.org/simple/django-permissions-policy/
DEBUG Found fresh response for: https://pypi.org/simple/django-cors-headers/
DEBUG Found fresh response for: https://pypi.org/simple/drf-spectacular/
DEBUG Found fresh response for: https://pypi.org/simple/pytz/
DEBUG Found fresh response for: https://pypi.org/simple/django-xworkflows/
DEBUG Found fresh response for: https://pypi.org/simple/certifi/
DEBUG Found fresh response for: https://pypi.org/simple/watchtower/
DEBUG Found fresh response for: https://pypi.org/simple/django-storages/
DEBUG Found fresh response for: https://pypi.org/simple/gunicorn/
DEBUG Found fresh response for: https://pypi.org/simple/django-constance/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/df/0c/bca11a08899586df8761ce302524a2f3d709770026f4a8b003853283c3b7/django_crashlog-1.2.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/requests/
DEBUG Found fresh response for: https://pypi.org/simple/redis/
DEBUG Found fresh response for: https://pypi.org/simple/drf-oidc-auth/
DEBUG Found fresh response for: https://pypi.org/simple/djangorestframework/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/97/73/4bc2445a673e768d5f5a898ad1c484d2c3b166b3a7077cf989efa21a80e8/django_filter-23.3-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/authlib/
DEBUG Found fresh response for: https://pypi.org/simple/celery/
DEBUG Found fresh response for: https://pypi.org/simple/django-extensions/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/44/6f/7120676b6d73228c96e17f1f794d8ab046fc910d781c8d151120c3f1569e/toml-0.10.2-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a7/f7/ae456ea653acf99c471287741f84f9e5c8a1458d1b44715ea94869e27b56/django_cors_headers-4.2.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/fc/22/230c5740f1d296ed60235bc7eb1c41f7cd11db593f59f49c8d371bc8292d/django_permissions_policy-4.17.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/rich/
DEBUG Found fresh response for: https://pypi.org/simple/sentry-sdk/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/92/b1/c979485857fe9bbca7ecf51567891d2bdf3e5ff17276d58df5e8f1454250/django_admin_autocomplete_filter-0.7.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/bf/00/7503a03fe7a30554ccd1861db0da652150a9bb6dcfd2bc37c686ad49bd11/drf_spectacular-0.26.4-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/32/4d/aaf7eff5deb402fd9a24a1449a8119f00d74ae9c2efa79f8ef9994261fc2/pytz-2023.3.post1-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/98/e9/023b8f75128d747d4aee79da84e4ac58eff63bb21f1c0aa7c452a353d207/celery-5.3.4-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/46/28/8fa4acad62e6951ad102f639f776e4e7e20033787d805fd0d031672886d6/django_xworkflows-1.0.0-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/4c/dd/2234eab22353ffc7d94e8d13177aaa050113286e93e7b40eae01fbf7c3d9/certifi-2023.7.22-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/23/41/36c7246c1c273fd316d9b8b81581074becc3137135f9ccd6307960a7879c/watchtower-3.0.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/85/60161a20e259bd92597e8d7fc16b07e09cec9400c3ab73346e8b4bf6ca05/django_storages-1.14-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e4/dd/5b190393e6066286773a67dfcc2f9492058e9b57c4867a95f1ba5caf0a83/gunicorn-20.1.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/70/8e/0e2d847013cb52cd35b38c009bb167a1a26b2ce6cd6965bf26b47bc0bf44/requests-2.31.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/83/13/efb68bd759aab4f8f1de5863f9e63f93fdd532ca6743c41e14a396256f98/django_constance-3.1.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/df/b2/dfdc17f701f7b587f6c89c2b9b6b5978c87a8a785555efc810b064c875de/redis-5.0.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c2/28/6151edfd9c1101f62566abe42d8e0d5b863df70dc8c9630776190cd40dbc/drf_oidc_auth-3.0.0-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ff/4b/3b46c0914ba4b7546a758c35fdfa8e7f017fcbe7f23c878239e93623337a/djangorestframework-3.14.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/81/6e/f4522542322c7f53783da5f65464a7dee137c687111624d2ac733e2a1b98/Authlib-1.2.1-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c1/d1/23ba6235ed82883bb416f57179d1db2c05f3fb8e5d83c18660f9ab6f09c9/rich-13.5.3-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a7/7e/ba12b9660642663f5273141018d2bec0a1cae1711f4f6d1093920e157946/django_extensions-3.2.3-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/62/3a/765a7699a26884dcbf8b071dbe2a2486cc1cafcfb5f5d2e64ffe745dd0c6/sentry_sdk-1.31.0-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of celery (<6)
DEBUG Selecting: celery==5.3.4 (celery-5.3.4-py3-none-any.whl)
DEBUG Adding transitive dependency: billiard>=4.1.0, <5.0
DEBUG Adding transitive dependency: kombu>=5.3.2, <6.0
DEBUG Adding transitive dependency: vine>=5.0.0, <6.0
DEBUG Adding transitive dependency: click>=8.1.2, <9.0
DEBUG Adding transitive dependency: click-didyoumean>=0.3.0
DEBUG Adding transitive dependency: click-repl>=0.2.0
DEBUG Adding transitive dependency: click-plugins>=1.1.1
DEBUG Adding transitive dependency: tzdata>=2022.7
DEBUG Adding transitive dependency: python-dateutil>=2.8.2
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/kombu/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/kombu/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/billiard/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/billiard/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/vine/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/vine/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/click/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/click/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/click-didyoumean/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/click-didyoumean/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/click-repl/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/click-repl/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/tzdata/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/tzdata/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/click-plugins/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/click-plugins/
DEBUG Found fresh response for: https://pypi.org/simple/lxml/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/python-dateutil/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/python-dateutil/
DEBUG Found fresh response for: https://pypi.org/simple/boto3/
DEBUG Searching for a compatible version of boto3 (>=1.21.25)
DEBUG Selecting: boto3==1.28.49 (boto3-1.28.49-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d6/56/9d5cb3438143a5aebad59088ca392950d74a531e1b96d0959144370b3b59/lxml-4.9.3-cp311-cp311-macosx_11_0_universal2.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/84/85/e895e6ca369f406432c5f5f186b107f1e1a6a2408b60a081de32021fd818/boto3-1.28.49-py3-none-any.whl.metadata
DEBUG Adding transitive dependency: botocore>=1.31.49, <1.32.0
DEBUG Adding transitive dependency: jmespath>=0.7.1, <2.0.0
DEBUG Adding transitive dependency: s3transfer>=0.6.0, <0.7.0
DEBUG Searching for a compatible version of django-admin-autocomplete-filter (==0.7.1)
DEBUG Selecting: django-admin-autocomplete-filter==0.7.1 (django_admin_autocomplete_filter-0.7.1-py3-none-any.whl)
DEBUG Adding transitive dependency: django>=2.0
DEBUG Adding transitive dependency: django>=4.2
DEBUG Searching for a compatible version of django-constance (>=2.8.0)
DEBUG Selecting: django-constance==3.1.0 (django_constance-3.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency: django-picklefield*
DEBUG Searching for a compatible version of django-cors-headers (>=3.11.0)
DEBUG Selecting: django-cors-headers==4.2.0 (django_cors_headers-4.2.0-py3-none-any.whl)
DEBUG Adding transitive dependency: django>=3.2
DEBUG Adding transitive dependency: django>=4.2
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/botocore/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/botocore/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/jmespath/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/jmespath/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/s3transfer/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/s3transfer/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-picklefield/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-picklefield/
DEBUG No cache entry for: https://pypi.org/simple/wfp-oidc-client/
DEBUG No credentials found for: https://pypi.org/simple/wfp-oidc-client/
DEBUG Found fresh response for: https://pypi.org/simple/django-environ/
DEBUG Searching for a compatible version of django-environ (==0.10.0)
DEBUG Selecting: django-environ==0.10.0 (django_environ-0.10.0-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/django-import-export/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/28/54/caed659e2f0629693b899fb1c0307919b1a7e29d7052e7f9fb3bb3ddc5ef/django_environ-0.10.0-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of django-extensions (>=3.1.5)
DEBUG Selecting: django-extensions==3.2.3 (django_extensions-3.2.3-py3-none-any.whl)
DEBUG Adding transitive dependency: django>=3.2
DEBUG Adding transitive dependency: django>=4.2
DEBUG Searching for a compatible version of django-filter (>=21.1)
DEBUG Selecting: django-filter==23.3 (django_filter-23.3-py3-none-any.whl)
DEBUG Adding transitive dependency: django>=3.2
DEBUG Adding transitive dependency: django>=4.2
DEBUG Searching for a compatible version of django-import-export (>=2.7.1)
DEBUG Selecting: django-import-export==3.3.1 (django_import_export-3.3.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/70/9d/32a81e701472c6d482f34089bfe5ec29f7b4ac537481a617504e8ae50f26/django_import_export-3.3.1-py3-none-any.whl.metadata
DEBUG Adding transitive dependency: diff-match-patch*
DEBUG Adding transitive dependency: django>=3.2
DEBUG Adding transitive dependency: django>=4.2
DEBUG Adding transitive dependency: tablib==3.5.0
DEBUG Adding transitive dependency: tablib[html]==3.5.0
DEBUG Adding transitive dependency: tablib[ods]==3.5.0
DEBUG Adding transitive dependency: tablib[xls]==3.5.0
DEBUG Adding transitive dependency: tablib[xlsx]==3.5.0
DEBUG Adding transitive dependency: tablib[yaml]==3.5.0
DEBUG Searching for a compatible version of django-storages (>=1.12.3)
DEBUG Selecting: django-storages==1.14 (django_storages-1.14-py3-none-any.whl)
DEBUG Adding transitive dependency: django>=3.2
DEBUG Adding transitive dependency: django>=4.2
DEBUG Searching for a compatible version of django-xworkflows (>=1.0.0)
DEBUG Selecting: django-xworkflows==1.0.0 (django_xworkflows-1.0.0-py2.py3-none-any.whl)
DEBUG Adding transitive dependency: django>=1.11
DEBUG Adding transitive dependency: django>=4.2
DEBUG Adding transitive dependency: xworkflows*
DEBUG Searching for a compatible version of djangorestframework (>=3.13.1)
DEBUG Selecting: djangorestframework==3.14.0 (djangorestframework-3.14.0-py3-none-any.whl)
DEBUG Adding transitive dependency: django>=3.0
DEBUG Adding transitive dependency: django>=4.2
DEBUG Adding transitive dependency: pytz*
DEBUG Searching for a compatible version of drf-oidc-auth (>=2.0.0)
DEBUG Selecting: drf-oidc-auth==3.0.0 (drf_oidc_auth-3.0.0-py2.py3-none-any.whl)
DEBUG Adding transitive dependency: authlib>=0.15.0
DEBUG Adding transitive dependency: cryptography>=2.6
DEBUG Adding transitive dependency: django>=2.2.0
DEBUG Adding transitive dependency: django>=4.2
DEBUG Adding transitive dependency: djangorestframework>=3.11.0
DEBUG Adding transitive dependency: requests>=2.20.0
DEBUG Searching for a compatible version of drf-spectacular (>=0.22.0)
DEBUG Selecting: drf-spectacular==0.26.4 (drf_spectacular-0.26.4-py3-none-any.whl)
DEBUG Adding transitive dependency: django>=2.2
DEBUG Adding transitive dependency: django>=4.2
DEBUG Adding transitive dependency: djangorestframework>=3.10.3
DEBUG Adding transitive dependency: uritemplate>=2.0.0
DEBUG Adding transitive dependency: pyyaml>=5.1
DEBUG Adding transitive dependency: jsonschema>=2.6.0
DEBUG Adding transitive dependency: inflection>=0.3.1
DEBUG Searching for a compatible version of gunicorn (==20.1.0)
DEBUG Selecting: gunicorn==20.1.0 (gunicorn-20.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency: setuptools>=3.0
DEBUG Found fresh response for: https://pypi.org/simple/psycopg2-binary/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/diff-match-patch/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/diff-match-patch/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/tablib/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/tablib/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/xworkflows/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/xworkflows/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/cryptography/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/cryptography/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/pyyaml/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/pyyaml/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/uritemplate/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/uritemplate/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/jsonschema/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/jsonschema/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/inflection/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/inflection/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/37/e6/7dd26ba148a02b7e78471ed7ce177e9890167994c5a1d4182361b9bee4cc/psycopg2_binary-2.9.7-cp311-cp311-macosx_10_9_x86_64.whl.metadata
DEBUG Searching for a compatible version of psycopg2-binary (>=2.9.3)
DEBUG Selecting: psycopg2-binary==2.9.7 (psycopg2_binary-2.9.7-cp311-cp311-macosx_10_9_x86_64.whl)
DEBUG Searching for a compatible version of pytz (*)
DEBUG Selecting: pytz==2023.3.post1 (pytz-2023.3.post1-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of redis (>=4.2.0)
DEBUG Selecting: redis==5.0.0 (redis-5.0.0-py3-none-any.whl)
DEBUG Adding transitive dependency: async-timeout>=4.0.2
DEBUG Searching for a compatible version of requests (>=2.27.1)
DEBUG Selecting: requests==2.31.0 (requests-2.31.0-py3-none-any.whl)
DEBUG Adding transitive dependency: charset-normalizer>=2, <4
DEBUG Adding transitive dependency: idna>=2.5, <4
DEBUG Adding transitive dependency: urllib3>=1.21.1, <3
DEBUG Adding transitive dependency: certifi>=2017.4.17
DEBUG Searching for a compatible version of rich (>=12.0.1)
DEBUG Selecting: rich==13.5.3 (rich-13.5.3-py3-none-any.whl)
DEBUG Adding transitive dependency: markdown-it-py>=2.2.0
DEBUG Adding transitive dependency: pygments>=2.13.0, <3.0.0
DEBUG Searching for a compatible version of sentry-sdk (>=1.11)
DEBUG Selecting: sentry-sdk==1.31.0 (sentry_sdk-1.31.0-py2.py3-none-any.whl)
DEBUG Adding transitive dependency: certifi*
DEBUG Adding transitive dependency: urllib3>=1.26.11
DEBUG Searching for a compatible version of toml (>=0.10.2)
DEBUG Selecting: toml==0.10.2 (toml-0.10.2-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of watchtower (>=3.0.0)
DEBUG Selecting: watchtower==3.0.1 (watchtower-3.0.1-py3-none-any.whl)
DEBUG Adding transitive dependency: boto3>=1.9.253, <2
DEBUG Searching for a compatible version of django-crashlog (>=1.2.0)
DEBUG Selecting: django-crashlog==1.2.0 (django_crashlog-1.2.0-py3-none-any.whl)
DEBUG Adding transitive dependency: django-admin-extra-urls*
DEBUG Searching for a compatible version of certifi (>=2022.12.7)
DEBUG Selecting: certifi==2023.7.22 (certifi-2023.7.22-py3-none-any.whl)
DEBUG Searching for a compatible version of lxml (>=4.9.1)
DEBUG Selecting: lxml==4.9.3 (lxml-4.9.3-cp311-cp311-macosx_11_0_universal2.whl)
DEBUG Searching for a compatible version of authlib (>=1.1.0)
DEBUG Selecting: authlib==1.2.1 (Authlib-1.2.1-py2.py3-none-any.whl)
DEBUG Adding transitive dependency: cryptography>=3.2
DEBUG Searching for a compatible version of django-permissions-policy (>=4.17.0)
DEBUG Selecting: django-permissions-policy==4.17.0 (django_permissions_policy-4.17.0-py3-none-any.whl)
DEBUG Adding transitive dependency: django>=3.2
DEBUG Adding transitive dependency: django>=4.2
DEBUG No cache entry for: https://pypi.org/simple/botocore/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/botocore/
DEBUG No credentials found for: https://pypi.org/simple/botocore/
DEBUG Found fresh response for: https://pypi.org/simple/click-didyoumean/
DEBUG Found fresh response for: https://pypi.org/simple/tzdata/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/setuptools/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/setuptools/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ad/36/4599267417fc78b587b1588e0647a468c60b36c02bb723d450d050738fa8/click_didyoumean-0.3.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d5/fb/a79efcab32b8a1f1ddca7f35109a50e4a80d42ac1c9187ab46522b2407d7/tzdata-2023.3-py2.py3-none-any.whl.metadata
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/async-timeout/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/async-timeout/
DEBUG No cache entry for: https://pypi.org/simple/django/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/django/
DEBUG No credentials found for: https://pypi.org/simple/django/
DEBUG Found fresh response for: https://pypi.org/simple/python-dateutil/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/36/7a/87837f39d0296e723bb9b62bbb257d0355c7f6128853c78955f57342a56d/python_dateutil-2.8.2-py2.py3-none-any.whl.metadata
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/charset-normalizer/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/charset-normalizer/
DEBUG Found fresh response for: https://pypi.org/simple/vine/
DEBUG Found fresh response for: https://pypi.org/simple/click-plugins/
DEBUG Found fresh response for: https://pypi.org/simple/click/
DEBUG Found fresh response for: https://pypi.org/simple/kombu/
DEBUG Found fresh response for: https://pypi.org/simple/jmespath/
DEBUG Found fresh response for: https://pypi.org/simple/billiard/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/idna/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/idna/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/8d/61/a7badb48186919a9fd7cf0ef427cab6d16e0ed474035c36fa64ddd72bfa2/vine-5.0.0-py2.py3-none-any.whl.metadata
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/urllib3/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/urllib3/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e9/da/824b92d9942f4e472702488857914bdd50f73021efea15b4cad9aca8ecef/click_plugins-1.1.1-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/00/2e/d53fa4befbf2cfa713304affc7ca780ce4fc1fd8710527771b58311a3229/click-8.1.7-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/86/dc/8a252a772d3a18008fab6fd3ec1bed16a6c3351260a1023c8e8f95759454/kombu-5.3.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/31/b4/b9b800c45527aadd64d5b442f9b932b00648617eb5d63d2c7a6587b7cafc/jmespath-1.0.1-py3-none-any.whl.metadata
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/markdown-it-py/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/markdown-it-py/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e3/a4/ae83eeb0563804a4148ff0e52f530e675bc90718c7c770cdfe4af1340c86/billiard-4.1.0-py3-none-any.whl.metadata
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/pygments/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/pygments/
DEBUG No cache entry for: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-admin-extra-urls/
DEBUG Request already has an authorization header: https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/django-admin-extra-urls/
DEBUG Found fresh response for: https://pypi.org/simple/click-repl/
DEBUG No cache entry for: https://pypi.org/simple/django-picklefield/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/django-picklefield/
DEBUG No credentials found for: https://pypi.org/simple/django-picklefield/
DEBUG No cache entry for: https://pypi.org/simple/s3transfer/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/s3transfer/
DEBUG No credentials found for: https://pypi.org/simple/s3transfer/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/52/40/9d857001228658f0d59e97ebd4c346fe73e138c6de1bce61dc568a57c7f8/click_repl-0.3.0-py3-none-any.whl.metadata
DEBUG No cache entry for: https://pypi.org/simple/jsonschema/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/jsonschema/
DEBUG No credentials found for: https://pypi.org/simple/jsonschema/
DEBUG No cache entry for: https://pypi.org/simple/inflection/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/inflection/
DEBUG No credentials found for: https://pypi.org/simple/inflection/
DEBUG No cache entry for: https://pypi.org/simple/async-timeout/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/async-timeout/
DEBUG No credentials found for: https://pypi.org/simple/async-timeout/
DEBUG No cache entry for: https://pypi.org/simple/cryptography/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/cryptography/
DEBUG No credentials found for: https://pypi.org/simple/cryptography/
DEBUG No cache entry for: https://pypi.org/simple/xworkflows/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/xworkflows/
DEBUG No credentials found for: https://pypi.org/simple/xworkflows/
DEBUG No cache entry for: https://pypi.org/simple/charset-normalizer/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/charset-normalizer/
DEBUG No credentials found for: https://pypi.org/simple/charset-normalizer/
DEBUG No cache entry for: https://pypi.org/simple/setuptools/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/setuptools/
DEBUG No credentials found for: https://pypi.org/simple/setuptools/
DEBUG Found fresh response for: https://pypi.org/simple/diff-match-patch/
DEBUG No cache entry for: https://pypi.org/simple/tablib/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/tablib/
DEBUG No credentials found for: https://pypi.org/simple/tablib/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/96/19/2654bccc833c946d94b84e4fa02d1ad1d2ff216bacad2e014a3dd820c735/diff_match_patch-20230430-py3-none-any.whl.metadata
DEBUG No cache entry for: https://pypi.org/simple/pyyaml/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/pyyaml/
DEBUG No credentials found for: https://pypi.org/simple/pyyaml/
DEBUG No cache entry for: https://pypi.org/simple/uritemplate/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/uritemplate/
DEBUG No credentials found for: https://pypi.org/simple/uritemplate/
DEBUG No cache entry for: https://pypi.org/simple/idna/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/idna/
DEBUG No credentials found for: https://pypi.org/simple/idna/
DEBUG No cache entry for: https://pypi.org/simple/django-admin-extra-urls/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/django-admin-extra-urls/
DEBUG No credentials found for: https://pypi.org/simple/django-admin-extra-urls/
DEBUG No cache entry for: https://pypi.org/simple/urllib3/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/urllib3/
DEBUG No credentials found for: https://pypi.org/simple/urllib3/
DEBUG No cache entry for: https://pypi.org/simple/pygments/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/pygments/
DEBUG No credentials found for: https://pypi.org/simple/pygments/
DEBUG Found fresh response for: https://pypi.org/simple/markdown-it-py/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl.metadata
DEBUG No compatible version found for: wfp-oidc-client
  × No solution found when resolving dependencies:
  ╰─▶ Because wfp-oidc-client>=5.0 was not found in the package registry and you require wfp-oidc-client>=5.0, we can conclude that the requirements are unsatisfiable.
```

> The core issue is I need to be able to reproduce. I'd love to fix it... I can try again today.

The only thing I can do I suppose is to send you the full log with verbose mode.
I'm using the latest version of uv, but still no luck.

I've tried both by specifying extra index url via cli param or via env var.
Azure Artifacts use a PAT for authenticating
That's all the info I can give you, hope they are useful for repro it

---

_Comment by @charliermarsh on 2024-03-27 21:35_

And `wfp-oidc-client` is a package on your internal index that has a version `>=5.0`, right?

---

_Comment by @iflare3g on 2024-03-28 20:20_

> And `wfp-oidc-client` is a package on your internal index that has a version `>=5.0`, right?

Exactly! The latest version is 5.0.1

---

_Comment by @charliermarsh on 2024-03-28 20:23_

Can you check the contents of https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/wfp-oidc-client/, and share them if there's nothing sensitive? Or email them to me if you prefer.

---

_Comment by @iflare3g on 2024-03-29 08:57_

> Can you check the contents of https://worldfoodprogramme.pkgs.visualstudio.com/_packaging/WFPPythonPackages/pypi/simple/ges/pypi/simple/wfp-oidc-client/, and share them if there's nothing sensitive? Or email them to me if you prefer.

Nothing huge sensitive, I'ce covered up some stuff with * but this is the result:

`{"count":1,"value":{"Message":"No HTTP resource was found that matches the request URI 'https://****.pkgs.visualstudio.com/_packaging/*****/pypi/simple/ges/pypi/simple/wfp-oidc-client/'."}}`


that I think makes sense on the behaviour of uv, I don't know how pip-tools is handling this stuff when it requires auth for getting packages from Azure Artifacts


If pip-tools has a verbosity that can help understand what happens that make it work, I can try and send it 

---

_Comment by @amenezes on 2024-04-16 04:44_

@charliermarsh,

I have similiar issue for only one dependency hosted on Gitlab.

``` bash
DEBUG Searching for a compatible version of pdfex (==1.2.0)
TRACE selecting candidate for package pdfex with range Range { segments: [(Included("1.2.0"), Included("1.2.0"))] } with 0 remote versions
TRACE exhausted all candidates for package PackageName("pdfex") with range Range { segments: [(Included("1.2.0"), Included("1.2.0"))] } after 0 steps
DEBUG No compatible version found for: pdfex
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of pdfex==1.2.0 and you require pdfex==1.2.0, we can conclude that the requirements are unsatisfiable.
```

When I try install the dependency using pip it`s works fine. Is there some more info that can I share to help in this fix?

---

_Comment by @amenezes on 2024-04-18 03:40_

A new observation that caught my attention during recent attempts is that if I comment out the `--extra-index-url` line that is causing issues in the requirements.txt file and run `uv pip install --extra-index-url <url> pdfex==1.2.1`, for instance, everything works perfectly.

In my use case, there are three `--extra-index-url` lines in the `requirements.txt` file.

---

_Comment by @charliermarsh on 2024-04-18 03:46_

Have you tried running with `--index-strategy unsafe-any-match`?

---

_Comment by @amenezes on 2024-04-18 15:26_

> Have you tried running with `--index-strategy unsafe-any-match`?

@charliermarsh,

It worked!🥳

---

_Comment by @iflare3g on 2024-04-19 10:24_

@charliermarsh  any other way I can help you in order to speed up solving this issue ? 
I have it working either with pip and pip-tools, if you need any kind of verbosity / log, tell me and I can try running it 

---

_Label `compatibility` removed by @konstin on 2024-07-09 09:15_

---

_Label `registry` removed by @konstin on 2024-07-09 09:15_

---

_Label `network` added by @konstin on 2024-07-09 09:15_

---
