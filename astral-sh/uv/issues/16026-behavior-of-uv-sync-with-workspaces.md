```yaml
number: 16026
title: "Behavior of \"uv sync\" with workspaces"
type: issue
state: closed
author: maxime-tremblay-hq
labels:
  - question
assignees: []
created_at: 2025-09-25T15:02:20Z
updated_at: 2025-11-01T20:03:12Z
url: https://github.com/astral-sh/uv/issues/16026
synced_at: 2026-01-12T16:02:21Z
```

# Behavior of "uv sync" with workspaces

---

_@maxime-tremblay-hq_

### Question

I am trying to use workspaces to manage a large project as described here https://docs.astral.sh/uv/concepts/projects/workspaces/

From the documentation 

> In a workspace, each package defines its own pyproject.toml, but the workspace shares a single lockfile, ensuring that the workspace operates with a consistent set of dependencies.
As such, uv lock operates on the entire workspace at once, while uv run and uv sync operate on the workspace root by default, though both accept a --package argument, allowing you to run a command in a particular workspace member from any workspace directory.

However, this is not the behavior that I observe. For example, consider this script that basically create a workspace with a single package and run a few `uv sync` in different locations.

```bash
log () {
  echo ""
  echo "$1"
  echo ""
  uv pip list
  echo ""
}

mkdir demo
cd demo
uv init . -q

uv add pytest -q

log "From workspace root before creating packages" 

mkdir packages
cd packages

uv init --lib some_package -q
cd some_package
uv add numpy -q

log "From some_package before syncing" 

uv sync -q

log "From some_package after syncing" 

cd ../..

log "From workspace root before syncing" 

uv sync -q

log "From workspace root after syncing" 
```

Running the script, I get the following output

```
From workspace root before creating packages

Package   Version
--------- -------
iniconfig 2.1.0
packaging 25.0
pluggy    1.6.0
pygments  2.19.2
pytest    8.4.2


From some_package before syncing

Package      Version Editable project location
------------ ------- -------------------------------------------------------------
iniconfig    2.1.0
numpy        2.3.3
packaging    25.0
pluggy       1.6.0
pygments     2.19.2
pytest       8.4.2
some-package 0.1.0   /mnt/automountdir/gpfs/home/dn3868/demo/packages/some_package


From some_package after syncing

Package      Version Editable project location
------------ ------- -------------------------------------------------------------
numpy        2.3.3
some-package 0.1.0   /mnt/automountdir/gpfs/home/dn3868/demo/packages/some_package


From workspace root before syncing

Package      Version Editable project location
------------ ------- -------------------------------------------------------------
numpy        2.3.3
some-package 0.1.0   /mnt/automountdir/gpfs/home/dn3868/demo/packages/some_package


From workspace root after syncing

Package   Version
--------- -------
iniconfig 2.1.0
packaging 25.0
pluggy    1.6.0
pygments  2.19.2
pytest    8.4.2
```

As you can see, the `uv sync` command seems to operate differently if I am at the root or inside a package. This seems to contradict the documentation. 

Am I missing something? Is this a bug? Maybe I don't understand the documentation.

### Platform

Rocky linux 8

### Version

0.8.16

---

_Label `question` added by @maxime-tremblay-hq on 2025-09-25 15:02_

---

_Comment by @zanieb on 2025-09-26 00:50_

Yeah we will infer `--package <member>` as the default when your working directory is that member. You can still use `--package` to select another member (or even the root package). We can consider tweaking the wording in the documentation.

---

_Comment by @maxime-tremblay-hq on 2025-09-26 13:29_

Ok. Thanks for the clarification. 

What is the best practice to include dev dependencies like pytest that are shared accross all packages? Should I include them in all my pyproject.toml files? Same for stuff like ruff configuration that I want to apply to the whole project.

---

_Comment by @charliermarsh on 2025-11-01 20:03_

We generally specify them in each workspace member (so, in each `pyproject.toml` file).

---

_Closed by @charliermarsh on 2025-11-01 20:03_

---
