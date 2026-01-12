```yaml
number: 13304
title: "\"uv download\" to create local directory of distributions + ability to install from the same"
type: issue
state: open
author: mdekstrand
labels:
  - enhancement
assignees: []
created_at: 2025-05-05T16:20:30Z
updated_at: 2025-12-12T04:13:21Z
url: https://github.com/astral-sh/uv/issues/13304
synced_at: 2026-01-12T16:01:24Z
```

# "uv download" to create local directory of distributions + ability to install from the same

---

_@mdekstrand_

### Summary

I am looking for options to archive my dependencies' distributions, primarily for bundling reproducible scientific workflows for archival and distribution.

I see some discussion about functionality like `venv-pack` or `conda-pack` (#8653, #6970), but they seem to be primarily about archiving the extracted virtual environment.

I think another solution could provide a lot of the same benefits, including supporting airgapped installs, without the need for solving the various arcane problems involved in bundling and relocating entire virtual environments: the ability to bundle the _distributions_, optionally also including the wheels built from sdists. The basic idea has two parts:

1. A command, maybe `uv download` or a new option for `uv export`, that copies the distributions from the cache into a specified directory, for either the current platform or all locked platforms.
2. An option to `uv sync` that uses the files from the exported directory (possibly just pre-seeding them into the cache).

### Example

Preparing the archive:

```console
$ uv download --all-platforms -o dist/dependencies/
248 distributions downloaded
```

Installing from archive:

```console
$ uv sync --dist-archive dist/dependencies
```

---

_Label `enhancement` added by @mdekstrand on 2025-05-05 16:20_

---

_Comment by @zanieb on 2025-05-05 16:24_

Related

- https://github.com/astral-sh/uv/issues/3163

---

_Comment by @tingerrr on 2025-11-25 09:58_

I think the downloaded archive would be in structure the same as a [flat index](https://docs.astral.sh/uv/concepts/indexes/#flat-indexes), so, all that's needed for installing it is a flat index pointing at that directory, right? Can a flat index be specified on the CLI?

Separately, as noted in the linked `pip downlaod` discussion, is it perhaps more appropriate to provide the equivalent of `pip wheel` (or at least a flag for it) since it builds wheels for packages which don't come with a pre-built one.

---

_Comment by @euank on 2025-12-12 04:13_

Currently, you can sorta create such a directory with:

(note using [tomlq](https://github.com/cryptaliagy/tomlq) and jq)
```
indexDir=$(mktemp -d)
tq -f uv.lock --output json '.' | jq '.. | objects | .url | select(. != null)' -cr | \
    xargs curl -sL --output-dir "$indexDir" --create-dirs --remote-name-all
```

This misses build deps (https://github.com/astral-sh/uv/issues/5190), but otherwise seems to work okay with `--find-links` and `uv pip install` I think.

I think it'd be nice for `uv` itself to support something like this though since it feels a little wrong to parse through `uv.lock` like that, and I wouldn't be surprised if there's edge cases this misses.

The above's what I ended up with over in [nixpkgs's anki pkg](https://github.com/NixOS/nixpkgs/blob/a816e8e506c106d297dd89a2c4807e39628f7b41/pkgs/by-name/an/anki/update.sh#L64-L66) (and used [here](https://github.com/NixOS/nixpkgs/blob/a816e8e506c106d297dd89a2c4807e39628f7b41/pkgs/by-name/an/anki/package.nix#L186)), so that hack did work in at least one case 😀

---
