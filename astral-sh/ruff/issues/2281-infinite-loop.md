```yaml
number: 2281
title: "[Infinite loop]"
type: issue
state: closed
author: jackdesert
labels: []
assignees: []
created_at: 2023-01-28T00:02:32Z
updated_at: 2023-01-28T03:20:15Z
url: https://github.com/astral-sh/ruff/issues/2281
synced_at: 2026-01-10T11:09:45Z
```

# [Infinite loop]

---

_Issue opened by @jackdesert on 2023-01-28 00:02_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

Version: ruff 0.0.236

Command: `ruff clean_logs.py --fix`

Contents of `clean_logs.py`:

    def main(original_prefix):
        """
        Delete all log streams in all log groups starting with `prefix`.

        This is called by handler.handler

        Questions: How is this triggered?
        When is this triggered?
        Does it only work on certain log groups?
        Does it check whether the log group is empty before deleting it?

        Parameters
        ----------
            original_prefix <str> : Which log prefix to clean
        """
        print(f'Received prefix {original_prefix}')

        prefixes = DEFAULT_PREFIXES if original_prefix == 'all' else [original_prefix]

        for prefix in prefixes:
            print(f'starting prefix {prefix}')
            groups = get_groups(prefix)
            print(f'Found {len(groups)} groups')
            print('\n'.join([x['logGroupName'] for x in groups]))
            for group in groups:
                _delete_streams_older_than_retention_period(group)
        print('Done')


    if __name__ == '__main__':
        main(sys.argv[1])



Output:

    warning: Failed to converge after 100 iterations.

    This likely indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BInfinite%20loop%5D

    quoting the contents of `clean_logs.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

    clean_logs.py:1:1: D100 Missing docstring in public module
    clean_logs.py:2:1: D417 Missing argument description in the docstring: `original_prefix`
    clean_logs.py:3:5: D212 Multi-line docstring summary should start at the first line
    clean_logs.py:19:16: F821 Undefined name `DEFAULT_PREFIXES`
    clean_logs.py:23:18: F821 Undefined name `get_groups`
    clean_logs.py:27:13: F821 Undefined name `_delete_streams_older_than_retention_period`
    clean_logs.py:32:10: F821 Undefined name `sys`
    Found 107 errors (100 fixed, 7 remaining).

---

_Comment by @charliermarsh on 2023-01-28 00:19_

Do you mind including your `pyproject.toml` or other configuration file?

---

_Comment by @jackdesert on 2023-01-28 02:04_

Here's the pyproject.toml:

```
[tool.black]
skip-string-normalization = 1

[tool.ruff]
# Enable Pyflakes `E` and `F` codes by default.
select = ["ALL"]

# It appears that a single "Q" ignores all codes that start with Q
# but you could also disable single errors instead of classes of errors
ignore = ["INP001", # implicit-namespace-package
          "ANN",    # type annotations
          "T20",    # flake8-print (warns when there are print statements)
          #"D211",   # no-blank-lines-before-class (See https://github.com/charliermarsh/ruff/issues/2281 )
]


# Allow autofix for all enabled rules (when `--fix`) is provided.
fixable = ["A", "B", "C", "D", "E", "F"]
unfixable = []

# Exclude a variety of commonly ignored directories.
exclude = [
    ".bzr",
    ".direnv",
    ".eggs",
    ".git",
    ".hg",
    ".mypy_cache",
    ".nox",
    ".pants.d",
    ".ruff_cache",
    ".svn",
    ".tox",
    ".venv",
    "__pypackages__",
    "_build",
    "buck-out",
    "build",
    "dist",
    "node_modules",
    "venv",
]
per-file-ignores = {}

# Same as Black.
line-length = 88

# Allow unused variables when underscore-prefixed.
dummy-variable-rgx = "^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$"

# Assume Python 3.10.
target-version = "py310"

[tool.ruff.mccabe]
# Unlike Flake8, default to a complexity level of 10.
max-complexity = 10

# This is how you tell Q000 errors to prefer single quotes
[tool.ruff.flake8-quotes]
inline-quotes = "single"
```

---

_Comment by @charliermarsh on 2023-01-28 02:08_

You'll need to disable one of `D203` and `D211`, and one of `D212` and `D213`, since they represent conflicting settings for docstrings

```rust
/// Pairs of checks that shouldn't be enabled together.
pub const INCOMPATIBLE_CODES: &[(Rule, Rule, &str)] = &[
    (
        Rule::OneBlankLineBeforeClass,
        Rule::NoBlankLineBeforeClass,
        "`one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are \
         incompatible. Consider ignoring `one-blank-line-before-class`.",
    ),
    (
        Rule::MultiLineSummaryFirstLine,
        Rule::MultiLineSummarySecondLine,
        "`multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are \
         incompatible. Consider ignoring one.",
    ),
];
```

You can set:

```toml
[tool.ruff.pydocstyle]
convention = "numpy"  # Or "google", or "pep-257"
```

...to enforce an internally-consistent configuration.

Let me know if you keep seeing this!


---

_Closed by @charliermarsh on 2023-01-28 02:08_

---

_Comment by @spaceone on 2023-01-28 02:23_

Maybe the message `This likely indicates a bug in ruff. If you could open an issue at:` could be prevented in the case conflicting rules are selected?

---

_Comment by @charliermarsh on 2023-01-28 02:28_

I'm kind of tempted to remove these conflicts rules, and just choose one.

---

_Comment by @jackdesert on 2023-01-28 03:13_

Do you mean to say that there are inherent issues with choosing `select = ["ALL"]` ?

I wanted to familiarize myself with all that ruff had to offer, so I turned on `select = ["ALL"]`

---

_Comment by @charliermarsh on 2023-01-28 03:20_

Yeah. I re-added a disclaimer to the README:

> As a special-case, Ruff also supports the ALL code, which enables all rules. Note that some of the pydocstyle rules conflict (e.g., D203 and D211) as they represent alternative docstring formats. Enabling ALL without further configuration may result in suboptimal behavior, especially for the pydocstyle plugin.

It's really just that you need to turn off `D203` and `D213`. I'm going to fix this. It's such an easy trap to fall into.


---
