```yaml
number: 8155
title: "Get `Consider setting a lower bound` warning from root package"
type: issue
state: closed
author: DanielYang59
labels:
  - bug
assignees: []
created_at: 2024-10-13T04:16:33Z
updated_at: 2024-12-05T01:59:10Z
url: https://github.com/astral-sh/uv/issues/8155
synced_at: 2026-01-12T15:59:21Z
```

# Get `Consider setting a lower bound` warning from root package

---

_@DanielYang59_

Presumably a partial duplicate of #5227, getting the following warning for the root package itself both [inside the Github workflow environment](https://github.com/materialsproject/pymatgen/actions/runs/11311329412/job/31457441329?pr=4073) (could also recreate in my MacOS machine), with [all dependencies in `ci,optional` have a lower bound](https://github.com/materialsproject/pymatgen/blob/4d5a319f300949ac345475f0d800653d32e95646/pyproject.toml#L92-L113):

```
warning: The direct dependency `pymatgen` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
warning: The direct dependency `pymatgen[ci]` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
warning: The direct dependency `pymatgen[optional]` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
```

With the following command in `test.yml` (could also recreate in MacOS command line):
```
uv pip install dist/pymatgen-2024.10.3.tar.gz
uv pip install pymatgen[ci,optional] --resolution=lowest
```

uv version: `uv-0.4.20`

---

Perhaps I was using the command incorrectly (Intend to install from sdist with extras)?


---

_Closed by @DanielYang59 on 2024-10-13 04:16_

---

_Reopened by @DanielYang59 on 2024-10-13 04:20_

---

_Label `bug` added by @konstin on 2024-10-13 08:17_

---

_Comment by @konstin on 2024-11-28 12:33_

The warnings for the extras were a bug, fixed in #9497. For the resolution lowest test, can you do the following:

```
uv pip install dist/pymatgen-2024.10.3.tar.gz pymatgen[ci,optional] --resolution=lowest
```
Separate `uv pip install` invocations don't know about each other. With a regular (highest) resolution, we try to keep the versions also installed by treating them as preferred in the resolver. But with the lowest resolution, we are simulation the "worst case" of a user with lots of conflicting requirements that force old dependency versions. If you place both the file and the in one command, the resolver pins pymatgen to 2024.10.3 (we need to install the package, we can pick some other version form somewhere else), so we know that everything about `pymatgen[ci,optional]` is constrained.

---

_Comment by @DanielYang59 on 2024-11-30 12:50_

Thanks a lot for the timely fix and for providing the additional context, I would update the install command :) I cannot appreciate more!

---

_Closed by @DanielYang59 on 2024-11-30 12:50_

---

_Comment by @DanielYang59 on 2024-11-30 13:53_

Update: I just build the `uv` from source to test the patch (with `cargo install --git https://github.com/astral-sh/uv uv`), looks like the warnings for extras are indeed gone, but I'm still getting the warning for the root package for some reason? Can you help me double check if I was using the wrong command?

```
uv pip install dist/pymatgen-2024.11.13-cp312-cp312-macosx_15_0_arm64.whl 'pymatgen[ci,optional]' --resolution=lowest

# Got the same warning installing from sdist
uv pip install dist/pymatgen-2024.11.13.tar.gz 'pymatgen[ci,optional]' --resolution=lowest
```

Gives:
```
Using Python 3.12.7 environment at: venv312
⠙ Resolving dependencies...                                                                                                                         
warning: The direct dependency `pymatgen` is unpinned. Consider setting a lower bound when using `--resolution lowest` to avoid using outdated versions.
Resolved 171 packages in 2.55s
Installed 1 package in 15ms
 + pymatgen==2024.11.13 (from file:///Users/yang/developer/pymatgen/dist/pymatgen-2024.11.13-cp312-cp312-macosx_15_0_arm64.whl)

warning: The transitive dependency `isoduration` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `pure-eval` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `scikit-learn` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `pytorch-lightning` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `vapory` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `filelock` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `symfc` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `tinycss2` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `aiohttp` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `nest-asyncio` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `notebook` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `stack-data` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `decorator` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `beautifulsoup4` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `uri-template` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `jupyterlab-pygments` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `rfc3339-validator` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `pydantic` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `wcwidth` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `cftime` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `iniconfig` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `pycparser` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `argon2-cffi-bindings` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `fqdn` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `defusedxml` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `bleach` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `lightning` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
warning: The transitive dependency `appnope` is unpinned. Consider setting a lower bound with a constraint when using `--resolution lowest` to avoid using outdated versions.
```

---

_Reopened by @DanielYang59 on 2024-11-30 13:59_

---

_Comment by @charliermarsh on 2024-11-30 14:41_

I think that should _arguably_ not error but what you want is:

```
uv pip install "dist/pymatgen-2024.11.13-cp311-cp311-macosx_11_0_arm64.whl[ci,optional]" --resolution=lowest
```

Right now, it's as if you provided two dependencies: the URL, alongside an unpinned `pymatgen`.

---

_Comment by @charliermarsh on 2024-11-30 14:53_

It would also be solved by https://github.com/astral-sh/uv/pull/9540.

---

_Comment by @DanielYang59 on 2024-12-01 03:52_

Thanks for the input and for the fix :) I would correct my `uv pip install` usage.

Also cheers for the wonderful tool 

I could confirm I'm not getting that unpinned warning (though another `failed to uninstall package ` warning popped up as I'm not installing in a "clean env" but reinstall in a previous env, but I guess it's not related to current issue):
```
>>> uv pip install dist/pymatgen-2024.11.13.tar.gz[ci,optional] --resolution=lowest

Using Python 3.12.7 environment at: venv312
Resolved 181 packages in 2.57s
   Built pymatgen @ file:///home/yang/developer/pymatgen/dist/pymatgen-2024.11.13.tar.gz
Prepared 3 packages in 21.92s
warning: Failed to uninstall package at venv312/lib/python3.12/site-packages/uncertainties.egg-info due to missing `top-level.txt` file. Installation may result in an incomplete environment.
Uninstalled 3 packages in 3ms
Installed 3 packages in 8ms
 - netcdf4==1.7.2
 + netcdf4==1.6.5
 + pymatgen==2024.11.13 (from file:///home/yang/developer/pymatgen/dist/pymatgen-2024.11.13.tar.gz)
 - uncertainties==2.4.4
 ~ uncertainties==3.1.4
```

---

And finally some off-topic comment in case it might be helpful, but I found the `--quiet` option for `uv build` too strong, i.e. it suppressed all output ([which agrees with the documentation](https://docs.astral.sh/uv/reference/cli/#uv-build)), perhaps at least show warnings and the final built finished like `Successfully built pymatgen-2024.11.13.tar.gz`)?
> --quiet, -q
>     Do not print any output



---

_Closed by @DanielYang59 on 2024-12-01 05:31_

---

_Comment by @zanieb on 2024-12-03 15:57_

@DanielYang59 there's a secret `--no-build-logs` flag, does that get you what you want output-wise?

---

_Comment by @DanielYang59 on 2024-12-04 02:32_

Hi @zanieb thanks for letting me know, this looks much rational, just wondering is it stable enough to be used in production (i.e. would it be renamed/removed at some point)?

```
>>> uv build --sdist --no-build-logs

Building source distribution...
Successfully built dist/pymatgen-2024.11.13.tar.gz
```

---

_Comment by @zanieb on 2024-12-04 17:29_

It's stable, we're using it in our tests. I can unhide it.

---

_Comment by @DanielYang59 on 2024-12-05 01:59_

Thanks a lot for the input and for the additional work :) Really appreciate that!

---
