```yaml
number: 8034
title: "Add `ruff version` with long version display"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/long-version
created_at: 2023-10-18T01:51:07Z
updated_at: 2023-10-20T19:07:42Z
url: https://github.com/astral-sh/ruff/pull/8034
synced_at: 2026-01-12T02:32:41Z
```

# Add `ruff version` with long version display

---

_Pull request opened by @zanieb on 2023-10-18 01:51_

Adds a new `ruff version` sub-command which displays long version information in the style of `cargo` and `rustc`. We include the number of commits since the last release tag if its a development build, in the style of Python's versioneer.

```
❯ ruff version
ruff 0.1.0+14 (947940e91 2023-10-18)
```

```
❯ ruff version --output-format json
{
  "version": "0.1.0",
  "commit_info": {
    "short_commit_hash": "947940e91",
    "commit_hash": "947940e91269f20f6b3f8f8c7c63f8e914680e80",
    "commit_date": "2023-10-18",
    "last_tag": "v0.1.0",
    "commits_since_last_tag": 14
  }
}%
```

```
❯ cargo version
cargo 1.72.1 (103a7ff2e 2023-08-15)
```
## Test plan

I've tested this manually locally, but want to at least add unit tests for the message formatting. We'd also want to check the next release to ensure the information is correct.

I checked build behavior with a detached head and branches.

## Future work

We could include rustc and cargo versions from the build, the current Python version, and other diagnostic information for bug reports.

The `--version` and `-V` output is unchanged. However, we could update it to display the long ruff version without the rust and cargo versions (this is what cargo does). We'll need to be careful to ensure this does not break downstream packages which parse our version string.

```
❯ ruff --version
ruff 0.1.0
```

The LSP should be updated to use `ruff version --output-format json` instead of parsing `ruff --version`.

---

_@zanieb reviewed on 2023-10-18 02:01_

---

_Review comment by @zanieb on `crates/ruff_cli/src/lib.rs`:1 on 2023-10-18 02:01_

Seems bad; required for

```
warning: use of `println!`
    --> /Users/mz/eng/src/astral-sh/ruff/target/debug/build/ruff_cli-05d917f7674e2fe3/out/shadow.rs:1008:2
     |
1008 |     println!("CLAP_LONG_VERSION:{CLAP_LONG_VERSION}\n");
     |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
     |
     = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#print_stdout
```

These are for a shadow utility that prints all the variables — we don't use it. Will need to investigate a better workaround.

---

_Comment by @zanieb on 2023-10-18 02:06_

I'm not sure I'm into shadow-rs for this — I think I may prefer to do something like `cargo`'s implementation.

---

_Comment by @charliermarsh on 2023-10-18 02:09_

I haven't looked at the code yet, but one thing to note is that we do parse the `ruff version` output in the LSP:

```python
from packaging.version import Version


def version(executable: str) -> Version:
    """Returns the version of the executable at the given path."""
    output = subprocess.check_output([executable, "--version"]).decode().strip()
    version = output.replace("ruff ", "")  # no removeprefix in 3.7 :/
    return Version(version)
```

---

_Comment by @zanieb on 2023-10-18 02:15_

This doesn't change any existing behavior — great note though it may be a good reason not to change `--version` to be long-form.

---

_Comment by @konstin on 2023-10-18 09:40_

I'd like to switch the lsp to use the json instead

---

_Comment by @zanieb on 2023-10-18 15:12_

That's a great idea!

---

_Comment by @zanieb on 2023-10-18 17:18_

I'm really pleased with the handwritten implementation. I adapted the version and commit information from https://github.com/zanieb/poetry-relax/blob/main/scripts/version, https://github.com/rust-lang/cargo/blob/master/build.rs, and https://github.com/rust-lang/cargo/blob/master/src/cargo/version.rs

---

_Review comment by @konstin on `crates/ruff_cli/build.rs`:48 on 2023-10-18 17:21_

How does this work caching wise, i.e. how do we avoid cargo using a cache old hash? I think the only solution is https://doc.rust-lang.org/cargo/reference/build-scripts.html#rerun-if-changed with `.git/HEAD`, which is also slightly annoying because it invalidates the cache everytime you do something with git.

I'd prefer to read files over subcommands (they have an overhead and weird os failure modes), but that might be to hard with git.

---

_@konstin reviewed on 2023-10-18 17:21_

---

_@zanieb reviewed on 2023-10-18 17:24_

---

_Review comment by @zanieb on `crates/ruff_cli/build.rs`:48 on 2023-10-18 17:24_

I think it should be fine for release builds. For local builds, yes it may be incorrect unless you `cargo clean`. I'm not sure if it's worth adding the rerun-if-changed?

I've only ever seen git subcommand used for this, it definitely seems hard to read files.

---

_@zanieb reviewed on 2023-10-18 17:33_

---

_Review comment by @zanieb on `crates/ruff_cli/build.rs`:48 on 2023-10-18 17:33_

We could use https://doc.rust-lang.org/cargo/reference/build-scripts.html#rerun-if-env-changed and set an environment variable in CI on release builds for safety? Could be used to force a refresh locally too.

---

_@konstin reviewed on 2023-10-18 17:43_

---

_Review comment by @konstin on `crates/ruff_cli/build.rs`:48 on 2023-10-18 17:43_

That sounds like a good workaround

---

_Comment by @github-actions[bot] on 2023-10-18 17:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @MichaReiser on 2023-10-18 21:55_

> I'd like to switch the lsp to use the json instead

How would we detect if the ruff version is new enough to support the new json output without using `--version` first?

---

_Comment by @charliermarsh on 2023-10-18 22:28_

Hah yes that’s true. Maintaining compatibility between Ruff and the LSP feels increasingly annoying.

---

_Comment by @zanieb on 2023-10-18 23:50_

> > I'd like to switch the lsp to use the json instead
> 
> How would we detect if the ruff version is new enough to support the new json output without using `--version` first?

True, that's pretty funny. We can go the "try and fail" way at the cost of some speed.

---

_Comment by @konstin on 2023-10-19 08:18_

> How would we detect if the ruff version is new enough to support the new json output without using --version first?

We don't, we have to wait until a ruff with a json version is the oldest supported version plus a bit of buffer for better error messages.

---

_Review comment by @zanieb on `crates/ruff_cli/build.rs`:48 on 2023-10-19 17:05_

`.git/HEAD` only contains a pointer to the current branch so I don't think that would work (unless git sets a new mtime on the file on each operation)

> Normally build scripts are re-run if any file inside the crate root changes, but this can be used to scope changes to just a small set of files.

It seems like we'd only be reducing scope by doing this?

Unfortunately the environment variable one is not "if set" it's "if changes" so it'd work well in CI but not so much for local resets.


---

_@zanieb reviewed on 2023-10-19 17:05_

---

_Review comment by @zanieb on `crates/ruff_cli/build.rs`:48 on 2023-10-19 18:10_

Addressed in https://github.com/astral-sh/ruff/pull/8034/commits/1208df9f40687c7a58aec301723919bd15087d3e

---

_@zanieb reviewed on 2023-10-19 18:10_

---

_@zanieb reviewed on 2023-10-19 18:12_

---

_Review comment by @zanieb on `crates/ruff_cli/build.rs`:45 on 2023-10-19 18:12_

Okay this is a little bit much but i tested this out and basically without this, if you change _any_ file in the `ruff_cli` crate e.g. add a file `foo` to the crate root then a rebuild is forced — this is entirely unnecessary so, here, if a git directory is present we will use commit information from it to determine if rebuilding should occur. 

---

_Comment by @zanieb on 2023-10-19 20:35_

Just needs some snapshot tests for the version formatting now.

---

_Comment by @zanieb on 2023-10-20 15:35_

I wish we could test the `ruff version` command output but it would change per commit and redacting it would be meaningless.

---

_Marked ready for review by @zanieb on 2023-10-20 15:35_

---

_@konstin approved on 2023-10-20 15:44_

---

_Merged by @zanieb on 2023-10-20 19:07_

---

_Closed by @zanieb on 2023-10-20 19:07_

---

_Branch deleted on 2023-10-20 19:07_

---
