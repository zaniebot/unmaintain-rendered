```yaml
number: 19661
title: PEP 639 license information
type: pull_request
state: open
author: DimitriPapadopoulos
labels: []
assignees: []
base: main
head: PEP639
created_at: 2025-07-31T12:33:52Z
updated_at: 2025-08-06T06:19:54Z
url: https://github.com/astral-sh/ruff/pull/19661
synced_at: 2026-01-12T15:56:44Z
```

# PEP 639 license information

---

_@DimitriPapadopoulos_

## Summary

New attempt at #19499, which was reverted by #19599 and #19624.

1. Declare licenses in `pyproject.toml` using keys [`license` and `license-files`](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license-and-license-files), as per PEP 639:
   * `license`:       SPDX license expression
   * `license-files`: list of license file glob patterns
2. ~~Keep the Trove classifier in an attempt to remain compatible with tools that do not support PEP 639 yet, such as pip-licenses. Hopefully the build system will not complain about the redundant/obsolete classifier.~~

In theory, maturin ≥ 1.9 supports PEP 639, which means the proper licensing metadata should be included in packages and seen by PyPI:
            https://www.maturin.rs/changelog.html
    
In practice, maturin < 1.9.3 does not take license-files into account and the `LICENSE` file is therefore missing from packages (https://github.com/astral-sh/ruff/pull/19599#issuecomment-3139658776).

## Test Plan

Check the package metadata (`PKG-INFO`) after:
```
uv build --sdist
```

Also test against the obsolete ecosystem (`pip-licenses`).

---

_@DimitriPapadopoulos reviewed on 2025-07-31 13:03_

---

_Review comment by @DimitriPapadopoulos on `pyproject.toml`:13 on 2025-07-31 13:03_

Note sure about maturin, but setuptools does not need an explicit `license-files` key according to [Controlling files in the distribution](https://setuptools.pypa.io/en/latest/userguide/miscellaneous.html):
> * All files specified by the `license-files` configuration parameter in `pyproject.toml` and/or equivalent in `setup.cfg`/`setup.py`; note that if you don’t explicitly set this parameter, `setuptools` will include any files that match the following glob patterns: `LICEN[CS]E*`, `COPYING*`, `NOTICE*`, `AUTHORS**`;

I wonder whether omitting an explicit `license-files` key might work around the maturin bug - if implicit is supported by maturin of course.

Would you prefer explicit or implicit?

---

_@MichaReiser reviewed on 2025-07-31 17:04_

---

_Review comment by @MichaReiser on `pyproject.toml`:13 on 2025-07-31 17:04_

> I wonder whether omitting an explicit license-files key might work around the maturin bug 

You can test this yourself: run `uv build --sdist` and check if the created zip contains the `LICENSE` file

---

_@DimitriPapadopoulos reviewed on 2025-07-31 17:29_

---

_Review comment by @DimitriPapadopoulos on `pyproject.toml`:13 on 2025-07-31 17:29_

The `License-File` metadata field is fine, but the `LICENSE` file itself is missing:
```console
$ uv build --sdist
Building source distribution...
Running `maturin pep517 write-sdist --sdist-directory /path/to/ruff/dist`
[...]
Successfully built dist/ruff-0.12.7.tar.gz
$ tar xf dist/ruff-0.12.7.tar.gz 
$ 
$ grep License ruff-0.12.7/PKG-INFO
Classifier: License :: OSI Approved :: MIT License
License-File: LICENSE
License-Expression: MIT
[...]
$ 
$ find ruff-0.12.7 -name 'LICENSE*'
ruff-0.12.7/crates/ruff_annotate_snippets/LICENSE-MIT
ruff-0.12.7/crates/ruff_annotate_snippets/LICENSE-APACHE
ruff-0.12.7/crates/ty_vendored/vendor/typeshed/LICENSE
$ 
```

Omitting `license-files` does not work around the maturin bug. However, implicit seems better than explicit, so I suggest removing the `license-files` key anyway.

---

_Comment by @DimitriPapadopoulos on 2025-08-04 19:58_

Works now:
```console
$ uv build --sdist
Building source distribution...
Running `maturin pep517 write-sdist --sdist-directory /path/to/ruff/dist`
[...]
📦 Built source distribution to /path/to/ruff/dist/ruff-0.12.7.tar.gz
ruff-0.12.7.tar.gz
Successfully built dist/ruff-0.12.7.tar.gz
$ 
$ tar xf dist/ruff-0.12.7.tar.gz 
$ 
$ grep License ruff-0.12.7/PKG-INFO
License-File: LICENSE
License-Expression: MIT
$ 
$ find ruff-0.12.7 -name 'LICENSE*'
ruff-0.12.7/LICENSE
ruff-0.12.7/crates/ruff_annotate_snippets/LICENSE-MIT
ruff-0.12.7/crates/ruff_annotate_snippets/LICENSE-APACHE
ruff-0.12.7/crates/ty_vendored/vendor/typeshed/LICENSE
$ 
```

Note that the `LICENSE` file is **not** picked up if I remove the `license-files` line. Unlike build systems such as [setuptools](https://setuptools.pypa.io/en/latest/userguide/pyproject_config.html#setuptools-specific-configuration) or [flit](https://flit.pypa.io/en/stable/pyproject_toml.html#new-style-metadata), maturin appears not to provide a default glob that would pick up common licence file names.

---

_Marked ready for review by @DimitriPapadopoulos on 2025-08-04 20:04_

---

_@DimitriPapadopoulos reviewed on 2025-08-04 20:06_

---

_Review comment by @DimitriPapadopoulos on `pyproject.toml`:13 on 2025-08-04 20:06_

It turns out maturin does **not** currently support omitting `license-files`.

---

_Review comment by @MichaReiser on `pyproject.toml`:25 on 2025-08-05 08:13_

We should keep this line (for the reason the comment says). we don't want to break downstream users for no good reason.

---

_@MichaReiser reviewed on 2025-08-05 08:13_

---

_@DimitriPapadopoulos reviewed on 2025-08-05 09:07_

---

_Review comment by @DimitriPapadopoulos on `pyproject.toml`:25 on 2025-08-05 09:07_

I've read https://github.com/astral-sh/ruff/pull/19599#issue-3269992286 but I don't think "this causes significant churn" any more:
* Lots of projetcs have moved to PEP 639, including:
  * [setuptools](https://github.com/pypa/setuptools/pull/4975)
  * [flit](https://github.com/pypa/flit/pull/715)
  * [build](https://github.com/pypa/build/pull/914)
  * [packaging](https://github.com/pypa/packaging/pull/881)
  * [installer](https://github.com/pypa/installer/pull/294)
  * [wheel](https://github.com/pypa/wheel/pull/670)
  * [maturin](https://github.com/PyO3/maturin/pull/2690)
  * [pip](https://github.com/pypa/pip/pull/13335)
  * [flake8](https://github.com/PyCQA/flake8/pull/1990) (removed Trove classifier but still using `setup.cfg`)
  * [pycodestyle](https://github.com/PyCQA/pycodestyle/pull/1280) (removed Trove classifier but still using `setup.cfg`)
  * [black](https://github.com/psf/black/pull/4479)
  * ...
* [pip-licences](https://github.com/raimon49/pip-licenses) is unmaintained and has been forked and superseded by [pip-licenses-cli](https://github.com/stefan6419846/pip-licenses-cli).
* [pip 25.0](https://ichard26.github.io/blog/2025/01/whats-new-in-pip-25.0/) supports [PEP 639: SPDX License Expressions](https://ichard26.github.io/blog/2025/01/whats-new-in-pip-25.0/#pep-639-spdx-license-expressions)
* `typing_extensions` have **not** reverted their change, instead tooling has been fixed

---

_@MichaReiser reviewed on 2025-08-05 09:16_

---

_Review comment by @MichaReiser on `pyproject.toml`:25 on 2025-08-05 09:16_

It still creates churn for no very good reason as the issues in typing extensions demonstrate. It's also not always easy for projects to update their dependencies.

I just don't see a strong reason for making this change other than: Some PEP recommends it. That's why I don't feel comfortable merging this as is.

---

_@DimitriPapadopoulos reviewed on 2025-08-05 19:01_

---

_Review comment by @DimitriPapadopoulos on `pyproject.toml`:25 on 2025-08-05 19:01_

My point was that projects will have to update their dependencies any way, as other dependencies will force them to. Additionally, ruff usually isn't a dependency of sdist or wheel packages, it's a tool for developers. It shouldn't even appear in the final licence list.

But then I agree it doesn't hurt to keep the classifier for a few years.

---

_Review comment by @DimitriPapadopoulos on `pyproject.toml`:25 on 2025-08-05 19:14_

After reverting and putting the classifier back, this PR changes licensing information in `PKG-INFO` from:
```
Classifier: License :: OSI Approved :: MIT License
License-File: LICENSE
```
to:
```
Classifier: License :: OSI Approved :: MIT License
License-File: LICENSE
License-Expression: MIT
```

---

_@DimitriPapadopoulos reviewed on 2025-08-05 19:14_

---

_@MichaReiser reviewed on 2025-08-06 06:19_

---

_Review comment by @MichaReiser on `pyproject.toml`:25 on 2025-08-06 06:19_

This seems fine?

---
