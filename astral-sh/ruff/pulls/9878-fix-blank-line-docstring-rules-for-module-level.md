```yaml
number: 9878
title: Fix blank-line docstring rules for module-level docstrings
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - docstring
assignees: []
merged: true
base: main
head: charlie/doc
created_at: 2024-02-07T21:03:23Z
updated_at: 2024-02-07T21:55:05Z
url: https://github.com/astral-sh/ruff/pull/9878
synced_at: 2026-01-12T15:55:30Z
```

# Fix blank-line docstring rules for module-level docstrings

---

_@charliermarsh_

## Summary

Given:

```python
"""Make a summary line.

Note:
----
  Per the code comment the next two lines are blank. "// The first blank line is the line containing the closing
      triple quotes, so we need at least two."

"""
```

It turns out we excluded the line ending in `"""`, because it's empty (unlike for functions, where it consists of the indent). This PR changes the `following_lines` iterator to always include the trailing newline, which gives us correct and consistent handling between function and module-level docstrings.

Closes https://github.com/astral-sh/ruff/issues/9877.


---

_Label `bug` added by @charliermarsh on 2024-02-07 21:03_

---

_Label `docstring` added by @charliermarsh on 2024-02-07 21:03_

---

_Comment by @github-actions[bot] on 2024-02-07 21:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -63 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -63 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/driving.py#L7'>src/bokeh/driving.py:7:1:</a> D413 [*] Missing blank line after last section ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L1004'>src/bokeh/plotting/glyph_api.py:1004:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L110'>src/bokeh/plotting/glyph_api.py:110:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L130'>src/bokeh/plotting/glyph_api.py:130:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L146'>src/bokeh/plotting/glyph_api.py:146:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L163'>src/bokeh/plotting/glyph_api.py:163:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L180'>src/bokeh/plotting/glyph_api.py:180:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L197'>src/bokeh/plotting/glyph_api.py:197:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L214'>src/bokeh/plotting/glyph_api.py:214:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L231'>src/bokeh/plotting/glyph_api.py:231:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L248'>src/bokeh/plotting/glyph_api.py:248:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L265'>src/bokeh/plotting/glyph_api.py:265:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L282'>src/bokeh/plotting/glyph_api.py:282:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L299'>src/bokeh/plotting/glyph_api.py:299:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L315'>src/bokeh/plotting/glyph_api.py:315:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L332'>src/bokeh/plotting/glyph_api.py:332:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L349'>src/bokeh/plotting/glyph_api.py:349:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L365'>src/bokeh/plotting/glyph_api.py:365:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L381'>src/bokeh/plotting/glyph_api.py:381:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L397'>src/bokeh/plotting/glyph_api.py:397:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L414'>src/bokeh/plotting/glyph_api.py:414:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L430'>src/bokeh/plotting/glyph_api.py:430:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L447'>src/bokeh/plotting/glyph_api.py:447:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L490'>src/bokeh/plotting/glyph_api.py:490:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L506'>src/bokeh/plotting/glyph_api.py:506:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L522'>src/bokeh/plotting/glyph_api.py:522:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L543'>src/bokeh/plotting/glyph_api.py:543:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L564'>src/bokeh/plotting/glyph_api.py:564:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L580'>src/bokeh/plotting/glyph_api.py:580:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L601'>src/bokeh/plotting/glyph_api.py:601:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L617'>src/bokeh/plotting/glyph_api.py:617:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L638'>src/bokeh/plotting/glyph_api.py:638:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L655'>src/bokeh/plotting/glyph_api.py:655:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L677'>src/bokeh/plotting/glyph_api.py:677:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L693'>src/bokeh/plotting/glyph_api.py:693:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L69'>src/bokeh/plotting/glyph_api.py:69:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L711'>src/bokeh/plotting/glyph_api.py:711:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L727'>src/bokeh/plotting/glyph_api.py:727:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L744'>src/bokeh/plotting/glyph_api.py:744:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L761'>src/bokeh/plotting/glyph_api.py:761:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L778'>src/bokeh/plotting/glyph_api.py:778:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L795'>src/bokeh/plotting/glyph_api.py:795:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L812'>src/bokeh/plotting/glyph_api.py:812:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L838'>src/bokeh/plotting/glyph_api.py:838:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L855'>src/bokeh/plotting/glyph_api.py:855:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L872'>src/bokeh/plotting/glyph_api.py:872:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L889'>src/bokeh/plotting/glyph_api.py:889:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L906'>src/bokeh/plotting/glyph_api.py:906:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L90'>src/bokeh/plotting/glyph_api.py:90:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L923'>src/bokeh/plotting/glyph_api.py:923:9:</a> D413 [*] Missing blank line after last section ("Examples")
... 13 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D413 | 63 | 0 | 63 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -63 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -63 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/driving.py#L7'>src/bokeh/driving.py:7:1:</a> D413 [*] Missing blank line after last section ("Example")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L1004'>src/bokeh/plotting/glyph_api.py:1004:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L110'>src/bokeh/plotting/glyph_api.py:110:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L130'>src/bokeh/plotting/glyph_api.py:130:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L146'>src/bokeh/plotting/glyph_api.py:146:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L163'>src/bokeh/plotting/glyph_api.py:163:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L180'>src/bokeh/plotting/glyph_api.py:180:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L197'>src/bokeh/plotting/glyph_api.py:197:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L214'>src/bokeh/plotting/glyph_api.py:214:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L231'>src/bokeh/plotting/glyph_api.py:231:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L248'>src/bokeh/plotting/glyph_api.py:248:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L265'>src/bokeh/plotting/glyph_api.py:265:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L282'>src/bokeh/plotting/glyph_api.py:282:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L299'>src/bokeh/plotting/glyph_api.py:299:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L315'>src/bokeh/plotting/glyph_api.py:315:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L332'>src/bokeh/plotting/glyph_api.py:332:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L349'>src/bokeh/plotting/glyph_api.py:349:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L365'>src/bokeh/plotting/glyph_api.py:365:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L381'>src/bokeh/plotting/glyph_api.py:381:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L397'>src/bokeh/plotting/glyph_api.py:397:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L414'>src/bokeh/plotting/glyph_api.py:414:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L430'>src/bokeh/plotting/glyph_api.py:430:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L447'>src/bokeh/plotting/glyph_api.py:447:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L490'>src/bokeh/plotting/glyph_api.py:490:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L506'>src/bokeh/plotting/glyph_api.py:506:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L522'>src/bokeh/plotting/glyph_api.py:522:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L543'>src/bokeh/plotting/glyph_api.py:543:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L564'>src/bokeh/plotting/glyph_api.py:564:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L580'>src/bokeh/plotting/glyph_api.py:580:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L601'>src/bokeh/plotting/glyph_api.py:601:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L617'>src/bokeh/plotting/glyph_api.py:617:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L638'>src/bokeh/plotting/glyph_api.py:638:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L655'>src/bokeh/plotting/glyph_api.py:655:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L677'>src/bokeh/plotting/glyph_api.py:677:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L693'>src/bokeh/plotting/glyph_api.py:693:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L69'>src/bokeh/plotting/glyph_api.py:69:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L711'>src/bokeh/plotting/glyph_api.py:711:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L727'>src/bokeh/plotting/glyph_api.py:727:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L744'>src/bokeh/plotting/glyph_api.py:744:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L761'>src/bokeh/plotting/glyph_api.py:761:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L778'>src/bokeh/plotting/glyph_api.py:778:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L795'>src/bokeh/plotting/glyph_api.py:795:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L812'>src/bokeh/plotting/glyph_api.py:812:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L838'>src/bokeh/plotting/glyph_api.py:838:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L855'>src/bokeh/plotting/glyph_api.py:855:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L872'>src/bokeh/plotting/glyph_api.py:872:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L889'>src/bokeh/plotting/glyph_api.py:889:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L906'>src/bokeh/plotting/glyph_api.py:906:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L90'>src/bokeh/plotting/glyph_api.py:90:9:</a> D413 [*] Missing blank line after last section ("Examples")
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L923'>src/bokeh/plotting/glyph_api.py:923:9:</a> D413 [*] Missing blank line after last section ("Examples")
... 13 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D413 | 63 | 0 | 63 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @charliermarsh on 2024-02-07 21:48_

---

_Closed by @charliermarsh on 2024-02-07 21:48_

---

_Branch deleted on 2024-02-07 21:48_

---

_Comment by @charliermarsh on 2024-02-07 21:48_

Ecosystem changes are 👍 

---

_Comment by @BrentWilkins on 2024-02-07 21:55_

Thanks!

---
