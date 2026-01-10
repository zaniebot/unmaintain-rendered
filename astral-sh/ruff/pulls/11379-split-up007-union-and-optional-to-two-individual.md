```yaml
number: 11379
title: Split UP007 Union and Optional to two individual rules
type: pull_request
state: closed
author: blueraft
labels: []
assignees: []
base: main
head: split-up007
created_at: 2024-05-12T11:43:52Z
updated_at: 2025-01-07T14:51:45Z
url: https://github.com/astral-sh/ruff/pull/11379
synced_at: 2026-01-10T20:34:00Z
```

# Split UP007 Union and Optional to two individual rules

---

_Pull request opened by @blueraft on 2024-05-12 11:43_

Resolves https://github.com/astral-sh/ruff/issues/4858

## Summary

- [x]  Drop Optional support from UP007 in preview mode

- [x] Add new rule in preview for use of Optional

- [ ] Add documentation about the transition to both rules -> I have added some documentation regarding this to the `rustdoc`, @zanieb do you want me to add this somewhere else too? 

## Test Plan

- `cargo test`
-  The updated docs render correctly.


---

_Renamed from "Split UP007 from Union and Optional to two individual rules" to "Split UP007 Union and Optional to two individual rules" by @blueraft on 2024-05-12 11:44_

---

_Converted to draft by @blueraft on 2024-05-12 11:45_

---

_Marked ready for review by @blueraft on 2024-05-12 11:46_

---

_Comment by @github-actions[bot] on 2024-05-12 11:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+4 -1183 violations, +0 -0 fixes in 5 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/76c5e723ea45304b3b9bdbe26fba8bf96f260c20/src/plasmapy/particles/particle_class.py#L2516'>src/plasmapy/particles/particle_class.py:2516:42:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `UP007`)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/76c5e723ea45304b3b9bdbe26fba8bf96f260c20/src/plasmapy/particles/particle_collections.py#L616'>src/plasmapy/particles/particle_collections.py:616:76:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `UP007`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/f4cdb6bcb0608f5928a57cbd9954b8ab8688f7c7/airflow/utils/log/logging_mixin.py#L75'>airflow/utils/log/logging_mixin.py:75:52:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `UP007`)
+ <a href='https://github.com/apache/airflow/blob/f4cdb6bcb0608f5928a57cbd9954b8ab8688f7c7/airflow/utils/log/logging_mixin.py#L77'>airflow/utils/log/logging_mixin.py:77:41:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `UP007`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+0 -1169 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AMP/Integrations/AMPv2/AMPv2.py#L3240'>Packs/AMP/Integrations/AMPv2/AMPv2.py:3240:11:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AMP/Integrations/AMPv2/AMPv2.py#L3241'>Packs/AMP/Integrations/AMPv2/AMPv2.py:3241:16:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AMP/Integrations/AMPv2/AMPv2.py#L3242'>Packs/AMP/Integrations/AMPv2/AMPv2.py:3242:12:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L756'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:756:24:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L34'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:34:105:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L35'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:35:48:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AccentureCTI_Feed/Integrations/ACTIIndicatorFeed/ACTIIndicatorFeed.py#L122'>Packs/AccentureCTI_Feed/Integrations/ACTIIndicatorFeed/ACTIIndicatorFeed.py:122:60:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AccentureCTI_Feed/Integrations/ACTIIndicatorFeed/ACTIIndicatorFeed.py#L134'>Packs/AccentureCTI_Feed/Integrations/ACTIIndicatorFeed/ACTIIndicatorFeed.py:134:33:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AgariPhishingDefense/Integrations/AgariPhishingDefense/AgariPhishingDefense.py#L586'>Packs/AgariPhishingDefense/Integrations/AgariPhishingDefense/AgariPhishingDefense.py:586:6:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AgariPhishingDefense/Integrations/AgariPhishingDefense/AgariPhishingDefense.py#L622'>Packs/AgariPhishingDefense/Integrations/AgariPhishingDefense/AgariPhishingDefense.py:622:6:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AlienVault_OTX/Integrations/AlienVault_OTX_v2/AlienVault_OTX_v2.py#L92'>Packs/AlienVault_OTX/Integrations/AlienVault_OTX_v2/AlienVault_OTX_v2.py:92:56:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/ApiModules/Scripts/NGINXApiModule/NGINXApiModule_test.py#L90'>Packs/ApiModules/Scripts/NGINXApiModule/NGINXApiModule_test.py:90:16:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AppNovi/Integrations/appNovi/appNovi.py#L49'>Packs/AppNovi/Integrations/appNovi/appNovi.py:49:26:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/AutoFocusTagsFeed/AutoFocusTagsFeed.py#L255'>Packs/AutoFocus/Integrations/AutoFocusTagsFeed/AutoFocusTagsFeed.py:255:30:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/AutoFocusTagsFeed/AutoFocusTagsFeed.py#L255'>Packs/AutoFocus/Integrations/AutoFocusTagsFeed/AutoFocusTagsFeed.py:255:53:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/AutoFocusTagsFeed/AutoFocusTagsFeed.py#L255'>Packs/AutoFocus/Integrations/AutoFocusTagsFeed/AutoFocusTagsFeed.py:255:71:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/AutoFocusTagsFeed/AutoFocusTagsFeed.py#L436'>Packs/AutoFocus/Integrations/AutoFocusTagsFeed/AutoFocusTagsFeed.py:436:33:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/FeedAutofocus/FeedAutofocus.py#L380'>Packs/AutoFocus/Integrations/FeedAutofocus/FeedAutofocus.py:380:58:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/FeedAutofocus/FeedAutofocus.py#L407'>Packs/AutoFocus/Integrations/FeedAutofocus/FeedAutofocus.py:407:81:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/FeedAutofocus/FeedAutofocus.py#L487'>Packs/AutoFocus/Integrations/FeedAutofocus/FeedAutofocus.py:487:74:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py#L105'>Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py:105:58:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py#L120'>Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py:120:81:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py#L141'>Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py:141:84:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py#L179'>Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py:179:74:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py#L66'>Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py:66:91:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Base/Scripts/ValidateContent/ValidateContent.py#L131'>Packs/Base/Scripts/ValidateContent/ValidateContent.py:131:52:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Base/Scripts/ValidateContent/ValidateContent.py#L166'>Packs/Base/Scripts/ValidateContent/ValidateContent.py:166:40:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Base/Scripts/ValidateContent/ValidateContent.py#L390'>Packs/Base/Scripts/ValidateContent/ValidateContent.py:390:19:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Base/Scripts/ValidateContent/ValidateContent.py#L391'>Packs/Base/Scripts/ValidateContent/ValidateContent.py:391:15:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Base/Scripts/ValidateContent/ValidateContent.py#L392'>Packs/Base/Scripts/ValidateContent/ValidateContent.py:392:19:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Base/Scripts/ValidateContent/ValidateContent.py#L61'>Packs/Base/Scripts/ValidateContent/ValidateContent.py:61:80:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Binalyze/Integrations/BinalyzeAIR/BinalyzeAIR.py#L18'>Packs/Binalyze/Integrations/BinalyzeAIR/BinalyzeAIR.py:18:61:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Binalyze/Integrations/BinalyzeAIR/BinalyzeAIR.py#L49'>Packs/Binalyze/Integrations/BinalyzeAIR/BinalyzeAIR.py:49:87:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Binalyze/Integrations/BinalyzeAIR/BinalyzeAIR.py#L84'>Packs/Binalyze/Integrations/BinalyzeAIR/BinalyzeAIR.py:84:59:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L1109'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:1109:75:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L1217'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:1217:47:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L1444'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:1444:88:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L2066'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:2066:77:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L2108'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:2108:96:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L2165'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:2165:86:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L2214'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:2214:84:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L2262'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:2262:83:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L2310'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:2310:12:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L2310'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:2310:21:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L2359'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:2359:85:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L2439'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:2439:85:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L2487'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:2487:12:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L2487'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:2487:21:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L2542'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:2542:83:</a> UP007 [*] Use `X | Y` for type annotations
... 1120 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -13 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L180'>latch/registry/record.py:180:57:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L182'>latch/registry/record.py:182:64:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L207'>latch/registry/record.py:207:53:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L209'>latch/registry/record.py:209:60:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L242'>latch/registry/record.py:242:10:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L248'>latch/registry/record.py:248:10:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L278'>latch/registry/record.py:278:10:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L284'>latch/registry/record.py:284:10:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L303'>latch/registry/record.py:303:41:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L74'>latch/registry/record.py:74:15:</a> UP007 Use `X | Y` for type annotations
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/7000c024f592c29ebcaabfd7c2d4d244c127b49a/packaging/docker/entrypoint.py#L54'>packaging/docker/entrypoint.py:54:32:</a> UP007 [*] Use `X | Y` for type annotations
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP007 | 1183 | 0 | 1183 | 0 | 0 |
| RUF100 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -882 violations, +0 -0 fixes in 4 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/f4cdb6bcb0608f5928a57cbd9954b8ab8688f7c7/airflow/utils/log/logging_mixin.py#L1'>airflow/utils/log/logging_mixin.py:1:1:</a> D100 Missing docstring in public module
- <a href='https://github.com/apache/airflow/blob/f4cdb6bcb0608f5928a57cbd9954b8ab8688f7c7/airflow/utils/log/logging_mixin.py#L1'>airflow/utils/log/logging_mixin.py:1:1:</a> D100 Missing docstring in public module
+ <a href='https://github.com/apache/airflow/blob/f4cdb6bcb0608f5928a57cbd9954b8ab8688f7c7/airflow/utils/log/logging_mixin.py#L209'>airflow/utils/log/logging_mixin.py:209:9:</a> ANN201 Missing return type annotation for public function `isatty`
- <a href='https://github.com/apache/airflow/blob/f4cdb6bcb0608f5928a57cbd9954b8ab8688f7c7/airflow/utils/log/logging_mixin.py#L209'>airflow/utils/log/logging_mixin.py:209:9:</a> ANN201 Missing return type annotation for public function `isatty`
+ <a href='https://github.com/apache/airflow/blob/f4cdb6bcb0608f5928a57cbd9954b8ab8688f7c7/airflow/utils/log/logging_mixin.py#L75'>airflow/utils/log/logging_mixin.py:75:52:</a> RUF100 [*] Unused `noqa` directive (unused: `UP007`)
+ <a href='https://github.com/apache/airflow/blob/f4cdb6bcb0608f5928a57cbd9954b8ab8688f7c7/airflow/utils/log/logging_mixin.py#L77'>airflow/utils/log/logging_mixin.py:77:41:</a> RUF100 [*] Unused `noqa` directive (unused: `UP007`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+0 -866 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AMP/Integrations/AMPv2/AMPv2.py#L3240'>Packs/AMP/Integrations/AMPv2/AMPv2.py:3240:11:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AMP/Integrations/AMPv2/AMPv2.py#L3241'>Packs/AMP/Integrations/AMPv2/AMPv2.py:3241:16:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AMP/Integrations/AMPv2/AMPv2.py#L3242'>Packs/AMP/Integrations/AMPv2/AMPv2.py:3242:12:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L756'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:756:24:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L34'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:34:105:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L35'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:35:48:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/ApiModules/Scripts/NGINXApiModule/NGINXApiModule_test.py#L90'>Packs/ApiModules/Scripts/NGINXApiModule/NGINXApiModule_test.py:90:16:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/AutoFocusTagsFeed/AutoFocusTagsFeed.py#L255'>Packs/AutoFocus/Integrations/AutoFocusTagsFeed/AutoFocusTagsFeed.py:255:30:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/AutoFocusTagsFeed/AutoFocusTagsFeed.py#L255'>Packs/AutoFocus/Integrations/AutoFocusTagsFeed/AutoFocusTagsFeed.py:255:53:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/AutoFocusTagsFeed/AutoFocusTagsFeed.py#L255'>Packs/AutoFocus/Integrations/AutoFocusTagsFeed/AutoFocusTagsFeed.py:255:71:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/AutoFocusTagsFeed/AutoFocusTagsFeed.py#L436'>Packs/AutoFocus/Integrations/AutoFocusTagsFeed/AutoFocusTagsFeed.py:436:33:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/FeedAutofocus/FeedAutofocus.py#L380'>Packs/AutoFocus/Integrations/FeedAutofocus/FeedAutofocus.py:380:58:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/FeedAutofocus/FeedAutofocus.py#L407'>Packs/AutoFocus/Integrations/FeedAutofocus/FeedAutofocus.py:407:81:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/FeedAutofocus/FeedAutofocus.py#L487'>Packs/AutoFocus/Integrations/FeedAutofocus/FeedAutofocus.py:487:74:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py#L105'>Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py:105:58:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py#L120'>Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py:120:81:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py#L141'>Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py:141:84:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py#L179'>Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py:179:74:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py#L66'>Packs/AutoFocus/Integrations/FeedAutofocusDaily/FeedAutofocusDaily.py:66:91:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Base/Scripts/ValidateContent/ValidateContent.py#L131'>Packs/Base/Scripts/ValidateContent/ValidateContent.py:131:52:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Base/Scripts/ValidateContent/ValidateContent.py#L166'>Packs/Base/Scripts/ValidateContent/ValidateContent.py:166:40:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Base/Scripts/ValidateContent/ValidateContent.py#L390'>Packs/Base/Scripts/ValidateContent/ValidateContent.py:390:19:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Base/Scripts/ValidateContent/ValidateContent.py#L391'>Packs/Base/Scripts/ValidateContent/ValidateContent.py:391:15:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Base/Scripts/ValidateContent/ValidateContent.py#L392'>Packs/Base/Scripts/ValidateContent/ValidateContent.py:392:19:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Base/Scripts/ValidateContent/ValidateContent.py#L61'>Packs/Base/Scripts/ValidateContent/ValidateContent.py:61:80:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Binalyze/Integrations/BinalyzeAIR/BinalyzeAIR.py#L18'>Packs/Binalyze/Integrations/BinalyzeAIR/BinalyzeAIR.py:18:61:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Binalyze/Integrations/BinalyzeAIR/BinalyzeAIR.py#L49'>Packs/Binalyze/Integrations/BinalyzeAIR/BinalyzeAIR.py:49:87:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Binalyze/Integrations/BinalyzeAIR/BinalyzeAIR.py#L84'>Packs/Binalyze/Integrations/BinalyzeAIR/BinalyzeAIR.py:84:59:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L1109'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:1109:75:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L1217'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:1217:47:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L1444'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:1444:88:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L2066'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:2066:77:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L2108'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:2108:96:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L2310'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:2310:12:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L2487'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:2487:12:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L777'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:777:30:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L777'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:777:62:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L955'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:955:85:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Box/Integrations/BoxV2/BoxV2.py#L1027'>Packs/Box/Integrations/BoxV2/BoxV2.py:1027:74:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Box/Integrations/BoxV2/BoxV2.py#L1094'>Packs/Box/Integrations/BoxV2/BoxV2.py:1094:34:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Box/Integrations/BoxV2/BoxV2.py#L1790'>Packs/Box/Integrations/BoxV2/BoxV2.py:1790:21:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Box/Integrations/BoxV2/BoxV2.py#L1793'>Packs/Box/Integrations/BoxV2/BoxV2.py:1793:16:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Box/Integrations/BoxV2/BoxV2.py#L1800'>Packs/Box/Integrations/BoxV2/BoxV2.py:1800:13:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Box/Integrations/BoxV2/BoxV2.py#L266'>Packs/Box/Integrations/BoxV2/BoxV2.py:266:43:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Box/Integrations/BoxV2/BoxV2.py#L287'>Packs/Box/Integrations/BoxV2/BoxV2.py:287:49:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Box/Integrations/BoxV2/BoxV2.py#L563'>Packs/Box/Integrations/BoxV2/BoxV2.py:563:49:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Box/Integrations/BoxV2/BoxV2.py#L563'>Packs/Box/Integrations/BoxV2/BoxV2.py:563:91:</a> UP007 [*] Use `X | Y` for type annotations
- <a href='https://github.com/demisto/content/blob/95ce3991a86d8b6860a5fb67853089334d01422f/Packs/Box/Integrations/BoxV2/BoxV2.py#L564'>Packs/Box/Integrations/BoxV2/BoxV2.py:564:41:</a> UP007 [*] Use `X | Y` for type annotations
... 818 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -13 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L180'>latch/registry/record.py:180:57:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L182'>latch/registry/record.py:182:64:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L207'>latch/registry/record.py:207:53:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L209'>latch/registry/record.py:209:60:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L242'>latch/registry/record.py:242:10:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L248'>latch/registry/record.py:248:10:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L278'>latch/registry/record.py:278:10:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L284'>latch/registry/record.py:284:10:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L303'>latch/registry/record.py:303:41:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/039d0e20885ea852cde81c775bc1176d62213f35/latch/registry/record.py#L74'>latch/registry/record.py:74:15:</a> UP007 Use `X | Y` for type annotations
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/7000c024f592c29ebcaabfd7c2d4d244c127b49a/packaging/docker/entrypoint.py#L54'>packaging/docker/entrypoint.py:54:32:</a> UP007 [*] Use `X | Y` for type annotations
</pre>

</p>
</details>
<details><summary>Changes by rule (4 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP007 | 880 | 0 | 880 | 0 | 0 |
| D100 | 2 | 1 | 1 | 0 | 0 |
| ANN201 | 2 | 1 | 1 | 0 | 0 |
| RUF100 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @VascoSch92 on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP007B.py`:1 on 2024-05-12 14:59_

Can I suggest a test case of the form `List[Optional[str, int]]` ?

To be sure the rule detect also nested `Optional`. Or it is already the case? :)

---

_@VascoSch92 reviewed on 2024-05-12 14:59_

---

_@blueraft reviewed on 2024-05-12 15:35_

---

_Review comment by @blueraft on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP007B.py`:1 on 2024-05-12 15:35_

Thanks for the suggestion, I've added a test case for nested `Optional`. A  test case for nested `Union` was already there, so I didn't modify that. 

---

_Review requested from @zanieb by @zanieb on 2024-05-14 14:54_

---

_Comment by @plusls on 2024-08-09 04:28_

any progress?

---

_Added to milestone `v0.7` by @MichaReiser on 2024-08-19 10:13_

---

_Removed from milestone `v0.7` by @AlexWaygood on 2024-10-17 15:18_

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-10-17 15:18_

---

_Removed from milestone `v0.8` by @AlexWaygood on 2024-11-18 11:38_

---

_Added to milestone `v0.9` by @AlexWaygood on 2024-11-18 11:38_

---

_Comment by @InSyncWithFoo on 2025-01-01 13:24_

Do you intend to continue the work on this PR soon? If not, I can take it over.

---

_Comment by @blueraft on 2025-01-02 12:54_

@InSyncWithFoo Feel free to take over, thanks!

---

_Comment by @MichaReiser on 2025-01-07 14:51_

Closing, because we merged #15313 

---

_Closed by @MichaReiser on 2025-01-07 14:51_

---
