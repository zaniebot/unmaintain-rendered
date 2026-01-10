---
number: 2152
title: "Running `uv pip install 'cryptography @ .'` repeatedly doesn't rebuild, even when there are source changes"
type: issue
state: closed
author: alex
labels:
  - needs-decision
assignees: []
created_at: 2024-03-04T12:21:48Z
updated_at: 2025-06-26T16:29:31Z
url: https://github.com/astral-sh/uv/issues/2152
synced_at: 2026-01-10T01:23:13Z
---

# Running `uv pip install 'cryptography @ .'` repeatedly doesn't rebuild, even when there are source changes

---

_Issue opened by @alex on 2024-03-04 12:21_

```
(cryptography) ~/p/cryptography ❯❯❯ uv --version
uv 0.1.13
```

We have a mixed Python+Rust codebase. Our dev process involves running `pip install .`, which I'm attempting to migrate to `uv pip install 'cryptography @ .'`. However, when run the second time, uv doesn't appear to actually rebuild the rust.

Here's an example output:

```
nox > [2024-03-04 07:19:07,068] uv pip install -c ci-constraints-requirements.txt 'cryptography @ .' -v
 uv::requirements::from_source source=cryptography @ .
 uv::requirements::from_source source=ci-constraints-requirements.txt
    0.000406s DEBUG uv_interpreter::python_environment Found a virtualenv through VIRTUAL_ENV at: /Users/alex_gaynor/projects/cryptography/.nox/local
    0.000534s DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.12.2, skipping probing: /Users/alex_gaynor/projects/cryptography/.nox/local/bin/python
    0.000547s DEBUG uv::commands::pip_install Using Python 3.12.2 environment at /Users/alex_gaynor/projects/cryptography/.nox/local/bin/python
Audited 1 package in 1ms
```

If there's any more information I can provide, let me know!

---

_Comment by @charliermarsh on 2024-03-04 14:27_

We currently only force a rebuild of local packages if there are changes to the `pyproject.toml` or other metadata files based on some heuristics. You can pass `--reinstall-package cryptography` (or `--reinstall`) to tell `uv` to rebuild anyway. 

Alternatively... we could sniff for metadata in the `pyproject.toml` that indicates that the package is a mixed Rust-Python package (like `maturin` or `setuptools-rust` in your case).


---

_Label `bug` added by @charliermarsh on 2024-03-04 14:27_

---

_Comment by @alex on 2024-03-04 14:31_

Hmm, I'm not sure `--reinstall-package cryptography` is sufficient?

Here's the output after making a change to the rust source:

```
nox > [2024-03-04 09:30:36,745] uv pip install -c ci-constraints-requirements.txt --reinstall-package cryptography 'cryptography @ .' -v
 uv::requirements::from_source source=cryptography @ .
 uv::requirements::from_source source=ci-constraints-requirements.txt
    0.000378s DEBUG uv_interpreter::python_environment Found a virtualenv through VIRTUAL_ENV at: /Users/alex_gaynor/projects/cryptography/.nox/local
    0.000513s DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.12.2, skipping probing: /Users/alex_gaynor/projects/cryptography/.nox/local/bin/python
    0.000521s DEBUG uv::commands::pip_install Using Python 3.12.2 environment at /Users/alex_gaynor/projects/cryptography/.nox/local/bin/python
    0.001170s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries 
 uv_resolver::resolver::solve 
      0.134703s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.12.2
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.134745s   0ms DEBUG uv_resolver::resolver Adding direct dependency: cryptography*
   uv_resolver::resolver::choose_version package=cryptography
        0.134807s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of cryptography @ file:///Users/alex_gaynor/projects/cryptography (*)
 uv_resolver::resolver::process_request request=Metadata cryptography @ file:///Users/alex_gaynor/projects/cryptography
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=cryptography @ file:///Users/alex_gaynor/projects/cryptography
        0.135023s   0ms DEBUG uv_distribution::source Using cached metadata for cryptography @ file:///Users/alex_gaynor/projects/cryptography
   uv_resolver::resolver::get_dependencies package=cryptography, version=43.0.0.dev1
     uv_resolver::resolver::distributions_wait package_id=7611bc6e1c38bd7c
        0.135059s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: cffi>=1.12
   uv_resolver::resolver::choose_version package=cffi
     uv_resolver::resolver::package_wait package_name=cffi
 uv_resolver::resolver::process_request request=Versions cffi
   uv_client::registry_client::simple_api package=cffi
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/Users/alex_gaynor/Library/Caches/uv/simple-v3/pypi/cffi.rkyv
 uv_resolver::resolver::process_request request=Prefetch cffi >=1.12
 uv_client::cached_client::from_path_sync path="/Users/alex_gaynor/Library/Caches/uv/simple-v3/pypi/cffi.rkyv"
          0.135420s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/cffi/
   uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=cffi==1.16.0
     uv_client::registry_client::wheel_metadata built_dist=cffi==1.16.0
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/alex_gaynor/Library/Caches/uv/wheels-v0/pypi/cffi/cffi-1.16.0-cp312-cp312-macosx_11_0_arm64.msgpack
        0.135774s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of cffi (>=1.12)
        0.135777s   0ms DEBUG uv_resolver::resolver Selecting: cffi==1.16.0 (cffi-1.16.0-cp312-cp312-macosx_11_0_arm64.whl)
 uv_client::cached_client::from_path_sync path="/Users/alex_gaynor/Library/Caches/uv/wheels-v0/pypi/cffi/cffi-1.16.0-cp312-cp312-macosx_11_0_arm64.msgpack"
   uv_resolver::resolver::get_dependencies package=cffi, version=1.16.0
     uv_resolver::resolver::distributions_wait package_id=cffi-1.16.0
              0.135827s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/b4/f6/b28d2bfb5fca9e8f9afc9d05eae245bed9f6ba5c2897fefee7a9abeaf091/cffi-1.16.0-cp312-cp312-macosx_11_0_arm64.whl.metadata
        0.135847s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: pycparser*
   uv_resolver::resolver::choose_version package=pycparser
     uv_resolver::resolver::package_wait package_name=pycparser
 uv_resolver::resolver::process_request request=Versions pycparser
   uv_client::registry_client::simple_api package=pycparser
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/Users/alex_gaynor/Library/Caches/uv/simple-v3/pypi/pycparser.rkyv
 uv_resolver::resolver::process_request request=Prefetch pycparser *
 uv_client::cached_client::from_path_sync path="/Users/alex_gaynor/Library/Caches/uv/simple-v3/pypi/pycparser.rkyv"
          0.135922s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/pycparser/
   uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=pycparser==2.21
     uv_client::registry_client::wheel_metadata built_dist=pycparser==2.21
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/alex_gaynor/Library/Caches/uv/wheels-v0/pypi/pycparser/pycparser-2.21-py2.py3-none-any.msgpack
        0.135964s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of pycparser (*)
        0.135967s   0ms DEBUG uv_resolver::resolver Selecting: pycparser==2.21 (pycparser-2.21-py2.py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=pycparser, version=2.21
     uv_resolver::resolver::distributions_wait package_id=pycparser-2.21
 uv_client::cached_client::from_path_sync path="/Users/alex_gaynor/Library/Caches/uv/wheels-v0/pypi/pycparser/pycparser-2.21-py2.py3-none-any.msgpack"
              0.136034s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/62/d5/5f610ebe421e85889f2e55e33b7f9a6795bd982198517d912eb1c76e1a53/pycparser-2.21-py2.py3-none-any.whl.metadata
Resolved 3 packages in 1ms
    0.136170s DEBUG uv_installer::plan Requirement already satisfied: cffi==1.16.0
    0.136292s DEBUG uv_installer::plan Path source requirement already cached: cryptography==43.0.0.dev1 (from file:///Users/alex_gaynor/projects/cryptography)
    0.136297s DEBUG uv_installer::plan Requirement already satisfied: pycparser==2.21
    0.136318s DEBUG uv_installer::plan Preserving seed package: wheel==0.42.0
    0.136320s DEBUG uv_installer::plan Unnecessary package: semantic-version==2.10.0
    0.136321s DEBUG uv_installer::plan Unnecessary package: pluggy==1.4.0
    0.136323s DEBUG uv_installer::plan Unnecessary package: check-sdist==0.1.3
    0.136324s DEBUG uv_installer::plan Unnecessary package: pretend==1.0.9
    0.136326s DEBUG uv_installer::plan Unnecessary package: pytest==8.0.2
    0.136327s DEBUG uv_installer::plan Unnecessary package: execnet==2.0.2
    0.136328s DEBUG uv_installer::plan Unnecessary package: coverage==7.4.3
    0.136329s DEBUG uv_installer::plan Unnecessary package: pyproject-hooks==1.0.0
    0.136331s DEBUG uv_installer::plan Unnecessary package: filelock==3.13.1
    0.136332s DEBUG uv_installer::plan Unnecessary package: virtualenv==20.25.1
    0.136334s DEBUG uv_installer::plan Preserving seed package: pip==24.0
    0.136335s DEBUG uv_installer::plan Unnecessary package: mypy-extensions==1.0.0
    0.136337s DEBUG uv_installer::plan Preserving seed package: setuptools==69.1.0
    0.136338s DEBUG uv_installer::plan Unnecessary package: ruff==0.3.0
    0.136339s DEBUG uv_installer::plan Unnecessary package: distlib==0.3.8
    0.136341s DEBUG uv_installer::plan Unnecessary package: iniconfig==2.0.0
    0.136342s DEBUG uv_installer::plan Unnecessary package: packaging==23.2
    0.136343s DEBUG uv_installer::plan Unnecessary package: pytest-cov==4.1.0
    0.136344s DEBUG uv_installer::plan Unnecessary package: nox==2023.4.22
    0.136346s DEBUG uv_installer::plan Unnecessary package: platformdirs==4.2.0
    0.136348s DEBUG uv_installer::plan Unnecessary package: flit==3.9.0
    0.136349s DEBUG uv_installer::plan Unnecessary package: colorlog==6.8.2
    0.136350s DEBUG uv_installer::plan Unnecessary package: bcrypt==4.1.2
    0.136352s DEBUG uv_installer::plan Unnecessary package: pathspec==0.12.1
    0.136353s DEBUG uv_installer::plan Unnecessary package: cryptography-vectors==42.0.4 (from file:///Users/alex_gaynor/projects/cryptography/vectors)
    0.136354s DEBUG uv_installer::plan Unnecessary package: requests==2.31.0
    0.136356s DEBUG uv_installer::plan Unnecessary package: build==1.1.1
    0.136357s DEBUG uv_installer::plan Unnecessary package: typing-extensions==4.10.0
    0.136359s DEBUG uv_installer::plan Unnecessary package: flit-core==3.9.0
    0.136360s DEBUG uv_installer::plan Unnecessary package: charset-normalizer==3.3.2
    0.136361s DEBUG uv_installer::plan Unnecessary package: urllib3==2.2.1
    0.136363s DEBUG uv_installer::plan Unnecessary package: docutils==0.20.1
    0.136364s DEBUG uv_installer::plan Unnecessary package: certifi==2024.2.2
    0.136365s DEBUG uv_installer::plan Unnecessary package: tomli-w==1.0.0
    0.136367s DEBUG uv_installer::plan Unnecessary package: mypy==1.8.0
    0.136368s DEBUG uv_installer::plan Unnecessary package: setuptools-rust==1.8.1
    0.136369s DEBUG uv_installer::plan Unnecessary package: cryptography-vectors==43.0.0.dev1 (from file:///Users/alex_gaynor/projects/cryptography/vectors)
    0.136371s DEBUG uv_installer::plan Unnecessary package: argcomplete==3.2.2
    0.136372s DEBUG uv_installer::plan Unnecessary package: pytest-xdist==3.5.0
    0.136374s DEBUG uv_installer::plan Unnecessary package: py-cpuinfo==9.0.0
    0.136375s DEBUG uv_installer::plan Unnecessary package: pytest-benchmark==4.0.0
    0.136376s DEBUG uv_installer::plan Unnecessary package: maturin==1.4.0
    0.136378s DEBUG uv_installer::plan Unnecessary package: idna==3.6
    0.136379s DEBUG uv_installer::plan Unnecessary package: click==8.1.7
    0.145393s DEBUG uv::commands::pip_install Uninstalled cryptography (109 files, 33 directories)
 uv_installer::installer::install num_wheels=1
Installed 1 package in 2ms
 - cryptography==43.0.0.dev1 (from file:///Users/alex_gaynor/projects/cryptography)
 + cryptography==43.0.0.dev1 (from file:///Users/alex_gaynor/projects/cryptography)
```

---

_Comment by @charliermarsh on 2024-03-04 14:34_

Ah, you may need `--refresh` / `--refresh-package cryptography` as well to avoid reading the cached metadata. (`--reinstall` says "ignore what's in the virtualenv", `--refresh` says "ignore what's in the cache".)

---

_Comment by @alex on 2024-03-04 14:50_

Yes, that works.

I don't know if specifically detecting rust projects is the way to go (this is general to anything with an extension module), but something to make this behavior more ergonomic and less special-casey would be valuable.

---

_Comment by @charliermarsh on 2024-03-04 14:52_

I agree.

---

_Referenced in [astral-sh/uv#2481](../../astral-sh/uv/issues/2481.md) on 2024-03-17 21:28_

---

_Label `bug` removed by @charliermarsh on 2024-06-04 20:36_

---

_Label `needs-decision` added by @charliermarsh on 2024-06-04 20:36_

---

_Comment by @charliermarsh on 2024-06-04 20:36_

I changed the behavior such that `--reinstall` should be sufficient on its own, without `--refresh`.

---

_Comment by @charliermarsh on 2024-09-06 21:17_

Do you think there's any way to solve this with https://github.com/astral-sh/uv/pull/7136? E.g., could you mark `target/debug/cryptography` or similar as invalidating the cache?

---

_Comment by @alex on 2024-09-06 21:26_

Hmm, maybe? I don't think the target folder is right though, it'd need to be the rust source, since you want to rebuild the target folder based on changes to the source.

That said, it feels like an abstraction failure to need to have installer specific stuff for this. (Maybe that just means it should be standardized though, ugh.)

---

_Comment by @charliermarsh on 2024-09-06 21:32_

You could probably add a `build.rs` script that wrote out to a file or something, but yeah. It's mostly our fault for wanting to avoid rebuilds in the first place.

---

_Comment by @alex on 2024-09-06 21:35_

Avoiding rebuilds has its own benefits :-)

On Fri, Sep 6, 2024 at 5:33 PM Charlie Marsh ***@***.***>
wrote:

> You could probably add a build.rs script that wrote out to a file or
> something, but yeah. It's mostly our fault for wanting to avoid rebuilds in
> the first place.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/2152#issuecomment-2334839597>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAAAGBCXPNYXQHNELTAZSW3ZVINSBAVCNFSM6AAAAABEFC4ATCVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDGMZUHAZTSNJZG4>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


-- 
All that is necessary for evil to succeed is for good people to do nothing.


---

_Comment by @charliermarsh on 2024-09-10 01:45_

For onlookers, we have a new API whereby you can add additional files to consider when invalidating the cache. You can also include the current Git commit (i.e., invalidate whenever the SHA changes).

Looks like this:

```toml
[tool.uv]
cache-keys = [{ file = "pyproject.toml" }, { file = "requirements.txt" }, { git = true }]
```

See: https://docs.astral.sh/uv/concepts/cache/#dynamic-metadata.

I don't think this will solve the extension module use-case described here, but it is helpful for things like `setuptools_scm`.


---

_Referenced in [astral-sh/uv#7246](../../astral-sh/uv/issues/7246.md) on 2024-09-10 08:16_

---

_Closed by @charliermarsh on 2025-06-26 16:29_

---

_Comment by @charliermarsh on 2025-06-26 16:29_

We actually do rebuild explicit paths like this now, when passed on the CLI.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-06-26 16:29_

---
