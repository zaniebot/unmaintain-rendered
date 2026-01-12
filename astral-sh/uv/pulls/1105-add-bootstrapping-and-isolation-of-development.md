```yaml
number: 1105
title: Add bootstrapping and isolation of development Python versions
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/bootstrap-python
created_at: 2024-01-25T19:48:05Z
updated_at: 2024-01-26T18:12:49Z
url: https://github.com/astral-sh/uv/pull/1105
synced_at: 2026-01-12T16:04:26Z
```

# Add bootstrapping and isolation of development Python versions

---

_@zanieb_

Replaces https://github.com/astral-sh/puffin/pull/1068 and #1070 which were more complicated than I wanted.

- Introduces a `.python-versions` file which defines the Python versions needed for development
- Adds a Bash script at `scripts/bootstrap/install` which installs the required Python versions from `python-build-standalone` to `./bin`
- Checks in a `versions.json` file with metadata about available versions on each platform and a `fetch-version` Python script derived from `rye` for updating the versions
- Updates CI to use these Python builds instead of the `setup-python` action
- Updates to the latest packse scenarios which require Python 3.8+ instead of 3.7+ since we cannot use 3.7 anymore and includes new test coverage of patch Python version requests
- Adds a `PUFFIN_PYTHON_PATH` variable to prevent lookup of system Python versions for isolation during development

Tested on Linux (via CI) and macOS (locally) — presumably it will be a bit more complicated to do proper Windows support.

---

_Label `testing` added by @zanieb on 2024-01-25 20:25_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/interpreter.rs`:235 on 2024-01-25 20:31_

I'd like to improve the error handling here in a follow-up

---

_@zanieb reviewed on 2024-01-25 20:31_

---

_@zanieb reviewed on 2024-01-25 20:32_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/lib.rs`:68 on 2024-01-25 20:32_

It's unclear to me if there's a better way to display this

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/python_query.rs`:33 on 2024-01-25 20:32_

See #1097 — these require each other because we cannot add tests over there until the versions are pinned as written here.

---

_@zanieb reviewed on 2024-01-25 20:32_

---

_@zanieb reviewed on 2024-01-25 20:33_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_compile_scenarios.rs`:257 on 2024-01-25 20:33_

This package had an invalid tag name and was failing to publish in the last couple packse builds.

---

_@zanieb reviewed on 2024-01-25 20:33_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_compile_scenarios.rs`:336 on 2024-01-25 20:33_

The following is new test coverage from packse for requests that include patch Python versions.

---

_Review comment by @zanieb on `scripts/bootstrap/fetch-versions`:1 on 2024-01-25 20:35_

This is a very simplified version of #1068 and the vast majority of it is identical to the Python script Rye uses to inline some Python version metadata.

---

_@zanieb reviewed on 2024-01-25 20:35_

---

_@zanieb reviewed on 2024-01-25 20:36_

---

_Review comment by @zanieb on `scripts/scenarios/update.py`:152 on 2024-01-25 20:36_

This fixes a minor bug where if you run the scenario update with a different Python version (without actually activating a virtual environment) `packse` would be missing from the path.

---

_@zanieb reviewed on 2024-01-25 20:37_

---

_Review comment by @zanieb on `scripts/bootstrap/install`:1 on 2024-01-25 20:37_

This is a bash implementation of #1068 such that we do not require any Python versions to be available to download Python.

---

_@zanieb reviewed on 2024-01-25 20:37_

---

_Review comment by @zanieb on `scripts/bootstrap/install`:1 on 2024-01-25 20:37_

I linted this with `shellcheck`

---

_@zanieb reviewed on 2024-01-25 20:38_

---

_Review comment by @zanieb on `scripts/bootstrap/install`:22 on 2024-01-25 20:38_

Please test me on your system!

---

_Renamed from "Add simple bootstrapping of Python versions" to "Add bootstrapping and isolation of Python versions" by @zanieb on 2024-01-25 20:39_

---

_Renamed from "Add bootstrapping and isolation of Python versions" to "Add bootstrapping and isolation of development Python versions" by @zanieb on 2024-01-25 20:39_

---

_@zanieb reviewed on 2024-01-25 21:26_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/interpreter.rs`:223 on 2024-01-25 21:26_

I worry about this name being too close to `PYTHONPATH` which has a very different meaning.

Considering `PUFFIN_PYTHON_EXECUTABLE_PATH` but geesh.

---

_Review comment by @zanieb on `.envrc`:3 on 2024-01-25 21:27_

A little hesitant to check this in, but `direnv` is a pretty nice way to manage these standards and there's no _requirement_ that you use it.

---

_@zanieb reviewed on 2024-01-25 21:27_

---

_Review requested from @charliermarsh by @zanieb on 2024-01-25 21:41_

---

_Review requested from @BurntSushi by @zanieb on 2024-01-25 21:41_

---

_Review requested from @konstin by @zanieb on 2024-01-25 21:41_

---

_Review comment by @charliermarsh on `crates/puffin-interpreter/src/python_query.rs`:171 on 2024-01-25 22:18_

I like backticks to make clear that it's a literal, personally.

---

_Review comment by @charliermarsh on `crates/puffin-interpreter/src/python_query.rs`:46 on 2024-01-25 22:18_

Should this return an error? This is effectively a panic.

---

_Review comment by @charliermarsh on `scripts/bootstrap/install`:1 on 2024-01-25 22:18_

Can we use `install.sh` instead of `install`?

---

_Review comment by @charliermarsh on `scripts/bootstrap/install`:1 on 2024-01-25 22:21_

Wow respect. _This_ I could see being written in Rust to avoid the Python dependency... but, now that this is here, let's give it a whirl :)

---

_Review comment by @charliermarsh on `scripts/bootstrap/install`:47 on 2024-01-25 22:21_

Wtf?

---

_Review comment by @charliermarsh on `scripts/bootstrap/fetch-versions`:1 on 2024-01-25 22:22_

In that case, I'm gonna skip reading this.

---

_Review comment by @charliermarsh on `scripts/bootstrap/fetch-versions`:49 on 2024-01-25 22:22_

One question though - does this download one interpreter per version? Or multiple? How does it choose which one?

---

_Review comment by @charliermarsh on `scripts/bootstrap/install`:29 on 2024-01-25 22:23_

I thought you needed `-o` for pipefail, like `set -euo pipefail`?

---

_Review comment by @charliermarsh on `scripts/bootstrap/install`:101 on 2024-01-25 22:23_

I don't know that I feel qualified to review this.

---

_Review comment by @charliermarsh on `CONTRIBUTING.md`:52 on 2024-01-25 22:24_

We recommend setting it _during development_, right?

---

_@charliermarsh approved on 2024-01-25 22:24_

Very impressive.

---

_@charliermarsh reviewed on 2024-01-25 22:25_

---

_Review comment by @charliermarsh on `crates/puffin-interpreter/src/interpreter.rs`:223 on 2024-01-25 22:25_

I'm okay with `PUFFIN_PYTHON_EXECUTABLE_PATH`.

---

_@charliermarsh reviewed on 2024-01-25 22:25_

---

_Review comment by @charliermarsh on `.envrc`:3 on 2024-01-25 22:25_

Happy to try it. Why does one have `export` but not the other? Is it intentional?

---

_@charliermarsh reviewed on 2024-01-25 22:27_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yml`:73 on 2024-01-25 22:27_

Do we need to set `PUFFIN_PYTHON_PATH` here, or it's fine because there are no "other Pythons" on the server?

---

_@zanieb reviewed on 2024-01-25 22:41_

---

_Review comment by @zanieb on `scripts/bootstrap/install`:1 on 2024-01-25 22:41_

We should definitely write this in Rust in the future. I found Rye's implementation to be relatively complicated though (it seemed hard to copy over) and this was a smaller scope than doing it in Rust (for me haha)

---

_@zanieb reviewed on 2024-01-25 22:42_

---

_Review comment by @zanieb on `.envrc`:3 on 2024-01-25 22:42_

I don't think `direnv` requires an export of `PATH` they special case it? Not sure what their intent is there.

---

_@zanieb reviewed on 2024-01-25 22:43_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/python_query.rs`:171 on 2024-01-25 22:43_

This is a side-effect of switching to the debug implementation for rendering. `OsString` doesn't implement display. Suggestions?

---

_@zanieb reviewed on 2024-01-25 22:43_

---

_Review comment by @zanieb on `scripts/bootstrap/install`:1 on 2024-01-25 22:43_

I don't love extensions on executables with shebangs but sure.

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/python_query.rs`:46 on 2024-01-25 22:44_

Probably, yes. I'm not sure how soon Konsti is going to write Windows version lookup using the registry which would (I think) support this. Happy to do it here though.

---

_@zanieb reviewed on 2024-01-25 22:44_

---

_@zanieb reviewed on 2024-01-25 22:47_

---

_Review comment by @zanieb on `scripts/bootstrap/install`:47 on 2024-01-25 22:47_

macOS has super old versions of GNU core utilities. You can get the latest with `brew install coreutils` but it installs them all as `g<cmd>` to avoid collisions. Pretty silly. I'll make the comment a little better haha.

---

_@zanieb reviewed on 2024-01-25 22:47_

---

_Review comment by @zanieb on `scripts/bootstrap/install`:29 on 2024-01-25 22:47_

Ah that explains why it didn't work! Oops.

---

_@zanieb reviewed on 2024-01-25 22:47_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:52 on 2024-01-25 22:47_

Yes. I can clarify

---

_@zanieb reviewed on 2024-01-25 22:47_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:73 on 2024-01-25 22:47_

There are no other Pythons on the server. It may be best practice to set it here too? I'm not sure if it matters.

---

_@charliermarsh reviewed on 2024-01-25 22:50_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yml`:73 on 2024-01-25 22:50_

I think it's fine not to set.

---

_@zanieb reviewed on 2024-01-25 22:50_

---

_Review comment by @zanieb on `scripts/bootstrap/fetch-versions`:49 on 2024-01-25 22:50_

This just generates the `version.json` file which has links to downloads. I believe the flavor preferences section here is used to select a single interpreter flavor _per_ combination of OS, architecture, and Python version.

---

_@zanieb reviewed on 2024-01-25 22:56_

---

_Review comment by @zanieb on `scripts/bootstrap/install`:101 on 2024-01-25 22:56_

As long as you test it I'm not _too_ worried about the implementation.

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/interpreter.rs`:223 on 2024-01-25 22:58_

I'll address this once I merge this full stack to avoid annoying rebases

---

_@zanieb reviewed on 2024-01-25 22:58_

---

_Review comment by @konstin on `CONTRIBUTING.md`:57 on 2024-01-26 13:01_

Shouldn't this be something like

```
PATH=$PWD/bin:$PATH PUFFIN_PYTHON_PATH=$PWD/bin cargo nextest run ...
```

Seems cumbersome to always having to copy those two commands before doing anything when not using direnv

---

_Review comment by @konstin on `crates/puffin-interpreter/src/interpreter.rs`:223 on 2024-01-26 13:02_

"path" is so overloaded anyway already

---

_Review comment by @konstin on `crates/puffin-interpreter/src/lib.rs`:68 on 2024-01-26 13:04_

If it's about thiserror syntax, `#[error("Couldn't find {} in PATH", _0.to_string_lossy())]`.



---

_Review comment by @konstin on `crates/puffin-interpreter/src/python_query.rs`:35 on 2024-01-26 13:04_

The function docstring needs to be updated

---

_Review comment by @konstin on `crates/puffin-interpreter/src/python_query.rs`:46 on 2024-01-26 13:05_

I'd go with an error for now

---

_Review comment by @konstin on `scripts/bootstrap/install.sh`:1 on 2024-01-26 13:09_

This script introduces a jq dependency which needs to be documented. For windows:

```
winget install jqlang.jq
```

or https://jqlang.github.io/jq/download/.

---

_@konstin reviewed on 2024-01-26 13:13_

The install script errors on windows 11 git bash:

```console
$ scripts/bootstrap/install.sh
which: no grealpath in (/mingw64/bin:/usr/bin:/c/Users/Konstantin/bin:/c/Python37/Scripts:/c/Python37:/c/Program Files (x86)/Intel/iCLS Client:/c/Program Files/Intel/iCLS Client:/c/Windows/system32:/c/Windows:/c/Windows/System32/Wbem:/c/Windows/System32/WindowsPowerShell/v1.0:/c/Program Files (x86)/Intel/Intel(R) Management Engine Components/DAL:/c/Program Files/Intel/Intel(R) Management Engine Components/DAL:/c/Program Files (x86)/Intel/Intel(R) Management Engine Components/IPT:/c/Program Files/Intel/Intel(R) Management Engine Components/IPT:/c/Program Files/Intel/WiFi/bin:/c/Program Files/Common Files/Intel/WirelessCommon:/c/Windows/System32/OpenSSH:/cmd:/c/Program Files/nodejs:/c/ProgramData/chocolatey/bin:/c/Program Files/PowerShell/7:/c/Users/Konstantin/AppData/Local/Programs/Python/Python312/Scripts:/c/Users/Konstantin/AppData/Local/Programs/Python/Python312:/c/Users/Konstantin/AppData/Local/Programs/Python/Python38/Scripts:/c/Users/Konstantin/AppData/Local/Programs/Python/Python38:/c/Users/Konstantin/.cargo/bin:/c/Users/Konstantin/AppData/Local/Microsoft/WindowsApps:/c/Users/Konstantin/AppData/Local/JetBrains/Toolbox/scripts:/c/users/konstantin/.local/bin:/c/Users/Konstantin/AppData/Roaming/npm:/c/Users/Konstantin/AppData/Local/Microsoft/WindowsApps:/c/Users/Konstantin/AppData/Local/Microsoft/WinGet/Packages/BurntSushi.ripgrep.MSVC_Microsoft.Winget.Source_8wekyb3d8bbwe/ripgrep-14.1.0-x86_64-pc-windows-msvc:/c/Users/Konstantin/AppData/Local/Microsoft/WinGet/Packages/jqlang.jq_Microsoft.Winget.Source_8wekyb3d8bbwe)
Installing cpython-3.8.12-mingw64_nt-10.0-22621-x86_64
No matching download for cpython-3.8.12-mingw64_nt-10.0-22621-x86_64
```

---

_Review comment by @BurntSushi on `.envrc`:2 on 2024-01-26 13:32_

Should these variables be quoted?

(It would be odd for these to require quoting on a Unix system, but if this gets used on Windows, spaces are much more common there IIRC.)

---

_Review comment by @BurntSushi on `.github/workflows/ci.yml`:73 on 2024-01-26 13:32_

Can you use direnv here instead?

---

_Review comment by @BurntSushi on `scripts/bootstrap/fetch-version-metadata.py`:79 on 2024-01-26 13:41_

I _think_ this can just be `"-(%s)$"`, although you would need to use `_suffix_re.search(...)` instead of `_suffix_re.match(...)` below.

---

_Review comment by @BurntSushi on `scripts/bootstrap/install.sh`:1 on 2024-01-26 13:45_

Additionally, I'm a fan of explicitly checking for dependencies used in a shell script if any outside of core tools are needed. For example, see https://github.com/BurntSushi/dotfiles/blob/2431b0d87614eabc0f022fd3617dcb0e5a5b4de4/bin/backup#L315-L322 and https://github.com/BurntSushi/dotfiles/blob/2431b0d87614eabc0f022fd3617dcb0e5a5b4de4/bin/backup#L341.

It's a big quality of life improvement because it avoids the scenario of getting half-way through execution and then getting a "`jq` not found" error.

---

_Review comment by @BurntSushi on `scripts/bootstrap/install.sh`:1 on 2024-01-26 13:46_

We could also write this in Python to avoid adding new dependencies. _And_ it might work on Windows. (Although I see some symlinking below...)

---

_Review comment by @BurntSushi on `scripts/bootstrap/install.sh`:43 on 2024-01-26 13:49_

`arch` isn't a standard command. (It's not on my system for example.) I think it is Debian specific. `uname --machine` might be what you're looking for?

---

_@BurntSushi approved on 2024-01-26 13:50_

Nice! Does this mean I don't need `pyenv` any more?

---

_@zanieb reviewed on 2024-01-26 14:44_

---

_Review comment by @zanieb on `scripts/bootstrap/install.sh`:1 on 2024-01-26 14:44_

Ah thanks missed that dependency in my list and I'm happy to add explicit checks.

I wrote the whole thing in Python first at #1068 — but it was annoying that it had more Python requirements and I really didn't like bootstrapping Python development with Python. 

---

_@zanieb reviewed on 2024-01-26 14:44_

---

_Review comment by @zanieb on `.envrc`:2 on 2024-01-26 14:44_

I'm not sure about the envrc syntax, but probably.

---

_@zanieb reviewed on 2024-01-26 14:45_

---

_Review comment by @zanieb on `scripts/bootstrap/fetch-version-metadata.py`:79 on 2024-01-26 14:45_

This is copied directly from Rye, hesitant to change?

---

_Review comment by @zanieb on `scripts/bootstrap/install.sh`:43 on 2024-01-26 14:46_

👍 weird its present on macOS and Ubuntu but can change

---

_@zanieb reviewed on 2024-01-26 14:46_

---

_@zanieb reviewed on 2024-01-26 14:48_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/lib.rs`:68 on 2024-01-26 14:48_

Thank you!

---

_Comment by @zanieb on 2024-01-26 15:53_

I plan on addressing this round of feedback then merging since I have a stack on top of this and we really need to codify these versions. The `bin` folder can be moved elsewhere if you want to use these versions of Python globally on your system (and avoid setting variables when using the project). I don't think we'll have Windows support initially since I can't test it, but I'm happy to collaborate on it. I can work on it via GitHub Actions in a follow-up if needed.

> Does this mean I don't need pyenv any more?

That's the goal!

---

_@zanieb reviewed on 2024-01-26 17:36_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:73 on 2024-01-26 17:36_

Sure

---

_Merged by @zanieb on 2024-01-26 18:12_

---

_Closed by @zanieb on 2024-01-26 18:12_

---

_Branch deleted on 2024-01-26 18:12_

---
