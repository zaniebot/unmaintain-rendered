```yaml
number: 6480
title: per-file-ignores have inconsistent behavior in symlinked directories
type: issue
state: open
author: derlin
labels:
  - bug
assignees: []
created_at: 2023-08-10T14:27:19Z
updated_at: 2024-01-04T03:59:12Z
url: https://github.com/astral-sh/ruff/issues/6480
synced_at: 2026-01-12T15:54:46Z
```

# per-file-ignores have inconsistent behavior in symlinked directories

---

_@derlin_

I am trying to ignore the `S101` rule in tests. I have a base `ruff.toml` which is used in `pyproject.toml`.

Adding this to `ruff.toml` doesn't work:

```toml
[per-file-ignores]
"**/test/**" = ["S101"] # <- still warnings in tests
```

However, adding the same in `pyproject.toml` works:
```toml
[tool.ruff]
    extend = "/path/to/ruff.toml"

[tool.ruff.per-file-ignores]
"**/test/**" = ["S101"] # <- this works as expected
```

I tried without wildcards as well and `extend-per-file-ignores`. Nothing added to `ruff.toml` is considered.

Ruff version: `0.0.284`


---

_Comment by @charliermarsh on 2023-08-10 15:08_

Does the pattern `"test"` work?

---

_Comment by @qdegraaf on 2023-08-10 15:17_

Can you link a minimal example? Setting up a small folder set-up like:
```
some_other_folder/
    test/
        foo.py
test/
    foo.py
foo.py
ruff.toml
```

With `foo.py` being:
```python
assert 3 > 1
```

And ruff.toml is:
```toml
[per-file-ignores]
"**/test/**" = ["S101"]
```

And running `ruff check . --select S101` with version `0.0.284` only raises the error for `foo.py` in the root:

```
foo.py:1:1: S101 Use of `assert` detected
```

Removing the `[per-file-ignores]` raises the error for all 3 files as intended.

---

_Comment by @charliermarsh on 2023-08-10 15:40_

I suspect it has to do with the use of `extend`, and where the paths are resolved relative to, or something.

---

_Comment by @derlin on 2023-08-10 17:55_

Sorry this wasn't very clear. The problem actually seems to be when `ruff.toml` is not at the root of the python project that is linted. Find below a reproducible example. 

The structure is:
```
/tmp
├── ruff-example
│   ├── .venv   # <- ruff is installed in this venv
│   ├── pyproject.toml 
│   └── src
│       └── test
│           └── foo.py
└── ruff.toml 
```

The content of `/tmp/ruff.toml`:
```yaml
select = ['S']
[per-file-ignores]
"**/test/**" = ["S101"]
```

The content of `/tmp/ruff-example/pyproject.toml`:
```yaml
[tool.ruff]
    extend = "/tmp/ruff.toml"
```

Running from the root of `/tmp` works as expected:
```python
/tmp/ruff-example/.venv/bin/ruff /tmp/ruff-example
```

Running it from the root of the source directory `ruff-example` fails:
```
cd /tmp/ruff-example
/tmp/ruff-example/.venv/bin/ruff .

src/test/foo.py:1:1: S101 Use of `assert` detected
Found 1 error.
```


---

_Comment by @derlin on 2023-08-14 07:08_

@qdegraaf added a minimal example of the problem: this has to do with the relative path taken from the `ruff.toml` location instead of the root of the project.

---

_Comment by @charliermarsh on 2023-08-14 13:51_

Thanks for following up :)

So, in general, paths are resolved relative to the configuration file that includes the `extend`, rather than the file that's being extended (so in this case, the paths are resolved relative to `pyproject.toml`, even if the settings themselves are defined in `ruff.toml`). 

This is intentional and while it may seem unintuitive in this case, it's generally preferable to the reverse, because those "extended" configuration files tend to be shared across projects. Imagine, e.g., that you have a global `pyproject.toml` that lives in your root directory, and a bunch of projects that extend it with project-specific modifications. In that case, it'd be hard to reuse the `pyproject.toml` if all paths like `./src` were being resolved relative to it.

I feel like what you're doing here should _maybe_ still be working though given the specifics of your regular expression, that part I don't quite understand.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-14 18:57_

---

_Comment by @charliermarsh on 2023-08-14 19:31_

Interestingly, everything works fine for me if I don't do this in `tmp`, and it also works fine if I _do_ do this in `tmp` but use a relative path in the the `pyproject.toml`, like:

```toml
[tool.ruff]
extend = "../ruff.toml"
```

---

_Comment by @charliermarsh on 2023-08-14 19:35_

For me at least, the problem is that when I use `extend = "/private/tmp/ruff.toml"`, internally the path gets resolved to: 
```
path: "/private/tmp/ruff-example/src/test/foo.py"
settings.per_file_ignores: [(GlobMatcher { pat: Glob { glob: "/tmp/**/test/**", re: "(?-u)^/tmp(?:/|/.*/)test/.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false, backslash_escape: true }, tokens: Tokens([Literal('/'), Literal('t'), Literal('m'), Literal('p'), RecursiveZeroOrMore, Literal('t'), Literal('e'), Literal('s'), Literal('t'), RecursiveSuffix]) }, re: Regex("(?-u)^/tmp(?:/|/.*/)test/.*$") }, GlobMatcher { pat: Glob { glob: "**/test/**", re: "(?-u)^(?:/?|.*/)test/.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('t'), Literal('e'), Literal('s'), Literal('t'), RecursiveSuffix]) }, re: Regex("(?-u)^(?:/?|.*/)test/.*$") }, {Assert})]
```

Notice how the Python file path is prefixed with `/private` but the globs are not. Using the relative path leads to them both being prefixed with `/private`. I don't really understand what's going on it but it seems specific to use the use `/tmp` and similar directories. Were you running into this in a "real" project?


---

_Comment by @charliermarsh on 2023-08-14 19:36_

Ah it must be related to symlinks? Apparently `/tmp` is a symlink to `/private/tmp` on macOS.

---

_Comment by @charliermarsh on 2023-08-14 19:45_

Alternatively, `ruff /tmp/ruff-example -n` also works for me, because then both paths omit the `/private` prefix.

---

_Comment by @charliermarsh on 2023-08-14 19:47_

I don't know what ideal behavior looks like here, I would need to do some research. Naively, it seems like we'd need to _always_ resolve symlinks in order to have the right behavior, but then we might end up reporting diagnostics on the symlinked location, which feels a bit off. @MichaReiser, do you happen to know how Rome handled symlinks generally?

---

_Label `needs-decision` added by @charliermarsh on 2023-08-14 19:47_

---

_Comment by @charliermarsh on 2023-08-14 19:56_

Actually, I think this specific case would be fixed if our `path.absolutize()` _didn't_ resolve symlinks (it appears that it does). It appears that resolving the current directory (if it is a symlink) isn't even _possible_ right now? https://stackoverflow.com/questions/71624942/how-to-get-the-current-working-directory-in-rust-if-it-is-a-symlink

---

_Comment by @charliermarsh on 2023-08-14 21:42_

@burntsushi - (Low priority) I notice that in [ripgrep](https://github.com/BurntSushi/ripgrep/blob/61733f6378b62fa2dc2e7f3eff2f2e7182069ca9/crates/core/args.rs#L1808), you query for `env::current_dir()` then fallback to `PWD`. It looks like [atuin](https://github.com/atuinsh/atuin/pull/783) does it in the other order. I'm wondering if we should use `PWD` over `env::current_dir()` to get more reasonable behavior with symlinks 🤔 Have you ever run into problems with this in ripgrep?

---

_Renamed from "per-file-ignores not working in ruff.toml" to "per-file-ignores have inconsistent behavior in symlinked directories" by @charliermarsh on 2023-08-15 01:02_

---

_Label `needs-decision` removed by @charliermarsh on 2023-08-15 01:02_

---

_Label `bug` added by @charliermarsh on 2023-08-15 01:02_

---

_Comment by @charliermarsh on 2023-08-15 01:02_

To summarize: there's some inconsistent behavior when the current directory contains a symlink. Otherwise, everything seems to be working as expected.

---

_Comment by @derlin on 2023-08-15 05:42_

Thank you for all those interesting insights. My actual setup is not using symlinks (I was just using tmp for the minimal example). Instead, ruff is running in a docker, with the ruff config located at `/presets/ruff.toml` and the project mounted in `/app`.

I couldn't make it work with the `**/test/**` regex, but it works if I use `/app/**/test/**` in the `ruff.toml`. The problem is, a CI may mount the folders somewhere else in the docker container, so this is not good enough. 

I tried calling ruff with `ruff check /app`, but still now luck.

**UPDATE**: it seems adding a leading `/` works -> `/**/test/**`.

---

_Comment by @BurntSushi on 2023-08-15 15:34_

@charliermarsh With respect to ripgrep, I believe that code was originally written with `std::env::current_dir`, which is a platform independent way of getting the CWD. And then an issue was filed pointing out cases where that fails, for example, when using ripgrep in a directory that had been removed. So in that context, querying for `PWD` is treated as a "fallback" to try and find the CWD even when `current_dir` fails. Potentially flipping this to ask for `PWD` first and falling back to `std::env::current_dir` might indeed be the better path. I don't think I considered symlinks in this context when writing `current_dir`

---

_Comment by @jaap3 on 2023-08-30 08:27_

@derlin Thanks for the workaround! Adding a leading slash does work (`"/*/tests/*"` seems to be sufficient)

---

_Comment by @MichaReiser on 2023-08-30 08:31_

>  @MichaReiser, do you happen to know how Rome handled symlinks generally?

No, but the relevant PRs are https://github.com/rome/tools/pull/3706 and https://github.com/rome/tools/pull/4732

---

_Unassigned @charliermarsh by @charliermarsh on 2024-01-04 03:59_

---
