```yaml
number: 1816
title: Split off ruff_cli crate from ruff library
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: ruff_cli
created_at: 2023-01-12T13:21:47Z
updated_at: 2023-01-15T07:37:40Z
url: https://github.com/astral-sh/ruff/pull/1816
synced_at: 2026-01-12T15:55:07Z
```

# Split off ruff_cli crate from ruff library

---

_@not-my-profile_

Please preserve the commits when merging.

---

_@not-my-profile reviewed on 2023-01-12 15:03_

---

_Review comment by @not-my-profile on `.github/workflows/ruff.yaml`:37 on 2023-01-12 15:03_

I am not sure if this works ... I haven't tested it. @messense do you think that should work?

---

_Comment by @charliermarsh on 2023-01-12 15:18_

Nice! This will be a big improvement.

Before I dig in, I just want to confirm: there are intended to be no behavior changes here, right? As in, Ruff itself should function identically? Purely an internal refactor?

---

_Comment by @not-my-profile on 2023-01-12 15:20_

Yes exactly.

---

_@charliermarsh reviewed on 2023-01-12 15:24_

---

_Review comment by @charliermarsh on `Cargo.toml`:7 on 2023-01-12 15:24_

I have some thoughts, but in your opinion, how does this compare to the following alternative?

> Move everything in `src` into `ruff_core`, keep `ruff_cli` as it is (roughly), then change the root to point to `ruff_cli/src/main.rs` or appropriate.

This is, IIUC, how [`ripgrep`](https://github.com/BurntSushi/ripgrep) is structured.

Right now, there's a slight oddity here in that the top-level project isn't what's being packaged and distributed (which relates to having to change the working directory on CI).


---

_Review comment by @charliermarsh on `.github/workflows/ruff.yaml`:17 on 2023-01-12 15:25_

Should this be `ruff_cli`?

---

_@charliermarsh reviewed on 2023-01-12 15:25_

---

_Review comment by @messense on `ruff_cli/pyproject.toml`:16 on 2023-01-12 15:25_

This won't work for sdist since maturin doesn't know where to put the LICENSE file in sdist at the moment.

```
🔗 Found bin bindings
📡 Using build options bindings from pyproject.toml
💥 maturin failed
  Caused by: Failed to build source distribution
  Caused by: Failed to add file from /Users/messense/Desktop/ruff/ruff_cli/../LICENSE to sdist as ruff-0.0.219/../LICENSE
  Caused by: paths in archives must not have `..` when setting path for ruff-0.0.219
```

---

_@charliermarsh reviewed on 2023-01-12 15:26_

---

_Review comment by @charliermarsh on `ruff_cli/pyproject.toml`:16 on 2023-01-12 15:26_

The `tool.setuptools` references below would need to be changed as well (though I think they're currently being ignored -- please change them though).

---

_Review comment by @messense on `ruff_cli/pyproject.toml`:16 on 2023-01-12 15:27_

It might be easier to keep `pyproject.toml` in top level, and add `manifest-path` option

```toml
[tool.maturin]
bindings = "bin"
manifest-path = "ruff_cli/Cargo.toml"
python-source = "python"
strip = true
```

Better keep `pyproject.toml` in the same level as `python` source directory, otherwise the python code in `python/ruff` won't be packaged.

---

_@messense reviewed on 2023-01-12 15:30_

---

_@messense reviewed on 2023-01-12 15:32_

---

_Review comment by @messense on `.github/workflows/ruff.yaml`:37 on 2023-01-12 15:32_

Should work for building wheels but will have trouble with sdist, see https://github.com/charliermarsh/ruff/pull/1816#discussion_r1068265123, if you move `pyproject.toml` back to top level and set `manifest-path` option in it then you don't need this `working-directory` option.

---

_@not-my-profile reviewed on 2023-01-12 15:34_

---

_Review comment by @not-my-profile on `.github/workflows/ruff.yaml`:17 on 2023-01-12 15:34_

Ah yes, good catch!

---

_@messense reviewed on 2023-01-12 15:36_

---

_Review comment by @messense on `ruff_cli/Cargo.toml`:32 on 2023-01-12 15:36_

You can use cargo workspace inheritance feature to manage common dependencies between workspace members, see https://doc.rust-lang.org/cargo/reference/workspaces.html#the-dependencies-table

---

_Review comment by @charliermarsh on `Cargo.toml`:7 on 2023-01-12 15:37_

(It would also be ok to say "We want to do this later, since it's another big change, but it is planned." I'm mostly trying to understand and align on the desired end-state.)

---

_@charliermarsh reviewed on 2023-01-12 15:37_

---

_Review comment by @not-my-profile on `ruff_cli/pyproject.toml`:16 on 2023-01-12 15:39_

Thanks, I made the changes you suggested :)

---

_@not-my-profile reviewed on 2023-01-12 15:39_

---

_Comment by @messense on 2023-01-12 15:45_

I just checked locally, looks like maturin is having trouble with this kind of layout, mostly related to `python-source` option:

* `python/ruff/*` isn't in the right place in sdist
* `python/ruff/*` isn't packaged in wheel

I'll take a look tomorrow.

---

_@not-my-profile reviewed on 2023-01-12 16:15_

---

_Review comment by @not-my-profile on `Cargo.toml`:7 on 2023-01-12 16:15_

I consider the "core" of ruff to be conceptually separate from the rule implementations, even if they are currently entangled. So I think the crate of the currently top-level `src` directory should keep the name `ruff`, as long as it contains both the core and the rule implementations.

While I think it makes some sense that the top-level `src` directory contains the code that contributors are most likely interested in editing, I'd be alright with moving `src` to `ruff/src` for the sake of consistency, however we might firstly want to think about further splitting up the crate, see #1820.

---

_Review comment by @not-my-profile on `ruff_cli/Cargo.toml`:32 on 2023-01-12 16:29_

Oh neat, I didn't know about that!

I guess you meant specifically for these verbose dependencies on `rustpython-*` right? I just changed `ruff::message` to `pub use rustpython_ast::Location;` and dropped the direct dependencies on rustpython-* from `ruff_cli`, which I think is even better :)

---

_@not-my-profile reviewed on 2023-01-12 16:29_

---

_Comment by @charliermarsh on 2023-01-12 16:53_

@not-my-profile - How big of a problem will it be for you if I make some changes to the `pyproject.toml` resolution code? (Specifically, to solve #1812.) I'm happy to wait until this is merged if it saves you a headache, though guessing that will be tomorrow or the next day and not today given the packaging needs.

---

_Converted to draft by @not-my-profile on 2023-01-12 17:21_

---

_Comment by @not-my-profile on 2023-01-12 17:26_

Thanks for asking :)

You could merge the first two commits from this PR first (I just opened #1822 for that). Then you can change the resolution code without causing headaches :) (The remaining two commits of this PR can be easily rebased.)

---

_Comment by @charliermarsh on 2023-01-12 22:38_

@not-my-profile - Hope you feel empowered to keep pushing on these sorts of refactors. They're making the project much stronger.

---

_@messense reviewed on 2023-01-13 02:39_

---

_Review comment by @messense on `ruff_cli/Cargo.toml`:13 on 2023-01-13 02:39_

```suggestion
# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[[bin]]
name = "ruff"
path = "src/main.rs"
doctest = false
```

To keep the binary name `ruff`.

---

_Review comment by @messense on `ruff_cli/Cargo.toml`:58 on 2023-01-13 03:04_

```suggestion
update-informer = ["dep:update-informer"]

[package.metadata.maturin]
name = "ruff"
```

To keep the python package name `ruff` after https://github.com/PyO3/maturin/pull/1409 lands.

---

_@messense reviewed on 2023-01-13 03:04_

---

_Comment by @not-my-profile on 2023-01-13 03:14_

Thanks @messense! I have applied your suggested changes locally ... I'll force push this once the new maturin version has been published so that I can also bump the minimum version in `pyproject.toml`:

```toml
[build-system]
requires = ["maturin>=0.14,<0.15"]
```

---

_Comment by @messense on 2023-01-13 05:19_

@not-my-profile maturin [v0.14.10](https://pypi.org/project/maturin/0.14.10/) is out.

---

_Marked ready for review by @not-my-profile on 2023-01-13 05:50_

---

_Review requested from @charliermarsh by @not-my-profile on 2023-01-13 19:45_

---

_Comment by @charliermarsh on 2023-01-13 19:46_

Will review (and hopefully merge) later today.

---

_Comment by @charliermarsh on 2023-01-14 01:33_

@not-my-profile - Can I squash the Clippy commit?

---

_@charliermarsh reviewed on 2023-01-14 01:34_

---

_Review comment by @charliermarsh on `ruff_cli/Cargo.toml`:10 on 2023-01-14 01:34_

@messense - This was also necessary. As-is, `ruff-0.0.220.dist-info/METADATA` was missing the README contents.

---

_Comment by @charliermarsh on 2023-01-14 01:35_

Okay, the packages are looking more similar now:

Before:

```
❯ unzip -l /Users/crmarsh/workspace/ruff/target/wheels/ruff-0.0.220-py3-none-macosx_11_0_arm64.whl
Archive:  /Users/crmarsh/workspace/ruff/target/wheels/ruff-0.0.220-py3-none-macosx_11_0_arm64.whl
  Length      Date    Time    Name
---------  ---------- -----   ----
   120482  01-14-2023 01:34   ruff-0.0.220.dist-info/METADATA
      103  01-14-2023 01:34   ruff-0.0.220.dist-info/WHEEL
     1070  01-14-2023 01:34   ruff-0.0.220.dist-info/license_files/LICENSE
        0  01-14-2023 01:34   ruff/__init__.py
      193  01-14-2023 01:34   ruff/__main__.py
 40412178  01-14-2023 01:34   ruff-0.0.220.data/scripts/ruff
      540  01-14-2023 01:34   ruff-0.0.220.dist-info/RECORD
```

After:
```
❯ unzip -l /Users/crmarsh/workspace/ruff/target/wheels/ruff-0.0.220-py3-none-macosx_11_0_arm64.whl
Archive:  /Users/crmarsh/workspace/ruff/target/wheels/ruff-0.0.220-py3-none-macosx_11_0_arm64.whl
  Length      Date    Time    Name
---------  ---------- -----   ----
   120482  01-14-2023 01:37   ruff-0.0.220.dist-info/METADATA
      103  01-14-2023 01:37   ruff-0.0.220.dist-info/WHEEL
     1070  01-14-2023 01:37   ruff-0.0.220.dist-info/license_files/LICENSE
        0  01-14-2023 01:37   ruff/__init__.py
      193  01-14-2023 01:37   ruff/__main__.py
 38732738  01-14-2023 01:37   ruff-0.0.220.data/scripts/ruff
      540  01-14-2023 01:37   ruff-0.0.220.dist-info/RECORD
```

---

_Comment by @charliermarsh on 2023-01-14 01:38_

@not-my-profile - In fact, can we squash this to a single commit, with the message you want? I had to fix one bug (the missing README), then merged to resolve conflicts and verify that the contents on `main` were identical.

---

_Comment by @not-my-profile on 2023-01-14 02:12_

Since the first commit doesn't edit the files of the second commit and the clippy commit isn't that important, sure ... squashed into one :) Thanks for the fixes!

---

_@messense approved on 2023-01-14 02:30_

---

_Merged by @charliermarsh on 2023-01-14 02:37_

---

_Closed by @charliermarsh on 2023-01-14 02:37_

---

_Comment by @charliermarsh on 2023-01-14 02:38_

Awesome - thank you both!

---

_Comment by @not-my-profile on 2023-01-14 03:20_

(I missed that we also have to update a command in the `playground.yml` workflow ... however `wasm-pack build` apparently doesn't support the new structure anyway ... I just opened #1860 to track that.)

---

_Comment by @charliermarsh on 2023-01-14 20:02_

For reasons I don't fully understand, I think this change broke GitHub's dependency tracking:

![Screen Shot 2023-01-14 at 3 00 48 PM](https://user-images.githubusercontent.com/1309177/212494082-7891b58b-e747-46ee-8ce4-cc3ba481df3d.png)


---

_Comment by @charliermarsh on 2023-01-14 20:03_

I'd really love to get this back. I don't understand why it broke as we still have the top-level `setup.py`.

---

_Comment by @not-my-profile on 2023-01-14 20:04_

Yeah I have also noticed that it's bugged ... when I refresh the page a couple of times it sometimes shows:

![image](https://user-images.githubusercontent.com/73739153/212494153-4ec762dd-7a92-416b-afbf-675270991946.png)


---

_Comment by @charliermarsh on 2023-01-14 20:05_

Oh, wait, I see that now too...

---

_Comment by @not-my-profile on 2023-01-14 20:06_

I don't think this PR changed any files that GitHub uses for dependency tracking ... maybe GitHub just cannot handle how many projects depend on ruff :P

---

_Comment by @charliermarsh on 2023-01-15 07:33_

Interestingly, I now see this, which toggles between the results. I wonder if there's any way to avoid the Rust version from showing up. Oh, maybe this is because we took the [ruff](https://crates.io/crates/ruff) crate?

<img width="1624" alt="Screen Shot 2023-01-15 at 2 32 14 AM" src="https://user-images.githubusercontent.com/1309177/212528547-8739f903-1f6f-44c1-a364-2bb52f17c2ac.png">


---

_Comment by @charliermarsh on 2023-01-15 07:37_

Ah, there's a setting for this! Ok, perfect.

<img width="1022" alt="Screen Shot 2023-01-15 at 2 37 01 AM" src="https://user-images.githubusercontent.com/1309177/212528706-6108006c-6db9-4d2e-832c-a2fda3040800.png">


---
