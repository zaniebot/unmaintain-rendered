```yaml
number: 16483
title: Make noqa parsing consistent and more robust
type: pull_request
state: merged
author: dylwil3
labels:
  - breaking
  - suppression
assignees: []
merged: true
base: micha/ruff-0.10
head: noqa-parsing
created_at: 2025-03-03T21:31:39Z
updated_at: 2025-03-11T19:50:32Z
url: https://github.com/astral-sh/ruff/pull/16483
synced_at: 2026-01-10T19:49:01Z
```

# Make noqa parsing consistent and more robust

---

_Pull request opened by @dylwil3 on 2025-03-03 21:31_

# Summary
The goal of this PR is to address various issues around parsing suppression comments by

1. Unifying the logic used to parse in-line (`# noqa`) and file-level (`# ruff: noqa`) noqa comments
2. Recovering from certain errors and surfacing warnings in these cases

Closes #15682 
Supersedes #12811 
Addresses https://github.com/astral-sh/ruff/pull/14229#discussion_r1835481018
Related: #14229 , #12809

--------------------------------

# Current suppression behavior
The implementations for parsing in-line and file-level noqa comments are entirely separate, so a number of discrepancies have arisen. You can unfurl some examples of this below.

<details>
   <summary> Examples of discrepancies </summary>

## Leading Hashes

This works:
```python
from typing import List ## noqa: UP035
```

This does not, and emits no warning:
```python
from typing import List
## ruff: noqa: UP035
```

## Missing colon
This silently ignores _all_ rules on the line, even though
the user intended a single rule to be ignored:

```python
from typing import List # noqa UP035
```

This prints a warning and does not suppress anything:
```python
from typing import List 
# ruff: noqa UP035
```

## Missing Delimiter
This works to suppress both violations:
```python
from typing import List # noqa: UP035F401
```

This warns and suppresses neither:
```python
from typing import List 
#ruff: noqa: UP035F401
```

## Multiple Commas

This works to suppress both violations:
```python
from typing import List #noqa: UP035,,,F401
```

This suppresses the first but not the second.
It does not emit a warning:
```python
from typing import List 
#ruff:noqa: UP035,,,F401
```

##  Missing whitespace before additional comments
This works:
```python
from typing import List #noqa: UP035abc
```

This warns and does nothing:
```python
from typing import List 
#ruff:noqa: UP035abc
```

## Additional comments after noqa
This works:
```python
from typing import List #noqa: UP035 and more comments
```

And so does this:
```python
from typing import List #noqa and more comments
```

And so does this:
```python
from typing import List 
#ruff: noqa: UP035 and more comments
```

But this emits a warning and suppresses nothing:
```python
from typing import List 
#ruff:noqa and more comments
```

</details>

# Design
The specification for the unified parsing logic is as follows:

1. A suppression comment must be contained in a comment range and begins with a prefix matching one of the following regexes:
- (file level) `#\s*(?:ruff|flake8)\s*:\s*(?i)noqa`
- (inline) `#\s*(?i)noqa`
2. If the character following the `noqa` is `'#'`, whitespace, or if there are no characters following, then the suppression comment suppressed _all_ rules. If the character following is `":"`, proceed to (3). Otherwise, warn the user for `InvalidSuffix`.
3. We expect a list of rule codes, separated by commas or whitespace. After splitting first on commas, and then on whitespace, we attempt to parse each item in turn. If parsing a nonempty item returns no rule codes, we stop. If no codes were found at all we warn the user with `MissingCodes`.
4. A rule code is a match for the regex `[A-Z]+[0-9]+`. While it is expected that there is one rule code per item, parsing continues even if that is not true, (e.g., `F401F841` or `F401,,F841`), and the user is warned of a `MissingDelimiter` or `MissingItem`. If an item does not consist entirely of codes (e.g. `F401abc`), parsing aborts with an error and the user is warned of an `InvalidCodeSuffix` - the exception is if we encounter `#`, in which case parsing stops and returns all codes collected to that point (e.g. `# noqa: F401#comment`).

# Implementation
After trimming the prefix for a file-level or inline noqa, the remaining text is passed to a lexer in the style of `SimpleTokenizer`. There is some small logic to "recover" in the case of missing delimiters or missing items.

# Other changes and affected rules/behavior

The only rules that appear to be affected are [blanket-noqa (PGH004)](https://docs.astral.sh/ruff/rules/blanket-noqa/#blanket-noqa-pgh004)) and [unused-noqa (RUF100)](https://docs.astral.sh/ruff/rules/unused-noqa/#unused-noqa-ruf100) (but we'll see if others come up in the ecosystem check). For example, we no longer parse the first code in `# noqa F401.F841` and instead emit a warning, which changes one of the snapshots for `RUF100`.

Internally, we no longer distinguish between a `Directive` and a `ParsedFileExemption` (keeping just the former), since these were used almost identically. They are carried around inside of `NoqaDirectiveLine` and `FileNoqaDirectedLine`, respectively, so there is little risk in misunderstanding the context if we just use `Directive` in both cases.

# Review

After responding to comments it may no longer be helpful to proceed commit by commit. However, to see changes in behavior it may be best to view the diff against the first commit rather than main, since it contains a few additional snapshots.

# Miscellaneous comments
- If folks think there are natural boundaries to do so, I'm happy to break this up into smaller PRs.
- Maybe @InSyncWithFoo and @koxudaxi would be interested in looking over at least the design spec here, given https://github.com/astral-sh/ruff/pull/14229#discussion_r1835481018?

---

_Label `breaking` added by @dylwil3 on 2025-03-03 21:31_

---

_Label `suppression` added by @dylwil3 on 2025-03-03 21:31_

---

_Label `do-not-merge` added by @dylwil3 on 2025-03-03 21:31_

---

_Added to milestone `v0.10` by @dylwil3 on 2025-03-03 21:31_

---

_Comment by @codspeed-hq[bot] on 2025-03-03 21:36_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dylwil3%3Anoqa-parsing)

### Merging #16483 will **not alter performance**

<sub>Comparing <code>dylwil3:noqa-parsing</code> (ae39f61) with <code>micha/ruff-0.10</code> (41cd905)</sub>



### Summary

`✅ 32` untouched benchmarks  





---

_Comment by @dylwil3 on 2025-03-03 21:38_

Oh jeez! Well that performance regression is no good. I'd still welcome thoughts on the "design spec" and I'll work on improving the implementation in the meantime. Converting to draft for now...

---

_Converted to draft by @dylwil3 on 2025-03-03 21:41_

---

_Comment by @github-actions[bot] on 2025-03-03 21:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -66 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/qdrant/qdrant-client">qdrant/qdrant-client</a> (+0 -66 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/aliases_api.py#L52'>qdrant_client/http/api/aliases_api.py:52:54:</a> F405 `AsyncApiClient` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/aliases_api.py#L5'>qdrant_client/http/api/aliases_api.py:5:27:</a> F811 Redefinition of unused `BaseModel` from line 4
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/aliases_api.py#L7'>qdrant_client/http/api/aliases_api.py:7:1:</a> F403 `from qdrant_client.http.models import *` used; unable to detect undefined names
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/beta_api.py#L51'>qdrant_client/http/api/beta_api.py:51:54:</a> F405 `AsyncApiClient` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/beta_api.py#L5'>qdrant_client/http/api/beta_api.py:5:27:</a> F811 Redefinition of unused `BaseModel` from line 4
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/beta_api.py#L7'>qdrant_client/http/api/beta_api.py:7:1:</a> F403 `from qdrant_client.http.models import *` used; unable to detect undefined names
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/collections_api.py#L52'>qdrant_client/http/api/collections_api.py:52:54:</a> F405 `AsyncApiClient` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/collections_api.py#L5'>qdrant_client/http/api/collections_api.py:5:27:</a> F811 Redefinition of unused `BaseModel` from line 4
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/collections_api.py#L7'>qdrant_client/http/api/collections_api.py:7:1:</a> F403 `from qdrant_client.http.models import *` used; unable to detect undefined names
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/distributed_api.py#L52'>qdrant_client/http/api/distributed_api.py:52:54:</a> F405 `AsyncApiClient` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/distributed_api.py#L5'>qdrant_client/http/api/distributed_api.py:5:27:</a> F811 Redefinition of unused `BaseModel` from line 4
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/distributed_api.py#L7'>qdrant_client/http/api/distributed_api.py:7:1:</a> F403 `from qdrant_client.http.models import *` used; unable to detect undefined names
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/indexes_api.py#L126'>qdrant_client/http/api/indexes_api.py:126:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/indexes_api.py#L144'>qdrant_client/http/api/indexes_api.py:144:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/indexes_api.py#L162'>qdrant_client/http/api/indexes_api.py:162:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/indexes_api.py#L180'>qdrant_client/http/api/indexes_api.py:180:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/indexes_api.py#L52'>qdrant_client/http/api/indexes_api.py:52:54:</a> F405 `AsyncApiClient` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/indexes_api.py#L59'>qdrant_client/http/api/indexes_api.py:59:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/indexes_api.py#L5'>qdrant_client/http/api/indexes_api.py:5:27:</a> F811 Redefinition of unused `BaseModel` from line 4
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/indexes_api.py#L7'>qdrant_client/http/api/indexes_api.py:7:1:</a> F403 `from qdrant_client.http.models import *` used; unable to detect undefined names
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/indexes_api.py#L94'>qdrant_client/http/api/indexes_api.py:94:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/points_api.py#L158'>qdrant_client/http/api/points_api.py:158:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/points_api.py#L192'>qdrant_client/http/api/points_api.py:192:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/points_api.py#L226'>qdrant_client/http/api/points_api.py:226:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/points_api.py#L356'>qdrant_client/http/api/points_api.py:356:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/points_api.py#L424'>qdrant_client/http/api/points_api.py:424:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/points_api.py#L458'>qdrant_client/http/api/points_api.py:458:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
... 32 additional changes omitted for rule F405
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/points_api.py#L5'>qdrant_client/http/api/points_api.py:5:27:</a> F811 Redefinition of unused `BaseModel` from line 4
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/points_api.py#L7'>qdrant_client/http/api/points_api.py:7:1:</a> F403 `from qdrant_client.http.models import *` used; unable to detect undefined names
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/search_api.py#L5'>qdrant_client/http/api/search_api.py:5:27:</a> F811 Redefinition of unused `BaseModel` from line 4
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/search_api.py#L7'>qdrant_client/http/api/search_api.py:7:1:</a> F403 `from qdrant_client.http.models import *` used; unable to detect undefined names
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/service_api.py#L5'>qdrant_client/http/api/service_api.py:5:27:</a> F811 Redefinition of unused `BaseModel` from line 4
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/service_api.py#L7'>qdrant_client/http/api/service_api.py:7:1:</a> F403 `from qdrant_client.http.models import *` used; unable to detect undefined names
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/snapshots_api.py#L5'>qdrant_client/http/api/snapshots_api.py:5:27:</a> F811 Redefinition of unused `BaseModel` from line 4
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/snapshots_api.py#L7'>qdrant_client/http/api/snapshots_api.py:7:1:</a> F403 `from qdrant_client.http.models import *` used; unable to detect undefined names
... 31 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F405 | 48 | 0 | 48 | 0 | 0 |
| F811 | 9 | 0 | 9 | 0 | 0 |
| F403 | 9 | 0 | 9 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -66 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/qdrant/qdrant-client">qdrant/qdrant-client</a> (+0 -66 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/aliases_api.py#L52'>qdrant_client/http/api/aliases_api.py:52:54:</a> F405 `AsyncApiClient` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/aliases_api.py#L5'>qdrant_client/http/api/aliases_api.py:5:27:</a> F811 Redefinition of unused `BaseModel` from line 4
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/aliases_api.py#L7'>qdrant_client/http/api/aliases_api.py:7:1:</a> F403 `from qdrant_client.http.models import *` used; unable to detect undefined names
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/beta_api.py#L51'>qdrant_client/http/api/beta_api.py:51:54:</a> F405 `AsyncApiClient` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/beta_api.py#L5'>qdrant_client/http/api/beta_api.py:5:27:</a> F811 Redefinition of unused `BaseModel` from line 4
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/beta_api.py#L7'>qdrant_client/http/api/beta_api.py:7:1:</a> F403 `from qdrant_client.http.models import *` used; unable to detect undefined names
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/collections_api.py#L52'>qdrant_client/http/api/collections_api.py:52:54:</a> F405 `AsyncApiClient` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/collections_api.py#L5'>qdrant_client/http/api/collections_api.py:5:27:</a> F811 Redefinition of unused `BaseModel` from line 4
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/collections_api.py#L7'>qdrant_client/http/api/collections_api.py:7:1:</a> F403 `from qdrant_client.http.models import *` used; unable to detect undefined names
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/distributed_api.py#L52'>qdrant_client/http/api/distributed_api.py:52:54:</a> F405 `AsyncApiClient` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/distributed_api.py#L5'>qdrant_client/http/api/distributed_api.py:5:27:</a> F811 Redefinition of unused `BaseModel` from line 4
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/distributed_api.py#L7'>qdrant_client/http/api/distributed_api.py:7:1:</a> F403 `from qdrant_client.http.models import *` used; unable to detect undefined names
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/indexes_api.py#L126'>qdrant_client/http/api/indexes_api.py:126:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/indexes_api.py#L144'>qdrant_client/http/api/indexes_api.py:144:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/indexes_api.py#L162'>qdrant_client/http/api/indexes_api.py:162:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/indexes_api.py#L180'>qdrant_client/http/api/indexes_api.py:180:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/indexes_api.py#L52'>qdrant_client/http/api/indexes_api.py:52:54:</a> F405 `AsyncApiClient` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/indexes_api.py#L59'>qdrant_client/http/api/indexes_api.py:59:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/indexes_api.py#L5'>qdrant_client/http/api/indexes_api.py:5:27:</a> F811 Redefinition of unused `BaseModel` from line 4
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/indexes_api.py#L7'>qdrant_client/http/api/indexes_api.py:7:1:</a> F403 `from qdrant_client.http.models import *` used; unable to detect undefined names
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/indexes_api.py#L94'>qdrant_client/http/api/indexes_api.py:94:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/points_api.py#L158'>qdrant_client/http/api/points_api.py:158:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/points_api.py#L192'>qdrant_client/http/api/points_api.py:192:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/points_api.py#L226'>qdrant_client/http/api/points_api.py:226:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/points_api.py#L356'>qdrant_client/http/api/points_api.py:356:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/points_api.py#L424'>qdrant_client/http/api/points_api.py:424:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/points_api.py#L458'>qdrant_client/http/api/points_api.py:458:19:</a> F405 `WriteOrdering` may be undefined, or defined from star imports
... 32 additional changes omitted for rule F405
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/points_api.py#L5'>qdrant_client/http/api/points_api.py:5:27:</a> F811 Redefinition of unused `BaseModel` from line 4
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/points_api.py#L7'>qdrant_client/http/api/points_api.py:7:1:</a> F403 `from qdrant_client.http.models import *` used; unable to detect undefined names
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/search_api.py#L5'>qdrant_client/http/api/search_api.py:5:27:</a> F811 Redefinition of unused `BaseModel` from line 4
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/search_api.py#L7'>qdrant_client/http/api/search_api.py:7:1:</a> F403 `from qdrant_client.http.models import *` used; unable to detect undefined names
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/service_api.py#L5'>qdrant_client/http/api/service_api.py:5:27:</a> F811 Redefinition of unused `BaseModel` from line 4
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/service_api.py#L7'>qdrant_client/http/api/service_api.py:7:1:</a> F403 `from qdrant_client.http.models import *` used; unable to detect undefined names
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/snapshots_api.py#L5'>qdrant_client/http/api/snapshots_api.py:5:27:</a> F811 Redefinition of unused `BaseModel` from line 4
- <a href='https://github.com/qdrant/qdrant-client/blob/eaee466fca445acff9b54e555b4fce3ef2a0f108/qdrant_client/http/api/snapshots_api.py#L7'>qdrant_client/http/api/snapshots_api.py:7:1:</a> F403 `from qdrant_client.http.models import *` used; unable to detect undefined names
... 31 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F405 | 48 | 0 | 48 | 0 | 0 |
| F811 | 9 | 0 | 9 | 0 | 0 |
| F403 | 9 | 0 | 9 | 0 | 0 |

</p>
</details>




---

_Comment by @InSyncWithFoo on 2025-03-03 23:25_

@dylwil3 `\r` is actually not allowed in a comment, since it signifies the end of that physical line (same as `\n` and `\r\n`) and thus also the end of the comment.

Additionally, the original implementation uses [`char.is_whitespace()`](https://doc.rust-lang.org/std/primitive.char.html#method.is_whitespace), which allows non-ASCII whitespace characters as well. In #12809, I argued that only tabs and spaces should be allowed (and I would do so again), but Charlie thought Ruff should be more permissive in that regard.

---

_Comment by @dylwil3 on 2025-03-03 23:45_

> @dylwil3 `\r` is actually not allowed in a comment, since it signifies the end of that physical line (same as `\n` and `\r\n`) and thus also the end of the comment.
> 
> Additionally, the original implementation uses [`char.is_whitespace()`](https://doc.rust-lang.org/std/primitive.char.html#method.is_whitespace), which allows non-ASCII whitespace characters as well. In #12809, I argued that only tabs and spaces should be allowed (and I would do so again), but Charlie thought Ruff should be more permissive in that regard.

Yeah, I tried removing disallowed whitespace as well and I don't think it makes a difference. I think the regex is too heavy to compete with the manual approach for this simple of a pattern.

I sort of agree regarding ascii whitespace, and I do like that the regex is a little more self documenting... I'm torn. 

I'll try re-implementing the manual approach tomorrow and see if it's worth it - I can always mention the regex we're implementing in the doc-comment even if it's not used directly.

---

_Comment by @dylwil3 on 2025-03-04 14:05_

Okay, performance fixed by reverting to manually implementing regex (now at least the file exemption and in-line cases are handled in visibly the same way).

I think the ecosystem check is actually correct, and is one of the changes in behavior consistent with the design: We are now allowing comments _after_ a file-level exemption like: `# ruff: noqa This is a bad file don't lint it`. Therefore, in the same way that `# noqa F401` is a probable typo that suppresses _all_ rules, so too does `# ruff: noqa F401` suppress _all_ rules. This mistake is caught by [blanket-noqa (PGH004)](https://docs.astral.sh/ruff/rules/blanket-noqa/#blanket-noqa-pgh004) (in preview).

As an aside: It'd be nice if there were a not-too-complex way to alert users to the footgun `# noqa F401` without them turning on `PGH004` (since it's not a default rule). Maybe `RUF100` could handle this (not a default but I think somewhat more popular)?

---

_Marked ready for review by @dylwil3 on 2025-03-04 14:05_

---

_@MichaReiser reviewed on 2025-03-04 14:32_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:346 on 2025-03-04 14:32_

Before I review this in detail. Have you considered using `Cursor`. I find that it can help simplify a lot of those basic *if it's this character, eat it*

---

_@dylwil3 reviewed on 2025-03-04 15:18_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/noqa.rs`:346 on 2025-03-04 15:18_

Sounds like a good idea! I'll check it out. Feel free to review the design (e.g. the tests and snapshots) independently of the implementation, since the former may result in changes in the latter anyway. You can also hold off on both while I look into `Cursor` - whatever is most convenient!

---

_@MichaReiser reviewed on 2025-03-04 15:20_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:346 on 2025-03-04 15:20_

I think I'll wait. Reviewing PRs of this size take a lot of time

---

_@MichaReiser reviewed on 2025-03-04 15:21_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:346 on 2025-03-04 15:21_

But I can do specific parts if you have something that you'd like to get early feedback

---

_@dylwil3 reviewed on 2025-03-07 00:40_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/noqa.rs`:346 on 2025-03-07 00:40_

Thanks so much for the suggestion to use `Cursor`! The implementation is now much simpler and hopefully easier to read/review.

---

_Comment by @dylwil3 on 2025-03-07 01:03_

Looks like ecosystem check revealed a few small issues - again reverting to draft briefly

Update: Fixed!

---

_Converted to draft by @dylwil3 on 2025-03-07 01:05_

---

_Marked ready for review by @dylwil3 on 2025-03-07 01:54_

---

_Comment by @MichaReiser on 2025-03-07 10:30_

Hmm, I don't understand the ecosystem results. Are we now ignoring the code for file level suppressions?

---

_Comment by @MichaReiser on 2025-03-07 10:36_

For reference, the related PRs in Red Knot are https://github.com/astral-sh/ruff/pull/15046, https://github.com/astral-sh/ruff/pull/15078, and https://github.com/astral-sh/ruff/pull/15081

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:126 on 2025-03-07 10:41_

I wasn't aware that Ruff sometimes tests if a rule is ignored at a specific location similar to how it is done in Red Knot. I assumed it only filters out diagnostics at the very end. 

I don't know if this would be a large change but it seems we now have multiple places where we lex noqa comments. Would it make more sense to analyze all comments once and extract all suppression comments (see linked Red Knot issues). Looking up if a rule is ignored would then be simplified to searching for a suppression comment for that line (or range), which should require fewer binary search steps than searching all comments. Emitting errors should then be as simple as iterating over all suppression comments and emitting errors for the incorrect once. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:245 on 2025-03-07 10:44_

I'm leaning towards moving the message into `LexicalWarning`. It's otherwise very easy to forget changing the message here when adding a new variant.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:331 on 2025-03-07 10:47_

And everywhere else where you used `TextSize::try_from` on a `&str` or `char`

```suggestion
        let comment_start = before_noqa.text_len() - '#'.text_len();
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:329 on 2025-03-07 10:51_

I find this code hard to read because we first search for `n` or `N` and then go back to the `#`. This complicates the offset calculations by a fair amount. For example, what I'm not a 100% sure is if the offset calculations correctly account for any whitespace that was removed by `trim_end`. It probably does but I find it hard to tell.

Would it be easier to find all `#` tokens, trim the start after the `#` and then test if what comes after is a noqa comment? This has the advantage that all operations are from left to right.
Not that it will make a significant performance difference, but jumping forward and backwards when doing memory lookups is bad for performance because it makes it harder for the CPU to predict which memory address you'll lookup next and is worth loading into the L1 or L2 cache.

For example, we could rewrite this to (using `Cursor`) and remove all manual offset calculations

```rust
    let line = &source[comment_range];
    let offset = comment_range.start();

    let mut cursor = Cursor::new(line);

    while !cursor.is_eof() {
        // Skip over any leading content.
        cursor.eat_while(|c| c != '#');

        let comment_start = cursor.token_len();

        if !cursor.eat_char('#') {
            continue;
        }

        // Skip over any whitespace
        cursor.eat_while(|c| c.is_whitespace());

        if !is_noqa_at_position(cursor.as_str(), 0) {
            continue;
        }

        for _ in 0.."noqa".len() {
            cursor.bump();
        }

        let noqa_end = cursor.token_len();

        let lexer = NoqaLexer::new(
            source,
            TextRange::new(offset + noqa_end, comment_range.end()),
        );

        return Ok(Some(lexer.lex(offset + comment_start)?));
    }

    Ok(None)
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:344 on 2025-03-07 10:54_

Is this spelling correct?
```suggestion
/// Lexes file-level exemption comment, e.g. `# ruff: noqa: F401`
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:345 on 2025-03-07 10:55_

Can you verify whether `pub(crate)` is needed or if we can reduce the visiblity to private?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:355 on 2025-03-07 10:56_

Same as for the other lexing function: I'd find it easier to reason about if the entire lexing only went from left to right and doesn't navigate both left and right.

---

_Comment by @dylwil3 on 2025-03-07 11:10_

> Hmm, I don't understand the ecosystem results. Are we now ignoring the code for file level suppressions?

There is a missing colon, it should be

```diff
- # flake8: noqa E501
+ # flake8: noqa: E501
```

See the comment above https://github.com/astral-sh/ruff/pull/16483#issuecomment-2697739241

Note this is now consistent with what we have always done for inline `noqa` comments, i.e. `# noqa E501` suppresses all rules on the line while `# noqa: E501` just suppresses `E501`.

We could instead allow this "syntax error" and merge in the logic used in the rule [blanket-noqa (PGH004)](https://docs.astral.sh/ruff/rules/blanket-noqa/#blanket-noqa-pgh004), but that seemed like a larger change.


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:338 on 2025-03-07 11:13_

What's the reason that we return the first code here instead of collecting all? Do we not support `# noqa: RUF100 # noqa: RUF101`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:466 on 2025-03-07 11:14_

It's not clear to me why we'd return `All` if the lexer is at a whitespace. What if we have `# noqa:        RUF100`? Would that return `All`?

Should this use `is_whitespace` similar to how you use `trim_end` in other places?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:482 on 2025-03-07 11:15_


```suggestion
        if !self.cursor.eat(':') {
            return Err(LexicalError::InvalidSuffix);
        }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:483 on 2025-03-07 11:17_

Although I'd probably rewrite this entire section to
```suggestion
        match self.cursor.bump() {
            Some('#') => {
                // We reached another nested `noqa` comment
                return Ok(NoqaLexerOutput {
                    warnings: self.warnings,
                    directive: Directive::All(All {
                        range: TextRange::new(comment_start, self.offset),
                    }),
                });
            }
            // Comment without a code
            None => {
                return Ok(NoqaLexerOutput {
                    warnings: self.warnings,
                    directive: Directive::All(All {
                        range: TextRange::new(comment_start, self.offset),
                    }),
                });
            }
            c if c.is_whitespace() => {
                // Unclear
            }
            ':' => {
                self.lex_codes()?;
            }
            _ => {
                return Err(LexicalError::InvaildSufix)
            }
        }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:465 on 2025-03-07 11:20_

It seems a bit strange to me that we pass the `comment_start` here. 

Overall, I suspect that having a single lexing function that tries to lex a `noqa` or a file excemption and the codes might be simpler. It would also allow us to emit a specific error message if we find a file level excemption code as an inlien code.


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:508 on 2025-03-07 11:21_

Nit: I prefer to always use `bump` because it ensures that:

a) The lexer always progresses
b) It handles the EOF case

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:520 on 2025-03-07 11:22_

I would probably move this out of the match:

```
// Skip over whitespace
let has_whitespace = self.cursor.eat_while(|c| c.is_whitespace());

if has_whitespace {
    if self.cursor.eat_char(',') {
        self.warnings.push(MissingItem...)
    }
}

match self.cursor.bump() {

}

```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:533 on 2025-03-07 11:24_


```suggestion
                if !self.cursor.eat_if(|c| c.is_ascii_digit()) {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:567 on 2025-03-07 11:24_

Nit: Use `bump`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:600 on 2025-03-07 11:27_

Should this push an error, considering that it isn't a valid code?

---

_@MichaReiser reviewed on 2025-03-07 11:30_

Thanks for looking into this. 

I've some smaller nit comments that I think will improve readability (and fewer offset calculations!). 

My main design question are:
* I find it hard to reason about the 3 different lexing methods and how they interact together. I wonder if we should have a single `NoqaLexer` that does the lexing for the entire comment instead. It may have helper methods similar to `lex_code` or `lex_codes` to split the logic but we avoid constructing multiple `Cursor`s. 
* whether it would make sense to parse all suppression comments eagerly and collect them into a `Vec<SuppressionComment>` similar to what we do in Red Knot. 

---

_@dylwil3 reviewed on 2025-03-07 12:43_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/noqa.rs`:466 on 2025-03-07 12:43_

Here our cursor starts at the end of the `noqa`, so in your example it would be at the `':'` not the whitespace. The idea for the whitespace is to support having a trailing comment after an `All` suppresion, like: `# noqa This line is so bad don't look at it`

And I agree it should be `is_whitespace`, good catch!

---

_@dylwil3 reviewed on 2025-03-07 12:52_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/noqa.rs`:600 on 2025-03-07 12:52_

I don't think so - the first "invalid code" is usually the beginning of a comment, so we just abort. Like:

```text
# noqa: F401 we import this for a reason
             ^ ---- this would hit that match arm 
```


---

_Comment by @dylwil3 on 2025-03-07 12:59_

Thanks Micha, for the detailed review and correcting my rookie `Cursor` usage 😄 !  

I agree that incorporating the prefix stripping into the lexer and keeping everything going from left to right would be easier to reason about - I hadn't done this because I thought there was some performance reason for the approach that was already present in the code. But I'll give it a try!

I like the idea of parsing the suppression comments eagerly, but I wonder if that should be a follow-up PR. I would have to think about when we parse them and where we store them; I'll have to check out what you've set up in Red Knot and see if the same thing works here.

---

_Comment by @MichaReiser on 2025-03-07 13:30_

> I like the idea of parsing the suppression comments eagerly, but I wonder if that should be a follow-up PR. I would have to think about when we parse them and where we store them; I'll have to check out what you've set up in Red Knot and see if the same thing works here.

I'm okay with that. Also considering that this should go into 0.10

---

_@dylwil3 reviewed on 2025-03-07 15:33_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/noqa.rs`:338 on 2025-03-07 15:33_

We don't currently support that: [Playground link](https://play.ruff.rs/bb6dabe0-2d74-48af-a7f9-4a541ebb3d5b)

But I'm not opposed to it!

---

_@dylwil3 reviewed on 2025-03-07 19:02_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/noqa.rs`:567 on 2025-03-07 19:02_

I don't think I can here - if the upcoming character is uppercase, then I want to leave it unconsumed until I lex the next code.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/noqa.rs`:520 on 2025-03-07 19:04_

I don't think I can `eat_char(',')` in the `if` here because I want the range for the MissingItem to be inside the commas - that's why I had it after pushing the warning.

---

_@dylwil3 reviewed on 2025-03-07 19:04_

---

_@MichaReiser reviewed on 2025-03-07 20:49_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:520 on 2025-03-07 20:49_

Makes sense. You could store the range in a variable but I can see how this is "not so nice"

---

_@MichaReiser reviewed on 2025-03-07 20:50_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:600 on 2025-03-07 20:50_

Let's add a few comments with examples to those match arms. I would find them useful :)

---

_@dylwil3 reviewed on 2025-03-07 22:28_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/noqa.rs`:345 on 2025-03-07 22:28_

made private, good catch!

---

_@dylwil3 reviewed on 2025-03-07 22:30_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/noqa.rs`:465 on 2025-03-07 22:30_

you're absolutely right! hopefully cleaner now

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/noqa.rs`:1251 on 2025-03-10 10:53_

nit: we could move the `if let ...` in `assert_codes_match_slices` (and rename the function accordingly) to reduce the diff

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/noqa.rs`:374 on 2025-03-10 11:02_

I'd find it useful to document what `source` means here. Looking at the implementation, I think it's only a subset of the entire file content. 

For [`parse_string_annotation`](https://github.com/astral-sh/ruff/blob/7d8fdb04c240d5c4f9f72a0e6aae42d0739fc099/crates/ruff_python_parser/src/lib.rs#L222-L225) we consider `source` to be the entire file content and the implementation would extract out the relevant subslice.

I'm not asking you to change but it would be useful to make sure we pass the correct source here.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/noqa.rs`:1361 on 2025-03-10 11:03_

What would happen if there are whitespaces between multiple hashes? i.e., `# # # noqa: F401`

---

_@dhruvmanila reviewed on 2025-03-10 11:06_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:318 on 2025-03-10 13:10_

Nit: Both `lex_inline_noqa` and `lex_file_exemption` need to do the slicing and range selection here. Should we add a `NoqaLexer::range(full_source, range)` or `NoqaLexer::in_range(full_source, range)` method instead?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:349 on 2025-03-10 13:13_

Is this comment still accurate or is it now the start of the comment? 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:344 on 2025-03-10 13:13_

Is this comment still accurate or is it the beginning of a comment (`#`)? We also don't know if it is a noqa comment at this point. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:431 on 2025-03-10 13:18_

Nit: You could consider introducing a `skip_whitespace` method on `Cursor` or on your Lexer

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:450 on 2025-03-10 13:19_

Nit
```suggestion
            if self.cursor.as_str().starts_with("ruff") {
                self.cursor.skip_bytes("ruff".len());
            } else if self.cursor.as_str().starts_with("flake8") {
                self.cursor.skip_bytes("flake8".len());
            } else {
                continue;
            }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:498 on 2025-03-10 13:23_

Okay, I think I now see what I find confusing about this. It means we e.g. will assume that `noqa : RUF100` suppresses all rules and not only RUF100. It may be worth calling out that this isn't a leagal comment.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:505 on 2025-03-10 13:29_

I'm wondering if we should be more lenient. E.g. `noqa : F401` will pass without any error but a user clearly meant to write `noqa: F401`. Should we just skip any whitespace after `noqa` and then decide on the next character?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:572 on 2025-03-10 13:34_

I think we could remove the need to store `line` by:


```suggestion
                let before_code = self.cursor.as_str();

                self.cursor.eat_while(|chr| chr.is_ascii_uppercase());
                if !self.cursor.eat_if(|c| c.is_ascii_digit()) {
                    // Fail hard if we're already attempting
                    // to lex squashed codes, e.g. `F401Fabc`
                    if self.missing_delimiter {
                        return Err(LexicalError::InvalidCodeSuffix);
                    }
                    // Otherwise we've reached the first invalid code,
                    // so it could be a comment and we just stop parsing.
                    // Ex) #noqa: F401 A comment
                    //       we're here^
                    self.cursor.skip_bytes(self.cursor.as_str().len());
                    return Ok(());
                }
                self.cursor.eat_while(|chr| chr.is_ascii_digit());
                // I believe `cursor` has a helper for this calculation
                let code = &before_code[..(before_code.len() - self.cursor.as_str().len())];
                self.push_code(code);

                if self.cursor.is_eof() {
                    return Ok(());
                }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:581 on 2025-03-10 13:36_

Can we add an example here? E.g. what about `noqa: RUF100 RUF200`? It's also not clear to me what you mean by non-ascii whitespace

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:636 on 2025-03-10 13:38_

Nit: I'd rename this to `position` or `offset`. It's unclear if `current` refers to the current offset or current character or something else.

---

_@MichaReiser approved on 2025-03-10 13:38_

---

_Comment by @MichaReiser on 2025-03-10 14:10_

For the changelog: Could you write a short summary detailing which comments are no longer accepted that previously were and how the difference will manifest for users?

---

_Label `do-not-merge` removed by @MichaReiser on 2025-03-10 14:25_

---

_Comment by @MichaReiser on 2025-03-10 14:26_

Same here. Feel free to merge this PR into the ruff-0.10 feature branch once it's ready

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/noqa.rs`:374 on 2025-03-10 21:37_

changed to `in_range` which _does_ take the entire source as the argument

---

_@dylwil3 reviewed on 2025-03-10 21:37_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/noqa.rs`:1361 on 2025-03-10 21:37_

added test (spoiler: we grab the `F401` correctly)

---

_@dylwil3 reviewed on 2025-03-10 21:37_

---

_@dylwil3 reviewed on 2025-03-10 21:38_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/noqa.rs`:349 on 2025-03-10 21:38_

updated (it's the start of the comment containing the noqa - and may be updated during lexing if there are multiple comments in the range)

---

_@dylwil3 reviewed on 2025-03-10 21:38_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/noqa.rs`:344 on 2025-03-10 21:38_

updated (now `line_length` anyway)

---

_@dylwil3 reviewed on 2025-03-10 21:38_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/noqa.rs`:431 on 2025-03-10 21:38_

added (as `eat_whitespace`)

---

_@dylwil3 reviewed on 2025-03-10 21:38_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/noqa.rs`:572 on 2025-03-10 21:38_

nice! still needed to store the length of the original line to compute ranges, but got rid of `line` 

---

_@dylwil3 reviewed on 2025-03-10 21:38_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/noqa.rs`:636 on 2025-03-10 21:38_

renamed to position

---

_@dylwil3 reviewed on 2025-03-10 21:38_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/noqa.rs`:581 on 2025-03-10 21:38_

example added. and, since everything else has been allowed to be arbitrary whitespace, I just decided to allow all unicode whitespace as a delimiter (anything that starts a new line is automatically prohibited since we started with a comment range)

---

_@dylwil3 reviewed on 2025-03-10 21:38_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/noqa.rs`:505 on 2025-03-10 21:38_

agreed, see above

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/noqa.rs`:498 on 2025-03-10 21:38_

made this more flexible - we now allow whitespace before the colon

---

_@dylwil3 reviewed on 2025-03-10 21:38_

---

_Comment by @dylwil3 on 2025-03-10 22:26_

For the changelog (let me know if you want me to actually put it in, say, `BREAKING_CHANGES.md` as part of this PR @MichaReiser):

- **Suppression comments**

   The syntax rules governing inline (`# noqa`) and file-level (`# ruff: noqa`) suppression comments have been unified and overall made more flexible. 
   
   In the following situations, a comment will suppress _more_ rules than it used to:
   - Leading comments are now allowed for file-level suppression comments, e.g. `# Some context # ruff: noqa`. Previously, this `noqa` was silently ignored.
   - Trailing comments are now allowed for file-level suppression comments, e.g. `# ruff: noqa  This file is rough!` and  are both allowed, whereas previously this resulted in an error and the suppression did not take effect.
   - To support the previous change, mistaken syntax like `ruff: noqa UP035` will now suppress _all_ rules (it is missing a colon after `noqa`). This is consistent with the already established in-line behavior, and is guarded against by the preview behavior of `PGH004`.
   - Missing items in lists of rules are now read with a warning, e.g. `#noqa: F401,,F841` will suppress `F401` and `F841` and emit `MissingItem` warning. 
   - Skipped delimiters in lists of rules are now read in both the in-line and file-level comments and a warning is emitted, e.g. `# ruff: noqa: F401F841` will suppress both `F401` and `F841` and a `MissingDelimiter` warning is emitted.
   
   In the following situations, a comment may suppress _fewer_ rules than it used to:
   - It is now _disallowed_ to have an invalid rule suffix in inline suppression comments, e.g. `# noqa: UP035abc`. This will log an error to the user and suppress no rules. This matches the current behavior for file-level suppression comments.
   - Whitespace is allowed between the colon and rule list, e.g. `#noqa : F401` previously suppressed _all_ rules, and will now only suppress `F401`. This used to be guarded against through `PGH004`, but that is now no longer needed. 
   
   
   The full specification is as follows:
   - An _in-line blanket noqa_ comment is given by a case-insensitive match for `#noqa` with optional whitespace after the `#` symbol, followed by either: the end of the comment, the beginning of a new comment (`'#'`), or whitespace followed by any character other than `':'`.
   - An _in-line rule suppression_ is given by first finding a case-insensitive match for `#noqa` with optional whitespace after the `#` symbol, optional whitespace after `noqa`, and followed by the symbol `':'`. After this we are expected to have a _list of rule codes_ which is given by sequences of uppercase ascii characters followed by ascii digits, separated by whitespace or commas. The list ends at the last valid code. We will attempt to interpret rules with a missing delimiter (e.g. `F401F841`), though a warning will be emitted in this case.
   - A _file-level exemption_ comment is given by a case-sensitive match for `#ruff:` or `#flake8:`, with optional whitespace after `'#'` and before `':'`, followed by optional whitespace and a case-insensitive match for `noqa`. After this, the specification is as in the in-line case.


---

_Merged by @dylwil3 on 2025-03-11 19:50_

---

_Closed by @dylwil3 on 2025-03-11 19:50_

---
