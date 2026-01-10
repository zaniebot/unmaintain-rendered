```yaml
number: 8334
title: "Create `Display` implementation for `Settings`"
type: issue
state: closed
author: MichaReiser
labels:
  - cli
assignees: []
created_at: 2023-10-30T02:49:23Z
updated_at: 2024-01-12T19:30:30Z
url: https://github.com/astral-sh/ruff/issues/8334
synced_at: 2026-01-10T11:09:50Z
```

# Create `Display` implementation for `Settings`

---

_Issue opened by @MichaReiser on 2023-10-30 02:49_

`ruff check . --show-settings` prints the settings using the `Debug` implementation. The result is that it prints our internal representation, which is hard to read (even impossible)

https://github.com/astral-sh/ruff/blob/fe485d791cc206391103cb44e53d9c77e2ab42fc/crates/ruff_cli/src/commands/show_settings.rs#L38

<details>
<summary>Current output</summary>
<pre>
Resolved settings for: "/Users/micha/astral/ruff/crates/flake8_to_ruff/examples/cryptography/pyproject.toml"
Settings path: "/Users/micha/astral/ruff/pyproject.toml"
Settings {
    cache_dir: "/Users/micha/astral/ruff/.ruff_cache",
    fix: false,
    fix_only: false,
    unsafe_fixes: Disabled,
    output_format: Text,
    show_fixes: false,
    show_source: false,
    file_resolver: FileResolverSettings {
        exclude: FilePatternSet {
            set: GlobSet {
                len: 25,
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
                                    103,
                                    105,
                                    116,
                                    45,
                                    114,
                                    101,
                                    119,
                                    114,
                                    105,
                                    116,
                                    101,
                                ]: [
                                    4,
                                ],
                                [
                                    46,
                                    104,
                                    103,
                                ]: [
                                    5,
                                ],
                                [
                                    46,
                                    105,
                                    112,
                                    121,
                                    110,
                                    98,
                                    95,
                                    99,
                                    104,
                                    101,
                                    99,
                                    107,
                                    112,
                                    111,
                                    105,
                                    110,
                                    116,
                                    115,
                                ]: [
                                    6,
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
                                    7,
                                ],
                                [
                                    46,
                                    110,
                                    111,
                                    120,
                                ]: [
                                    8,
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
                                    9,
                                ],
                                [
                                    46,
                                    112,
                                    121,
                                    101,
                                    110,
                                    118,
                                ]: [
                                    10,
                                ],
                                [
                                    46,
                                    112,
                                    121,
                                    116,
                                    101,
                                    115,
                                    116,
                                    95,
                                    99,
                                    97,
                                    99,
                                    104,
                                    101,
                                ]: [
                                    11,
                                ],
                                [
                                    46,
                                    112,
                                    121,
                                    116,
                                    121,
                                    112,
                                    101,
                                ]: [
                                    12,
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
                                    13,
                                ],
                                [
                                    46,
                                    115,
                                    118,
                                    110,
                                ]: [
                                    14,
                                ],
                                [
                                    46,
                                    116,
                                    111,
                                    120,
                                ]: [
                                    15,
                                ],
                                [
                                    46,
                                    118,
                                    101,
                                    110,
                                    118,
                                ]: [
                                    16,
                                ],
                                [
                                    46,
                                    118,
                                    115,
                                    99,
                                    111,
                                    100,
                                    101,
                                ]: [
                                    17,
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
                                    18,
                                ],
                                [
                                    95,
                                    98,
                                    117,
                                    105,
                                    108,
                                    100,
                                ]: [
                                    19,
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
                                    20,
                                ],
                                [
                                    98,
                                    117,
                                    105,
                                    108,
                                    100,
                                ]: [
                                    21,
                                ],
                                [
                                    100,
                                    105,
                                    115,
                                    116,
                                ]: [
                                    22,
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
                                    23,
                                ],
                                [
                                    118,
                                    101,
                                    110,
                                    118,
                                ]: [
                                    24,
                                ],
                            },
                        ),
                    ),
                    Suffix(
                        SuffixStrategy {
                            matcher: AhoCorasick(
                                dfa::DFA(
                                D 000000: \x00 => 0
                                F 000001:
                                 >000002: \x00 => 2
                                  000003: \x00 => 0
                                match kind: Standard
                                prefilter: false
                                state length: 4
                                pattern length: 0
                                shortest pattern length: 18446744073709551615
                                longest pattern length: 0
                                alphabet length: 1
                                stride: 1
                                byte classes: ByteClasses(0 => [0-255])
                                memory usage: 16
                                )
                                ,
                            ),
                            map: [],
                            longest: 0,
                        },
                    ),
                    Prefix(
                        PrefixStrategy {
                            matcher: AhoCorasick(
                                dfa::DFA(
                                D 000000: \x00 => 0
                                F 000001:
                                 >000002: \x00 => 2
                                  000003: \x00 => 0
                                match kind: Standard
                                prefilter: false
                                state length: 4
                                pattern length: 0
                                shortest pattern length: 18446744073709551615
                                longest pattern length: 0
                                alphabet length: 1
                                stride: 1
                                byte classes: ByteClasses(0 => [0-255])
                                memory usage: 16
                                )
                                ,
                            ),
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
            cache_key: 10629084736300657394,
        },
        extend_exclude: FilePatternSet {
            set: GlobSet {
                len: 2,
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
                                    47,
                                    85,
                                    115,
                                    101,
                                    114,
                                    115,
                                    47,
                                    109,
                                    105,
                                    99,
                                    104,
                                    97,
                                    47,
                                    97,
                                    115,
                                    116,
                                    114,
                                    97,
                                    108,
                                    47,
                                    114,
                                    117,
                                    102,
                                    102,
                                    47,
                                    99,
                                    114,
                                    97,
                                    116,
                                    101,
                                    115,
                                    47,
                                    114,
                                    117,
                                    102,
                                    102,
                                    95,
                                    108,
                                    105,
                                    110,
                                    116,
                                    101,
                                    114,
                                    47,
                                    114,
                                    101,
                                    115,
                                    111,
                                    117,
                                    114,
                                    99,
                                    101,
                                    115,
                                ]: [
                                    0,
                                ],
                                [
                                    47,
                                    85,
                                    115,
                                    101,
                                    114,
                                    115,
                                    47,
                                    109,
                                    105,
                                    99,
                                    104,
                                    97,
                                    47,
                                    97,
                                    115,
                                    116,
                                    114,
                                    97,
                                    108,
                                    47,
                                    114,
                                    117,
                                    102,
                                    102,
                                    47,
                                    99,
                                    114,
                                    97,
                                    116,
                                    101,
                                    115,
                                    47,
                                    114,
                                    117,
                                    102,
                                    102,
                                    95,
                                    112,
                                    121,
                                    116,
                                    104,
                                    111,
                                    110,
                                    95,
                                    102,
                                    111,
                                    114,
                                    109,
                                    97,
                                    116,
                                    116,
                                    101,
                                    114,
                                    47,
                                    114,
                                    101,
                                    115,
                                    111,
                                    117,
                                    114,
                                    99,
                                    101,
                                    115,
                                ]: [
                                    1,
                                ],
                            },
                        ),
                    ),
                    Suffix(
                        SuffixStrategy {
                            matcher: AhoCorasick(
                                dfa::DFA(
                                D 000000: \x00 => 0
                                F 000001:
                                 >000002: \x00 => 2
                                  000003: \x00 => 0
                                match kind: Standard
                                prefilter: false
                                state length: 4
                                pattern length: 0
                                shortest pattern length: 18446744073709551615
                                longest pattern length: 0
                                alphabet length: 1
                                stride: 1
                                byte classes: ByteClasses(0 => [0-255])
                                memory usage: 16
                                )
                                ,
                            ),
                            map: [],
                            longest: 0,
                        },
                    ),
                    Prefix(
                        PrefixStrategy {
                            matcher: AhoCorasick(
                                dfa::DFA(
                                D 000000: \x00 => 0
                                F 000001:
                                 >000002: \x00 => 2
                                  000003: \x00 => 0
                                match kind: Standard
                                prefilter: false
                                state length: 4
                                pattern length: 0
                                shortest pattern length: 18446744073709551615
                                longest pattern length: 0
                                alphabet length: 1
                                stride: 1
                                byte classes: ByteClasses(0 => [0-255])
                                memory usage: 16
                                )
                                ,
                            ),
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
            cache_key: 10840170291812037825,
        },
        force_exclude: false,
        include: FilePatternSet {
            set: GlobSet {
                len: 3,
                strats: [
                    Extension(
                        ExtensionStrategy(
                            {
                                [
                                    46,
                                    112,
                                    121,
                                    105,
                                ]: [
                                    1,
                                ],
                                [
                                    46,
                                    112,
                                    121,
                                ]: [
                                    0,
                                ],
                            },
                        ),
                    ),
                    BasenameLiteral(
                        BasenameLiteralStrategy(
                            {
                                [
                                    112,
                                    121,
                                    112,
                                    114,
                                    111,
                                    106,
                                    101,
                                    99,
                                    116,
                                    46,
                                    116,
                                    111,
                                    109,
                                    108,
                                ]: [
                                    2,
                                ],
                            },
                        ),
                    ),
                    Literal(
                        LiteralStrategy(
                            {},
                        ),
                    ),
                    Suffix(
                        SuffixStrategy {
                            matcher: AhoCorasick(
                                dfa::DFA(
                                D 000000: \x00 => 0
                                F 000001:
                                 >000002: \x00 => 2
                                  000003: \x00 => 0
                                match kind: Standard
                                prefilter: false
                                state length: 4
                                pattern length: 0
                                shortest pattern length: 18446744073709551615
                                longest pattern length: 0
                                alphabet length: 1
                                stride: 1
                                byte classes: ByteClasses(0 => [0-255])
                                memory usage: 16
                                )
                                ,
                            ),
                            map: [],
                            longest: 0,
                        },
                    ),
                    Prefix(
                        PrefixStrategy {
                            matcher: AhoCorasick(
                                dfa::DFA(
                                D 000000: \x00 => 0
                                F 000001:
                                 >000002: \x00 => 2
                                  000003: \x00 => 0
                                match kind: Standard
                                prefilter: false
                                state length: 4
                                pattern length: 0
                                shortest pattern length: 18446744073709551615
                                longest pattern length: 0
                                alphabet length: 1
                                stride: 1
                                byte classes: ByteClasses(0 => [0-255])
                                memory usage: 16
                                )
                                ,
                            ),
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
            cache_key: 787281028166742793,
        },
        extend_include: FilePatternSet {
            set: GlobSet {
                len: 0,
                strats: [],
            },
            cache_key: 14492805990617963705,
        },
        respect_gitignore: true,
        project_root: "/Users/micha/astral/ruff",
    },
    linter: LinterSettings {
        exclude: FilePatternSet {
            set: GlobSet {
                len: 0,
                strats: [],
            },
            cache_key: 14492805990617963705,
        },
        project_root: "/Users/micha/astral/ruff",
        rules: RuleTable {
            enabled: {
                MultipleImportsOnOneLine,
                ModuleImportNotAtTopOfFile,
                MultipleStatementsOnOneLineColon,
                MultipleStatementsOnOneLineSemicolon,
                UselessSemicolon,
                NoneComparison,
                TrueFalseComparison,
                NotInTest,
                NotIsTest,
                TypeComparison,
                BareExcept,
                LambdaAssignment,
                AmbiguousVariableName,
                AmbiguousClassName,
                AmbiguousFunctionName,
                IOError,
                SyntaxError,
                UnusedImport,
                ImportShadowedByLoopVar,
                UndefinedLocalWithImportStar,
                LateFutureImport,
                UndefinedLocalWithImportStarUsage,
                UndefinedLocalWithNestedImportStarUsage,
                FutureFeatureNotDefined,
                PercentFormatInvalidFormat,
                PercentFormatExpectedMapping,
                PercentFormatExpectedSequence,
                PercentFormatExtraNamedArguments,
                PercentFormatMissingArgument,
                PercentFormatMixedPositionalAndNamed,
                PercentFormatPositionalCountMismatch,
                PercentFormatStarRequiresSequence,
                PercentFormatUnsupportedFormatCharacter,
                StringDotFormatInvalidFormat,
                StringDotFormatExtraNamedArguments,
                StringDotFormatExtraPositionalArguments,
                StringDotFormatMissingArguments,
                StringDotFormatMixingAutomatic,
                FStringMissingPlaceholders,
                MultiValueRepeatedKeyLiteral,
                MultiValueRepeatedKeyVariable,
                ExpressionsInStarAssignment,
                MultipleStarredExpressions,
                AssertTuple,
                IsLiteral,
                InvalidPrintSyntax,
                IfTuple,
                BreakOutsideLoop,
                ContinueOutsideLoop,
                YieldOutsideFunction,
                ReturnOutsideFunction,
                DefaultExceptNotLast,
                ForwardAnnotationSyntaxError,
                RedefinedWhileUnused,
                UndefinedName,
                UndefinedExport,
                UndefinedLocal,
                UnusedVariable,
                UnusedAnnotation,
                RaiseNotImplemented,
            },
            should_fix: {
                MultipleImportsOnOneLine,
                ModuleImportNotAtTopOfFile,
                MultipleStatementsOnOneLineColon,
                MultipleStatementsOnOneLineSemicolon,
                UselessSemicolon,
                NoneComparison,
                TrueFalseComparison,
                NotInTest,
                NotIsTest,
                TypeComparison,
                BareExcept,
                LambdaAssignment,
                AmbiguousVariableName,
                AmbiguousClassName,
                AmbiguousFunctionName,
                IOError,
                SyntaxError,
                UnusedImport,
                ImportShadowedByLoopVar,
                UndefinedLocalWithImportStar,
                LateFutureImport,
                UndefinedLocalWithImportStarUsage,
                UndefinedLocalWithNestedImportStarUsage,
                FutureFeatureNotDefined,
                PercentFormatInvalidFormat,
                PercentFormatExpectedMapping,
                PercentFormatExpectedSequence,
                PercentFormatExtraNamedArguments,
                PercentFormatMissingArgument,
                PercentFormatMixedPositionalAndNamed,
                PercentFormatPositionalCountMismatch,
                PercentFormatStarRequiresSequence,
                PercentFormatUnsupportedFormatCharacter,
                StringDotFormatInvalidFormat,
                StringDotFormatExtraNamedArguments,
                StringDotFormatExtraPositionalArguments,
                StringDotFormatMissingArguments,
                StringDotFormatMixingAutomatic,
                FStringMissingPlaceholders,
                MultiValueRepeatedKeyLiteral,
                MultiValueRepeatedKeyVariable,
                ExpressionsInStarAssignment,
                MultipleStarredExpressions,
                AssertTuple,
                IsLiteral,
                InvalidPrintSyntax,
                IfTuple,
                BreakOutsideLoop,
                ContinueOutsideLoop,
                YieldOutsideFunction,
                ReturnOutsideFunction,
                DefaultExceptNotLast,
                ForwardAnnotationSyntaxError,
                RedefinedWhileUnused,
                UndefinedName,
                UndefinedExport,
                UndefinedLocal,
                UnusedVariable,
                UnusedAnnotation,
                RaiseNotImplemented,
            },
        },
        per_file_ignores: [],
        extend_unsafe_fixes: {},
        extend_safe_fixes: {},
        target_version: Py37,
        preview: Disabled,
        explicit_preview_rules: false,
        allowed_confusables: {},
        builtins: [],
        dummy_variable_rgx: Regex(
            "^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$",
        ),
        external: [],
        ignore_init_module_imports: false,
        logger_objects: [],
        namespace_packages: [],
        src: [
            "/Users/micha/astral/ruff",
        ],
        tab_size: IndentWidth(
            4,
        ),
        line_length: LineLength(
            88,
        ),
        task_tags: [
            "TODO",
            "FIXME",
            "XXX",
        ],
        typing_modules: [],
        flake8_annotations: Settings {
            mypy_init_return: false,
            suppress_dummy_args: false,
            suppress_none_returning: false,
            allow_star_arg_any: false,
            ignore_fully_untyped: false,
        },
        flake8_bandit: Settings {
            hardcoded_tmp_directory: [
                "/tmp",
                "/var/tmp",
                "/dev/shm",
            ],
            check_typed_exception: false,
        },
        flake8_bugbear: Settings {
            extend_immutable_calls: [],
        },
        flake8_builtins: Settings {
            builtins_ignorelist: [],
        },
        flake8_comprehensions: Settings {
            allow_dict_calls_with_keyword_arguments: false,
        },
        flake8_copyright: Settings {
            notice_rgx: Regex(
                "(?i)Copyright\\s+(\\(C\\)\\s+)?\\d{4}(-\\d{4})*",
            ),
            author: None,
            min_file_size: 0,
        },
        flake8_errmsg: Settings {
            max_string_length: 0,
        },
        flake8_gettext: Settings {
            functions_names: [
                "_",
                "gettext",
                "ngettext",
            ],
        },
        flake8_implicit_str_concat: Settings {
            allow_multiline: true,
        },
        flake8_import_conventions: Settings {
            aliases: {
                "matplotlib": "mpl",
                "matplotlib.pyplot": "plt",
                "pandas": "pd",
                "seaborn": "sns",
                "tensorflow": "tf",
                "networkx": "nx",
                "plotly.express": "px",
                "polars": "pl",
                "numpy": "np",
                "panel": "pn",
                "pyarrow": "pa",
                "altair": "alt",
                "tkinter": "tk",
                "holoviews": "hv",
            },
            banned_aliases: {},
            banned_from: {},
        },
        flake8_pytest_style: Settings {
            fixture_parentheses: true,
            parametrize_names_type: Tuple,
            parametrize_values_type: List,
            parametrize_values_row_type: Tuple,
            raises_require_match_for: [
                Pattern {
                    original: "BaseException",
                    tokens: [
                        Char(
                            'B',
                        ),
                        Char(
                            'a',
                        ),
                        Char(
                            's',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            'E',
                        ),
                        Char(
                            'x',
                        ),
                        Char(
                            'c',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            'p',
                        ),
                        Char(
                            't',
                        ),
                        Char(
                            'i',
                        ),
                        Char(
                            'o',
                        ),
                        Char(
                            'n',
                        ),
                    ],
                    is_recursive: false,
                },
                Pattern {
                    original: "Exception",
                    tokens: [
                        Char(
                            'E',
                        ),
                        Char(
                            'x',
                        ),
                        Char(
                            'c',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            'p',
                        ),
                        Char(
                            't',
                        ),
                        Char(
                            'i',
                        ),
                        Char(
                            'o',
                        ),
                        Char(
                            'n',
                        ),
                    ],
                    is_recursive: false,
                },
                Pattern {
                    original: "ValueError",
                    tokens: [
                        Char(
                            'V',
                        ),
                        Char(
                            'a',
                        ),
                        Char(
                            'l',
                        ),
                        Char(
                            'u',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            'E',
                        ),
                        Char(
                            'r',
                        ),
                        Char(
                            'r',
                        ),
                        Char(
                            'o',
                        ),
                        Char(
                            'r',
                        ),
                    ],
                    is_recursive: false,
                },
                Pattern {
                    original: "OSError",
                    tokens: [
                        Char(
                            'O',
                        ),
                        Char(
                            'S',
                        ),
                        Char(
                            'E',
                        ),
                        Char(
                            'r',
                        ),
                        Char(
                            'r',
                        ),
                        Char(
                            'o',
                        ),
                        Char(
                            'r',
                        ),
                    ],
                    is_recursive: false,
                },
                Pattern {
                    original: "IOError",
                    tokens: [
                        Char(
                            'I',
                        ),
                        Char(
                            'O',
                        ),
                        Char(
                            'E',
                        ),
                        Char(
                            'r',
                        ),
                        Char(
                            'r',
                        ),
                        Char(
                            'o',
                        ),
                        Char(
                            'r',
                        ),
                    ],
                    is_recursive: false,
                },
                Pattern {
                    original: "EnvironmentError",
                    tokens: [
                        Char(
                            'E',
                        ),
                        Char(
                            'n',
                        ),
                        Char(
                            'v',
                        ),
                        Char(
                            'i',
                        ),
                        Char(
                            'r',
                        ),
                        Char(
                            'o',
                        ),
                        Char(
                            'n',
                        ),
                        Char(
                            'm',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            'n',
                        ),
                        Char(
                            't',
                        ),
                        Char(
                            'E',
                        ),
                        Char(
                            'r',
                        ),
                        Char(
                            'r',
                        ),
                        Char(
                            'o',
                        ),
                        Char(
                            'r',
                        ),
                    ],
                    is_recursive: false,
                },
                Pattern {
                    original: "socket.error",
                    tokens: [
                        Char(
                            's',
                        ),
                        Char(
                            'o',
                        ),
                        Char(
                            'c',
                        ),
                        Char(
                            'k',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            't',
                        ),
                        Char(
                            '.',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            'r',
                        ),
                        Char(
                            'r',
                        ),
                        Char(
                            'o',
                        ),
                        Char(
                            'r',
                        ),
                    ],
                    is_recursive: false,
                },
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
        flake8_self: Settings {
            ignore_names: [
                "_make",
                "_asdict",
                "_replace",
                "_fields",
                "_field_defaults",
                "_name_",
                "_value_",
            ],
        },
        flake8_tidy_imports: Settings {
            ban_relative_imports: Parents,
            banned_api: {},
            banned_module_level_imports: [],
        },
        flake8_type_checking: Settings {
            strict: false,
            exempt_modules: [
                "typing",
            ],
            runtime_evaluated_base_classes: [],
            runtime_evaluated_decorators: [],
        },
        flake8_unused_arguments: Settings {
            ignore_variadic_names: false,
        },
        isort: Settings {
            required_imports: {},
            combine_as_imports: false,
            force_single_line: false,
            force_sort_within_sections: false,
            case_sensitive: false,
            force_wrap_aliases: false,
            force_to_top: {},
            known_modules: KnownModules {
                known: [],
                has_submodules: false,
            },
            detect_same_package: true,
            order_by_type: true,
            relative_imports_order: FurthestToClosest,
            single_line_exclusions: {},
            split_on_trailing_comma: true,
            classes: {},
            constants: {},
            variables: {},
            no_lines_before: {},
            lines_after_imports: -1,
            lines_between_types: 0,
            forced_separate: [],
            section_order: [
                Known(
                    Future,
                ),
                Known(
                    StandardLibrary,
                ),
                Known(
                    ThirdParty,
                ),
                Known(
                    FirstParty,
                ),
                Known(
                    LocalFolder,
                ),
            ],
        },
        mccabe: Settings {
            max_complexity: 10,
        },
        pep8_naming: Settings {
            ignore_names: [
                Pattern {
                    original: "setUp",
                    tokens: [
                        Char(
                            's',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            't',
                        ),
                        Char(
                            'U',
                        ),
                        Char(
                            'p',
                        ),
                    ],
                    is_recursive: false,
                },
                Pattern {
                    original: "tearDown",
                    tokens: [
                        Char(
                            't',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            'a',
                        ),
                        Char(
                            'r',
                        ),
                        Char(
                            'D',
                        ),
                        Char(
                            'o',
                        ),
                        Char(
                            'w',
                        ),
                        Char(
                            'n',
                        ),
                    ],
                    is_recursive: false,
                },
                Pattern {
                    original: "setUpClass",
                    tokens: [
                        Char(
                            's',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            't',
                        ),
                        Char(
                            'U',
                        ),
                        Char(
                            'p',
                        ),
                        Char(
                            'C',
                        ),
                        Char(
                            'l',
                        ),
                        Char(
                            'a',
                        ),
                        Char(
                            's',
                        ),
                        Char(
                            's',
                        ),
                    ],
                    is_recursive: false,
                },
                Pattern {
                    original: "tearDownClass",
                    tokens: [
                        Char(
                            't',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            'a',
                        ),
                        Char(
                            'r',
                        ),
                        Char(
                            'D',
                        ),
                        Char(
                            'o',
                        ),
                        Char(
                            'w',
                        ),
                        Char(
                            'n',
                        ),
                        Char(
                            'C',
                        ),
                        Char(
                            'l',
                        ),
                        Char(
                            'a',
                        ),
                        Char(
                            's',
                        ),
                        Char(
                            's',
                        ),
                    ],
                    is_recursive: false,
                },
                Pattern {
                    original: "setUpModule",
                    tokens: [
                        Char(
                            's',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            't',
                        ),
                        Char(
                            'U',
                        ),
                        Char(
                            'p',
                        ),
                        Char(
                            'M',
                        ),
                        Char(
                            'o',
                        ),
                        Char(
                            'd',
                        ),
                        Char(
                            'u',
                        ),
                        Char(
                            'l',
                        ),
                        Char(
                            'e',
                        ),
                    ],
                    is_recursive: false,
                },
                Pattern {
                    original: "tearDownModule",
                    tokens: [
                        Char(
                            't',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            'a',
                        ),
                        Char(
                            'r',
                        ),
                        Char(
                            'D',
                        ),
                        Char(
                            'o',
                        ),
                        Char(
                            'w',
                        ),
                        Char(
                            'n',
                        ),
                        Char(
                            'M',
                        ),
                        Char(
                            'o',
                        ),
                        Char(
                            'd',
                        ),
                        Char(
                            'u',
                        ),
                        Char(
                            'l',
                        ),
                        Char(
                            'e',
                        ),
                    ],
                    is_recursive: false,
                },
                Pattern {
                    original: "asyncSetUp",
                    tokens: [
                        Char(
                            'a',
                        ),
                        Char(
                            's',
                        ),
                        Char(
                            'y',
                        ),
                        Char(
                            'n',
                        ),
                        Char(
                            'c',
                        ),
                        Char(
                            'S',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            't',
                        ),
                        Char(
                            'U',
                        ),
                        Char(
                            'p',
                        ),
                    ],
                    is_recursive: false,
                },
                Pattern {
                    original: "asyncTearDown",
                    tokens: [
                        Char(
                            'a',
                        ),
                        Char(
                            's',
                        ),
                        Char(
                            'y',
                        ),
                        Char(
                            'n',
                        ),
                        Char(
                            'c',
                        ),
                        Char(
                            'T',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            'a',
                        ),
                        Char(
                            'r',
                        ),
                        Char(
                            'D',
                        ),
                        Char(
                            'o',
                        ),
                        Char(
                            'w',
                        ),
                        Char(
                            'n',
                        ),
                    ],
                    is_recursive: false,
                },
                Pattern {
                    original: "setUpTestData",
                    tokens: [
                        Char(
                            's',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            't',
                        ),
                        Char(
                            'U',
                        ),
                        Char(
                            'p',
                        ),
                        Char(
                            'T',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            's',
                        ),
                        Char(
                            't',
                        ),
                        Char(
                            'D',
                        ),
                        Char(
                            'a',
                        ),
                        Char(
                            't',
                        ),
                        Char(
                            'a',
                        ),
                    ],
                    is_recursive: false,
                },
                Pattern {
                    original: "failureException",
                    tokens: [
                        Char(
                            'f',
                        ),
                        Char(
                            'a',
                        ),
                        Char(
                            'i',
                        ),
                        Char(
                            'l',
                        ),
                        Char(
                            'u',
                        ),
                        Char(
                            'r',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            'E',
                        ),
                        Char(
                            'x',
                        ),
                        Char(
                            'c',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            'p',
                        ),
                        Char(
                            't',
                        ),
                        Char(
                            'i',
                        ),
                        Char(
                            'o',
                        ),
                        Char(
                            'n',
                        ),
                    ],
                    is_recursive: false,
                },
                Pattern {
                    original: "longMessage",
                    tokens: [
                        Char(
                            'l',
                        ),
                        Char(
                            'o',
                        ),
                        Char(
                            'n',
                        ),
                        Char(
                            'g',
                        ),
                        Char(
                            'M',
                        ),
                        Char(
                            'e',
                        ),
                        Char(
                            's',
                        ),
                        Char(
                            's',
                        ),
                        Char(
                            'a',
                        ),
                        Char(
                            'g',
                        ),
                        Char(
                            'e',
                        ),
                    ],
                    is_recursive: false,
                },
                Pattern {
                    original: "maxDiff",
                    tokens: [
                        Char(
                            'm',
                        ),
                        Char(
                            'a',
                        ),
                        Char(
                            'x',
                        ),
                        Char(
                            'D',
                        ),
                        Char(
                            'i',
                        ),
                        Char(
                            'f',
                        ),
                        Char(
                            'f',
                        ),
                    ],
                    is_recursive: false,
                },
            ],
            classmethod_decorators: [],
            staticmethod_decorators: [],
        },
        pycodestyle: Settings {
            max_line_length: LineLength(
                88,
            ),
            max_doc_length: None,
            ignore_overlong_task_comments: false,
        },
        pydocstyle: Settings {
            convention: None,
            ignore_decorators: {},
            property_decorators: {},
        },
        pyflakes: Settings {
            extend_generics: [],
        },
        pylint: Settings {
            allow_magic_value_types: [
                Str,
                Bytes,
            ],
            max_args: 5,
            max_returns: 6,
            max_bool_expr: 5,
            max_branches: 12,
            max_statements: 50,
            max_public_methods: 20,
        },
        pyupgrade: Settings {
            keep_runtime_typing: false,
        },
    },
    formatter: FormatterSettings {
        exclude: FilePatternSet {
            set: GlobSet {
                len: 0,
                strats: [],
            },
            cache_key: 14492805990617963705,
        },
        preview: Disabled,
        line_width: LineWidth(
            88,
        ),
        indent_style: Space,
        indent_width: IndentWidth(
            4,
        ),
        quote_style: Double,
        magic_trailing_comma: Respect,
        line_ending: Auto,
    },
}
</pre>
</details>

We should instead use the Setting's (not yet existing) `Display` implementation and ensure we print human readable values for each setting. 

---

_Label `cli` added by @MichaReiser on 2023-10-30 02:49_

---

_Comment by @dhruvmanila on 2023-10-30 03:02_

I agree. Do you think we should display it in a similar format `{ key: value }` or would it be more useful to have it in the toml format?

---

_Comment by @MichaReiser on 2023-10-30 03:06_

Not sure. TOML format is probably more work and might be misleading because users could assume it is the loaded configuration as stored on disk whereas it is the resolved/merged configuration. We would also need to retain whether the user used `tool.ruff` or `ruff`. 

That's why I would prefer a format that's different enough for users to understand it is not their `.toml` file. Although it has the advantage that users are familiar with the toml syntax.

---

_Comment by @MichaReiser on 2023-10-31 21:42_

Related: https://github.com/astral-sh/ruff/discussions/8396

---

_Comment by @zanieb on 2023-11-01 03:49_

There's also some discussion on this interface in my [internal CLI document](https://www.notion.so/astral-sh/CLI-Design-b415f0ff925643f08bdb5be9bb949e83?pvs=4#bf65e78aaf6a4445915f035bab29a0fa)

---

_Comment by @dhruvmanila on 2023-11-01 04:14_

I see you've mentioned human readable format but do we also want it to be machine readable? Maybe it could be useful in some integrations.

Personally, I find the YAML format easier to read in such scenarios which showcases the hierarchy and the `key:value` pairs in a simplified format. GitHub uses it to display the settings for each action step:
```yaml
with:
    repository: astral-sh/ruff
    token: ***
    ssh-strict: true
    # ...
env:
    CARGO_INCREMENTAL: 0
    CARGO_NET_RETRY: 10
    # ...
```

---

_Comment by @zanieb on 2023-11-01 04:19_

I think for this use-case a flat display would be best e.g. 

```
fix = true
formatter.preview = false
formatter.line-width = 88
....
```

This would be valid toml but different from the format generally used by users.

---

_Comment by @dhruvmanila on 2023-11-01 04:39_

Maybe it's just me but I find that there are too many duplications in options at the same hierarchy 😅 

```yaml
fix: true
format:
  preview: false
  line-width: 88
...
```

Although this does mean that for options common to multiple sections, the section name is above a few lines where the flat display would be much better.

At the end, I'm fine with anything as long as it's generally readable :)

---

_Comment by @zanieb on 2023-11-01 04:49_

Yeah I worry the section name is lost in that format and the most important use-case for me is actually to be able to grep the result for the setting I care about which does not work with nesting.

---

_Comment by @snowsignal on 2024-01-11 03:52_

I'm looking into this - my plan is to use @zanieb's formatting suggestion. Having a nested display would look nice, but I think `grep`-ability is more important in this specific case.

---

_Assigned to @snowsignal by @zanieb on 2024-01-11 03:58_

---

_Closed by @charliermarsh on 2024-01-12 19:30_

---
