```yaml
number: 15411
title: "[`pyflakes`] stabilize raising `F822` in `__init__.py` files."
type: pull_request
state: closed
author: hassec
labels:
  - breaking
assignees: []
base: main
head: stabilize_f822_for_init
created_at: 2025-01-10T22:58:55Z
updated_at: 2025-01-17T09:08:03Z
url: https://github.com/astral-sh/ruff/pull/15411
synced_at: 2026-01-10T20:34:00Z
```

# [`pyflakes`] stabilize raising `F822` in `__init__.py` files.

---

_Pull request opened by @hassec on 2025-01-10 22:58_

## Summary

Stabilize raising `F822` in `__init__.py` files. 
This has been in preview since #11370 which was first released in [v0.4.7](https://github.com/astral-sh/ruff/releases/tag/v0.4.7)

## Test Plan

<!-- How was it tested? -->

Since this is now tested as part of the normal `__init__.py` file test, as seen by the change to `ruff_linter__rules__pyflakes__tests__init.snap`, I've removed the preview specific one, which would now be redundant. 


---

_Comment by @github-actions[bot] on 2025-01-10 23:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+411 -0 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PostHog/HouseWatch/blob/77a70fd5e8b18de37f044df434e95b46c4837f22/housewatch/tasks/__init__.py#L5'>housewatch/tasks/__init__.py:5:43:</a> F822 Undefined name `customer_report` in `__all__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/a2d5a2543b7026a1e743ea825cd66b6b1ebc8e93/airflow/datasets/__init__.py#L61'>airflow/datasets/__init__.py:61:12:</a> F822 Undefined name `Dataset` in `__all__`
+ <a href='https://github.com/apache/airflow/blob/a2d5a2543b7026a1e743ea825cd66b6b1ebc8e93/airflow/datasets/__init__.py#L61'>airflow/datasets/__init__.py:61:23:</a> F822 Undefined name `DatasetAlias` in `__all__`
+ <a href='https://github.com/apache/airflow/blob/a2d5a2543b7026a1e743ea825cd66b6b1ebc8e93/airflow/datasets/__init__.py#L61'>airflow/datasets/__init__.py:61:39:</a> F822 Undefined name `DatasetAll` in `__all__`
+ <a href='https://github.com/apache/airflow/blob/a2d5a2543b7026a1e743ea825cd66b6b1ebc8e93/airflow/datasets/__init__.py#L61'>airflow/datasets/__init__.py:61:53:</a> F822 Undefined name `DatasetAny` in `__all__`
+ <a href='https://github.com/apache/airflow/blob/a2d5a2543b7026a1e743ea825cd66b6b1ebc8e93/airflow/datasets/__init__.py#L61'>airflow/datasets/__init__.py:61:67:</a> F822 Undefined name `expand_alias_to_datasets` in `__all__`
+ <a href='https://github.com/apache/airflow/blob/a2d5a2543b7026a1e743ea825cd66b6b1ebc8e93/providers/src/airflow/providers/cncf/kubernetes/utils/__init__.py#L19'>providers/src/airflow/providers/cncf/kubernetes/utils/__init__.py:19:12:</a> F822 Undefined name `xcom_sidecar` in `__all__`
+ <a href='https://github.com/apache/airflow/blob/a2d5a2543b7026a1e743ea825cd66b6b1ebc8e93/providers/src/airflow/providers/cncf/kubernetes/utils/__init__.py#L19'>providers/src/airflow/providers/cncf/kubernetes/utils/__init__.py:19:28:</a> F822 Undefined name `pod_manager` in `__all__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+374 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L1000'>libs/community/langchain_community/llms/__init__.py:1000:5:</a> F822 Undefined name `You` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L899'>libs/community/langchain_community/llms/__init__.py:899:5:</a> F822 Undefined name `AI21` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L900'>libs/community/langchain_community/llms/__init__.py:900:5:</a> F822 Undefined name `AlephAlpha` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L901'>libs/community/langchain_community/llms/__init__.py:901:5:</a> F822 Undefined name `AmazonAPIGateway` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L902'>libs/community/langchain_community/llms/__init__.py:902:5:</a> F822 Undefined name `Anthropic` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L903'>libs/community/langchain_community/llms/__init__.py:903:5:</a> F822 Undefined name `Anyscale` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L904'>libs/community/langchain_community/llms/__init__.py:904:5:</a> F822 Undefined name `Aphrodite` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L905'>libs/community/langchain_community/llms/__init__.py:905:5:</a> F822 Undefined name `Arcee` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L906'>libs/community/langchain_community/llms/__init__.py:906:5:</a> F822 Undefined name `Aviary` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L907'>libs/community/langchain_community/llms/__init__.py:907:5:</a> F822 Undefined name `AzureMLOnlineEndpoint` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L908'>libs/community/langchain_community/llms/__init__.py:908:5:</a> F822 Undefined name `AzureOpenAI` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L909'>libs/community/langchain_community/llms/__init__.py:909:5:</a> F822 Undefined name `BaichuanLLM` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L910'>libs/community/langchain_community/llms/__init__.py:910:5:</a> F822 Undefined name `Banana` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L911'>libs/community/langchain_community/llms/__init__.py:911:5:</a> F822 Undefined name `Baseten` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L912'>libs/community/langchain_community/llms/__init__.py:912:5:</a> F822 Undefined name `Beam` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L913'>libs/community/langchain_community/llms/__init__.py:913:5:</a> F822 Undefined name `Bedrock` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L914'>libs/community/langchain_community/llms/__init__.py:914:5:</a> F822 Undefined name `CTransformers` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L915'>libs/community/langchain_community/llms/__init__.py:915:5:</a> F822 Undefined name `CTranslate2` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L916'>libs/community/langchain_community/llms/__init__.py:916:5:</a> F822 Undefined name `CerebriumAI` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L917'>libs/community/langchain_community/llms/__init__.py:917:5:</a> F822 Undefined name `ChatGLM` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L918'>libs/community/langchain_community/llms/__init__.py:918:5:</a> F822 Undefined name `Clarifai` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L919'>libs/community/langchain_community/llms/__init__.py:919:5:</a> F822 Undefined name `Cohere` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L920'>libs/community/langchain_community/llms/__init__.py:920:5:</a> F822 Undefined name `Databricks` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L921'>libs/community/langchain_community/llms/__init__.py:921:5:</a> F822 Undefined name `DeepInfra` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L922'>libs/community/langchain_community/llms/__init__.py:922:5:</a> F822 Undefined name `DeepSparse` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L923'>libs/community/langchain_community/llms/__init__.py:923:5:</a> F822 Undefined name `EdenAI` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L924'>libs/community/langchain_community/llms/__init__.py:924:5:</a> F822 Undefined name `FakeListLLM` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L925'>libs/community/langchain_community/llms/__init__.py:925:5:</a> F822 Undefined name `Fireworks` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L926'>libs/community/langchain_community/llms/__init__.py:926:5:</a> F822 Undefined name `ForefrontAI` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L927'>libs/community/langchain_community/llms/__init__.py:927:5:</a> F822 Undefined name `Friendli` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L928'>libs/community/langchain_community/llms/__init__.py:928:5:</a> F822 Undefined name `GPT4All` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L929'>libs/community/langchain_community/llms/__init__.py:929:5:</a> F822 Undefined name `GigaChat` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L930'>libs/community/langchain_community/llms/__init__.py:930:5:</a> F822 Undefined name `GooglePalm` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L931'>libs/community/langchain_community/llms/__init__.py:931:5:</a> F822 Undefined name `GooseAI` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L932'>libs/community/langchain_community/llms/__init__.py:932:5:</a> F822 Undefined name `GradientLLM` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L933'>libs/community/langchain_community/llms/__init__.py:933:5:</a> F822 Undefined name `HuggingFaceEndpoint` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L934'>libs/community/langchain_community/llms/__init__.py:934:5:</a> F822 Undefined name `HuggingFaceHub` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L935'>libs/community/langchain_community/llms/__init__.py:935:5:</a> F822 Undefined name `HuggingFacePipeline` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L936'>libs/community/langchain_community/llms/__init__.py:936:5:</a> F822 Undefined name `HuggingFaceTextGenInference` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L937'>libs/community/langchain_community/llms/__init__.py:937:5:</a> F822 Undefined name `HumanInputLLM` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L938'>libs/community/langchain_community/llms/__init__.py:938:5:</a> F822 Undefined name `IpexLLM` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L939'>libs/community/langchain_community/llms/__init__.py:939:5:</a> F822 Undefined name `JavelinAIGateway` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L940'>libs/community/langchain_community/llms/__init__.py:940:5:</a> F822 Undefined name `KoboldApiLLM` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L941'>libs/community/langchain_community/llms/__init__.py:941:5:</a> F822 Undefined name `Konko` in `__all__`
+ <a href='https://github.com/langchain-ai/langchain/blob/0a54aedb85aa2bac66910f9eefa03375f68145e4/libs/community/langchain_community/llms/__init__.py#L942'>libs/community/langchain_community/llms/__init__.py:942:5:</a> F822 Undefined name `LlamaCpp` in `__all__`
... 329 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/a81d52fd19062ec3128f43285eff55cc9e76a511/pandas/core/internals/__init__.py#L11'>pandas/core/internals/__init__.py:11:5:</a> F822 Undefined name `ExtensionBlock` in `__all__`
+ <a href='https://github.com/pandas-dev/pandas/blob/a81d52fd19062ec3128f43285eff55cc9e76a511/pandas/core/internals/__init__.py#L9'>pandas/core/internals/__init__.py:9:5:</a> F822 Undefined name `Block` in `__all__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+27 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/77bb87d50a9944806312cefab2b96608482277a2/astropy/__init__.py#L18'>astropy/__init__.py:18:5:</a> F822 Undefined name `config` in `__all__`
+ <a href='https://github.com/astropy/astropy/blob/77bb87d50a9944806312cefab2b96608482277a2/astropy/__init__.py#L19'>astropy/__init__.py:19:5:</a> F822 Undefined name `constants` in `__all__`
+ <a href='https://github.com/astropy/astropy/blob/77bb87d50a9944806312cefab2b96608482277a2/astropy/__init__.py#L20'>astropy/__init__.py:20:5:</a> F822 Undefined name `convolution` in `__all__`
+ <a href='https://github.com/astropy/astropy/blob/77bb87d50a9944806312cefab2b96608482277a2/astropy/__init__.py#L21'>astropy/__init__.py:21:5:</a> F822 Undefined name `coordinates` in `__all__`
+ <a href='https://github.com/astropy/astropy/blob/77bb87d50a9944806312cefab2b96608482277a2/astropy/__init__.py#L22'>astropy/__init__.py:22:5:</a> F822 Undefined name `cosmology` in `__all__`
+ <a href='https://github.com/astropy/astropy/blob/77bb87d50a9944806312cefab2b96608482277a2/astropy/__init__.py#L23'>astropy/__init__.py:23:5:</a> F822 Undefined name `io` in `__all__`
+ <a href='https://github.com/astropy/astropy/blob/77bb87d50a9944806312cefab2b96608482277a2/astropy/__init__.py#L24'>astropy/__init__.py:24:5:</a> F822 Undefined name `modeling` in `__all__`
+ <a href='https://github.com/astropy/astropy/blob/77bb87d50a9944806312cefab2b96608482277a2/astropy/__init__.py#L25'>astropy/__init__.py:25:5:</a> F822 Undefined name `nddata` in `__all__`
+ <a href='https://github.com/astropy/astropy/blob/77bb87d50a9944806312cefab2b96608482277a2/astropy/__init__.py#L26'>astropy/__init__.py:26:5:</a> F822 Undefined name `samp` in `__all__`
+ <a href='https://github.com/astropy/astropy/blob/77bb87d50a9944806312cefab2b96608482277a2/astropy/__init__.py#L27'>astropy/__init__.py:27:5:</a> F822 Undefined name `stats` in `__all__`
... 17 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F822 | 411 | 411 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `breaking` added by @MichaReiser on 2025-01-11 08:52_

---

_Comment by @MichaReiser on 2025-01-11 08:54_

Thanks. We intended to stabilize this feature as part of 0.8 but realized it's blocked by resolving https://github.com/astral-sh/ruff/issues/12897. We need to resolve this issue before we consider stabilizing it in the next minor.

---

_Closed by @MichaReiser on 2025-01-11 08:54_

---

_Comment by @hassec on 2025-01-16 18:49_

@MichaReiser now that #12897 is fixed, would you be open to reopening this PR?

---

_Comment by @MichaReiser on 2025-01-17 09:08_

We tend to wait about a month after the last "significant" change is made to a preview feature before stabilizing it as part of the next minor. I suggest to leave it closed because there's nothing actionable we can do with the PR before we start working on 0.10, which is roughly in 2 months. 

---
