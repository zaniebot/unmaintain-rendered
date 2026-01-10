```yaml
number: 4333
title: "uv fails to resolve 'jsonschema' and 'reana-client' environment only on Python 3.12 while pip succeeds"
type: issue
state: closed
author: matthewfeickert
labels:
  - resolver
  - great writeup
assignees: []
created_at: 2024-06-14T22:02:14Z
updated_at: 2024-06-17T22:43:36Z
url: https://github.com/astral-sh/uv/issues/4333
synced_at: 2026-01-10T05:31:37Z
```

# uv fails to resolve 'jsonschema' and 'reana-client' environment only on Python 3.12 while pip succeeds

---

_Issue opened by @matthewfeickert on 2024-06-14 22:02_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hi. On Python 3.12, the following install command (I'm the maintainer of [`recast-atlas`](https://github.com/recast-hep/recast-atlas)) is failing

```
uv --no-cache pip install --upgrade 'recast-atlas[reana]'
```

as it fails to find multiple valid `snakemake` versions and continues to try to install very old versions

```
...
⠦ snakemake==6.8.0                                                                                                                                                                                          error: Failed to download and build `snakemake==6.8.0`
  Caused by: Failed to build: `snakemake==6.8.0`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
/tmp/.tmpSW1PeD/built-wheels-v3/pypi/snakemake/6.8.0/ksm_tI9YfgvZzz1dLI7Dn/snakemake-6.8.0.tar.gz/versioneer.py:421: SyntaxWarning: invalid escape sequence '\s'
  LONG_VERSION_PY['git'] = '''
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/tmp/.tmpSW1PeD/environments-v0/.tmpMXqKmT/lib/python3.12/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=['wheel'])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/tmp/.tmpSW1PeD/environments-v0/.tmpMXqKmT/lib/python3.12/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
    self.run_setup()
  File "/tmp/.tmpSW1PeD/environments-v0/.tmpMXqKmT/lib/python3.12/site-packages/setuptools/build_meta.py", line 487, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/tmp/.tmpSW1PeD/environments-v0/.tmpMXqKmT/lib/python3.12/site-packages/setuptools/build_meta.py", line 311, in run_setup
    exec(code, locals())
  File "<string>", line 28, in <module>
  File "/tmp/.tmpSW1PeD/built-wheels-v3/pypi/snakemake/6.8.0/ksm_tI9YfgvZzz1dLI7Dn/snakemake-6.8.0.tar.gz/versioneer.py", line 1480, in get_version
    return get_versions()["version"]
           ^^^^^^^^^^^^^^
  File "/tmp/.tmpSW1PeD/built-wheels-v3/pypi/snakemake/6.8.0/ksm_tI9YfgvZzz1dLI7Dn/snakemake-6.8.0.tar.gz/versioneer.py", line 1412, in get_versions
    cfg = get_config_from_root(root)
          ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/tmp/.tmpSW1PeD/built-wheels-v3/pypi/snakemake/6.8.0/ksm_tI9YfgvZzz1dLI7Dn/snakemake-6.8.0.tar.gz/versioneer.py", line 342, in get_config_from_root
    parser = configparser.SafeConfigParser()
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
AttributeError: module 'configparser' has no attribute 'SafeConfigParser'. Did you mean: 'RawConfigParser'?
---
```

I can reduce this down to a simpler minimal failing condition of

```
uv --no-cache pip install --upgrade 'jsonschema>=3.0.0' 'reana-client>=0.8.0'
```

['reana-client>=0.8.0' is the equivalent of the `reana-client` source and](https://github.com/reanahub/reana-client/blob/3bdf8025a73a4f381cffa158410e682ad6456334/setup.py#L45-L54)

```python
install_requires = [
    "click>=7",
    "pathspec==0.9.0",
    "jsonpointer>=2.0",
    "reana-commons[yadage,snakemake,cwl]>=0.9.8,<0.10.0",
    "tablib>=0.12.1,<0.13",
    "werkzeug>=0.14.1 ; python_version<'3.10'",
    "werkzeug>=0.15.0 ; python_version>='3.10'",
    "swagger_spec_validator>=2.4.0,<3.0.0; python_version<'3.7'",
]
```

yet installing the same components collectively succeeds

```
uv --no-cache pip install --upgrade 'jsonschema>=3.0.0' 'click>=7' 'pathspec==0.9.0' 'jsonpointer>=2.0' 'reana-commons[yadage,snakemake,cwl]>=0.9.8,<0.10.0' 'tablib>=0.12.1,<0.13' 'werkzeug>=0.15.0'
```

`pip` succeeds in all cases:

```
python -m pip --no-cache-dir install --upgrade 'jsonschema>=3.0.0' 'reana-client>=0.8.0'
```

```
python -m pip --no-cache-dir install --upgrade 'recast-atlas[reana]'
```

and `uv` succeeds for Python 3.8 through Python 3.11 &mdash; it only fails for Python 3.12.


It is also worth noting that

```
uv --no-cache pip install --upgrade 'reana-client>=0.8.0'
```

succeeds, and only the interaction with `jsonschema>=3.0.0` seems to cause this to fail for `uv`.


Here's the other requested info:

* The current uv platform: The `python:3.12` Docker container image
```console
# uname -a
Linux 9afbd4cb667b 6.5.0-35-generic #35~22.04.1-Ubuntu SMP PREEMPT_DYNAMIC Tue May  7 09:00:52 UTC 2 x86_64 GNU/Linux
```
* The current uv version (`uv --version`): `uv 0.2.11`

## Full reproducers

### uv

Install succeeds on Python 3.11

```
# docker run --rm -ti -v /tmp:/tmp python:3.11 /bin/bash
python -m pip --quiet install --upgrade uv
uv venv uv-venv && . uv-venv/bin/activate && uv --quiet pip install --upgrade pip setuptools wheel
uv pip --verbose --no-cache install --upgrade 'jsonschema>=3.0.0' 'reana-client>=0.8.0' &> /tmp/uv-py311-jsonschema-reana-client-pass.txt
```

[uv-py311-jsonschema-reana-client-pass.txt](https://github.com/user-attachments/files/15844220/uv-py311-jsonschema-reana-client-pass.txt)

Install fails on Python 3.12

```
# docker run --rm -ti -v /tmp:/tmp python:3.12 /bin/bash
python -m pip --quiet install --upgrade uv
uv venv uv-venv && . uv-venv/bin/activate && uv --quiet pip install --upgrade pip setuptools wheel
uv pip --verbose --no-cache install --upgrade 'jsonschema>=3.0.0' 'reana-client>=0.8.0' &> /tmp/uv-jsonschema-reana-client-fail.txt
```

[uv-jsonschema-reana-client-fail.txt](https://github.com/user-attachments/files/15844210/uv-jsonschema-reana-client-fail.txt)

Install succeeds (though should be the same information as the fail above)

```
# docker run --rm -ti -v /tmp:/tmp python:3.12 /bin/bash
python -m pip --quiet install --upgrade uv
uv venv uv-venv && . uv-venv/bin/activate && uv --quiet pip install --upgrade pip setuptools wheel
uv --no-cache pip install --upgrade 'jsonschema>=3.0.0' 'click>=7' 'pathspec==0.9.0' 'jsonpointer>=2.0' 'reana-commons[yadage,snakemake,cwl]>=0.9.8,<0.10.0' 'tablib>=0.12.1,<0.13' 'werkzeug>=0.15.0' &> /tmp/uv-jsonschema-reana-client-components-pass.txt
```

[uv-jsonschema-reana-client-components-pass.txt](https://github.com/user-attachments/files/15844213/uv-jsonschema-reana-client-components-pass.txt)

Install succeeds

```
# docker run --rm -ti -v /tmp:/tmp python:3.12 /bin/bash
python -m pip --quiet install --upgrade uv
uv venv uv-venv && . uv-venv/bin/activate && uv --quiet pip install --upgrade pip setuptools wheel
uv pip --verbose --no-cache install --upgrade 'reana-client>=0.8.0' &> /tmp/uv-reana-client-pass.txt
```

[uv-reana-client-pass.txt](https://github.com/user-attachments/files/15844214/uv-reana-client-pass.txt)

### pip

All succeed

```
# docker run --rm -ti -v /tmp:/tmp python:3.12 /bin/bash
python -m pip --quiet install --upgrade uv
python -m venv venv && . venv/bin/activate && python -m pip --quiet install --upgrade pip setuptools wheel
python -m pip --no-cache-dir install --verbose --upgrade 'jsonschema>=3.0.0' 'reana-client>=0.8.0' &> /tmp/pip-jsonschema-reana-client-pass.txt
```

[pip-jsonschema-reana-client-pass.txt](https://github.com/user-attachments/files/15844215/pip-jsonschema-reana-client-pass.txt)

```
# docker run --rm -ti -v /tmp:/tmp python:3.12 /bin/bash
python -m pip --quiet install --upgrade uv
python -m venv venv && . venv/bin/activate && python -m pip --quiet install --upgrade pip setuptools wheel
python -m pip --no-cache-dir install --verbose --upgrade 'recast-atlas[reana]>=0.4.0' &> /tmp/pip-recast-atlas-pass.txt
```

[pip-recast-atlas-pass.txt](https://github.com/user-attachments/files/15844217/pip-recast-atlas-pass.txt)

---

_Label `great writeup` added by @zanieb on 2024-06-15 01:41_

---

_Comment by @zanieb on 2024-06-15 02:06_

Hi thank you for the wonderful thorough write up. Sorry this broke your CI. Nothing is leaping out to me here, but it _is_ known that we can end up backtracking on a package until we fail due to some ancient build.

After some fiddling with the reproduction, it looks like we're trying to solve for `reana-commons[snakemake]` and we eventually reach version `0.9.2.1` which pins `snakemake==6.8.0` which apparently isn't able to be built on Python 3.12.

```
DEBUG Adding transitive dependency for reana-commons[snakemake]==0.9.2.1: snakemake==6.8.0
```

If I add another bound on snakemake in your minimal case to help the resolver get around this undeclared incompatibility, the resolution succeeds:

```
uv --no-cache pip install --upgrade 'jsonschema>=3.0.0' 'reana-client>=0.8.0' 'snakemake>6.8.0' --dry-run
```

This works for your original case as well:

```
uv --no-cache pip install --upgrade 'recast-atlas[reana]'  'snakemake>6.8.0' --dry-run
```

Resolution is still much slower than we'd like because we backtrack on the wrong package, but it's a very hard problem to choose which package to backtrack on (more at, e.g., #1398).

I don't have time to dig deeper right now and perhaps @konstin will have more insights, I'll check back in after the weekend. I think that if you add a conditional lower bound on snakemake for Python 3.12 it should unblock you.



---

_Label `resolver` added by @zanieb on 2024-06-15 02:06_

---

_Comment by @matthewfeickert on 2024-06-15 19:14_

Thanks very much for being so responsive, @zanieb! Your suggestion indeed works for me in https://github.com/recast-hep/recast-atlas/pull/144. :+1: 

The tools I write are usually supporting many backends, and so end up being difficult to solve for when the entire environment is solved at once in CI, so I'm personally happy to say that this is resolved. I will leave it open though for you to make the decision on if this should get closed out, or if this should be a https://github.com/astral-sh/uv/labels/resolver edge case test for the future.

---

_Comment by @konstin on 2024-06-17 12:52_

To add a bit more technical insight on why this is happening: When uv has to pick a version, is prefers making a decision for packages with a more specific bounds (e.g. `==1.2.3`) over less specific bounds (e.g. `*`), and then it visits packages in the order it first saw them. With `uv pip install 'jsonschema>=3.0.0' 'reana-client>=0.8.0'`, we try to pick the highest version of jsonschema first, and when we encounter an incompatibility, we reduce reana-client instead until we reach reana-client 0.9.0 and fail soon after. Using `uv pip install 'reana-client>=0.8.0' 'jsonschema>=3.0.0'` for example works because now the priorities are inverted. You can watch this process in debug logs, e.g. with `uv pip install 'jsonschema>=3.0.0' 'reana-client>=0.8.0' -v 2>&1 | grep -E "Selecting: |Adding transitive dependency"`.

While we try to mitigate changing the wrong package's version, a dependency resolver can ultimately only guess which path to take, and can even unintentionally be forced on the bad path by an unlucky combination of requirements. The best way to prevent this to add lower bounds to a package's requirements, so users and resolver can never accidentally get the wrong resolution with too low versions, or, as in this case, a version that fails too build.

---

_Comment by @konstin on 2024-06-17 12:54_

I'm going to close since we have a workaround (https://github.com/recast-hep/recast-atlas/pull/144) and understand the cause. When can reopen it if there are any specific change's in uv's prioritization we'd need to do.

---

_Closed by @konstin on 2024-06-17 12:54_

---

_Comment by @matthewfeickert on 2024-06-17 22:43_

> With `uv pip install 'jsonschema>=3.0.0' 'reana-client>=0.8.0'`, we try to pick the highest version of jsonschema first, and when we encounter an incompatibility, we reduce reana-client instead until we reach reana-client 0.9.0 and fail soon after. Using `uv pip install 'reana-client>=0.8.0' 'jsonschema>=3.0.0'` for example works because now the priorities are inverted. You can watch this process in debug logs, e.g. with `uv pip install 'jsonschema>=3.0.0' 'reana-client>=0.8.0' -v 2>&1 | grep -E "Selecting: |Adding transitive dependency"`.

Ah this is actually very interesting and helpful to know for debugging. I didn't understand that my choice of package name positioning was being used at solve time (I had assumed that it was arbitrary and `uv` created its own solve order)! Thanks very much for mentioning this.

---
