```yaml
number: 6407
title: Incorrect precedence of UV_INDEX_URL vs --index-url in reqs file
type: issue
state: open
author: wimglenn
labels:
  - documentation
  - compatibility
assignees: []
created_at: 2024-08-22T04:55:34Z
updated_at: 2025-01-17T16:12:26Z
url: https://github.com/astral-sh/uv/issues/6407
synced_at: 2026-01-12T15:59:04Z
```

# Incorrect precedence of UV_INDEX_URL vs --index-url in reqs file

---

_@wimglenn_

If an index url has been specified directly in the [requirements file](https://pip.pypa.io/en/stable/reference/requirements-file-format/#global-options) using `-i` or `--index-url`, this should have precedence over env var.

Currently uv will use UV_INDEX_URL if it's set, and ignore the one in the requirements file. Reproducer:

```
$ uv --version
uv 0.3.1 (be17d132a 2024-08-21)
$ python3 -VV
Python 3.12.4 (v3.12.4:8e8a4baf65, Jun  6 2024, 17:33:18) [Clang 13.0.0 (clang-1300.0.29.30)]
$ python3 -m venv .venv --without-pip
$ vi reqs.txt
$ cat reqs.txt 
--index-url https://test.pypi.org/simple
uv-reqs-example==0.1
$ UV_INDEX_URL=https://pypi.org/simple uv pip install -r reqs.txt
  × No solution found when resolving dependencies:
  ╰─▶ Because uv-reqs-example was not found in the package registry and you require uv-reqs-example==0.1, we can conclude that your requirements are unsatisfiable.
$ UV_INDEX_URL=https://example.org/ uv pip install -r reqs.txt 
⠴ Resolving dependencies... 
error: HTTP status server error (500 Internal Server Error) for url (https://example.org/uv-reqs-example/)
```

This is in contrast to pip, where the index url from environment is lower priority to the cmd:

```
$ uv pip install pip
Resolved 1 package in 308ms
Prepared 1 package in 573ms
Installed 1 package in 11ms
 + pip==24.2
$ PIP_INDEX_URL=https://example.org/ .venv/bin/pip install -r reqs.txt
Looking in indexes: https://test.pypi.org/simple
Collecting uv-reqs-example==0.1 (from -r reqs.txt (line 2))
  Downloading https://test-files.pythonhosted.org/packages/78/20/c8431c9645d1390acce2f2da698539f473818e77a168d135c09279dd7bf5/uv_reqs_example-0.1-py3-none-any.whl.metadata (58 bytes)
Downloading https://test-files.pythonhosted.org/packages/78/20/c8431c9645d1390acce2f2da698539f473818e77a168d135c09279dd7bf5/uv_reqs_example-0.1-py3-none-any.whl (986 bytes)
Installing collected packages: uv-reqs-example
Successfully installed uv-reqs-example-0.1
```

This makes `uv` + UV_INDEX_URL somewhat dangerous to use as a drop-in replacement for `pip` + PIP_INDEX_URL. A failure to install is not the worst scenario, the worst is that you _succeed_ to install potentially totally different packages from a totally different index (note: checksums in the reqs file could protect you from that).

---

_Comment by @charliermarsh on 2024-08-22 12:38_

Hmm, I think it's arguably unclear _what_ we should use here. I would find it more dangerous that the environment variable (which is presumedly set by the user) is ignored, and the one in a `requirements.txt` in a file was picked up (which could be from another project or similar). But I can see an argument either way. I'd rather document this as an incompatibility than change it though, I think.

---

_Label `documentation` added by @charliermarsh on 2024-08-22 12:38_

---

_Label `compatibility` added by @charliermarsh on 2024-08-22 12:38_

---

_Comment by @wimglenn on 2024-08-23 02:59_

_Below, I'm intending to convince you that retaining compatibility with pip is a better choice than documenting as an incompatibility._

Consider when there is a custom _default_ index set. This is pretty common in corporate environments, where PyPI packages may need to be cached, whitelisted or shadowed with patched versions, e.g. by using an internal [devpi-server](https://github.com/devpi/devpi/tree/main/server).

The custom index is set as the default index (for all users) by `/etc/pip.conf` file, owned by root and placed by sysadmin or config mgmt. Now, uv doesn't look at `/etc/pip.conf`, ~nor allow a global default configuration~ (no longer true since uv 0.4.26 [#7851](https://github.com/astral-sh/uv/pull/7851)), only considers the user's `~/.config/uv/uv.toml` - so this is only an option to set a default index for _pip_.

The next-best option for the sysadmin to put a sensible default index for uv would probably be setting `UV_INDEX_URL` (perhaps in `/etc/environment`, or by wrapping the uv executable).

However, if the `UV_INDEX_URL` takes precedence over the index specified by requirements files, that's an unfortunate situation - users would have to _unset_ the env var (which they might not even know about) to allow using the index which the requirements file was attempting to provide. 

As I mentioned earlier, the simple failure mode is that packages are not found, but the more confusing and hard-to-debug failure mode is if the installation succeeds but uses the wrong packages (because it was using the wrong index). Switching from `pip` to `uv pip` should not be so error-prone.

The failure mode I describe might sound like a weird edge-case, but could actually be quite common especially with devpi-server, where "team-specific" indices could inherit and mirror a default index, to host a mix of their own/patched local versions layered over the default versions. 

> ... and the one in a `requirements.txt` in a file was picked up (which could be from another project or similar).

I'm not sure what you mean here? Unless I'm mistaken, it is part of the [requirements file format](https://pip.pypa.io/en/stable/reference/requirements-file-format/#global-options) that the file can control the index-url. This is not dangerous, because requirements files and their custom indices can not be used during dependency resolving from some other top-level installation candidate. The requirements file would have been installed directly with `-r`. At least, it's not any more dangerous than a [direct url requirement](https://peps.python.org/pep-0508/#examples), as specified in PEP 508.

---

_Comment by @sebastianw on 2025-01-17 16:11_

I'm currently faced with this problem. I would love to specify index rules in the pyproject.toml file and completely ignore --index and --extra-index definitions in `requirements.txt` as these are provided partially by external projects (in this case external Ansible collections).

We use uv like this:

```
uv pip compile -U --generate-hashes --output-file requirements.txt requirements.in collections/ansible_collections/*/*/requirements.txt -n --no-strip-extras --emit-index-url --index-strategy unsafe-best-match --emit-index-annotation
```

All external Ansible collections are ina `collections/ansible_collections` and we add all their `requirement.txt` to our pinned dependencies. `--index-strategy unsafe-best-match` is something I would like to avoid.

I have two internal projects that should be looked up ONLY internally (in two different indexes) and the rest on pypi. 

---
