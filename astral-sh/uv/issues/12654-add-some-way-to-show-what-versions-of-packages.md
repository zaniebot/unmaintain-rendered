```yaml
number: 12654
title: "Add some way to show what versions of packages were actually used for a `uv run` invocation"
type: issue
state: open
author: cjw296
labels:
  - enhancement
assignees: []
created_at: 2025-04-03T14:52:10Z
updated_at: 2025-06-05T14:26:51Z
url: https://github.com/astral-sh/uv/issues/12654
synced_at: 2026-01-12T16:01:09Z
```

# Add some way to show what versions of packages were actually used for a `uv run` invocation

---

_@cjw296_

### Summary

`uv run` has a number of options that affect package resolution and what versions of packages actually end up getting used, which may differ from the normal project environment, for example, from a CI run of mine:

- [highest deps](https://github.com/toirl/pytest-sqlalchemy/actions/runs/14237273196/job/39898942970)
- [lowest direct deps](https://github.com/toirl/pytest-sqlalchemy/actions/runs/14237273196/job/39898942964)

Stuff I've tried already:

- `uv run -v --all-extras --dev $WITH --resolution $RESOLUTION ...` does include the information I'm looking for:

```
Prepared 11 packages in 1.27s
Installed 11 packages in 6ms
 + coverage==7.8.0
 + execnet==2.1.1
 + greenlet==3.1.1
 + iniconfig==2.1.0
 + packaging==24.2
 + pluggy==1.5.0
 + pytest==8.0.0
 + pytest-sqlalchemy==0.2.0 (from file:///home/runner/work/pytest-sqlalchemy/pytest-sqlalchemy)
 + pytest-xdist==3.6.1
 + sqlalchemy==1.4.0
 + sqlalchemy-utils==0.41.0
```

But there's so much other debug logging that it's pretty unusable.

- `uv tree` provides what would potentially be ideal, but it doesn't support `--with` or `--all-extras`.

### Example

Either or both of:

```
uv run --show-tree --all-extras --dev $WITH --resolution $RESOLUTION ...
Resolved 11 packages in ...ms
 + coverage==7.8.0
...
 + sqlalchemy-utils==0.41.0
... run output here...
```

Make sure `uv tree` supports the same options as `uv sync` and `uv run`
```
uv tree --all-extras --dev $WITH --resolution $RESOLUTION
```

---

_Label `enhancement` added by @cjw296 on 2025-04-03 14:52_

---

_Comment by @janosh on 2025-06-05 14:04_

i have a very similar suggestion (happy to open a new issue for it if appropriate). as @cjw296 points out, running `uv run script.py` in CI shows the download size of each package but not the (arguably more important) package version. [example output](https://github.com/Radical-AI/torch-sim/actions/runs/15468359275/job/43545931499?pr=206):

```bash
uv run --with . examples/scripts/5_Workflow/5.1_a2c_silicon_batched.py
  shell: /usr/bin/bash -e {0}
  env:
    pythonLocation: /opt/hostedtoolcache/Python/3.11.12/x64
    PKG_CONFIG_PATH: /opt/hostedtoolcache/Python/3.11.12/x64/lib/pkgconfig
    Python_ROOT_DIR: /opt/hostedtoolcache/Python/3.11.12/x64
    Python2_ROOT_DIR: /opt/hostedtoolcache/Python/3.11.12/x64
    Python3_ROOT_DIR: /opt/hostedtoolcache/Python/3.11.12/x64
    LD_LIBRARY_PATH: /opt/hostedtoolcache/Python/3.11.12/x64/lib
    UV_CACHE_DIR: /home/runner/work/_temp/setup-uv-cache
Downloading nvidia-cufft-cu12 (190.9MiB)
Downloading scipy (35.9MiB)
Downloading numpy (16.0MiB)
Downloading plotly (15.5MiB)
Downloading pandas (11.8MiB)
Downloading sympy (6.0MiB)
...
```

i think the default should be changed to showing resolved package versions instead of download size (or both). ideally, there should also be a way for users to control what package info gets printed (version, size, maybe reason why version was chosen, resolved download URL, ...)

---

_Comment by @janosh on 2025-06-05 14:09_

related issue that would also be closed by printing package version with `uv run` (in CI) https://github.com/astral-sh/uv/issues/13137

---

_Comment by @konstin on 2025-06-05 14:26_

One workaround for that option currently not existing is this three step hack:

```
uv sync --extra ... --group ... # Pass the same arguments as you would to `uv run`
uv pip list # Show the packages and their versions
uv run --no-sync ...
```

---
