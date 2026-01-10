---
number: 8241
title: "Exclude failure for `tool.ruff[lint|format]`"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - cli
assignees: []
created_at: 2023-10-26T05:27:44Z
updated_at: 2023-10-27T00:03:42Z
url: https://github.com/astral-sh/ruff/issues/8241
synced_at: 2026-01-10T01:22:48Z
---

# Exclude failure for `tool.ruff[lint|format]`

---

_Issue opened by @dhruvmanila on 2023-10-26 05:27_

Detected here: https://github.com/astral-sh/ruff/pull/8240#issuecomment-1780379180

With the following directory structure:
```
.
├── pyproject.toml [1]
└── tests
    └── packages
        └── test-bad-syntax
            └── pyproject.toml [2]
```

And, in `pyproject.toml` at

[1]
```toml
[tool.ruff.lint]
exclude = ["tests/packages/test-bad-syntax"]
```

[2]
```toml
[build-system]
requires = ['bad' 'syntax']
```

Running the command:
```console
$ ruff check .  
ruff failed
  Cause: TOML parse error at line 2, column 19
  |
2 | requires = ['bad' 'syntax']
  |                   ^
invalid array
expected `]`
```

With:
```console
$ ruff version
ruff 0.1.2 (3127c79b2 2023-10-24)
```

It works with `[tool.ruff]`. The `main` branch fails as well.

---

_Label `bug` added by @dhruvmanila on 2023-10-26 05:27_

---

_Comment by @MichaReiser on 2023-10-26 07:37_

I think the issue here is that `exclude` and `lint.exclude` are semantically different. 

* `exclude`: Excludes files from discovery. 
* `lint.exclude`: Excludes files from linting.
 

The proper fix is to change their configuration to use `exclude` instead of `lint.exclude`. 

---

_Comment by @MichaReiser on 2023-10-26 08:00_

See: https://github.com/pypa/build/pull/697 I asked if we could improve our documentation to make the distinction more clear.

---

_Label `bug` removed by @MichaReiser on 2023-10-26 08:01_

---

_Label `bug` added by @MichaReiser on 2023-10-26 08:01_

---

_Label `cli` added by @MichaReiser on 2023-10-26 08:01_

---

_Comment by @charliermarsh on 2023-10-26 15:12_

But it still feels like `lint.exclude` should respect the same semantics for exclusion, no?

---

_Comment by @MichaReiser on 2023-10-26 23:38_

> But it still feels like `lint.exclude` should respect the same semantics for exclusion, no?

I wouldn't know how that would work in a unified `check` command. The way it would work there is:

* Ruff detects all files, only respecting `exclude`
* Ruff calls `lint` for files not in `exclude` or `lint.exclude`
* Ruff calls `format` for files not in `exclude` or `format.exclude`

Applying the same semantics in `lint.exclude` would mean that it is necessary to run file discovery twice, once for `exclude` merged with `lint.exclude` and once with `format` merged with `format.exclude`. This would remove all benefits of having a single toolchain because we need to perform file discovery, parsing, setting resolution, etc. twice. 

---

_Comment by @charliermarsh on 2023-10-26 23:47_

I don't know, but isn't it strange that setting `lint.exclude` to `exclude = ["tests/packages/test-bad-syntax"]` doesn't exclude files in `./tests/packages/test-bad-syntax` from linting?


---

_Comment by @MichaReiser on 2023-10-26 23:51_

> I don't know, but isn't it strange that setting `lint.exclude` to `exclude = ["tests/packages/test-bad-syntax"]` doesn't exclude files in `./tests/packages/test-bad-syntax` from linting?

It does exclude files in `test-bad-syntax` from linting. The reported problem is that `test-bad-syntax` contains a `pyproject.toml` with a syntax error and ruff tries to load it when discovering the python files of the project when using `lint.exclude` but it does not when adding the directory to `exclude`.

---

_Comment by @charliermarsh on 2023-10-26 23:54_

Hmm, are you sure? I don't believe that's the case, because we're only testing against the file itself, and not its parent directories.

Given:

```toml
[tool.ruff.lint]
exclude = ["tests/packages/test-bad-syntax"]
```

Running:

```
❯ cargo run -p ruff_cli -- check .
    Finished dev [unoptimized + debuginfo] target(s) in 0.10s
     Running `/Users/crmarsh/workspace/ruff/target/debug/ruff check .`
warning: Detected debug build without --no-cache.
tests/packages/test-bad-syntax/foo.py:1:8: F401 [*] `os` imported but unused
Found 1 error.
```

If I change the `pyproject.toml` to:

```toml
[tool.ruff.lint]
exclude = ["tests/packages/test-bad-syntax/*.py"]
```

Then:

```
❯ cargo run -p ruff_cli -- check .
    Finished dev [unoptimized + debuginfo] target(s) in 0.10s
     Running `/Users/crmarsh/workspace/ruff/target/debug/ruff check .`
warning: Detected debug build without --no-cache.
```

---

_Comment by @MichaReiser on 2023-10-27 00:03_

Hmm interesting. Let's create a new issue for this because this is unrelated to the original report. 


https://github.com/astral-sh/ruff/issues/8267

---

_Closed by @MichaReiser on 2023-10-27 00:03_

---

_Comment by @charliermarsh on 2023-10-27 00:03_

(Sorry, I think I misunderstood the root cause of the reported issue.)

---
