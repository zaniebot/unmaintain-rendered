---
number: 1555
title: pydocstyle plugin should implement match and match_dir options
type: issue
state: closed
author: marscher
labels: []
assignees: []
created_at: 2023-01-02T13:45:14Z
updated_at: 2023-01-03T00:13:26Z
url: https://github.com/astral-sh/ruff/issues/1555
synced_at: 2026-01-07T13:12:14-06:00
---

# pydocstyle plugin should implement match and match_dir options

---

_Issue opened by @marscher on 2023-01-02 13:45_

Sometimes it's useful to exclude some files and or directories from this plugin, e.g. you want to allow tests to match the rigid docstyle conventions.
Since I want other linting plugins to be run on the pydocstyle excluded files, Ruffs global exclude does not seem to be the solution.

```toml
[ruff.tool.pydocstyle]
# convention numpy is currently equivalent to ignoring 'D107', 'D203', 'D212', 'D213', 'D402', 'D413'
convention = "numpy"
match = "(?!__)(?!_version)(?!conftest).*\\.py"
match_dir = "^(?!(tests|tags|asdf)).*"
```

---

_Comment by @not-my-profile on 2023-01-02 13:56_

You can just configure `tool.ruff.per-file-ignores` as shown in the README.

---

_Comment by @marscher on 2023-01-02 14:33_

right, that would be redundant. Thank you very much for pointing it out.

---

_Closed by @marscher on 2023-01-02 14:33_

---

_Comment by @marscher on 2023-01-02 14:37_

This does not support regex, right? It'd be tedious to list every file matching my pattern manually repeating the ignored rules.

---

_Comment by @not-my-profile on 2023-01-02 15:09_

See https://docs.rs/globset/latest/globset/#syntax for the supported syntax.

---

_Comment by @charliermarsh on 2023-01-02 16:28_

It does support regex. Any provided pattern will be checked both against each file/directory base name and full path.

---

_Comment by @marscher on 2023-01-02 21:57_

Thanks, eventually this should be mentioned in the documentation. I should have taken a look into the output of `ruff --show-settings`. However the backslashes within my regex get escaped, so this could be why it does not match.

pyproject.toml:
```
[tool.ruff]
per-file-ignores = {"^(?!(tests|tags|asdf)).*" = ["D"]}
```


I just wonder, why the matching the beginning of a line is at the end of the source path. Even if this is removed, the regex does not match "/home/marscher/sources/weldx/tests" for instance. 

excerpt of `ruff --show-settings`:
```
    per_file_ignores: [
        (
            GlobMatcher {
                pat: Glob {
                    glob: "/home/marscher/sources/weldx/^(?!(tests|tags|asdf)).*",
                    re: "(?-u)^/home/marscher/sources/weldx/\\^\\(.!\\(tests\\|tags\\|asdf\\)\\)\\..*$",
                    opts: GlobOptions {
                        case_insensitive: false,
                        literal_separator: false,
                        backslash_escape: true,
                    },
                    tokens: Tokens(
                        [
                            Literal(
                                '/',
                            ),
                            Literal(
                                'h',
                            ),
                            Literal(
                                'o',
                            ),
                            Literal(
                                'm',
                            ),
                            Literal(
                                'e',
                            ),
                            Literal(
                                '/',
                            ),
                            Literal(
                                'm',
                            ),
                            Literal(
                                'a',
                            ),
                            Literal(
                                'r',
                            ),
                            Literal(
                                's',
                            ),
                            Literal(
                                'c',
                            ),
                            Literal(
                                'h',
                            ),
                            Literal(
                                'e',
                            ),
                            Literal(
                                'r',
                            ),
                            Literal(
                                '/',
                            ),
                            Literal(
                                's',
                            ),
                            Literal(
                                'o',
                            ),
                            Literal(
                                'u',
                            ),
                            Literal(
                                'r',
                            ),
                            Literal(
                                'c',
                            ),
                            Literal(
                                'e',
                            ),
                            Literal(
                                's',
                            ),
                            Literal(
                                '/',
                            ),
                            Literal(
                                'w',
                            ),
                            Literal(
                                'e',
                            ),
                            Literal(
                                'l',
                            ),
                            Literal(
                                'd',
                            ),
                            Literal(
                                'x',
                            ),
                            Literal(
                                '/',
                            ),
                            Literal(
                                '^',
                            ),
                            Literal(
                                '(',
                            ),
                            Any,
                            Literal(
                                '!',
                            ),
                            Literal(
                                '(',
                            ),
                            Literal(
                                't',
                            ),
                            Literal(
                                'e',
                            ),
                            Literal(
                                's',
                            ),
                            Literal(
                                't',
                            ),
                            Literal(
                                's',
                            ),
                            Literal(
                                '|',
                            ),
                            Literal(
                                't',
                            ),
                            Literal(
                                'a',
                            ),
                            Literal(
                                'g',
                            ),
                            Literal(
                                's',
                            ),
                            Literal(
                                '|',
                            ),
                            Literal(
                                'a',
                            ),
                            Literal(
                                's',
                            ),
                            Literal(
                                'd',
                            ),
                            Literal(
                                'f',
                            ),
                            Literal(
                                ')',
                            ),
                            Literal(
                                ')',
                            ),
                            Literal(
                                '.',
                            ),
                            ZeroOrMore,
                        ],
                    ),
                },
                re: (?-u)^/home/marscher/sources/weldx/\^\(.!\(tests\|tags\|asdf\)\)\..*$,
            },
            GlobMatcher {
                pat: Glob {
                    glob: "^(?!(tests|tags|asdf)).*",
                    re: "(?-u)^\\^\\(.!\\(tests\\|tags\\|asdf\\)\\)\\..*$",
                    opts: GlobOptions {
                        case_insensitive: false,
                        literal_separator: false,
                        backslash_escape: true,
                    },
                    tokens: Tokens(
                        [
                            Literal(
                                '^',
                            ),
                            Literal(
                                '(',
                            ),
                            Any,
                            Literal(
                                '!',
                            ),
                            Literal(
                                '(',
                            ),
                            Literal(
                                't',
                            ),
                            Literal(
                                'e',
                            ),
                            Literal(
                                's',
                            ),
                            Literal(
                                't',
                            ),
                            Literal(
                                's',
                            ),
                            Literal(
                                '|',
                            ),
                            Literal(
                                't',
                            ),
                            Literal(
                                'a',
                            ),
                            Literal(
                                'g',
                            ),
                            Literal(
                                's',
                            ),
                            Literal(
                                '|',
                            ),
                            Literal(
                                'a',
                            ),
                            Literal(
                                's',
                            ),
                            Literal(
                                'd',
                            ),
                            Literal(
                                'f',
                            ),
                            Literal(
                                ')',
                            ),
                            Literal(
                                ')',
                            ),
                            Literal(
                                '.',
                            ),
                            ZeroOrMore,
                        ],
                    ),
                },
                re: (?-u)^\^\(.!\(tests\|tags\|asdf\)\)\..*$,
            },
            {
                D104,
                D101,
                D419,
                D416,
                D413,
                D410,
                D407,
                D404,
                D400,
                D215,
                D212,
                D209,
                D206,
                D203,
                D200,
                D105,
                D102,
                D417,
                D414,
                D411,
                D408,
                D405,
                D402,
                D300,
                D213,
                D210,
                D207,
                D204,
                D201,
                D106,
                D103,
                D100,
                D418,
                D415,
                D412,
                D409,
                D406,
                D403,
                D301,
                D214,
                D211,
                D208,
                D205,
                D202,
                D107,
            },
        ),
    ],
```


---

_Comment by @charliermarsh on 2023-01-02 22:00_

The path is appended to the project root, since we normalize all paths to absolute paths internally.

---

_Comment by @marscher on 2023-01-03 00:13_

Right. But then `.*(?!(tests|tags|asdf)).*` should match, but does not. The refered Rust documentation is helpful though, as the GlobSet does not support regular expressions at all, but just `**` to descend into any level of directory and `*` as a plain wildcard.

---
