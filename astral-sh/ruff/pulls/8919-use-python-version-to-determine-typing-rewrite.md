```yaml
number: 8919
title: Use Python version to determine typing rewrite safety
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/up-safe
created_at: 2023-11-30T01:51:07Z
updated_at: 2023-11-30T03:22:05Z
url: https://github.com/astral-sh/ruff/pull/8919
synced_at: 2026-01-10T23:40:55Z
```

# Use Python version to determine typing rewrite safety

---

_Pull request opened by @charliermarsh on 2023-11-30 01:51_

## Summary

These rewrites are only (potentially) unsafe on Python versions that predate their introduction into the standard library and grammar, so it seems correct to mark them as safe on those later versions.


---

_Label `preview` added by @charliermarsh on 2023-11-30 01:51_

---

_Review requested from @zanieb by @charliermarsh on 2023-11-30 01:52_

---

_Comment by @github-actions[bot] on 2023-11-30 02:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +2694 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+0 -0 violations, +2692 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/AMP/Integrations/AMPv2/AMPv2.py#L3240'>Packs/AMP/Integrations/AMPv2/AMPv2.py:3240:11:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/AMP/Integrations/AMPv2/AMPv2.py#L3240'>Packs/AMP/Integrations/AMPv2/AMPv2.py:3240:11:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/AMP/Integrations/AMPv2/AMPv2.py#L3241'>Packs/AMP/Integrations/AMPv2/AMPv2.py:3241:16:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/AMP/Integrations/AMPv2/AMPv2.py#L3241'>Packs/AMP/Integrations/AMPv2/AMPv2.py:3241:16:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/AMP/Integrations/AMPv2/AMPv2.py#L3242'>Packs/AMP/Integrations/AMPv2/AMPv2.py:3242:12:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/AMP/Integrations/AMPv2/AMPv2.py#L3242'>Packs/AMP/Integrations/AMPv2/AMPv2.py:3242:12:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L756'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:756:24:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L756'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:756:24:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L34'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:34:105:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L34'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:34:105:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L35'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:35:48:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L35'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:35:48:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/AccentureCTI_Feed/Integrations/ACTIIndicatorFeed/ACTIIndicatorFeed.py#L122'>Packs/AccentureCTI_Feed/Integrations/ACTIIndicatorFeed/ACTIIndicatorFeed.py:122:60:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/AccentureCTI_Feed/Integrations/ACTIIndicatorFeed/ACTIIndicatorFeed.py#L122'>Packs/AccentureCTI_Feed/Integrations/ACTIIndicatorFeed/ACTIIndicatorFeed.py:122:60:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/AccentureCTI_Feed/Integrations/ACTIIndicatorFeed/ACTIIndicatorFeed.py#L134'>Packs/AccentureCTI_Feed/Integrations/ACTIIndicatorFeed/ACTIIndicatorFeed.py:134:33:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/AccentureCTI_Feed/Integrations/ACTIIndicatorFeed/ACTIIndicatorFeed.py#L134'>Packs/AccentureCTI_Feed/Integrations/ACTIIndicatorFeed/ACTIIndicatorFeed.py:134:33:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/AgariPhishingDefense/Integrations/AgariPhishingDefense/AgariPhishingDefense.py#L586'>Packs/AgariPhishingDefense/Integrations/AgariPhishingDefense/AgariPhishingDefense.py:586:6:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/AgariPhishingDefense/Integrations/AgariPhishingDefense/AgariPhishingDefense.py#L586'>Packs/AgariPhishingDefense/Integrations/AgariPhishingDefense/AgariPhishingDefense.py:586:6:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/AgariPhishingDefense/Integrations/AgariPhishingDefense/AgariPhishingDefense.py#L622'>Packs/AgariPhishingDefense/Integrations/AgariPhishingDefense/AgariPhishingDefense.py:622:6:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/AgariPhishingDefense/Integrations/AgariPhishingDefense/AgariPhishingDefense.py#L622'>Packs/AgariPhishingDefense/Integrations/AgariPhishingDefense/AgariPhishingDefense.py:622:6:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/Akamai_SIEM/Integrations/Akamai_SIEM/Akamai_SIEM.py#L111'>Packs/Akamai_SIEM/Integrations/Akamai_SIEM/Akamai_SIEM.py:111:21:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/Akamai_SIEM/Integrations/Akamai_SIEM/Akamai_SIEM.py#L111'>Packs/Akamai_SIEM/Integrations/Akamai_SIEM/Akamai_SIEM.py:111:21:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/Akamai_SIEM/Integrations/Akamai_SIEM/Akamai_SIEM.py#L121'>Packs/Akamai_SIEM/Integrations/Akamai_SIEM/Akamai_SIEM.py:121:42:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/Akamai_SIEM/Integrations/Akamai_SIEM/Akamai_SIEM.py#L121'>Packs/Akamai_SIEM/Integrations/Akamai_SIEM/Akamai_SIEM.py:121:42:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/Akamai_SIEM/Integrations/Akamai_SIEM/Akamai_SIEM.py#L256'>Packs/Akamai_SIEM/Integrations/Akamai_SIEM/Akamai_SIEM.py:256:22:</a> UP007 Use `X | Y` for type annotations
... 2666 additional changes omitted for rule UP007
- <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/GoogleDocs/Integrations/GoogleDocs/GoogleDocs.py#L315'>Packs/GoogleDocs/Integrations/GoogleDocs/GoogleDocs.py:315:20:</a> UP006 Use `collections.defaultdict` instead of `typing.DefaultDict` for type annotation
+ <a href='https://github.com/demisto/content/blob/a523dba37ff5d2db3ad444c8bdbd5be06e6fbffc/Packs/GoogleDocs/Integrations/GoogleDocs/GoogleDocs.py#L315'>Packs/GoogleDocs/Integrations/GoogleDocs/GoogleDocs.py:315:20:</a> UP006 [*] Use `collections.defaultdict` instead of `typing.DefaultDict` for type annotation
... 2665 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/119397e52cc4ea5210fd50093c7e5098fdff16f9/packaging/docker/entrypoint.py#L54'>packaging/docker/entrypoint.py:54:32:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/rotki/rotki/blob/119397e52cc4ea5210fd50093c7e5098fdff16f9/packaging/docker/entrypoint.py#L54'>packaging/docker/entrypoint.py:54:32:</a> UP007 [*] Use `X | Y` for type annotations
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP007 | 2692 | 0 | 0 | 2692 | 0 |
| UP006 | 2 | 0 | 0 | 2 | 0 |

</p>
</details>




---

_@zanieb approved on 2023-11-30 02:35_

---

_Merged by @charliermarsh on 2023-11-30 03:22_

---

_Closed by @charliermarsh on 2023-11-30 03:22_

---

_Branch deleted on 2023-11-30 03:22_

---
