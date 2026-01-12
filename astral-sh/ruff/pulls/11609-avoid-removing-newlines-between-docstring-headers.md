```yaml
number: 11609
title: Avoid removing newlines between docstring headers and rST blocks
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - docstring
assignees: []
merged: true
base: main
head: charlie/sphinx-directive
created_at: 2024-05-30T04:19:57Z
updated_at: 2024-05-30T17:29:21Z
url: https://github.com/astral-sh/ruff/pull/11609
synced_at: 2026-01-12T15:55:38Z
```

# Avoid removing newlines between docstring headers and rST blocks

---

_@charliermarsh_

Given:

```python
def func():
    """
    Example:

    .. code-block:: python

        import foo
    """
```

Removing the newline after the `Example:` header breaks Sphinx rendering.

See: https://github.com/astral-sh/ruff/issues/11577


---

_Label `bug` added by @charliermarsh on 2024-05-30 04:20_

---

_Label `docstring` added by @charliermarsh on 2024-05-30 04:20_

---

_Comment by @charliermarsh on 2024-05-30 04:20_

\cc @harupy 

---

_Comment by @github-actions[bot] on 2024-05-30 04:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -93 violations, +0 -0 fixes in 2 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -92 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L100'>src/bokeh/command/subcommands/file_output.py:100:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L70'>src/bokeh/command/subcommands/file_output.py:70:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/util.py#L193'>src/bokeh/command/util.py:193:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/json_encoder.py#L103'>src/bokeh/core/json_encoder.py:103:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/any.py#L60'>src/bokeh/core/property/any.py:60:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/color.py#L100'>src/bokeh/core/property/color.py:100:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptors.py#L263'>src/bokeh/core/property/descriptors.py:263:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/either.py#L57'>src/bokeh/core/property/either.py:57:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L136'>src/bokeh/core/property/numeric.py:136:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L181'>src/bokeh/core/property/numeric.py:181:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L214'>src/bokeh/core/property/numeric.py:214:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L255'>src/bokeh/core/property/numeric.py:255:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/primitive.py#L131'>src/bokeh/core/property/primitive.py:131:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/primitive.py#L166'>src/bokeh/core/property/primitive.py:166:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/primitive.py#L212'>src/bokeh/core/property/primitive.py:212:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/primitive.py#L77'>src/bokeh/core/property/primitive.py:77:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/string.py#L59'>src/bokeh/core/property/string.py:59:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L78'>src/bokeh/core/query.py:78:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/models.py#L123'>src/bokeh/document/models.py:123:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/model.py#L464'>src/bokeh/model/model.py:464:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/util.py#L154'>src/bokeh/model/util.py:154:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/plots.py#L157'>src/bokeh/models/plots.py:157:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L438'>src/bokeh/models/sources.py:438:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L493'>src/bokeh/models/sources.py:493:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/tools.py#L616'>src/bokeh/models/tools.py:616:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1696'>src/bokeh/palettes.py:1696:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1725'>src/bokeh/palettes.py:1725:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1754'>src/bokeh/palettes.py:1754:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1783'>src/bokeh/palettes.py:1783:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1812'>src/bokeh/palettes.py:1812:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1845'>src/bokeh/palettes.py:1845:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1874'>src/bokeh/palettes.py:1874:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1906'>src/bokeh/palettes.py:1906:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L309'>src/bokeh/plotting/_figure.py:309:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L1005'>src/bokeh/plotting/glyph_api.py:1005:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L115'>src/bokeh/plotting/glyph_api.py:115:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L131'>src/bokeh/plotting/glyph_api.py:131:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L147'>src/bokeh/plotting/glyph_api.py:147:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L164'>src/bokeh/plotting/glyph_api.py:164:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L181'>src/bokeh/plotting/glyph_api.py:181:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L198'>src/bokeh/plotting/glyph_api.py:198:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L215'>src/bokeh/plotting/glyph_api.py:215:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L232'>src/bokeh/plotting/glyph_api.py:232:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L249'>src/bokeh/plotting/glyph_api.py:249:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L266'>src/bokeh/plotting/glyph_api.py:266:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L283'>src/bokeh/plotting/glyph_api.py:283:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L300'>src/bokeh/plotting/glyph_api.py:300:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L316'>src/bokeh/plotting/glyph_api.py:316:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L333'>src/bokeh/plotting/glyph_api.py:333:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
... 43 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/ff4377516926e701cbf2a7e95693fcba8cddfe5e/latch/types/metadata.py#L463'>latch/types/metadata.py:463:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D412 | 93 | 0 | 93 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -93 violations, +0 -0 fixes in 2 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -92 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L100'>src/bokeh/command/subcommands/file_output.py:100:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L70'>src/bokeh/command/subcommands/file_output.py:70:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/util.py#L193'>src/bokeh/command/util.py:193:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/json_encoder.py#L103'>src/bokeh/core/json_encoder.py:103:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/any.py#L60'>src/bokeh/core/property/any.py:60:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/color.py#L100'>src/bokeh/core/property/color.py:100:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptors.py#L263'>src/bokeh/core/property/descriptors.py:263:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/either.py#L57'>src/bokeh/core/property/either.py:57:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L136'>src/bokeh/core/property/numeric.py:136:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L181'>src/bokeh/core/property/numeric.py:181:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L214'>src/bokeh/core/property/numeric.py:214:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L255'>src/bokeh/core/property/numeric.py:255:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/primitive.py#L131'>src/bokeh/core/property/primitive.py:131:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/primitive.py#L166'>src/bokeh/core/property/primitive.py:166:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/primitive.py#L212'>src/bokeh/core/property/primitive.py:212:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/primitive.py#L77'>src/bokeh/core/property/primitive.py:77:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/string.py#L59'>src/bokeh/core/property/string.py:59:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L78'>src/bokeh/core/query.py:78:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/models.py#L123'>src/bokeh/document/models.py:123:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/model.py#L464'>src/bokeh/model/model.py:464:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/util.py#L154'>src/bokeh/model/util.py:154:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/plots.py#L157'>src/bokeh/models/plots.py:157:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L438'>src/bokeh/models/sources.py:438:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L493'>src/bokeh/models/sources.py:493:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/tools.py#L616'>src/bokeh/models/tools.py:616:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1696'>src/bokeh/palettes.py:1696:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1725'>src/bokeh/palettes.py:1725:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1754'>src/bokeh/palettes.py:1754:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1783'>src/bokeh/palettes.py:1783:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1812'>src/bokeh/palettes.py:1812:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1845'>src/bokeh/palettes.py:1845:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1874'>src/bokeh/palettes.py:1874:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1906'>src/bokeh/palettes.py:1906:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L309'>src/bokeh/plotting/_figure.py:309:9:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L1005'>src/bokeh/plotting/glyph_api.py:1005:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L115'>src/bokeh/plotting/glyph_api.py:115:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L131'>src/bokeh/plotting/glyph_api.py:131:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L147'>src/bokeh/plotting/glyph_api.py:147:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L164'>src/bokeh/plotting/glyph_api.py:164:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L181'>src/bokeh/plotting/glyph_api.py:181:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L198'>src/bokeh/plotting/glyph_api.py:198:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L215'>src/bokeh/plotting/glyph_api.py:215:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L232'>src/bokeh/plotting/glyph_api.py:232:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L249'>src/bokeh/plotting/glyph_api.py:249:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L266'>src/bokeh/plotting/glyph_api.py:266:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L283'>src/bokeh/plotting/glyph_api.py:283:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L300'>src/bokeh/plotting/glyph_api.py:300:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L316'>src/bokeh/plotting/glyph_api.py:316:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L333'>src/bokeh/plotting/glyph_api.py:333:1:</a> D412 [*] No blank lines allowed between a section header and its content ("Examples")
... 43 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/ff4377516926e701cbf2a7e95693fcba8cddfe5e/latch/types/metadata.py#L463'>latch/types/metadata.py:463:5:</a> D412 [*] No blank lines allowed between a section header and its content ("Example")
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D412 | 93 | 0 | 93 | 0 | 0 |

</p>
</details>




---

_Comment by @harupy on 2024-05-30 04:44_

@charliermarsh thanks for fixing this!

---

_@MichaReiser approved on 2024-05-30 07:20_

---

_Merged by @charliermarsh on 2024-05-30 17:29_

---

_Closed by @charliermarsh on 2024-05-30 17:29_

---

_Branch deleted on 2024-05-30 17:29_

---
