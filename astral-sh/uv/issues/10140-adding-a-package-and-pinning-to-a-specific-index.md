---
number: 10140
title: "Adding a package and pinning to a specific index by name only with `uv add`"
type: issue
state: open
author: menzenski
labels:
  - enhancement
assignees: []
created_at: 2024-12-24T12:50:10Z
updated_at: 2026-01-06T18:56:19Z
url: https://github.com/astral-sh/uv/issues/10140
synced_at: 2026-01-10T01:24:50Z
---

# Adding a package and pinning to a specific index by name only with `uv add`

---

_Issue opened by @menzenski on 2024-12-24 12:50_

This issue is something of a followup to https://github.com/astral-sh/uv/issues/6582, which was a duplicate of https://github.com/astral-sh/uv/issues/171. That issue has comments https://github.com/astral-sh/uv/issues/171#issuecomment-2426757234 and https://github.com/astral-sh/uv/issues/171#issuecomment-2426403030 which seems to be the same thing I am interested in here (but I can't find anything in search that suggests those have been opened as independent issues yet).

### Desired Behavior

Given an index definition named `my_artifactory` in `pyproject.toml`:

```toml
[[tool.uv.index]]
name = "my_artifactory"
url = "https://myorg.jfrog.io/artifactory/api/pypi/pypi/simple"
explicit = true
```

I would like to be able to run `uv add my-private-package --index my_artifactory` (or similar command - the important part here is that the CLI command references an index by name only, not by name and URL), with the resulting behavior that the package has been added and pinned to that specific index. Desired output in `pyproject.toml`:

```toml
dependencies = [
    "my-private-package>=0.18.0",
]

[tool.uv.sources]
my-private-package = { index = "my_artifactory" }
```

### Current Workaround

My current workaround is to add the `tool.uv.sources` entry first, manually in the `pyproject.toml` file:

```toml
[tool.uv.sources]
my-private-package = { index = "my_artifactory" }
```

Then I can run `uv add my-private-package`. But I would prefer not to be manually updating `pyproject.toml` for this - it would be nice to be able to do all "add/update dependency" tasks via the `uv` CLI.

### Comparison to Poetry

I am trying to migrate from Poetry to uv. With Poetry, I can specify an explicit source in `pyproject.toml`:

```toml
[[tool.poetry.source]]
name = "my_artifactory"
url = "https://myorg.jfrog.io/artifactory/api/pypi/pypi/simple"
priority = "explicit"
```

and add a dependency using `poetry add` with a `--source my_artifactory` option, e.g.:

```shell
$ poetry add my-private-package --source my_artifactory
```

The result of this command is a package that is pinned to that specific source:

```toml
[tool.poetry.dependencies]
my-private-package = {version = "^0.18.0", source = "my_artifactory"}
```

I am hoping that the same behavior is possible with `uv`.

---

_Label `enhancement` added by @charliermarsh on 2024-12-24 13:05_

---

_Comment by @charliermarsh on 2024-12-24 13:05_

I agree that this should work.

---

_Comment by @vmgustavo on 2025-01-15 14:41_

Yes please. It is a great feature for whoever uses private repositories a lot.

---

_Assigned to @Gankra by @Gankra on 2025-01-15 15:28_

---

_Referenced in [astral-sh/uv#12796](../../astral-sh/uv/issues/12796.md) on 2025-04-10 14:03_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-04-10 14:11_

---

_Unassigned @Gankra by @jtfmumm on 2025-04-10 14:11_

---

_Comment by @charliermarsh on 2025-04-10 14:15_

If it's easier to do this with a separate argument (like `--index-name`), I'm fine with that.

---

_Comment by @charliermarsh on 2025-04-10 14:15_

(As in, using `uv add foo --index-name bar` instead of `uv add foo --index https://...`, when `bar` already exists.)

---

_Comment by @zanieb on 2025-04-10 15:59_

Don't we accept `--index <name>` elsewhere? or am I misremembering?

---

_Comment by @Gankra on 2025-04-10 17:37_

The complexity/issue here is that `uv add` already takes `--index`, by inheriting (flattening) in `ResolverInstallerArgs`, which inherits `IndexArgs` which actually defines the flag. That version of the index flag is used by tons of different commands, and since it's currently always a URL it's eagerly parsed/resolved at CLI parsing time and everything expects to be able to pass it directly to stuff that wants indexes.

So adding support for this special "lazily resolved by name" index mode forces tons of other commands to suddenly contend with having to also implement this lazy resolving or at least adding a "not supported" check/error.

`uv publish` *does* take an index by name but it doesn't used `IndexArgs` so it doesn't have this extra splash-work to do.

I started implementing this, but backed off once the combinatoric explosion of things that needed to be updated started to reveal itself to me.

---

_Referenced in [astral-sh/uv#13445](../../astral-sh/uv/issues/13445.md) on 2025-05-14 15:57_

---

_Comment by @jamesowers-cohere on 2025-08-01 10:48_

Just to add to this. There's currently something that's quite confusing when using `uv pip install` or `uv tool install` (this relates more to the linked other issue: https://github.com/astral-sh/uv/issues/12796). 

I have a `/etc/uv/uv.toml` containing some index definitions, e.g.:

```
keyring-provider = "subprocess"

[[index]]
name = "my-index-py"
url = "https://oauth2accesstoken@.../my-index-py/simple"
publish-url = "https://oauth2accesstoken@.../my-index-py"
explicit = true

[[index]]
name = "my-index-py-test"
url = "https://oauth2accesstoken@.../my-index-py-test/simple"
publish-url = "https://oauth2accesstoken@.../my-index-py-test"
explicit = true
```

When I execute

```shell
uv tool install --index my-index-py ...
```

or

```shell
uv pip install --index my-index-py ...
```

**no error is returned**. Even when I use `-vv` to get more verbose output, it is very unclear to see that the `--index my-index-py` is just being ignored. This is particularly confusing in the context when there is a package in both `my-index-py` and `pypi` with the same name. **I will get the one from `pypi` and no error will be thrown**. This is a bit of a security concern for me!

In the interim whilst this feature is considered/developed, please may we raise an error if a user specifies `--index ...` and that index is not able to be used?

---

_Comment by @zanieb on 2025-08-01 13:34_

Is there not a warning as added in https://github.com/astral-sh/uv/pull/14152 ?

---

_Comment by @pzagieboylo on 2025-09-05 20:46_

Is there any movement on this one? I've just discovered I wanted this too. My situation: I have a package only available from a private repository. I already have this private repository defined in `tool.uv.index`, marked `explicit = true`. However, I'm also doing local development on this package, so I want the current project to pick up the local version temporarily, but with an easy way to set it back to the private repository once I've published whatever changes I made in the dependency package (to make sure that the version I actually published still works correctly). 

I tried doing this with `uv pip install --editable ./path/to/local/package`, which works very temporarily, but `uv sync` automatically pops me back to the "official" version from the repository (and my project is otherwise set up to `uv sync` QUITE OFTEN, so this solution doesn't work well). I can instead use `uv add --editable ./path/to/local/package`, which changes the source in `tool.uv.sources` more permanently, but now I have no easy way to set it back to what it should be! I eventually stumbled on `uv add package --index my-index=https://url/to/private/repository`, which seems to fix `pyproject.toml`, but even `uv sync` still leaves my actual site-packages using the reference instead of downloading the real version from the repository, although I think this might just be a version resolution conflict. (It also insists on moving the `tool.uv.index` section to be before `tool.uv` anytime I mention an explicit index; not sure why this is.) I only get the actual result I want if I `uv remove package` first, which seems excessive, or by going into `pyproject.toml` manually and changing the entry in `tool.uv.sources` back to what it should be.

What I really want, when I'm ready to go back to the official version, is to be able to say `uv add 'package>=1.0.next' --index-name my-index` (or whatever the proposed syntax is) and have it: 1) stop using the local version; 2) update the requirement in `project.dependencies`; 3) retrieve and install the published version; and 4) note in `tool.uv.sources` that it did so.

Am I actually just hoist on a cross of my own construction, and there's a better way that I should be doing this whole thing?

---

_Comment by @justinTM on 2026-01-06 18:42_

> Is there not a warning as added in [#14152](https://github.com/astral-sh/uv/pull/14152) ?

yup:

```console
❯ /home/ec2-user/.local/bin/uv add --active --refresh mypkg --index idx
warning: Relative paths passed to `--index` or `--default-index` should be disambiguated from index names (use `./idx`). Support for ambiguous values will be removed in the future
```

here's the nasty workaround to avoid future breakage:

```bash
get_uv_index_url_from_pyproject() {
  local index_name="$1"
  local pyproject_file="${_cwd}/../pyproject.toml"

  [ -z "$index_name" ] && return 1
  [ ! -f "$pyproject_file" ] && return 1

  python3 - "$index_name" "$pyproject_file" <<'PY' || exit_status=$?
import sys, tomllib
name = sys.argv[1]
pyproject = sys.argv[2]
with open(pyproject, "rb") as fh:
    data = tomllib.load(fh)
for entry in data.get("tool", {}).get("uv", {}).get("index", []):
    if entry.get("name") == name and entry.get("url"):
        print(entry["url"])
        raise SystemExit(0)
raise SystemExit(1)
PY
  return "${exit_status:-0}"
}
```

```console
❯ get_uv_index_url_from_pyproject idx
https://git.services.<host>/api/v4/projects/<id>/packages/pypi/simple
```

when it should be `uv add mypkg --index idx`

---

_Unassigned @jtfmumm by @konstin on 2026-01-06 18:56_

---
