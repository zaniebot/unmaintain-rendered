```yaml
number: 8962
title: "Show full dependency chain when `uv pip compile` encounters an error running setup.py"
type: issue
state: closed
author: alex
labels:
  - error messages
  - resolver
assignees: []
created_at: 2024-11-09T01:09:40Z
updated_at: 2024-11-15T00:09:25Z
url: https://github.com/astral-sh/uv/issues/8962
synced_at: 2026-01-12T15:59:39Z
```

# Show full dependency chain when `uv pip compile` encounters an error running setup.py

---

_@alex_

When using `--resolution=lowest` (or `lowest-direct`), it's sadly not uncommon to hit errors running `setup.py` from _very_ old versions of things.

Unfortunately, `uv pip compile` does not include the full chain of how a dependency was depended on, which can make it difficult to debug. For example:

```
~/p/cryptography ❯❯❯ uv pip compile --resolution=lowest-direct --universal -p 3.7 --extra=docs --extra=docstest --extra=pep8test --extra=test --extra=test-randomorder --extra=nox --extra=sdist --unsafe-package=cffi --unsafe-package=pycparser --unsafe-package=setuptools --unsafe-package=cryptography-vectors pyproject.toml > ci-constraints-requirements.txt && git diff
warning: The requested Python version 3.7 is not available; 3.13.0 will be used to build dependencies instead.
⠙ Resolving dependencies...                                                                                                  warning: The direct dependency `build` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
warning: The direct dependency `certifi` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
warning: The direct dependency `check-sdist{python_full_version >= '3.8'}` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
warning: The direct dependency `click` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
warning: The direct dependency `cryptography-vectors` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
warning: The direct dependency `mypy` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
warning: The direct dependency `nox` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
warning: The direct dependency `pretend` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
warning: The direct dependency `pytest-benchmark` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
warning: The direct dependency `pytest-cov` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
warning: The direct dependency `pytest-randomly` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
warning: The direct dependency `pytest-xdist` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
warning: The direct dependency `readme-renderer` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
warning: The direct dependency `ruff` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
⠋ atomicwrites==1.4.1                                                                                                          × Failed to download and build `wsgiref==0.1.2`
  ╰─▶ Build backend failed to determine requirements with `build_wheel()` (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 14, in <module>
          requires = get_requires_for_build({})
        File "/Users/alex_gaynor/Library/Caches/uv/builds-v0/.tmpefXcVs/lib/python3.13/site-packages/setuptools/build_meta.py",
      line 333, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=[])
                 ~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/alex_gaynor/Library/Caches/uv/builds-v0/.tmpefXcVs/lib/python3.13/site-packages/setuptools/build_meta.py",
      line 303, in _get_build_requires
          self.run_setup()
          ~~~~~~~~~~~~~~^^
        File "/Users/alex_gaynor/Library/Caches/uv/builds-v0/.tmpefXcVs/lib/python3.13/site-packages/setuptools/build_meta.py",
      line 521, in run_setup
          super().run_setup(setup_script=setup_script)
          ~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/alex_gaynor/Library/Caches/uv/builds-v0/.tmpefXcVs/lib/python3.13/site-packages/setuptools/build_meta.py",
      line 319, in run_setup
          exec(code, locals())
          ~~~~^^^^^^^^^^^^^^^^
        File "<string>", line 5, in <module>
        File
      "/Users/alex_gaynor/Library/Caches/uv/sdists-v6/pypi/wsgiref/0.1.2/TC3J4nFCD9kJFnrhcTIuq/wsgiref-0.1.2.zip/ez_setup/__init__.py",
      line 170
          print "Setuptools version",version,"or greater has been installed."
          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
      SyntaxError: Missing parentheses in call to 'print'. Did you mean print(...)?

~/p/cryptography ❯❯❯ 
```

Here, neither `wsgiref` nor `atomicwrites` is depended on directly, so I'm not sure which package I need to attach a minimal bound to.

---

_Label `enhancement` added by @charliermarsh on 2024-11-09 02:09_

---

_Label `resolver` added by @charliermarsh on 2024-11-09 02:09_

---

_Comment by @charliermarsh on 2024-11-09 02:09_

Yeah I think something like this makes sense.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-10 17:44_

---

_Comment by @charliermarsh on 2024-11-10 22:29_

One problem here is that if this happens during installation (rather than resolution), we may have already discarded the resolution itself (which would explain why it's included).

---

_Comment by @charliermarsh on 2024-11-10 22:30_

Actually, at the point we need it do we do still have it.

---

_Label `error messages` added by @charliermarsh on 2024-11-11 00:47_

---

_Label `enhancement` removed by @charliermarsh on 2024-11-11 00:47_

---

_Comment by @charliermarsh on 2024-11-11 01:25_

I'm working on this, I think it will be a great feature, but it's non-trivial.

---

_Comment by @dimaqq on 2024-11-14 04:50_

Feature idea: multi-dimensional `uv bisect` to determine the actual lower bounds for dependencies 🎉 

---

_Closed by @charliermarsh on 2024-11-14 20:48_

---

_Closed by @charliermarsh on 2024-11-14 20:48_

---

_Comment by @alex on 2024-11-14 20:58_

Thanks!

On Thu, Nov 14, 2024 at 3:48 PM Charlie Marsh ***@***.***>
wrote:

> Closed #8962 <https://github.com/astral-sh/uv/issues/8962> as completed
> via fe477c3
> <https://github.com/astral-sh/uv/commit/fe477c3417a427e61123ef9b1e4ec19209b21948>
> .
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/8962#event-15304244761>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAAAGBDABG2BQ6H4J47YIBD2AUEDDAVCNFSM6AAAAABROR6YZGVHI2DSMVQWIX3LMV45UABCJFZXG5LFIV3GK3TUJZXXI2LGNFRWC5DJN5XDWMJVGMYDIMRUGQ3TMMI>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


-- 
All that is necessary for evil to succeed is for good people to do nothing.


---

_Comment by @charliermarsh on 2024-11-14 21:54_

@alex -- Do you know how to reproduce this failure? I'd be interested to test the behavior on it.

---

_Comment by @alex on 2024-11-14 22:01_

`uv pip compile --resolution=lowest-direct --universal -p 3.7 --extra=docs --extra=docstest --extra=pep8test --extra=test --extra=test-randomorder --extra=nox --extra=sdist --unsafe-package=cffi --unsafe-package=pycparser --unsafe-package=setuptools --unsafe-package=cryptography-vectors pyproject.toml` in a checkout of github.com/pyca/cryptography

---

_Comment by @charliermarsh on 2024-11-14 22:25_

Ok I think there's some bug that's preventing it from showing up there in that more complex scenario, so won't be in this release -- sorry!

---

_Comment by @charliermarsh on 2024-11-15 00:09_

Fixed! 

```
`wsgiref` was included because `mypy==0.1` depends on `yaro==0.6.4` which depends on `wsgiref`
```

---
