```yaml
number: 4581
title: Use inline table for dependencies in lockfile
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/indent-dependencies
created_at: 2024-06-27T09:57:16Z
updated_at: 2024-06-27T18:06:47Z
url: https://github.com/astral-sh/uv/pull/4581
synced_at: 2026-01-12T16:06:19Z
```

# Use inline table for dependencies in lockfile

---

_@konstin_

Use indented inline tables for `distribution.dependencies`, `distribution.optional-dependencies` and `distribution.dev-dependencies`.

The new style is more concise (see examples below) and it makes the association between a distribution and its dependencies clearer (previously, they were both individual `[[...]]` blocks separated by newlines). The style is optimized for small, meaningful diffs by placing each dependency on a single line with a final trailing comma. Whenever a dependency is added, removed or changed, there should be a one line diff in `distribution.dependencies`. The final trailing comma ensures that adding a dependency doesn't change the line ahead.

Part of #3611

## Examples

### Simple workspace package

Before:
```toml
[[distribution]]
name = "bird-feeder"
version = "1.0.0"
source = "editable+packages/bird-feeder"

[[distribution.dependencies]]
name = "anyio"

[[distribution.dependencies]]
name = "seeds"
```

After:
```toml
[[distribution]]
name = "bird-feeder"
version = "1.0.0"
source = "editable+packages/bird-feeder"
dependencies = [
    { name = "anyio" },
    { name = "seeds" },
]
```

### Flask

Before:
```toml
[[distribution]]
name = "flask"
version = "3.0.2"
source = "registry+https://pypi.org/simple"
sdist = { url = "https://files.pythonhosted.org/packages/3f/e0/a89e8120faea1edbfca1a9b171cff7f2bf62ec860bbafcb2c2387c0317be/flask-3.0.2.tar.gz", hash = "sha256:822c03f4b799204250a7ee84b1eddc40665395333973dfb9deebfe425fefcb7d", size = 675248 }
wheels = [{ url = "https://files.pythonhosted.org/packages/93/a6/aa98bfe0eb9b8b15d36cdfd03c8ca86a03968a87f27ce224fb4f766acb23/flask-3.0.2-py3-none-any.whl", hash = "sha256:3232e0e9c850d781933cf0207523d1ece087eb8d87b23777ae38456e2fbe7c6e", size = 101300 }]

[[distribution.dependencies]]
name = "blinker"

[[distribution.dependencies]]
name = "click"

[[distribution.dependencies]]
name = "itsdangerous"

[[distribution.dependencies]]
name = "jinja2"

[[distribution.dependencies]]
name = "werkzeug"

[distribution.optional-dependencies]

[[distribution.optional-dependencies.dotenv]]
name = "python-dotenv"
```

After:
```toml
[[distribution]]
name = "flask"
version = "3.0.2"
source = "registry+https://pypi.org/simple"
sdist = { url = "https://files.pythonhosted.org/packages/3f/e0/a89e8120faea1edbfca1a9b171cff7f2bf62ec860bbafcb2c2387c0317be/flask-3.0.2.tar.gz", hash = "sha256:822c03f4b799204250a7ee84b1eddc40665395333973dfb9deebfe425fefcb7d", size = 675248 }
dependencies = [
    { name = "blinker" },
    { name = "click" },
    { name = "itsdangerous" },
    { name = "jinja2" },
    { name = "werkzeug" },
]
wheels = [{ url = "https://files.pythonhosted.org/packages/93/a6/aa98bfe0eb9b8b15d36cdfd03c8ca86a03968a87f27ce224fb4f766acb23/flask-3.0.2-py3-none-any.whl", hash = "sha256:3232e0e9c850d781933cf0207523d1ece087eb8d87b23777ae38456e2fbe7c6e", size = 101300 }]

[distribution.optional-dependencies]
dotenv = [
    { name = "python-dotenv" },
]
```

### Forking

Before:
```toml
[[distribution]]
name = "project"
version = "0.1.0"
source = "editable+."

[[distribution.dependencies]]
name = "package-a"
version = "4.3.0"
source = "registry+https://astral-sh.github.io/packse/0.3.29/simple-html/"
marker = "sys_platform == 'darwin'"

[[distribution.dependencies]]
name = "package-a"
version = "4.4.0"
source = "registry+https://astral-sh.github.io/packse/0.3.29/simple-html/"
marker = "sys_platform == 'linux'"

[[distribution.dependencies]]
name = "package-b"
marker = "sys_platform == 'linux'"

[[distribution.dependencies]]
name = "package-c"
marker = "sys_platform == 'darwin'"
```

After:
```toml
[[distribution]]
name = "project"
version = "0.1.0"
source = "editable+."
dependencies = [
    { name = "package-a", version = "4.3.0", source = "registry+https://astral-sh.github.io/packse/0.3.29/simple-html/", marker = "sys_platform == 'darwin'" },
    { name = "package-a", version = "4.4.0", source = "registry+https://astral-sh.github.io/packse/0.3.29/simple-html/", marker = "sys_platform == 'linux'" },
    { name = "package-b", marker = "sys_platform == 'linux'" },
    { name = "package-c", marker = "sys_platform == 'darwin'" },
]
```

---

_Label `preview` added by @konstin on 2024-06-27 09:57_

---

_Review requested from @BurntSushi by @konstin on 2024-06-27 09:57_

---

_@konstin reviewed on 2024-06-27 09:58_

---

_Review comment by @konstin on `crates/uv/tests/edit.rs`:79 on 2024-06-27 09:58_

I'm wondering if we should just use a string for them, instead of an object with a single string?

---

_@konstin reviewed on 2024-06-27 10:15_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:2152 on 2024-06-27 10:15_

Part of the decision to make here is if we want to ident single item lists and whether to indent by tabs or by spaces, cc @ibraheemdev re https://github.com/astral-sh/uv/commit/85183c1c36265f9d6942f9fb8a683c0e943243d6#diff-221a46775c0f0caff0d7be0a7669cc329bdf2c245c4b1f2400a19cc5a4f92f55R188-R194.

I believe we should use 4 spaces since it is what the ruff formatter through black and PEP does.

I'd favor indenting the item in a list item list for diff-ability. The wheels list doesn't usually change, but i'd indent it the same way for consistency (#4582).

---

_Review requested from @ibraheemdev by @konstin on 2024-06-27 10:15_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:2152 on 2024-06-27 10:28_

In comparison to our style, prettier uses two spaces and collapses short lines.

---

_@konstin reviewed on 2024-06-27 10:28_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:2152 on 2024-06-27 11:58_

In terms of indenting single item lists, I think that actually makes a lot of sense. In particular, I feel like a key practical advantage here is if there is a change that adds something to a single item list, then the diff is more "straight-forward." This probably won't happen much (if at all) for wheels, but I expect it will with dependencies. And I agree that if we do it for dependencies, we might as well do it for wheels for consistency.

As for tabs versus spaces. I've thought a little about this, and I think I weakly prefer tabs. Most of the gripes I have with tabs are related to their use when editing code. But in terms of reading it, as long as they're used correctly (which we can ensure here since we control serialization of the lock file), I do feel like they confer one important advantage: it lets folks configure the visual indentation via their tab width. [You can even do this via GitHub too.](https://stackoverflow.com/questions/8833953/how-to-change-tab-size-on-github)

I'm fine with 4 spaces, but I do think we should consider tabs.

---

_Review comment by @BurntSushi on `crates/uv/tests/edit.rs`:79 on 2024-06-27 12:01_

This is a good question. Cargo uses strings for dependencies, and I originally wanted to as well, but we have more information that would be straining the limits of "good sense" to cram into a string here. But, in the common case of a dependency just being a single package name? Hmmm.

I think I like this idea. My weak concern is when you have a mixture of inline tables and package name strings. I feel like that might look a little odd and inhibit quick scanning. If you agree that that is odd, then another possibility is to use name strings only when every dependency is a single package name. I think I'd be cool with that.

---

_@BurntSushi approved on 2024-06-27 12:02_

So much better!!!

---

_@ibraheemdev approved on 2024-06-27 18:03_

This is really nice!

---

_@ibraheemdev reviewed on 2024-06-27 18:03_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/lock.rs`:2152 on 2024-06-27 18:03_

Don't really have a strong preference here, either is fine.

---

_@konstin reviewed on 2024-06-27 18:06_

---

_Review comment by @konstin on `crates/uv/tests/edit.rs`:79 on 2024-06-27 18:06_

This would be nice, but then introducing the first non-name dependency would result in a massive diff, and removing it again. My main thinking is that there will be few forking cases, so they look like the exception they are between the non-forking cases.

---

_Merged by @konstin on 2024-06-27 18:06_

---

_Closed by @konstin on 2024-06-27 18:06_

---

_Branch deleted on 2024-06-27 18:06_

---
