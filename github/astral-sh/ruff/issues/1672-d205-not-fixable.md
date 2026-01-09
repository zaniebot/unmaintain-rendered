---
number: 1672
title: D205 not fixable
type: issue
state: closed
author: baggiponte
labels:
  - question
assignees: []
created_at: 2023-01-06T01:19:47Z
updated_at: 2023-01-06T10:30:51Z
url: https://github.com/astral-sh/ruff/issues/1672
synced_at: 2026-01-07T13:12:14-06:00
---

# D205 not fixable

---

_Issue opened by @baggiponte on 2023-01-06 01:19_

When I run `ruff` (`0.0.211`) on my project dir, I get:

```bash
src/futura_aws/s3.py:183:9: D205 1 blank line required between summary line and description
found 1 error(s).
```

According to the docs, this should be fixable, but I cannot make it work, whether I run `ruff src --fix`

My settings:

```toml
[tool.ruff]
line-length = 88
extend-select = ["D", "I"]
fix = true
fixable = ["D", "E501", "I001"]

[tool.ruff.pydocstyle]
convention = "numpy"

[tool.ruff.per-file-ignores]
"__init__.py" = ["F401"]

```

<details>
<summary>Extended configs</summary>

```
Resolved settings for: "/Users/lucabaggi/Documents/dev/futura-aws/src/futura_aws/__init__.py"
Settings {
    allowed_confusables: {},
    cache_dir: "/Users/lucabaggi/Documents/dev/futura-aws/.ruff_cache",
    dummy_variable_rgx: ^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$,
    enabled: {
        E401,
        F822,
        F704,
        D411,
        F631,
        D405,
        F525,
        D300,
        F508,
        D210,
        F502,
        D204,
        F403,
        D106,
        E902,
        D100,
        E721,
        E402,
        F823,
        D418,
        F706,
        D412,
        F632,
        D406,
        F541,
        D301,
        F509,
        D211,
        F503,
        D205,
        F404,
        E999,
        D101,
        E722,
        E501,
        F841,
        D419,
        F707,
        F633,
        D407,
        F601,
        D400,
        F521,
        F504,
        D206,
        F405,
        D200,
        D102,
        E731,
        E711,
        F842,
        F722,
        D414,
        F634,
        D408,
        F602,
        F522,
        F505,
        D207,
        F406,
        D201,
        D103,
        E741,
        E712,
        F901,
        F811,
        F701,
        D409,
        F621,
        D403,
        F523,
        D214,
        F506,
        D208,
        F407,
        D202,
        F401,
        D104,
        E742,
        E713,
        F821,
        F702,
        D410,
        F622,
        D404,
        F524,
        D215,
        F507,
        D209,
        F501,
        F402,
        D105,
        E743,
        E714,
        I001,
    },
    exclude: GlobSet {
        len: 19,
        strats: [
            Extension(
                ExtensionStrategy(
                    {},
                ),
            ),
            BasenameLiteral(
                BasenameLiteralStrategy(
                    {},
                ),
            ),
            Literal(
                LiteralStrategy(
                    {
                        [
                            46,
                            98,
                            122,
                            114,
                        ]: [
                            0,
                        ],
                        [
                            46,
                            100,
                            105,
                            114,
                            101,
                            110,
                            118,
                        ]: [
                            1,
                        ],
                        [
                            46,
                            101,
                            103,
                            103,
                            115,
                        ]: [
                            2,
                        ],
                        [
                            46,
                            103,
                            105,
                            116,
                        ]: [
                            3,
                        ],
                        [
                            46,
                            104,
                            103,
                        ]: [
                            4,
                        ],
                        [
                            46,
                            109,
                            121,
                            112,
                            121,
                            95,
                            99,
                            97,
                            99,
                            104,
                            101,
                        ]: [
                            5,
                        ],
                        [
                            46,
                            110,
                            111,
                            120,
                        ]: [
                            6,
                        ],
                        [
                            46,
                            112,
                            97,
                            110,
                            116,
                            115,
                            46,
                            100,
                        ]: [
                            7,
                        ],
                        [
                            46,
                            114,
                            117,
                            102,
                            102,
                            95,
                            99,
                            97,
                            99,
                            104,
                            101,
                        ]: [
                            8,
                        ],
                        [
                            46,
                            115,
                            118,
                            110,
                        ]: [
                            9,
                        ],
                        [
                            46,
                            116,
                            111,
                            120,
                        ]: [
                            10,
                        ],
                        [
                            46,
                            118,
                            101,
                            110,
                            118,
                        ]: [
                            11,
                        ],
                        [
                            95,
                            95,
                            112,
                            121,
                            112,
                            97,
                            99,
                            107,
                            97,
                            103,
                            101,
                            115,
                            95,
                            95,
                        ]: [
                            12,
                        ],
                        [
                            95,
                            98,
                            117,
                            105,
                            108,
                            100,
                        ]: [
                            13,
                        ],
                        [
                            98,
                            117,
                            99,
                            107,
                            45,
                            111,
                            117,
                            116,
                        ]: [
                            14,
                        ],
                        [
                            98,
                            117,
                            105,
                            108,
                            100,
                        ]: [
                            15,
                        ],
                        [
                            100,
                            105,
                            115,
                            116,
                        ]: [
                            16,
                        ],
                        [
                            110,
                            111,
                            100,
                            101,
                            95,
                            109,
                            111,
                            100,
                            117,
                            108,
                            101,
                            115,
                        ]: [
                            17,
                        ],
                        [
                            118,
                            101,
                            110,
                            118,
                        ]: [
                            18,
                        ],
                    },
                ),
            ),
            Suffix(
                SuffixStrategy {
                    matcher: AhoCorasick {
                        imp: DFA(
                            PremultipliedByteClass(
                                PremultipliedByteClass(
                                    Repr {
                                        match_kind: Standard,
                                        anchored: false,
                                        premultiplied: true,
                                        start_id: 2,
                                        max_pattern_len: 0,
                                        pattern_count: 0,
                                        state_count: 3,
                                        max_match: 1,
                                        heap_bytes: 96,
                                        prefilter: None,
                                        byte_classes: ByteClasses( 0 => [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99, 100, 101, 102, 103, 104, 105, 106, 107, 108, 109, 110, 111, 112, 113, 114, 115, 116, 117, 118, 119, 120, 121, 122, 123, 124, 125, 126, 127, 128, 129, 130, 131, 132, 133, 134, 135, 136, 137, 138, 139, 140, 141, 142, 143, 144, 145, 146, 147, 148, 149, 150, 151, 152, 153, 154, 155, 156, 157, 158, 159, 160, 161, 162, 163, 164, 165, 166, 167, 168, 169, 170, 171, 172, 173, 174, 175, 176, 177, 178, 179, 180, 181, 182, 183, 184, 185, 186, 187, 188, 189, 190, 191, 192, 193, 194, 195, 196, 197, 198, 199, 200, 201, 202, 203, 204, 205, 206, 207, 208, 209, 210, 211, 212, 213, 214, 215, 216, 217, 218, 219, 220, 221, 222, 223, 224, 225, 226, 227, 228, 229, 230, 231, 232, 233, 234, 235, 236, 237, 238, 239, 240, 241, 242, 243, 244, 245, 246, 247, 248, 249, 250, 251, 252, 253, 254, 255]),
                                        trans: [
                                            2,
                                            1,
                                            2,
                                        ],
                                        matches: [
                                            [],
                                            [],
                                            [],
                                        ],
                                    },
                                ),
                            ),
                        ),
                        match_kind: Standard,
                    },
                    map: [],
                    longest: 0,
                },
            ),
            Prefix(
                PrefixStrategy {
                    matcher: AhoCorasick {
                        imp: DFA(
                            PremultipliedByteClass(
                                PremultipliedByteClass(
                                    Repr {
                                        match_kind: Standard,
                                        anchored: false,
                                        premultiplied: true,
                                        start_id: 2,
                                        max_pattern_len: 0,
                                        pattern_count: 0,
                                        state_count: 3,
                                        max_match: 1,
                                        heap_bytes: 96,
                                        prefilter: None,
                                        byte_classes: ByteClasses( 0 => [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99, 100, 101, 102, 103, 104, 105, 106, 107, 108, 109, 110, 111, 112, 113, 114, 115, 116, 117, 118, 119, 120, 121, 122, 123, 124, 125, 126, 127, 128, 129, 130, 131, 132, 133, 134, 135, 136, 137, 138, 139, 140, 141, 142, 143, 144, 145, 146, 147, 148, 149, 150, 151, 152, 153, 154, 155, 156, 157, 158, 159, 160, 161, 162, 163, 164, 165, 166, 167, 168, 169, 170, 171, 172, 173, 174, 175, 176, 177, 178, 179, 180, 181, 182, 183, 184, 185, 186, 187, 188, 189, 190, 191, 192, 193, 194, 195, 196, 197, 198, 199, 200, 201, 202, 203, 204, 205, 206, 207, 208, 209, 210, 211, 212, 213, 214, 215, 216, 217, 218, 219, 220, 221, 222, 223, 224, 225, 226, 227, 228, 229, 230, 231, 232, 233, 234, 235, 236, 237, 238, 239, 240, 241, 242, 243, 244, 245, 246, 247, 248, 249, 250, 251, 252, 253, 254, 255]),
                                        trans: [
                                            2,
                                            1,
                                            2,
                                        ],
                                        matches: [
                                            [],
                                            [],
                                            [],
                                        ],
                                    },
                                ),
                            ),
                        ),
                        match_kind: Standard,
                    },
                    map: [],
                    longest: 0,
                },
            ),
            RequiredExtension(
                RequiredExtensionStrategy(
                    {},
                ),
            ),
            Regex(
                RegexSetStrategy {
                    matcher: RegexSet([]),
                    map: [],
                },
            ),
        ],
    },
    extend_exclude: GlobSet {
        len: 0,
        strats: [],
    },
    external: {},
    fix: true,
    fix_only: false,
    fixable: {
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
        D104,
        D101,
        E501,
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
        I001,
    },
    force_exclude: false,
    format: Text,
    ignore_init_module_imports: false,
    line_length: 88,
    per_file_ignores: [
        (
            GlobMatcher {
                pat: Glob {
                    glob: "/Users/lucabaggi/Documents/dev/futura-aws/__init__.py",
                    re: "(?-u)^/Users/lucabaggi/Documents/dev/futura\\-aws/__init__\\.py$",
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
                                'U',
                            ),
                            Literal(
                                's',
                            ),
                            Literal(
                                'e',
                            ),
                            Literal(
                                'r',
                            ),
                            Literal(
                                's',
                            ),
                            Literal(
                                '/',
                            ),
                            Literal(
                                'l',
                            ),
                            Literal(
                                'u',
                            ),
                            Literal(
                                'c',
                            ),
                            Literal(
                                'a',
                            ),
                            Literal(
                                'b',
                            ),
                            Literal(
                                'a',
                            ),
                            Literal(
                                'g',
                            ),
                            Literal(
                                'g',
                            ),
                            Literal(
                                'i',
                            ),
                            Literal(
                                '/',
                            ),
                            Literal(
                                'D',
                            ),
                            Literal(
                                'o',
                            ),
                            Literal(
                                'c',
                            ),
                            Literal(
                                'u',
                            ),
                            Literal(
                                'm',
                            ),
                            Literal(
                                'e',
                            ),
                            Literal(
                                'n',
                            ),
                            Literal(
                                't',
                            ),
                            Literal(
                                's',
                            ),
                            Literal(
                                '/',
                            ),
                            Literal(
                                'd',
                            ),
                            Literal(
                                'e',
                            ),
                            Literal(
                                'v',
                            ),
                            Literal(
                                '/',
                            ),
                            Literal(
                                'f',
                            ),
                            Literal(
                                'u',
                            ),
                            Literal(
                                't',
                            ),
                            Literal(
                                'u',
                            ),
                            Literal(
                                'r',
                            ),
                            Literal(
                                'a',
                            ),
                            Literal(
                                '-',
                            ),
                            Literal(
                                'a',
                            ),
                            Literal(
                                'w',
                            ),
                            Literal(
                                's',
                            ),
                            Literal(
                                '/',
                            ),
                            Literal(
                                '_',
                            ),
                            Literal(
                                '_',
                            ),
                            Literal(
                                'i',
                            ),
                            Literal(
                                'n',
                            ),
                            Literal(
                                'i',
                            ),
                            Literal(
                                't',
                            ),
                            Literal(
                                '_',
                            ),
                            Literal(
                                '_',
                            ),
                            Literal(
                                '.',
                            ),
                            Literal(
                                'p',
                            ),
                            Literal(
                                'y',
                            ),
                        ],
                    ),
                },
                re: (?-u)^/Users/lucabaggi/Documents/dev/futura\-aws/__init__\.py$,
            },
            GlobMatcher {
                pat: Glob {
                    glob: "__init__.py",
                    re: "(?-u)^__init__\\.py$",
                    opts: GlobOptions {
                        case_insensitive: false,
                        literal_separator: false,
                        backslash_escape: true,
                    },
                    tokens: Tokens(
                        [
                            Literal(
                                '_',
                            ),
                            Literal(
                                '_',
                            ),
                            Literal(
                                'i',
                            ),
                            Literal(
                                'n',
                            ),
                            Literal(
                                'i',
                            ),
                            Literal(
                                't',
                            ),
                            Literal(
                                '_',
                            ),
                            Literal(
                                '_',
                            ),
                            Literal(
                                '.',
                            ),
                            Literal(
                                'p',
                            ),
                            Literal(
                                'y',
                            ),
                        ],
                    ),
                },
                re: (?-u)^__init__\.py$,
            },
            {
                F401,
            },
        ),
    ],
    required_version: None,
    respect_gitignore: true,
    show_source: false,
    src: [
        "/Users/lucabaggi/Documents/dev/futura-aws",
    ],
    target_version: Py310,
    task_tags: [
        "TODO",
        "FIXME",
        "XXX",
    ],
    update_check: true,
    flake8_annotations: Settings {
        mypy_init_return: false,
        suppress_dummy_args: false,
        suppress_none_returning: false,
        allow_star_arg_any: false,
    },
    flake8_bandit: Settings {
        hardcoded_tmp_directory: [
            "/tmp",
            "/var/tmp",
            "/dev/shm",
        ],
    },
    flake8_bugbear: Settings {
        extend_immutable_calls: [],
    },
    flake8_errmsg: Settings {
        max_string_length: 0,
    },
    flake8_import_conventions: Settings {
        aliases: {
            "numpy": "np",
            "matplotlib.pyplot": "plt",
            "pandas": "pd",
            "altair": "alt",
            "seaborn": "sns",
        },
    },
    flake8_pytest_style: Settings {
        fixture_parentheses: true,
        parametrize_names_type: Tuple,
        parametrize_values_type: List,
        parametrize_values_row_type: Tuple,
        raises_require_match_for: [
            "BaseException",
            "Exception",
            "ValueError",
            "OSError",
            "IOError",
            "EnvironmentError",
            "socket.error",
        ],
        raises_extend_require_match_for: [],
        mark_parentheses: true,
    },
    flake8_quotes: Settings {
        inline_quotes: Double,
        multiline_quotes: Double,
        docstring_quotes: Double,
        avoid_escape: true,
    },
    flake8_tidy_imports: Settings {
        ban_relative_imports: Parents,
        banned_api: {},
    },
    flake8_unused_arguments: Settings {
        ignore_variadic_names: false,
    },
    isort: Settings {
        combine_as_imports: false,
        force_wrap_aliases: false,
        split_on_trailing_comma: true,
        force_single_line: false,
        single_line_exclusions: {},
        known_first_party: {},
        known_third_party: {},
        order_by_type: true,
        extra_standard_library: {},
    },
    mccabe: Settings {
        max_complexity: 10,
    },
    pep8_naming: Settings {
        ignore_names: [
            "setUp",
            "tearDown",
            "setUpClass",
            "tearDownClass",
            "setUpModule",
            "tearDownModule",
            "asyncSetUp",
            "asyncTearDown",
            "setUpTestData",
            "failureException",
            "longMessage",
            "maxDiff",
        ],
        classmethod_decorators: [
            "classmethod",
        ],
        staticmethod_decorators: [
            "staticmethod",
        ],
    },
    pycodestyle: Settings {
        ignore_overlong_task_comments: false,
    },
    pydocstyle: Settings {
        convention: Some(
            Numpy,
        ),
    },
    pyupgrade: Settings {
        keep_runtime_typing: false,
    },
}
```

</details>

Please do let me know how can I be of more help! Thank you for your time.

---

_Comment by @charliermarsh on 2023-01-06 01:24_

I think D205 falls into the category of "sometimes fixable", in that we can fix it if there are _too many_ blanks lines but not _too few_ (the latter kept triggering false positives). There's some discussion here: https://github.com/charliermarsh/ruff/issues/1084.

Given that caveat, does it seem like Ruff is working as intended? Or no?

(Really appreciate all the settings you included here. If you want to include the docstring, that'd be great too! Thank you!)


---

_Label `question` added by @charliermarsh on 2023-01-06 01:24_

---

_Comment by @not-my-profile on 2023-01-06 04:02_

I think we should document whether autofixes always work or only sometimes work and in that case document when the autofix isn't available ... I added that as a bullet point to #1467.

---

_Referenced in [astral-sh/ruff#1682](../../astral-sh/ruff/pulls/1682.md) on 2023-01-06 04:19_

---

_Comment by @charliermarsh on 2023-01-06 04:20_

For now, to marginally improve things, we'll no longer tell you the rule is "potentially fixable" in the command output if the error is the non-fixable case.

---

_Closed by @charliermarsh on 2023-01-06 04:20_

---

_Comment by @baggiponte on 2023-01-06 10:29_

Thank you for your prompt reply!

> I think D205 falls into the category of "sometimes fixable", in that we can fix it if there are too many blanks lines but not too few (the latter kept triggering false positives).
> [...]
> (Really appreciate all the settings you included here. If you want to include the docstring, that'd be great too! Thank you!)

Sure, it basically was this:

```
$ ruff src --show-source
src/futura_aws/s3.py:144:9: D205 1 blank line required between summary line and description
    |
144 |           """Create a new bucket. Errors if the bucket already exists,
    |   _________^
145 | |         or the name is not available.
146 | |
147 | |         """
    | |___________^ D205
    |
      = help: Insert single blank line

Found 1 error(s).
```

So, as you said, there were too few lines. As a side note: this happened because I split the line on two separate rows because it was too long - sometimes I wish `ruff` could run `black` too, or could autofix E501 (even in docstrings, where `black` does not work).

> Given that caveat, does it seem like Ruff is working as intended? Or no?

Of course, always amazing!




---
