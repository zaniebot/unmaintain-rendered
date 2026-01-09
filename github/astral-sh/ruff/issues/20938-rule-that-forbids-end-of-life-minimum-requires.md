---
number: 20938
title: "Rule that forbids end-of-life minimum `requires-python` version in `pyproject.toml`"
type: issue
state: open
author: Jayman2000
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-10-17T12:32:00Z
updated_at: 2025-10-27T12:26:01Z
url: https://github.com/astral-sh/ruff/issues/20938
synced_at: 2026-01-07T13:12:16-06:00
---

# Rule that forbids end-of-life minimum `requires-python` version in `pyproject.toml`

---

_Issue opened by @Jayman2000 on 2025-10-17 12:32_

### Summary

Let’s say that I have a `pyproject.toml` file that looks like this:

```toml
[project]
name = "foo"
version = "0.dev0"
requires-python = ">=3.7"
```

[Python 3.7 has reached end of life](https://devguide.python.org/versions) so the `requires-python` field should really be updated. It would be nice if there was a ruff rule that would give you an error when your `pyproject.toml` file has an end-of-life minimum Python version.

---

_Referenced in [Jayman2000/jasons-pre-commit-hooks#64](../../Jayman2000/jasons-pre-commit-hooks/issues/64.md) on 2025-10-17 12:42_

---

_Comment by @ntBre on 2025-10-17 12:52_

Ah that's interesting! I think this would be our first configuration-related diagnostic other than [invalid-pyproject-toml (RUF200)](https://docs.astral.sh/ruff/rules/invalid-pyproject-toml/#invalid-pyproject-toml-ruf200).

---

_Label `rule` added by @ntBre on 2025-10-17 12:52_

---

_Label `needs-decision` added by @ntBre on 2025-10-17 12:52_

---

_Comment by @amyreese on 2025-10-21 00:28_

I think this could be interesting/useful, but I'd expect the EOL threshold to be pinned in the UV rule/build, so that CI for projects doesn't suddenly start failing with no other changes, just because the EOL date has passed. Maybe just base it on the minimum/default version that Ruff supports, so it only starts complaining when projects update their (hopefully) pinned version of Ruff to the next minor release after the EOL date?

---

_Comment by @Jayman2000 on 2025-10-27 12:26_

> I think this could be interesting/useful, but I'd expect the EOL threshold to be pinned in the UV rule/build, so that CI for projects doesn't suddenly start failing with no other changes, just because the EOL date has passed.

I like this idea. I use [Nix Flake Checker](https://github.com/DeterminateSystems/flake-checker) to do a similar check (Nix Flake Checker will give you an error if you have a [Nix flake](https://nix.dev/concepts/flakes) that uses an EOL [Nixpkgs](https://github.com/NixOS/nixpkgs/) branch). [Nix Flake Checker has a hardcoded list of Nixpkgs branches and their support statuses](https://github.com/DeterminateSystems/flake-checker/blob/v0.2.8/ref-statuses.json) so you won’t get errors about using an EOL Nixpkgs branch until after you have updated Nix Flake Checker. This makes it easier for me to have a clean commit history (I can switch to a different Nixpkgs branch in one commit, and update Nix Flake Checker in a different commit). Additionally, having a hardcoded list means that Nix Flake Checker doesn’t have to make an additional network request every time it runs in order to check if a Nixpkgs branch is EOL yet.

In short, I’ve used another tool that does something similar to what you’re suggesting, and I like the way that that tool works. I think that it would be good if Ruff worked that way.

> Maybe just base it on the minimum/default version that Ruff supports, so it only starts complaining when projects update their (hopefully) pinned version of Ruff to the next minor release after the EOL date?

This might have some unintended consequences. [According to Ruff’s `pyproject.toml` file, the minimum version of Python that Ruff supports is Python 3.7.](https://github.com/astral-sh/ruff/blob/db0e921db1466f8d23e28c27eaba19983a4367f0/pyproject.toml#L11) If we were to implement this rule idea right now and base it on Ruff’s minimum Python version, then Ruff wouldn’t give any errors when encountering `pyproject.toml` files that allow Python versions 3.7 and 3.8 despite the fact that [those two versions are EOL](https://devguide.python.org/versions). We could update Ruff’s `pyproject.toml` file so that it says `requires-python = ">=3.9"`, but then people who pin their Ruff version in their [`uv.lock`](https://docs.astral.sh/uv/guides/projects/#uvlock) file would never see the error about using an EOL Python version. I figured this out by creating this `pyproject.toml` file:

```toml
[project]
name = "uv-requires-python-test"
version = "0.dev0"
requires-python = ">=3.6"
dependencies = [ "ruff" ]
```

When I run `uv lock --upgrade` with that `pyproject.toml` file, it locks Ruff’s version to 0.0.17 because that’s the latest version of Ruff that supports Python 3.6. If I change the `pyproject.toml` file to this…

```toml
[project]
name = "uv-requires-python-test"
version = "0.dev0"
requires-python = ">=3.7"
dependencies = [ "ruff" ]
```

…and rerun `uv lock --upgrade`, then it locks Ruff’s version to 0.14.2. This means that if someone has pinned their version of Ruff using a `uv.lock` file, then they will only get newer versions of Ruff if their minimum Python version isn’t lower than Ruff’s minimum Python version. If Ruff’s minimum Python version needs to be increased in order for these errors to start appearing, then users who pin their version of Ruff using a `uv.lock` file will never see these errors.

---
