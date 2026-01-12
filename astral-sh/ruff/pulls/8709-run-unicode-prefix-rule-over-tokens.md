```yaml
number: 8709
title: Run unicode prefix rule over tokens
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/panci
created_at: 2023-11-16T02:11:11Z
updated_at: 2023-11-16T02:38:23Z
url: https://github.com/astral-sh/ruff/pull/8709
synced_at: 2026-01-12T15:55:26Z
```

# Run unicode prefix rule over tokens

---

_@charliermarsh_

## Summary

It seems like the range of an `ExprStringLiteral` can be somewhat unreliable when the string is part of an implicit concatenation with an f-string. Using the tokens themselves is more reliable.

Closes #8680.
Closes https://github.com/astral-sh/ruff/issues/7784.

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-11-16 02:11_

---

_Label `bug` added by @charliermarsh on 2023-11-16 02:11_

---

_@dhruvmanila approved on 2023-11-16 02:19_

Thanks! I think this fixes https://github.com/astral-sh/ruff/issues/7784 as well and the new node can bring this back to the AST checker.

---

_Comment by @github-actions[bot] on 2023-11-16 02:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+77 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+77 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1033'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1033:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1034'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1034:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1035'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1035:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1036'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1036:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1037'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1037:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1038'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1038:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1039'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1039:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1049'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1049:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1050'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1050:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1051'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1051:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1052'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1052:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1053'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1053:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1054'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1054:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1055'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1055:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1078'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1078:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1079'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1079:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1080'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1080:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1081'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1081:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1082'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1082:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1083'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1083:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1084'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1084:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1085'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1085:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1235'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1235:62:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L527'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:527:73:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L528'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:528:73:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L384'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:384:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L385'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:385:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L386'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:386:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L387'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:387:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L388'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:388:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L389'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:389:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L390'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:390:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L400'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:400:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L401'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:401:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L402'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:402:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L403'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:403:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L404'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:404:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L405'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:405:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L406'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:406:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L429'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:429:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L430'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:430:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L431'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:431:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L432'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:432:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L433'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:433:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L434'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:434:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L435'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:435:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L436'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:436:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/Gmail/Integrations/Gmail/Gmail_test.py#L18'>Packs/Gmail/Integrations/Gmail/Gmail_test.py:18:27:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/Gmail/Integrations/Gmail/Gmail_test.py#L94'>Packs/Gmail/Integrations/Gmail/Gmail_test.py:94:22:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/GmailSingleUser/Integrations/GmailSingleUser/GmailSingleUser_test.py#L105'>Packs/GmailSingleUser/Integrations/GmailSingleUser/GmailSingleUser_test.py:105:22:</a> UP025 [*] Remove unicode literals from strings
... 27 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP025 | 77 | 77 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+77 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+77 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1033'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1033:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1034'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1034:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1035'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1035:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1036'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1036:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1037'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1037:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1038'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1038:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1039'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1039:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1049'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1049:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1050'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1050:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1051'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1051:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1052'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1052:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1053'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1053:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1054'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1054:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1055'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1055:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1078'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1078:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1079'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1079:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1080'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1080:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1081'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1081:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1082'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1082:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1083'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1083:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1084'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1084:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1085'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1085:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L1235'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:1235:62:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L527'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:527:73:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py#L528'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles_test.py:528:73:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L384'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:384:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L385'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:385:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L386'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:386:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L387'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:387:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L388'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:388:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L389'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:389:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L390'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:390:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L400'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:400:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L401'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:401:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L402'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:402:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L403'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:403:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L404'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:404:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L405'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:405:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L406'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:406:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L429'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:429:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L430'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:430:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L431'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:431:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L432'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:432:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L433'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:433:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L434'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:434:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L435'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:435:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py#L436'>Packs/CommonScripts/Scripts/ParseEmailFilesV2/ParseEmailFilesV2_test.py:436:16:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/Gmail/Integrations/Gmail/Gmail_test.py#L18'>Packs/Gmail/Integrations/Gmail/Gmail_test.py:18:27:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/Gmail/Integrations/Gmail/Gmail_test.py#L94'>Packs/Gmail/Integrations/Gmail/Gmail_test.py:94:22:</a> UP025 [*] Remove unicode literals from strings
+ <a href='https://github.com/demisto/content/blob/c74df4d278b1db8a6bb87cc23298273b17a1e671/Packs/GmailSingleUser/Integrations/GmailSingleUser/GmailSingleUser_test.py#L105'>Packs/GmailSingleUser/Integrations/GmailSingleUser/GmailSingleUser_test.py:105:22:</a> UP025 [*] Remove unicode literals from strings
... 27 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP025 | 77 | 77 | 0 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2023-11-16 02:30_

---

_Closed by @charliermarsh on 2023-11-16 02:30_

---

_Branch deleted on 2023-11-16 02:30_

---
