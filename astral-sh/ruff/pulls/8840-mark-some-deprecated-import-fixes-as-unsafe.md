```yaml
number: 8840
title: "Mark some `deprecated-import` fixes as unsafe"
type: pull_request
state: closed
author: tjkuson
labels: []
assignees: []
base: main
head: split-safety
created_at: 2023-11-26T12:53:57Z
updated_at: 2024-04-07T00:46:08Z
url: https://github.com/astral-sh/ruff/pull/8840
synced_at: 2026-01-12T15:55:27Z
```

# Mark some `deprecated-import` fixes as unsafe

---

_@tjkuson_

## Summary

Checked each import to see if its replacement is the same as the original. If the objects were different, marked the fix as unsafe.

Closes #7436.

## Test Plan

`cargo test`


---

_Marked ready for review by @tjkuson on 2023-11-26 13:07_

---

_Comment by @github-actions[bot] on 2023-11-26 13:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -362 fixes in 41 projects)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +0 -118 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/release/action.py#L13'>release/action.py:13:1:</a> UP035 Import from `collections.abc` instead: `Sequence`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/release/action.py#L13'>release/action.py:13:1:</a> UP035 [*] Import from `collections.abc` instead: `Sequence`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/release/pipeline.py#L13'>release/pipeline.py:13:1:</a> UP035 Import from `collections.abc` instead: `Sequence`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/release/pipeline.py#L13'>release/pipeline.py:13:1:</a> UP035 [*] Import from `collections.abc` instead: `Sequence`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/release/ui.py#L14'>release/ui.py:14:1:</a> UP035 Import from `collections.abc` instead: `Sequence`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/release/ui.py#L14'>release/ui.py:14:1:</a> UP035 [*] Import from `collections.abc` instead: `Sequence`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/application/application.py#L33'>src/bokeh/application/application.py:33:1:</a> UP035 Import from `collections.abc` instead: `Awaitable`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/application/application.py#L33'>src/bokeh/application/application.py:33:1:</a> UP035 [*] Import from `collections.abc` instead: `Awaitable`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/application/handlers/directory.py#L59'>src/bokeh/application/handlers/directory.py:59:1:</a> UP035 Import from `collections.abc` instead: `Coroutine`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/application/handlers/directory.py#L59'>src/bokeh/application/handlers/directory.py:59:1:</a> UP035 [*] Import from `collections.abc` instead: `Coroutine`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/client/websocket.py#L25'>src/bokeh/client/websocket.py:25:1:</a> UP035 Import from `collections.abc` instead: `Awaitable`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/client/websocket.py#L25'>src/bokeh/client/websocket.py:25:1:</a> UP035 [*] Import from `collections.abc` instead: `Awaitable`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/colors/util.py#L24'>src/bokeh/colors/util.py:24:1:</a> UP035 Import from `collections.abc` instead: `Iterator`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/colors/util.py#L24'>src/bokeh/colors/util.py:24:1:</a> UP035 [*] Import from `collections.abc` instead: `Iterator`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/command/bootstrap.py#L48'>src/bokeh/command/bootstrap.py:48:1:</a> UP035 Import from `collections.abc` instead: `Sequence`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/command/bootstrap.py#L48'>src/bokeh/command/bootstrap.py:48:1:</a> UP035 [*] Import from `collections.abc` instead: `Sequence`
... 102 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+0 -0 violations, +0 -244 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AMP/Integrations/AMPv2/AMPv2.py#L8'>Packs/AMP/Integrations/AMPv2/AMPv2.py:8:1:</a> UP035 Import from `collections.abc` instead: `Callable`, `MutableMapping`, `MutableSequence`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AMP/Integrations/AMPv2/AMPv2.py#L8'>Packs/AMP/Integrations/AMPv2/AMPv2.py:8:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`, `MutableMapping`, `MutableSequence`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AWS-Lambda/Integrations/AWS_Lambda/AWS_Lambda_test.py#L2'>Packs/AWS-Lambda/Integrations/AWS_Lambda/AWS_Lambda_test.py:2:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AWS-Lambda/Integrations/AWS_Lambda/AWS_Lambda_test.py#L2'>Packs/AWS-Lambda/Integrations/AWS_Lambda/AWS_Lambda_test.py:2:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AWS-SecurityHub/Integrations/AWSSecurityHubEventCollector/AWSSecurityHubEventCollector.py#L4'>Packs/AWS-SecurityHub/Integrations/AWSSecurityHubEventCollector/AWSSecurityHubEventCollector.py:4:1:</a> UP035 Import from `collections.abc` instead: `Iterator`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AWS-SecurityHub/Integrations/AWSSecurityHubEventCollector/AWSSecurityHubEventCollector.py#L4'>Packs/AWS-SecurityHub/Integrations/AWSSecurityHubEventCollector/AWSSecurityHubEventCollector.py:4:1:</a> UP035 [*] Import from `collections.abc` instead: `Iterator`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AWS_WAF/Integrations/AWSWAF/AWSWAF.py#L5'>Packs/AWS_WAF/Integrations/AWSWAF/AWSWAF.py:5:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AWS_WAF/Integrations/AWSWAF/AWSWAF.py#L5'>Packs/AWS_WAF/Integrations/AWSWAF/AWSWAF.py:5:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/Akamai_SIEM/Integrations/Akamai_SIEM/Akamai_SIEM.py#L9'>Packs/Akamai_SIEM/Integrations/Akamai_SIEM/Akamai_SIEM.py:9:1:</a> UP035 Import from `collections.abc` instead: `Sequence`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/Akamai_SIEM/Integrations/Akamai_SIEM/Akamai_SIEM.py#L9'>Packs/Akamai_SIEM/Integrations/Akamai_SIEM/Akamai_SIEM.py:9:1:</a> UP035 [*] Import from `collections.abc` instead: `Sequence`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/Arkime/Integrations/Arkime/Arkime.py#L6'>Packs/Arkime/Integrations/Arkime/Arkime.py:6:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/Arkime/Integrations/Arkime/Arkime.py#L6'>Packs/Arkime/Integrations/Arkime/Arkime.py:6:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/Arkime/Integrations/Arkime/Arkime_test.py#L3'>Packs/Arkime/Integrations/Arkime/Arkime_test.py:3:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/Arkime/Integrations/Arkime/Arkime_test.py#L3'>Packs/Arkime/Integrations/Arkime/Arkime_test.py:3:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AtlassianConfluenceCloud/Integrations/AtlassianConfluenceCloud/AtlassianConfluenceCloud.py#L5'>Packs/AtlassianConfluenceCloud/Integrations/AtlassianConfluenceCloud/AtlassianConfluenceCloud.py:5:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AtlassianConfluenceCloud/Integrations/AtlassianConfluenceCloud/AtlassianConfluenceCloud.py#L5'>Packs/AtlassianConfluenceCloud/Integrations/AtlassianConfluenceCloud/AtlassianConfluenceCloud.py:5:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AutoFocus/Integrations/AutofocusV2/AutofocusV2.py#L11'>Packs/AutoFocus/Integrations/AutofocusV2/AutofocusV2.py:11:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AutoFocus/Integrations/AutofocusV2/AutofocusV2.py#L11'>Packs/AutoFocus/Integrations/AutofocusV2/AutofocusV2.py:11:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AzureStorageContainer/Integrations/AzureStorageContainer/AzureStorageContainer.py#L6'>Packs/AzureStorageContainer/Integrations/AzureStorageContainer/AzureStorageContainer.py:6:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AzureStorageContainer/Integrations/AzureStorageContainer/AzureStorageContainer.py#L6'>Packs/AzureStorageContainer/Integrations/AzureStorageContainer/AzureStorageContainer.py:6:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AzureStorageFileShare/Integrations/AzureStorageFileShare/AzureStorageFileShare.py#L6'>Packs/AzureStorageFileShare/Integrations/AzureStorageFileShare/AzureStorageFileShare.py:6:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AzureStorageFileShare/Integrations/AzureStorageFileShare/AzureStorageFileShare.py#L6'>Packs/AzureStorageFileShare/Integrations/AzureStorageFileShare/AzureStorageFileShare.py:6:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AzureStorageQueue/Integrations/AzureStorageQueue/AzureStorageQueue.py#L3'>Packs/AzureStorageQueue/Integrations/AzureStorageQueue/AzureStorageQueue.py:3:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AzureStorageQueue/Integrations/AzureStorageQueue/AzureStorageQueue.py#L3'>Packs/AzureStorageQueue/Integrations/AzureStorageQueue/AzureStorageQueue.py:3:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L5'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:5:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L5'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:5:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/BmcITSM/Integrations/BmcITSM/BmcITSM.py#L6'>Packs/BmcITSM/Integrations/BmcITSM/BmcITSM.py:6:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/BmcITSM/Integrations/BmcITSM/BmcITSM.py#L6'>Packs/BmcITSM/Integrations/BmcITSM/BmcITSM.py:6:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/CTIX/Integrations/CTIXv3/CTIXv3.py#L7'>Packs/CTIX/Integrations/CTIXv3/CTIXv3.py:7:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/CTIX/Integrations/CTIXv3/CTIXv3.py#L7'>Packs/CTIX/Integrations/CTIXv3/CTIXv3.py:7:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/Carbon_Black_Enterprise_Response/Integrations/CarbonBlackResponseV2/CarbonBlackResponseV2.py#L7'>Packs/Carbon_Black_Enterprise_Response/Integrations/CarbonBlackResponseV2/CarbonBlackResponseV2.py:7:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/Carbon_Black_Enterprise_Response/Integrations/CarbonBlackResponseV2/CarbonBlackResponseV2.py#L7'>Packs/Carbon_Black_Enterprise_Response/Integrations/CarbonBlackResponseV2/CarbonBlackResponseV2.py:7:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/CircleCI/Integrations/CircleCI/CircleCI.py#L1'>Packs/CircleCI/Integrations/CircleCI/CircleCI.py:1:1:</a> UP035 Import from `collections.abc` instead: `Callable`
... 211 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP035 | 362 | 0 | 0 | 0 | 362 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -356 fixes in 41 projects)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +0 -112 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/release/action.py#L13'>release/action.py:13:1:</a> UP035 Import from `collections.abc` instead: `Sequence`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/release/action.py#L13'>release/action.py:13:1:</a> UP035 [*] Import from `collections.abc` instead: `Sequence`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/release/pipeline.py#L13'>release/pipeline.py:13:1:</a> UP035 Import from `collections.abc` instead: `Sequence`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/release/pipeline.py#L13'>release/pipeline.py:13:1:</a> UP035 [*] Import from `collections.abc` instead: `Sequence`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/release/ui.py#L14'>release/ui.py:14:1:</a> UP035 Import from `collections.abc` instead: `Sequence`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/release/ui.py#L14'>release/ui.py:14:1:</a> UP035 [*] Import from `collections.abc` instead: `Sequence`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/application/handlers/directory.py#L59'>src/bokeh/application/handlers/directory.py:59:1:</a> UP035 Import from `collections.abc` instead: `Coroutine`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/application/handlers/directory.py#L59'>src/bokeh/application/handlers/directory.py:59:1:</a> UP035 [*] Import from `collections.abc` instead: `Coroutine`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/client/websocket.py#L25'>src/bokeh/client/websocket.py:25:1:</a> UP035 Import from `collections.abc` instead: `Awaitable`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/client/websocket.py#L25'>src/bokeh/client/websocket.py:25:1:</a> UP035 [*] Import from `collections.abc` instead: `Awaitable`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/colors/util.py#L24'>src/bokeh/colors/util.py:24:1:</a> UP035 Import from `collections.abc` instead: `Iterator`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/colors/util.py#L24'>src/bokeh/colors/util.py:24:1:</a> UP035 [*] Import from `collections.abc` instead: `Iterator`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/command/bootstrap.py#L48'>src/bokeh/command/bootstrap.py:48:1:</a> UP035 Import from `collections.abc` instead: `Sequence`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/command/bootstrap.py#L48'>src/bokeh/command/bootstrap.py:48:1:</a> UP035 [*] Import from `collections.abc` instead: `Sequence`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/command/subcommand.py#L27'>src/bokeh/command/subcommand.py:27:1:</a> UP035 Import from `collections.abc` instead: `Sequence`
... 97 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+0 -0 violations, +0 -244 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AMP/Integrations/AMPv2/AMPv2.py#L8'>Packs/AMP/Integrations/AMPv2/AMPv2.py:8:1:</a> UP035 Import from `collections.abc` instead: `Callable`, `MutableMapping`, `MutableSequence`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AMP/Integrations/AMPv2/AMPv2.py#L8'>Packs/AMP/Integrations/AMPv2/AMPv2.py:8:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`, `MutableMapping`, `MutableSequence`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AWS-Lambda/Integrations/AWS_Lambda/AWS_Lambda_test.py#L2'>Packs/AWS-Lambda/Integrations/AWS_Lambda/AWS_Lambda_test.py:2:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AWS-Lambda/Integrations/AWS_Lambda/AWS_Lambda_test.py#L2'>Packs/AWS-Lambda/Integrations/AWS_Lambda/AWS_Lambda_test.py:2:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AWS-SecurityHub/Integrations/AWSSecurityHubEventCollector/AWSSecurityHubEventCollector.py#L4'>Packs/AWS-SecurityHub/Integrations/AWSSecurityHubEventCollector/AWSSecurityHubEventCollector.py:4:1:</a> UP035 Import from `collections.abc` instead: `Iterator`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AWS-SecurityHub/Integrations/AWSSecurityHubEventCollector/AWSSecurityHubEventCollector.py#L4'>Packs/AWS-SecurityHub/Integrations/AWSSecurityHubEventCollector/AWSSecurityHubEventCollector.py:4:1:</a> UP035 [*] Import from `collections.abc` instead: `Iterator`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AWS_WAF/Integrations/AWSWAF/AWSWAF.py#L5'>Packs/AWS_WAF/Integrations/AWSWAF/AWSWAF.py:5:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AWS_WAF/Integrations/AWSWAF/AWSWAF.py#L5'>Packs/AWS_WAF/Integrations/AWSWAF/AWSWAF.py:5:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/Akamai_SIEM/Integrations/Akamai_SIEM/Akamai_SIEM.py#L9'>Packs/Akamai_SIEM/Integrations/Akamai_SIEM/Akamai_SIEM.py:9:1:</a> UP035 Import from `collections.abc` instead: `Sequence`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/Akamai_SIEM/Integrations/Akamai_SIEM/Akamai_SIEM.py#L9'>Packs/Akamai_SIEM/Integrations/Akamai_SIEM/Akamai_SIEM.py:9:1:</a> UP035 [*] Import from `collections.abc` instead: `Sequence`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/Arkime/Integrations/Arkime/Arkime.py#L6'>Packs/Arkime/Integrations/Arkime/Arkime.py:6:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/Arkime/Integrations/Arkime/Arkime.py#L6'>Packs/Arkime/Integrations/Arkime/Arkime.py:6:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/Arkime/Integrations/Arkime/Arkime_test.py#L3'>Packs/Arkime/Integrations/Arkime/Arkime_test.py:3:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/Arkime/Integrations/Arkime/Arkime_test.py#L3'>Packs/Arkime/Integrations/Arkime/Arkime_test.py:3:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AtlassianConfluenceCloud/Integrations/AtlassianConfluenceCloud/AtlassianConfluenceCloud.py#L5'>Packs/AtlassianConfluenceCloud/Integrations/AtlassianConfluenceCloud/AtlassianConfluenceCloud.py:5:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AtlassianConfluenceCloud/Integrations/AtlassianConfluenceCloud/AtlassianConfluenceCloud.py#L5'>Packs/AtlassianConfluenceCloud/Integrations/AtlassianConfluenceCloud/AtlassianConfluenceCloud.py:5:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AutoFocus/Integrations/AutofocusV2/AutofocusV2.py#L11'>Packs/AutoFocus/Integrations/AutofocusV2/AutofocusV2.py:11:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AutoFocus/Integrations/AutofocusV2/AutofocusV2.py#L11'>Packs/AutoFocus/Integrations/AutofocusV2/AutofocusV2.py:11:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AzureStorageContainer/Integrations/AzureStorageContainer/AzureStorageContainer.py#L6'>Packs/AzureStorageContainer/Integrations/AzureStorageContainer/AzureStorageContainer.py:6:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AzureStorageContainer/Integrations/AzureStorageContainer/AzureStorageContainer.py#L6'>Packs/AzureStorageContainer/Integrations/AzureStorageContainer/AzureStorageContainer.py:6:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AzureStorageFileShare/Integrations/AzureStorageFileShare/AzureStorageFileShare.py#L6'>Packs/AzureStorageFileShare/Integrations/AzureStorageFileShare/AzureStorageFileShare.py:6:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AzureStorageFileShare/Integrations/AzureStorageFileShare/AzureStorageFileShare.py#L6'>Packs/AzureStorageFileShare/Integrations/AzureStorageFileShare/AzureStorageFileShare.py:6:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AzureStorageQueue/Integrations/AzureStorageQueue/AzureStorageQueue.py#L3'>Packs/AzureStorageQueue/Integrations/AzureStorageQueue/AzureStorageQueue.py:3:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/AzureStorageQueue/Integrations/AzureStorageQueue/AzureStorageQueue.py#L3'>Packs/AzureStorageQueue/Integrations/AzureStorageQueue/AzureStorageQueue.py:3:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L5'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:5:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L5'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:5:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/BmcITSM/Integrations/BmcITSM/BmcITSM.py#L6'>Packs/BmcITSM/Integrations/BmcITSM/BmcITSM.py:6:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/BmcITSM/Integrations/BmcITSM/BmcITSM.py#L6'>Packs/BmcITSM/Integrations/BmcITSM/BmcITSM.py:6:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/CTIX/Integrations/CTIXv3/CTIXv3.py#L7'>Packs/CTIX/Integrations/CTIXv3/CTIXv3.py:7:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/CTIX/Integrations/CTIXv3/CTIXv3.py#L7'>Packs/CTIX/Integrations/CTIXv3/CTIXv3.py:7:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/Carbon_Black_Enterprise_Response/Integrations/CarbonBlackResponseV2/CarbonBlackResponseV2.py#L7'>Packs/Carbon_Black_Enterprise_Response/Integrations/CarbonBlackResponseV2/CarbonBlackResponseV2.py:7:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/Carbon_Black_Enterprise_Response/Integrations/CarbonBlackResponseV2/CarbonBlackResponseV2.py#L7'>Packs/Carbon_Black_Enterprise_Response/Integrations/CarbonBlackResponseV2/CarbonBlackResponseV2.py:7:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/CircleCI/Integrations/CircleCI/CircleCI.py#L1'>Packs/CircleCI/Integrations/CircleCI/CircleCI.py:1:1:</a> UP035 Import from `collections.abc` instead: `Callable`
- <a href='https://github.com/demisto/content/blob/84a7e5e7507c0363e530265ebf37e2be056ec73a/Packs/CircleCI/Integrations/CircleCI/CircleCI.py#L1'>Packs/CircleCI/Integrations/CircleCI/CircleCI.py:1:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
... 210 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP035 | 356 | 0 | 0 | 0 | 356 |

</p>
</details>




---

_Renamed from "Mark some `deprecated-import` fixes as unsage" to "Mark some `deprecated-import` fixes as unsafe" by @tjkuson on 2023-11-26 15:18_

---

_@zanieb approved on 2023-11-27 16:13_

LGTM without actually checking each type

@charliermarsh want to review as well?

---

_Comment by @charliermarsh on 2023-11-28 05:52_

@tjkuson - What was the methodology for determining whether the imports were the same, or changed?

---

_Comment by @tjkuson on 2023-11-28 09:59_

I compared object identity with an assertion:

```python
from old import foo as old_foo
from new import foo as new_foo

assert old_foo is new_foo
```

---

_Comment by @charliermarsh on 2023-12-01 00:49_

It may be challenging but... I feel like these could still be considered safe if the usages are exclusively in typing contexts / type annotations. What do you think?

---

_Comment by @tjkuson on 2023-12-01 13:54_

> It may be challenging but... I feel like these could still be considered safe if the usages are exclusively in typing contexts / type annotations. What do you think?

As in making Ruff determine if an import is being used for typing only? There's already logic that checks if an import only used for typing, right? If so, it shouldn't be too difficult, unless I am missing something…

---

_Comment by @zanieb on 2024-03-12 18:41_

@AlexWaygood are you interested in taking over review of this?

---

_Comment by @AlexWaygood on 2024-03-12 18:44_

> @AlexWaygood are you interested in taking over review of this?

Sure, I'll take a look tomorrow

---

_Review requested from @AlexWaygood by @AlexWaygood on 2024-03-12 18:44_

---

_Assigned to @AlexWaygood by @zanieb on 2024-03-12 18:52_

---

_Comment by @charliermarsh on 2024-04-07 00:46_

I think I'm going to close this. I appreciate the work (thank you as always) and it's a reasonable proposal but ultimately I think it adds more overhead to the maintenance of the rule and to users than is worthwhile. If we see a real issue in practice from marking these as safe, I will of course revisit.


---

_Closed by @charliermarsh on 2024-04-07 00:46_

---
