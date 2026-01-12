```yaml
number: 8743
title: Remove incorrect deprecation label for stdout and stderr
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dep
created_at: 2023-11-17T17:23:04Z
updated_at: 2023-11-17T17:34:56Z
url: https://github.com/astral-sh/ruff/pull/8743
synced_at: 2026-01-10T23:40:55Z
```

# Remove incorrect deprecation label for stdout and stderr

---

_Pull request opened by @charliermarsh on 2023-11-17 17:23_

Closes https://github.com/astral-sh/ruff/issues/8738.

---

_Label `bug` added by @charliermarsh on 2023-11-17 17:23_

---

_Merged by @charliermarsh on 2023-11-17 17:34_

---

_Closed by @charliermarsh on 2023-11-17 17:34_

---

_Branch deleted on 2023-11-17 17:34_

---

_Comment by @github-actions[bot] on 2023-11-17 17:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+7 -7 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+7 -7 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/CommonScripts/Scripts/ReadPDFFileV2/ReadPDFFileV2.py#L123'>Packs/CommonScripts/Scripts/ReadPDFFileV2/ReadPDFFileV2.py:123:25:</a> UP022 Prefer `capture_output` over sending `stdout` and `stderr` to `PIPE`
- <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/CommonScripts/Scripts/ReadPDFFileV2/ReadPDFFileV2.py#L123'>Packs/CommonScripts/Scripts/ReadPDFFileV2/ReadPDFFileV2.py:123:25:</a> UP022 Sending `stdout` and `stderr` to `PIPE` is deprecated, use `capture_output`
+ <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L113'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:113:18:</a> UP022 Prefer `capture_output` over sending `stdout` and `stderr` to `PIPE`
- <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L113'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:113:18:</a> UP022 Sending `stdout` and `stderr` to `PIPE` is deprecated, use `capture_output`
+ <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L117'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:117:18:</a> UP022 Prefer `capture_output` over sending `stdout` and `stderr` to `PIPE`
- <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L117'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:117:18:</a> UP022 Sending `stdout` and `stderr` to `PIPE` is deprecated, use `capture_output`
+ <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L71'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:71:18:</a> UP022 Prefer `capture_output` over sending `stdout` and `stderr` to `PIPE`
- <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L71'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:71:18:</a> UP022 Sending `stdout` and `stderr` to `PIPE` is deprecated, use `capture_output`
+ <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L73'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:73:18:</a> UP022 Prefer `capture_output` over sending `stdout` and `stderr` to `PIPE`
- <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L73'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:73:18:</a> UP022 Sending `stdout` and `stderr` to `PIPE` is deprecated, use `capture_output`
+ <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L79'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:79:18:</a> UP022 Prefer `capture_output` over sending `stdout` and `stderr` to `PIPE`
- <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L79'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:79:18:</a> UP022 Sending `stdout` and `stderr` to `PIPE` is deprecated, use `capture_output`
+ <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L81'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:81:18:</a> UP022 Prefer `capture_output` over sending `stdout` and `stderr` to `PIPE`
- <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L81'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:81:18:</a> UP022 Sending `stdout` and `stderr` to `PIPE` is deprecated, use `capture_output`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP022 | 14 | 7 | 7 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+7 -7 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+7 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/CommonScripts/Scripts/ReadPDFFileV2/ReadPDFFileV2.py#L123'>Packs/CommonScripts/Scripts/ReadPDFFileV2/ReadPDFFileV2.py:123:25:</a> UP022 Prefer `capture_output` over sending `stdout` and `stderr` to `PIPE`
- <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/CommonScripts/Scripts/ReadPDFFileV2/ReadPDFFileV2.py#L123'>Packs/CommonScripts/Scripts/ReadPDFFileV2/ReadPDFFileV2.py:123:25:</a> UP022 Sending `stdout` and `stderr` to `PIPE` is deprecated, use `capture_output`
+ <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L113'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:113:18:</a> UP022 Prefer `capture_output` over sending `stdout` and `stderr` to `PIPE`
- <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L113'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:113:18:</a> UP022 Sending `stdout` and `stderr` to `PIPE` is deprecated, use `capture_output`
+ <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L117'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:117:18:</a> UP022 Prefer `capture_output` over sending `stdout` and `stderr` to `PIPE`
- <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L117'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:117:18:</a> UP022 Sending `stdout` and `stderr` to `PIPE` is deprecated, use `capture_output`
+ <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L71'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:71:18:</a> UP022 Prefer `capture_output` over sending `stdout` and `stderr` to `PIPE`
- <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L71'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:71:18:</a> UP022 Sending `stdout` and `stderr` to `PIPE` is deprecated, use `capture_output`
+ <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L73'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:73:18:</a> UP022 Prefer `capture_output` over sending `stdout` and `stderr` to `PIPE`
- <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L73'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:73:18:</a> UP022 Sending `stdout` and `stderr` to `PIPE` is deprecated, use `capture_output`
+ <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L79'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:79:18:</a> UP022 Prefer `capture_output` over sending `stdout` and `stderr` to `PIPE`
- <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L79'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:79:18:</a> UP022 Sending `stdout` and `stderr` to `PIPE` is deprecated, use `capture_output`
+ <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L81'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:81:18:</a> UP022 Prefer `capture_output` over sending `stdout` and `stderr` to `PIPE`
- <a href='https://github.com/demisto/content/blob/8b88a55c26c21e245b7cf4568e7cde1ea2e97c11/Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py#L81'>Packs/PaloAltoNetworks_PAN_OS_EDL_Management/Integrations/PaloAltoNetworks_PAN_OS_EDL_Management/PaloAltoNetworks_PAN_OS_EDL_Management.py:81:18:</a> UP022 Sending `stdout` and `stderr` to `PIPE` is deprecated, use `capture_output`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP022 | 14 | 7 | 7 | 0 | 0 |

</p>
</details>




---
