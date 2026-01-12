```yaml
number: 12728
title: Update dependency black to v24
type: pull_request
state: merged
author: renovate
labels:
  - internal
  - security
assignees: []
merged: true
base: main
head: renovate/pypi-black-vulnerability
created_at: 2024-08-07T07:42:19Z
updated_at: 2024-08-10T12:34:39Z
url: https://github.com/astral-sh/ruff/pull/12728
synced_at: 2026-01-12T15:55:42Z
```

# Update dependency black to v24

---

_@renovate_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [black](https://togithub.com/psf/black) ([changelog](https://togithub.com/psf/black/blob/main/CHANGES.md)) | `==23.10.0` -> `==24.3.0` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/black/24.3.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/black/24.3.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/black/23.10.0/24.3.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/black/23.10.0/24.3.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

### GitHub Vulnerability Alerts

#### [CVE-2024-21503](https://nvd.nist.gov/vuln/detail/CVE-2024-21503)

Versions of the package black before 24.3.0 are vulnerable to Regular Expression Denial of Service (ReDoS) via the lines_with_leading_tabs_expanded function in the strings.py file. An attacker could exploit this vulnerability by crafting a malicious input that causes a denial of service.

Exploiting this vulnerability is possible when running Black on untrusted input, or if you habitually put thousands of leading tab characters in your docstrings.

---

### Release Notes

<details>
<summary>psf/black (black)</summary>

### [`v24.3.0`](https://togithub.com/psf/black/blob/HEAD/CHANGES.md#2430)

[Compare Source](https://togithub.com/psf/black/compare/24.2.0...24.3.0)

##### Highlights

This release is a milestone: it fixes Black's first CVE security vulnerability. If you
run Black on untrusted input, or if you habitually put thousands of leading tab
characters in your docstrings, you are strongly encouraged to upgrade immediately to fix
[CVE-2024-21503](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2024-21503).

This release also fixes a bug in Black's AST safety check that allowed Black to make
incorrect changes to certain f-strings that are valid in Python 3.12 and higher.

##### Stable style

-   Don't move comments along with delimiters, which could cause crashes ([#&#8203;4248](https://togithub.com/psf/black/issues/4248))
-   Strengthen AST safety check to catch more unsafe changes to strings. Previous versions
    of Black would incorrectly format the contents of certain unusual f-strings containing
    nested strings with the same quote type. Now, Black will crash on such strings until
    support for the new f-string syntax is implemented. ([#&#8203;4270](https://togithub.com/psf/black/issues/4270))
-   Fix a bug where line-ranges exceeding the last code line would not work as expected
    ([#&#8203;4273](https://togithub.com/psf/black/issues/4273))

##### Performance

-   Fix catastrophic performance on docstrings that contain large numbers of leading tab
    characters. This fixes
    [CVE-2024-21503](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2024-21503).
    ([#&#8203;4278](https://togithub.com/psf/black/issues/4278))

##### Documentation

-   Note what happens when `--check` is used with `--quiet` ([#&#8203;4236](https://togithub.com/psf/black/issues/4236))

### [`v24.2.0`](https://togithub.com/psf/black/blob/HEAD/CHANGES.md#2420)

[Compare Source](https://togithub.com/psf/black/compare/24.1.1...24.2.0)

##### Stable style

-   Fixed a bug where comments where mistakenly removed along with redundant parentheses
    ([#&#8203;4218](https://togithub.com/psf/black/issues/4218))

##### Preview style

-   Move the `hug_parens_with_braces_and_square_brackets` feature to the unstable style
    due to an outstanding crash and proposed formatting tweaks ([#&#8203;4198](https://togithub.com/psf/black/issues/4198))
-   Fixed a bug where base expressions caused inconsistent formatting of \*\* in tenary
    expression ([#&#8203;4154](https://togithub.com/psf/black/issues/4154))
-   Checking for newline before adding one on docstring that is almost at the line limit
    ([#&#8203;4185](https://togithub.com/psf/black/issues/4185))
-   Remove redundant parentheses in `case` statement `if` guards ([#&#8203;4214](https://togithub.com/psf/black/issues/4214)).

##### Configuration

-   Fix issue where *Black* would ignore input files in the presence of symlinks ([#&#8203;4222](https://togithub.com/psf/black/issues/4222))
-   *Black* now ignores `pyproject.toml` that is missing a `tool.black` section when
    discovering project root and configuration. Since *Black* continues to use version
    control as an indicator of project root, this is expected to primarily change behavior
    for users in a monorepo setup (desirably). If you wish to preserve previous behavior,
    simply add an empty `[tool.black]` to the previously discovered `pyproject.toml`
    ([#&#8203;4204](https://togithub.com/psf/black/issues/4204))

##### Output

-   Black will swallow any `SyntaxWarning`s or `DeprecationWarning`s produced by the `ast`
    module when performing equivalence checks ([#&#8203;4189](https://togithub.com/psf/black/issues/4189))

##### Integrations

-   Add a JSONSchema and provide a validate-pyproject entry-point ([#&#8203;4181](https://togithub.com/psf/black/issues/4181))

### [`v24.1.1`](https://togithub.com/psf/black/blob/HEAD/CHANGES.md#2411)

[Compare Source](https://togithub.com/psf/black/compare/24.1.0...24.1.1)

Bugfix release to fix a bug that made Black unusable on certain file systems with strict
limits on path length.

##### Preview style

-   Consistently add trailing comma on typed parameters ([#&#8203;4164](https://togithub.com/psf/black/issues/4164))

##### Configuration

-   Shorten the length of the name of the cache file to fix crashes on file systems that
    do not support long paths ([#&#8203;4176](https://togithub.com/psf/black/issues/4176))

### [`v24.1.0`](https://togithub.com/psf/black/blob/HEAD/CHANGES.md#2410)

[Compare Source](https://togithub.com/psf/black/compare/23.12.1...24.1.0)

##### Highlights

This release introduces the new 2024 stable style ([#&#8203;4106](https://togithub.com/psf/black/issues/4106)), stabilizing the following
changes:

-   Add parentheses around `if`-`else` expressions ([#&#8203;2278](https://togithub.com/psf/black/issues/2278))
-   Dummy class and function implementations consisting only of `...` are formatted more
    compactly ([#&#8203;3796](https://togithub.com/psf/black/issues/3796))
-   If an assignment statement is too long, we now prefer splitting on the right-hand side
    ([#&#8203;3368](https://togithub.com/psf/black/issues/3368))
-   Hex codes in Unicode escape sequences are now standardized to lowercase ([#&#8203;2916](https://togithub.com/psf/black/issues/2916))
-   Allow empty first lines at the beginning of most blocks ([#&#8203;3967](https://togithub.com/psf/black/issues/3967), [#&#8203;4061](https://togithub.com/psf/black/issues/4061))
-   Add parentheses around long type annotations ([#&#8203;3899](https://togithub.com/psf/black/issues/3899))
-   Enforce newline after module docstrings ([#&#8203;3932](https://togithub.com/psf/black/issues/3932), [#&#8203;4028](https://togithub.com/psf/black/issues/4028))
-   Fix incorrect magic trailing comma handling in return types ([#&#8203;3916](https://togithub.com/psf/black/issues/3916))
-   Remove blank lines before class docstrings ([#&#8203;3692](https://togithub.com/psf/black/issues/3692))
-   Wrap multiple context managers in parentheses if combined in a single `with` statement
    ([#&#8203;3489](https://togithub.com/psf/black/issues/3489))
-   Fix bug in line length calculations for power operations ([#&#8203;3942](https://togithub.com/psf/black/issues/3942))
-   Add trailing commas to collection literals even if there's a comment after the last
    entry ([#&#8203;3393](https://togithub.com/psf/black/issues/3393))
-   When using `--skip-magic-trailing-comma` or `-C`, trailing commas are stripped from
    subscript expressions with more than 1 element ([#&#8203;3209](https://togithub.com/psf/black/issues/3209))
-   Add extra blank lines in stubs in a few cases ([#&#8203;3564](https://togithub.com/psf/black/issues/3564), [#&#8203;3862](https://togithub.com/psf/black/issues/3862))
-   Accept raw strings as docstrings ([#&#8203;3947](https://togithub.com/psf/black/issues/3947))
-   Split long lines in case blocks ([#&#8203;4024](https://togithub.com/psf/black/issues/4024))
-   Stop removing spaces from walrus operators within subscripts ([#&#8203;3823](https://togithub.com/psf/black/issues/3823))
-   Fix incorrect formatting of certain async statements ([#&#8203;3609](https://togithub.com/psf/black/issues/3609))
-   Allow combining `# fmt: skip` with other comments ([#&#8203;3959](https://togithub.com/psf/black/issues/3959))

There are already a few improvements in the `--preview` style, which are slated for the
2025 stable style. Try them out and
[share your feedback](https://togithub.com/psf/black/issues). In the past, the preview
style has included some features that we were not able to stabilize. This year, we're
adding a separate `--unstable` style for features with known problems. Now, the
`--preview` style only includes features that we actually expect to make it into next
year's stable style.

##### Stable style

Several bug fixes were made in features that are moved to the stable style in this
release:

-   Fix comment handling when parenthesising conditional expressions ([#&#8203;4134](https://togithub.com/psf/black/issues/4134))
-   Fix bug where spaces were not added around parenthesized walruses in subscripts,
    unlike other binary operators ([#&#8203;4109](https://togithub.com/psf/black/issues/4109))
-   Remove empty lines before docstrings in async functions ([#&#8203;4132](https://togithub.com/psf/black/issues/4132))
-   Address a missing case in the change to allow empty lines at the beginning of all
    blocks, except immediately before a docstring ([#&#8203;4130](https://togithub.com/psf/black/issues/4130))
-   For stubs, fix logic to enforce empty line after nested classes with bodies ([#&#8203;4141](https://togithub.com/psf/black/issues/4141))

##### Preview style

-   Add `--unstable` style, covering preview features that have known problems that would
    block them from going into the stable style. Also add the `--enable-unstable-feature`
    flag; for example, use
    `--enable-unstable-feature hug_parens_with_braces_and_square_brackets` to apply this
    preview feature throughout 2024, even if a later Black release downgrades the feature
    to unstable ([#&#8203;4096](https://togithub.com/psf/black/issues/4096))
-   Format module docstrings the same as class and function docstrings ([#&#8203;4095](https://togithub.com/psf/black/issues/4095))
-   Fix crash when using a walrus in a dictionary ([#&#8203;4155](https://togithub.com/psf/black/issues/4155))
-   Fix unnecessary parentheses when wrapping long dicts ([#&#8203;4135](https://togithub.com/psf/black/issues/4135))
-   Stop normalizing spaces before `# fmt: skip` comments ([#&#8203;4146](https://togithub.com/psf/black/issues/4146))

##### Configuration

-   Print warning when configuration in `pyproject.toml` contains an invalid key ([#&#8203;4165](https://togithub.com/psf/black/issues/4165))
-   Fix symlink handling, properly ignoring symlinks that point outside of root ([#&#8203;4161](https://togithub.com/psf/black/issues/4161))
-   Fix cache mtime logic that resulted in false positive cache hits ([#&#8203;4128](https://togithub.com/psf/black/issues/4128))
-   Remove the long-deprecated `--experimental-string-processing` flag. This feature can
    currently be enabled with `--preview --enable-unstable-feature string_processing`.
    ([#&#8203;4096](https://togithub.com/psf/black/issues/4096))

##### Integrations

-   Revert the change to run Black's pre-commit integration only on specific git hooks
    ([#&#8203;3940](https://togithub.com/psf/black/issues/3940)) for better compatibility with older versions of pre-commit ([#&#8203;4137](https://togithub.com/psf/black/issues/4137))

### [`v23.12.1`](https://togithub.com/psf/black/blob/HEAD/CHANGES.md#23121)

[Compare Source](https://togithub.com/psf/black/compare/23.12.0...23.12.1)

##### Packaging

-   Fixed a bug that included dependencies from the `d` extra by default ([#&#8203;4108](https://togithub.com/psf/black/issues/4108))

### [`v23.12.0`](https://togithub.com/psf/black/blob/HEAD/CHANGES.md#23120)

[Compare Source](https://togithub.com/psf/black/compare/23.11.0...23.12.0)

##### Highlights

It's almost 2024, which means it's time for a new edition of *Black*'s stable style!
Together with this release, we'll put out an alpha release 24.1a1 showcasing the draft
2024 stable style, which we'll finalize in the January release. Please try it out and
[share your feedback](https://togithub.com/psf/black/issues/4042).

This release (23.12.0) will still produce the 2023 style. Most but not all of the
changes in `--preview` mode will be in the 2024 stable style.

##### Stable style

-   Fix bug where `# fmt: off` automatically dedents when used with the `--line-ranges`
    option, even when it is not within the specified line range. ([#&#8203;4084](https://togithub.com/psf/black/issues/4084))
-   Fix feature detection for parenthesized context managers ([#&#8203;4104](https://togithub.com/psf/black/issues/4104))

##### Preview style

-   Prefer more equal signs before a break when splitting chained assignments ([#&#8203;4010](https://togithub.com/psf/black/issues/4010))
-   Standalone form feed characters at the module level are no longer removed ([#&#8203;4021](https://togithub.com/psf/black/issues/4021))
-   Additional cases of immediately nested tuples, lists, and dictionaries are now
    indented less ([#&#8203;4012](https://togithub.com/psf/black/issues/4012))
-   Allow empty lines at the beginning of all blocks, except immediately before a
    docstring ([#&#8203;4060](https://togithub.com/psf/black/issues/4060))
-   Fix crash in preview mode when using a short `--line-length` ([#&#8203;4086](https://togithub.com/psf/black/issues/4086))
-   Keep suites consisting of only an ellipsis on their own lines if they are not
    functions or class definitions ([#&#8203;4066](https://togithub.com/psf/black/issues/4066)) ([#&#8203;4103](https://togithub.com/psf/black/issues/4103))

##### Configuration

-   `--line-ranges` now skips *Black*'s internal stability check in `--safe` mode. This
    avoids a crash on rare inputs that have many unformatted same-content lines. ([#&#8203;4034](https://togithub.com/psf/black/issues/4034))

##### Packaging

-   Upgrade to mypy 1.7.1 ([#&#8203;4049](https://togithub.com/psf/black/issues/4049)) ([#&#8203;4069](https://togithub.com/psf/black/issues/4069))
-   Faster compiled wheels are now available for CPython 3.12 ([#&#8203;4070](https://togithub.com/psf/black/issues/4070))

##### Integrations

-   Enable 3.12 CI ([#&#8203;4035](https://togithub.com/psf/black/issues/4035))
-   Build docker images in parallel ([#&#8203;4054](https://togithub.com/psf/black/issues/4054))
-   Build docker images with 3.12 ([#&#8203;4055](https://togithub.com/psf/black/issues/4055))

### [`v23.11.0`](https://togithub.com/psf/black/blob/HEAD/CHANGES.md#23110)

[Compare Source](https://togithub.com/psf/black/compare/23.10.1...23.11.0)

##### Highlights

-   Support formatting ranges of lines with the new `--line-ranges` command-line option
    ([#&#8203;4020](https://togithub.com/psf/black/issues/4020))

##### Stable style

-   Fix crash on formatting bytes strings that look like docstrings ([#&#8203;4003](https://togithub.com/psf/black/issues/4003))
-   Fix crash when whitespace followed a backslash before newline in a docstring ([#&#8203;4008](https://togithub.com/psf/black/issues/4008))
-   Fix standalone comments inside complex blocks crashing Black ([#&#8203;4016](https://togithub.com/psf/black/issues/4016))
-   Fix crash on formatting code like `await (a ** b)` ([#&#8203;3994](https://togithub.com/psf/black/issues/3994))
-   No longer treat leading f-strings as docstrings. This matches Python's behaviour and
    fixes a crash ([#&#8203;4019](https://togithub.com/psf/black/issues/4019))

##### Preview style

-   Multiline dicts and lists that are the sole argument to a function are now indented
    less ([#&#8203;3964](https://togithub.com/psf/black/issues/3964))
-   Multiline unpacked dicts and lists as the sole argument to a function are now also
    indented less ([#&#8203;3992](https://togithub.com/psf/black/issues/3992))
-   In f-string debug expressions, quote types that are visible in the final string are
    now preserved ([#&#8203;4005](https://togithub.com/psf/black/issues/4005))
-   Fix a bug where long `case` blocks were not split into multiple lines. Also enable
    general trailing comma rules on `case` blocks ([#&#8203;4024](https://togithub.com/psf/black/issues/4024))
-   Keep requiring two empty lines between module-level docstring and first function or
    class definition ([#&#8203;4028](https://togithub.com/psf/black/issues/4028))
-   Add support for single-line format skip with other comments on the same line ([#&#8203;3959](https://togithub.com/psf/black/issues/3959))

##### Configuration

-   Consistently apply force exclusion logic before resolving symlinks ([#&#8203;4015](https://togithub.com/psf/black/issues/4015))
-   Fix a bug in the matching of absolute path names in `--include` ([#&#8203;3976](https://togithub.com/psf/black/issues/3976))

##### Performance

-   Fix mypyc builds on arm64 on macOS ([#&#8203;4017](https://togithub.com/psf/black/issues/4017))

##### Integrations

-   Black's pre-commit integration will now run only on git hooks appropriate for a code
    formatter ([#&#8203;3940](https://togithub.com/psf/black/issues/3940))

### [`v23.10.1`](https://togithub.com/psf/black/blob/HEAD/CHANGES.md#23101)

[Compare Source](https://togithub.com/psf/black/compare/23.10.0...23.10.1)

##### Highlights

-   Maintenance release to get a fix out for GitHub Action edge case ([#&#8203;3957](https://togithub.com/psf/black/issues/3957))

##### Preview style

-   Fix merging implicit multiline strings that have inline comments ([#&#8203;3956](https://togithub.com/psf/black/issues/3956))
-   Allow empty first line after block open before a comment or compound statement ([#&#8203;3967](https://togithub.com/psf/black/issues/3967))

##### Packaging

-   Change Dockerfile to hatch + compile black ([#&#8203;3965](https://togithub.com/psf/black/issues/3965))

##### Integrations

-   The summary output for GitHub workflows is now suppressible using the `summary`
    parameter. ([#&#8203;3958](https://togithub.com/psf/black/issues/3958))
-   Fix the action failing when Black check doesn't pass ([#&#8203;3957](https://togithub.com/psf/black/issues/3957))

##### Documentation

-   It is known Windows documentation CI is broken
[https://github.com/psf/black/issues/3968](https://togithub.com/psf/black/issues/3968)3968

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOC4xOC4xNyIsInVwZGF0ZWRJblZlciI6IjM4LjE4LjE3IiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCIsInNlY3VyaXR5Il19-->


---

_Label `internal` added by @renovate[bot] on 2024-08-07 07:42_

---

_Label `security` added by @renovate[bot] on 2024-08-07 07:42_

---

_Comment by @MichaReiser on 2024-08-07 07:50_

Or we remove it? Do we even need black?

---

_Comment by @dhruvmanila on 2024-08-07 08:55_

We use it here:

https://github.com/astral-sh/ruff/blob/50ff5c754421474b353f94edfdc1fed7a8c3b82b/scripts/check_docs_formatted.py#L130

which is run as part of CI:

https://github.com/astral-sh/ruff/blob/50ff5c754421474b353f94edfdc1fed7a8c3b82b/.github/workflows/ci.yaml#L532-L533

---

_Comment by @dhruvmanila on 2024-08-07 08:59_

I think we might have to update all of them manually

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-10 11:43_

---

_Comment by @github-actions[bot] on 2024-08-10 11:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-08-10 12:02_

---

_Merged by @dhruvmanila on 2024-08-10 12:34_

---

_Closed by @dhruvmanila on 2024-08-10 12:34_

---

_Branch deleted on 2024-08-10 12:34_

---
