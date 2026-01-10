```yaml
number: 309
title: Confusing error messages on package resolution failure
type: issue
state: closed
author: zanieb
labels: []
assignees: []
created_at: 2023-11-03T15:25:11Z
updated_at: 2024-02-26T21:12:12Z
url: https://github.com/astral-sh/uv/issues/309
synced_at: 2026-01-10T05:40:31Z
```

# Confusing error messages on package resolution failure

---

_Issue opened by @zanieb on 2023-11-03 15:25_

This issue is to collect all confusing error messages on package resolution failure.

Upstream issues:
- https://github.com/pubgrub-rs/pubgrub/issues/152
- https://github.com/pubgrub-rs/pubgrub/issues/151
- https://github.com/pubgrub-rs/pubgrub/issues/150
- https://github.com/pubgrub-rs/pubgrub/issues/149

---

_Comment by @zanieb on 2023-11-03 15:27_

```
# example.in
httpx==0.8.0
httpx==0.9.0
```
```
$ cargo run -p puffin-cli -- pip-compile example.in
  × No solution found when resolving dependencies:
  ╰─▶ root depends on httpx ∅
```

Two incompatible versions of the same package results in a useless error

---

_Comment by @zanieb on 2023-11-03 15:33_

```
# example.in
httpx==0.8.0
h2==0.3.21
```
```
$ cargo run -p puffin-cli -- pip-compile example.in
  × No solution found when resolving dependencies:
  ╰─▶ Because httpx 0.8.0 depends on h2 >=3.dev0, <4.dev0 and root depends on h2 0.3.21, root, httpx 0.8.0 are incompatible.
      And because root depends on httpx 0.8.0, root is forbidden.
```

A pinned incompatible version of an indirect requirement.

This one is relatively reasonable? It's just quite verbose. We need to do better on `root` is forbidden cases.

---

_Comment by @charliermarsh on 2023-11-04 19:58_

```
# example.in
werkzeug==7.0.0
```

This version doesn't exist, so you get:

```
$ cargo run -p puffin-cli -- pip-compile requirements.in --verbose --no-cache
  × No solution found when resolving dependencies:
  ╰─▶ root depends on werkzeug 7.0.0
```

---

_Comment by @konstin on 2023-11-06 14:06_

The lack of version range merging makes this one unreadable:
```
cargo run --bin puffin -q -- pip-compile scripts/benchmarks/requirements/pydantic_all.in 
  × No solution found when resolving dependencies:
  ╰─▶ Because pydantic >=2.0.3, <2.1.0, >2.1.0, <2.1.1, >2.1.1, <2.2.0, >2.2.0, <2.2.1, >2.2.1, <2.3.0,
      >2.3.0, <2.4.0, >2.4.0, <2.4.1, >2.4.1, <2.4.2, >2.4.2 depends on pydantic-core 2.3.0 and pydantic
      2.1.0 depends on pydantic-core 2.4.0, pydantic >=2.0.3, <2.1.1, >2.1.1, <2.2.0, >2.2.0, <2.2.1, >2.2.1,
      <2.3.0, >2.3.0, <2.4.0, >2.4.0, <2.4.1, >2.4.1, <2.4.2, >2.4.2 depends on pydantic-core 2.3.0, 2.4.0.
      And because pydantic 2.1.1 depends on pydantic-core 2.4.0 and pydantic 2.2.0 depends on pydantic-core
      2.6.0, pydantic >=2.0.3, <2.2.1, >2.2.1, <2.3.0, >2.3.0, <2.4.0, >2.4.0, <2.4.1, >2.4.1, <2.4.2, >2.4.2
      depends on pydantic-core 2.3.0, 2.4.0, 2.6.0.
      And because pydantic 2.2.1 depends on pydantic-core 2.6.1 and pydantic 2.3.0 depends on pydantic-core
      2.6.3, pydantic >=2.0.3, <2.4.0, >2.4.0, <2.4.1, >2.4.1, <2.4.2, >2.4.2 depends on pydantic-core 2.3.0,
      2.4.0, 2.6.0, 2.6.1, 2.6.3.
      And because pydantic 2.4.0 depends on pydantic-core 2.10.0 and pydantic 2.4.1 depends on pydantic-core
      2.10.1, pydantic >=2.0.3, <2.4.2, >2.4.2 depends on pydantic-core 2.3.0, 2.4.0, 2.6.0, 2.6.1, 2.6.3,
      2.10.0, 2.10.1.
      And because pydantic 2.4.2 depends on pydantic-core 2.10.1 and pydantic-extra-types 2.1.0 depends
      on pydantic >=2.0.3, pydantic-extra-types 2.1.0 depends on pydantic-core 2.3.0, 2.4.0, 2.6.0, 2.6.1,
      2.6.3, 2.10.0, 2.10.1.
      And because root 0a0.dev0 depends on pydantic-core 2.11.0 and root 0a0.dev0 depends on pydantic-extra-
      types, root 0a0.dev0 is forbidden.
```

The requirements are using `pydantic-extra-types @ git+https://github.com/pydantic/pydantic-extra-types.git@main`, which means there is actually only one version possible.

Compared to the pip error:
```
ERROR: Cannot install pydantic-core==2.11.0 and pydantic-extra-types because these package versions have conflicting dependencies.

The conflict is caused by:
    The user requested pydantic-core==2.11.0
    pydantic 2.4.2 depends on pydantic-core==2.10.1
    The user requested pydantic-core==2.11.0
    pydantic 2.4.1 depends on pydantic-core==2.10.1
    The user requested pydantic-core==2.11.0
    pydantic 2.4.0 depends on pydantic-core==2.10.0
    The user requested pydantic-core==2.11.0
    pydantic 2.3.0 depends on pydantic-core==2.6.3
    The user requested pydantic-core==2.11.0
    pydantic 2.2.1 depends on pydantic-core==2.6.1
    The user requested pydantic-core==2.11.0
    pydantic 2.2.0 depends on pydantic-core==2.6.0
    The user requested pydantic-core==2.11.0
    pydantic 2.1.1 depends on pydantic-core==2.4.0
    The user requested pydantic-core==2.11.0
    pydantic 2.1.0 depends on pydantic-core==2.4.0
    The user requested pydantic-core==2.11.0
    pydantic 2.0.3 depends on pydantic-core==2.3.0
```

---

_Comment by @konstin on 2023-11-10 13:23_

Extras are handles badly. EDIT: Filed as #386
```
$ cargo run --bin puffin-dev -q -- resolve-cli "torch[accelerate, agents, all, audio, codecarbon, deepspeed, deepspeed-testing, dev, dev-tensorflow, dev-torch, docs, docs_specific, flax, flax-speech, ftfy, integrati
ons, ja, modelcreation, onnx, onnxruntime, optuna, quality,  retrieval, sagemaker, sentencepiece, serving, sigopt, sklearn, speech, testing, tf, tf-cpu, tf-speech, timm, tokenizers, torch, torch-speech, torch-vision, torchhub, video, vision]"
puffin-dev failed
  Caused by: No solution found when resolving build dependencies for source distribution: torch[accelerate,agents,all,audio,codecarbon,deepspeed,deepspeed-testing,dev,dev-tensorflow,dev-torch,docs,docs-specific,flax,flax-speech,ftfy,integrations,ja,modelcreation,onnx,onnxruntime,optuna,quality,retrieval,sagemaker,sentencepiece,serving,sigopt,sklearn,speech,testing,tf,tf-cpu,tf-speech,timm,tokenizers,torch,torch-speech,torch-vision,torchhub,video,vision]
  Caused by: Because there is no version of torch[sagemaker] available matching <1.7.1, >1.7.1, <1.8.0, >1.8.0, <1.8.1, >1.8.1, <1.9.0, >1.9.0, <1.9.1, >1.9.1, <1.10.0, >1.10.0, <1.10.1, >1.10.1, <1.10.2, >1.10.2, <1.11.0, >1.11.0, <1.12.0, >1.12.0, <1.12.1, >1.12.1, <1.13.0, >1.13.0, <1.13.1, >1.13.1, <2.0.0, >2.0.0, <2.0.1, >2.0.1, <2.1.0, >2.1.0 and dependencies of torch[sagemaker] at version ==1.7.1 are unavailable, torch[sagemaker]<1.8.0, >1.8.0, <1.8.1, >1.8.1, <1.9.0, >1.9.0, <1.9.1, >1.9.1, <1.10.0, >1.10.0, <1.10.1, >1.10.1, <1.10.2, >1.10.2, <1.11.0, >1.11.0, <1.12.0, >1.12.0, <1.12.1, >1.12.1, <1.13.0, >1.13.0, <1.13.1, >1.13.1, <2.0.0, >2.0.0, <2.0.1, >2.0.1, <2.1.0, >2.1.0 is forbidden.
And because dependencies of torch[sagemaker] at version ==1.8.0 are unavailable and dependencies of torch[sagemaker] at version ==1.8.1 are unavailable, torch[sagemaker]<1.9.0, >1.9.0, <1.9.1, >1.9.1, <1.10.0, >1.10.0, <1.10.1, >1.10.1, <1.10.2, >1.10.2, <1.11.0, >1.11.0, <1.12.0, >1.12.0, <1.12.1, >1.12.1, <1.13.0, >1.13.0, <1.13.1, >1.13.1, <2.0.0, >2.0.0, <2.0.1, >2.0.1, <2.1.0, >2.1.0 is forbidden.
And because dependencies of torch[sagemaker] at version ==1.9.0 are unavailable and dependencies of torch[sagemaker] at version ==1.9.1 are unavailable, torch[sagemaker]<1.10.0, >1.10.0, <1.10.1, >1.10.1, <1.10.2, >1.10.2, <1.11.0, >1.11.0, <1.12.0, >1.12.0, <1.12.1, >1.12.1, <1.13.0, >1.13.0, <1.13.1, >1.13.1, <2.0.0, >2.0.0, <2.0.1, >2.0.1, <2.1.0, >2.1.0 is forbidden.
And because dependencies of torch[sagemaker] at version ==1.10.0 are unavailable and dependencies of torch[sagemaker] at version ==1.10.1 are unavailable, torch[sagemaker]<1.10.2, >1.10.2, <1.11.0, >1.11.0, <1.12.0, >1.12.0, <1.12.1, >1.12.1, <1.13.0, >1.13.0, <1.13.1, >1.13.1, <2.0.0, >2.0.0, <2.0.1, >2.0.1, <2.1.0, >2.1.0 is forbidden.
And because dependencies of torch[sagemaker] at version ==1.10.2 are unavailable and dependencies of torch[sagemaker] at version ==1.11.0 are unavailable, torch[sagemaker]<1.12.0, >1.12.0, <1.12.1, >1.12.1, <1.13.0, >1.13.0, <1.13.1, >1.13.1, <2.0.0, >2.0.0, <2.0.1, >2.0.1, <2.1.0, >2.1.0 is forbidden.
And because dependencies of torch[sagemaker] at version ==1.12.0 are unavailable and dependencies of torch[sagemaker] at version ==1.12.1 are unavailable, torch[sagemaker]<1.13.0, >1.13.0, <1.13.1, >1.13.1, <2.0.0, >2.0.0, <2.0.1, >2.0.1, <2.1.0, >2.1.0 is forbidden.
And because dependencies of torch[sagemaker] at version ==1.13.0 are unavailable and dependencies of torch[sagemaker] at version ==1.13.1 are unavailable, torch[sagemaker]<2.0.0, >2.0.0, <2.0.1, >2.0.1, <2.1.0, >2.1.0 is forbidden.
And because dependencies of torch[sagemaker] at version ==2.0.0 are unavailable and dependencies of torch[sagemaker] at version ==2.0.1 are unavailable, torch[sagemaker]<2.1.0, >2.1.0 is forbidden.
And because dependencies of torch[sagemaker] at version ==2.1.0 are unavailable and root ==0a0.dev0 depends on torch[sagemaker], version solving failed.
```

---

_Comment by @konstin on 2023-11-22 10:24_

Resolving `lightgbm-ray`:

```
Error for lightgbm-ray (5677/8000, 8106 ms):: No solution found when resolving: lightgbm-ray

Caused by:
    Because there is no version of ray available matching >=2.7 and xgboost-ray==0.1.19 depends on ray>=2.7, xgboost-ray==0.1.19 is forbidden. (1)
    
    Because there is no version of ray available matching >=2.0, <2.7 and there is no version of ray available matching >=2.7, ray>=2.0 is forbidden.
    And because xgboost-ray==0.1.18 depends on ray>=2.0, xgboost-ray==0.1.18 is forbidden. (2)
    And because xgboost-ray==0.1.19 is forbidden (1), xgboost-ray==0.1.18 | ==0.1.19 is forbidden. (3)
    
    Because there is no version of ray available matching >=2.7 and there is no version of ray available matching >=2.0, <2.7, ray>=2.0 is forbidden.
    And because xgboost-ray==0.1.17 depends on ray>=2.0, xgboost-ray==0.1.17 is forbidden. (4)
    And because xgboost-ray==0.1.18 | ==0.1.19 is forbidden (3), xgboost-ray==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden. (5)
    
    Because there is no version of ray available matching >=1.10, <2.0 and there is no version of ray available matching >=2.7, ray>=1.10, <2.0 | >=2.7 is forbidden.
    And because there is no version of ray in >=2.0, <2.7 and xgboost-ray ==0.1.16 depends on ray >=1.10, xgboost-ray==0.1.16 is forbidden. (6)
    And because xgboost-ray==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden (5), xgboost-ray==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden. (7)
    
    Because there is no version of ray available matching >=2.7 and there is no version of ray available matching >=2.0, <2.7, ray>=2.0 is forbidden.
    And because there is no version of ray in >=1.10, <2.0 and xgboost-ray ==0.1.15 depends on ray >=1.10, xgboost-ray==0.1.15 is forbidden. (8)
    And because xgboost-ray==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden (7), xgboost-ray==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden. (9)
    
    Because there is no version of ray available matching >=2.7 and there is no version of ray available matching >=2.0, <2.7, ray>=2.0 is forbidden.
    And because there is no version of ray in >=1.10, <2.0 and xgboost-ray ==0.1.14 depends on ray >=1.10, xgboost-ray==0.1.14 is forbidden. (10)
    And because xgboost-ray==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden (9), xgboost-ray==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden. (11)
    
    Because there is no version of ray available matching >=2.7 and there is no version of ray available matching >=2.0, <2.7, ray>=2.0 is forbidden.
    And because there is no version of ray in >=1.10, <2.0 and xgboost-ray ==0.1.13 depends on ray >=1.10, xgboost-ray==0.1.13 is forbidden. (12)
    And because xgboost-ray==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden (11), xgboost-ray==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden. (13)
    
    Because there is no version of ray available matching >=2.7 and there is no version of ray available matching >=2.0, <2.7, ray>=2.0 is forbidden.
    And because there is no version of ray in >=1.10, <2.0 and xgboost-ray ==0.1.12 depends on ray >=1.10, xgboost-ray==0.1.12 is forbidden. (14)
    And because xgboost-ray==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden (13), xgboost-ray==0.1.12 | ==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.12, <0.1.13 | >0.1.13, <0.1.14 | >0.1.14, <0.1.15 | >0.1.15, <0.1.16 | >0.1.16, <0.1.17 | >0.1.17, <0.1.18 | >0.1.18, <0.1.19 | >0.1.19, xgboost-ray>=0.1.12 is forbidden. (15)
    
    Because there is no version of ray available matching >=2.7 and there is no version of ray available matching >=2.0, <2.7, ray>=2.0 is forbidden.
    And because there is no version of ray in >=1.10, <2.0 and xgboost-ray ==0.1.11 depends on ray >=1.10, xgboost-ray==0.1.11 is forbidden. (16)
    And because xgboost-ray>=0.1.12 is forbidden (15), xgboost-ray==0.1.11 | >=0.1.12 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.11, <0.1.12, xgboost-ray>=0.1.11 is forbidden. (17)
    
    Because there is no version of ray available matching >=2.7 and there is no version of ray available matching >=2.0, <2.7, ray>=2.0 is forbidden.
    And because there is no version of ray in >=1.10, <2.0 and xgboost-ray ==0.1.10 depends on ray >=1.10, xgboost-ray==0.1.10 is forbidden. (18)
    And because xgboost-ray>=0.1.11 is forbidden (17), xgboost-ray==0.1.10 | >=0.1.11 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.10, <0.1.11, xgboost-ray>=0.1.10 is forbidden. (19)
    
    Because there is no version of ray available matching >=2.7 and there is no version of ray available matching >=2.0, <2.7, ray>=2.0 is forbidden.
    And because there is no version of ray in >=1.10, <2.0 and xgboost-ray ==0.1.9 depends on ray >=1.10, xgboost-ray==0.1.9 is forbidden. (20)
    And because xgboost-ray>=0.1.10 is forbidden (19), xgboost-ray==0.1.9 | >=0.1.10 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.9, <0.1.10, xgboost-ray>=0.1.9 is forbidden. (21)
    
    Because there is no version of ray available matching >=1.6, <1.10 and there is no version of ray available matching >=2.7, ray>=1.6, <1.10 | >=2.7 is forbidden.
    And because there is no version of ray available matching >=2.0, <2.7, ray>=1.6, <1.10 | >=2.0 is forbidden.
    And because there is no version of ray in >=1.10, <2.0 and xgboost-ray ==0.1.8 depends on ray >=1.6, xgboost-ray==0.1.8 is forbidden. (22)
    And because xgboost-ray>=0.1.9 is forbidden (21), xgboost-ray==0.1.8 | >=0.1.9 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.8, <0.1.9, xgboost-ray>=0.1.8 is forbidden. (23)
    
    Because there is no version of ray available matching >=2.7 and there is no version of ray available matching >=2.0, <2.7, ray>=2.0 is forbidden.
    And because there is no version of ray available matching >=1.10, <2.0, ray>=1.10 is forbidden.
    And because there is no version of ray in >=1.6, <1.10 and xgboost-ray ==0.1.7 depends on ray >=1.6, xgboost-ray==0.1.7 is forbidden. (24)
    And because xgboost-ray>=0.1.8 is forbidden (23), xgboost-ray==0.1.7 | >=0.1.8 is forbidden. (25)
    
    Because there is no version of ray available matching >=2.7 and there is no version of ray available matching >=2.0, <2.7, ray>=2.0 is forbidden.
    And because there is no version of ray available matching >=1.10, <2.0, ray>=1.10 is forbidden.
    And because there is no version of ray in >=1.6, <1.10 and xgboost-ray ==0.1.6 depends on ray >=1.6, xgboost-ray==0.1.6 is forbidden. (26)
    And because xgboost-ray==0.1.7 | >=0.1.8 is forbidden (25), xgboost-ray==0.1.6 | ==0.1.7 | >=0.1.8 is forbidden. (27)
    
    Because there is no version of ray available matching >=2.7 and there is no version of ray available matching >=2.0, <2.7, ray>=2.0 is forbidden.
    And because there is no version of ray available matching >=1.10, <2.0, ray>=1.10 is forbidden.
    And because there is no version of ray in >=1.6, <1.10 and xgboost-ray ==0.1.5 depends on ray >=1.6, xgboost-ray==0.1.5 is forbidden. (28)
    And because xgboost-ray==0.1.6 | ==0.1.7 | >=0.1.8 is forbidden (27), xgboost-ray==0.1.5 | ==0.1.6 | ==0.1.7 | >=0.1.8 is forbidden. (29)
    
    Because there is no version of ray available matching <1.6 and there is no version of ray available matching >=2.7, ray<1.6 | >=2.7 is forbidden.
    And because there is no version of ray in >=2.0, <2.7 and there is no version of ray in >=1.10, <2.0, ray<1.6 | >=1.10 is forbidden.
    And because there is no version of ray in >=1.6, <1.10 and xgboost-ray ==0.1.4 depends on ray, xgboost-ray==0.1.4 is forbidden. (30)
    And because xgboost-ray==0.1.5 | ==0.1.6 | ==0.1.7 | >=0.1.8 is forbidden (29), xgboost-ray==0.1.4 | ==0.1.5 | ==0.1.6 | ==0.1.7 | >=0.1.8 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.4, <0.1.5 | >0.1.5, <0.1.6 | >0.1.6, <0.1.7 | >0.1.7, <0.1.8, xgboost-ray>=0.1.4 is forbidden. (31)
    
    Because there is no version of ray available matching >=2.7 and there is no version of ray available matching >=2.0, <2.7, ray>=2.0 is forbidden.
    And because there is no version of ray in >=1.10, <2.0 and there is no version of ray in >=1.6, <1.10, ray>=1.6 is forbidden.
    And because there is no version of ray in <1.6 and xgboost-ray ==0.1.3 depends on ray, xgboost-ray==0.1.3 is forbidden. (32)
    And because xgboost-ray>=0.1.4 is forbidden (31), xgboost-ray==0.1.3 | >=0.1.4 is forbidden. (33)
    
    Because there is no version of ray available matching >=2.7 and there is no version of ray available matching >=2.0, <2.7, ray>=2.0 is forbidden.
    And because there is no version of ray in >=1.10, <2.0 and there is no version of ray in >=1.6, <1.10, ray>=1.6 is forbidden.
    And because there is no version of ray in <1.6 and xgboost-ray ==0.1.2 depends on ray, xgboost-ray==0.1.2 is forbidden. (34)
    And because xgboost-ray==0.1.3 | >=0.1.4 is forbidden (33), xgboost-ray==0.1.2 | ==0.1.3 | >=0.1.4 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.2, <0.1.3 | >0.1.3, <0.1.4, xgboost-ray>=0.1.2 is forbidden.
    And because lightgbm-ray ==0.0.1 depends on xgboost-ray >=0.1.2 and there is no version of lightgbm-ray in <0.0.1 | >0.0.1, <0.0.2 | >0.0.2, <0.1.0 | >0.1.0, <0.1.1 | >0.1.1, <0.1.2 | >0.1.2, <0.1.3 | >0.1.3, <0.1.4 | >0.1.4, <0.1.5 | >0.1.5, <0.1.7 | >0.1.7, <0.1.8 | >0.1.8, <0.1.9 | >0.1.9, lightgbm-ray<0.0.2 | >0.0.2, <0.1.0 | >0.1.0, <0.1.1 | >0.1.1, <0.1.2 | >0.1.2, <0.1.3 | >0.1.3, <0.1.4 | >0.1.4, <0.1.5 | >0.1.5, <0.1.7 | >0.1.7, <0.1.8 | >0.1.8, <0.1.9 | >0.1.9 is forbidden. (35)
    
    Because xgboost-ray==0.1.19 is forbidden (1) and xgboost-ray==0.1.18 is forbidden (2), xgboost-ray==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.17 is forbidden (4), xgboost-ray==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.16 is forbidden (6), xgboost-ray==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.15 is forbidden (8), xgboost-ray==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.14 is forbidden (10), xgboost-ray==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.13 is forbidden (12), xgboost-ray==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.12 is forbidden (14), xgboost-ray==0.1.12 | ==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.12, <0.1.13 | >0.1.13, <0.1.14 | >0.1.14, <0.1.15 | >0.1.15, <0.1.16 | >0.1.16, <0.1.17 | >0.1.17, <0.1.18 | >0.1.18, <0.1.19 | >0.1.19, xgboost-ray>=0.1.12 is forbidden.
    And because xgboost-ray==0.1.11 is forbidden (16), xgboost-ray==0.1.11 | >=0.1.12 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.11, <0.1.12, xgboost-ray>=0.1.11 is forbidden.
    And because xgboost-ray==0.1.10 is forbidden (18), xgboost-ray==0.1.10 | >=0.1.11 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.10, <0.1.11, xgboost-ray>=0.1.10 is forbidden.
    And because xgboost-ray==0.1.9 is forbidden (20), xgboost-ray==0.1.9 | >=0.1.10 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.9, <0.1.10, xgboost-ray>=0.1.9 is forbidden.
    And because xgboost-ray==0.1.8 is forbidden (22), xgboost-ray==0.1.8 | >=0.1.9 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.8, <0.1.9, xgboost-ray>=0.1.8 is forbidden.
    And because xgboost-ray==0.1.7 is forbidden (24), xgboost-ray==0.1.7 | >=0.1.8 is forbidden.
    And because xgboost-ray==0.1.6 is forbidden (26), xgboost-ray==0.1.6 | ==0.1.7 | >=0.1.8 is forbidden.
    And because xgboost-ray==0.1.5 is forbidden (28), xgboost-ray==0.1.5 | ==0.1.6 | ==0.1.7 | >=0.1.8 is forbidden.
    And because xgboost-ray==0.1.4 is forbidden (30), xgboost-ray==0.1.4 | ==0.1.5 | ==0.1.6 | ==0.1.7 | >=0.1.8 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.4, <0.1.5 | >0.1.5, <0.1.6 | >0.1.6, <0.1.7 | >0.1.7, <0.1.8, xgboost-ray>=0.1.4 is forbidden.
    And because xgboost-ray==0.1.3 is forbidden (32), xgboost-ray==0.1.3 | >=0.1.4 is forbidden.
    And because xgboost-ray==0.1.2 is forbidden (34), xgboost-ray==0.1.2 | ==0.1.3 | >=0.1.4 is forbidden.
    And because there is no version of xgboost-ray in >0.1.2, <0.1.3 | >0.1.3, <0.1.4 and lightgbm-ray ==0.0.2 depends on xgboost-ray >=0.1.2, lightgbm-ray==0.0.2 is forbidden.
    And because lightgbm-ray<0.0.2 | >0.0.2, <0.1.0 | >0.1.0, <0.1.1 | >0.1.1, <0.1.2 | >0.1.2, <0.1.3 | >0.1.3, <0.1.4 | >0.1.4, <0.1.5 | >0.1.5, <0.1.7 | >0.1.7, <0.1.8 | >0.1.8, <0.1.9 | >0.1.9 is forbidden (35), lightgbm-ray<0.1.0 | >0.1.0, <0.1.1 | >0.1.1, <0.1.2 | >0.1.2, <0.1.3 | >0.1.3, <0.1.4 | >0.1.4, <0.1.5 | >0.1.5, <0.1.7 | >0.1.7, <0.1.8 | >0.1.8, <0.1.9 | >0.1.9 is forbidden. (36)
    
    Because xgboost-ray==0.1.2 is forbidden (34) and there is no version of xgboost-ray available matching >0.1.2, <0.1.3 | >0.1.3, <0.1.4, xgboost-ray>=0.1.2, <0.1.3 | >0.1.3, <0.1.4 is forbidden.
    And because xgboost-ray==0.1.3 is forbidden (32), xgboost-ray>=0.1.2, <0.1.4 is forbidden.
    And because xgboost-ray==0.1.19 is forbidden (1), xgboost-ray>=0.1.2, <0.1.4 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.18 is forbidden (2), xgboost-ray>=0.1.2, <0.1.4 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.17 is forbidden (4), xgboost-ray>=0.1.2, <0.1.4 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.16 is forbidden (6), xgboost-ray>=0.1.2, <0.1.4 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.15 is forbidden (8), xgboost-ray>=0.1.2, <0.1.4 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.14 is forbidden (10), xgboost-ray>=0.1.2, <0.1.4 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.13 is forbidden (12), xgboost-ray>=0.1.2, <0.1.4 | ==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.12 is forbidden (14), xgboost-ray>=0.1.2, <0.1.4 | ==0.1.12 | ==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.12, <0.1.13 | >0.1.13, <0.1.14 | >0.1.14, <0.1.15 | >0.1.15, <0.1.16 | >0.1.16, <0.1.17 | >0.1.17, <0.1.18 | >0.1.18, <0.1.19 | >0.1.19, xgboost-ray>=0.1.2, <0.1.4 | >=0.1.12 is forbidden.
    And because xgboost-ray==0.1.11 is forbidden (16), xgboost-ray>=0.1.2, <0.1.4 | ==0.1.11 | >=0.1.12 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.11, <0.1.12, xgboost-ray>=0.1.2, <0.1.4 | >=0.1.11 is forbidden.
    And because xgboost-ray==0.1.10 is forbidden (18), xgboost-ray>=0.1.2, <0.1.4 | ==0.1.10 | >=0.1.11 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.10, <0.1.11, xgboost-ray>=0.1.2, <0.1.4 | >=0.1.10 is forbidden.
    And because xgboost-ray==0.1.9 is forbidden (20), xgboost-ray>=0.1.2, <0.1.4 | ==0.1.9 | >=0.1.10 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.9, <0.1.10, xgboost-ray>=0.1.2, <0.1.4 | >=0.1.9 is forbidden.
    And because xgboost-ray==0.1.8 is forbidden (22), xgboost-ray>=0.1.2, <0.1.4 | ==0.1.8 | >=0.1.9 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.8, <0.1.9, xgboost-ray>=0.1.2, <0.1.4 | >=0.1.8 is forbidden.
    And because xgboost-ray==0.1.7 is forbidden (24), xgboost-ray>=0.1.2, <0.1.4 | ==0.1.7 | >=0.1.8 is forbidden.
    And because xgboost-ray==0.1.6 is forbidden (26), xgboost-ray>=0.1.2, <0.1.4 | ==0.1.6 | ==0.1.7 | >=0.1.8 is forbidden.
    And because xgboost-ray==0.1.5 is forbidden (28), xgboost-ray>=0.1.2, <0.1.4 | ==0.1.5 | ==0.1.6 | ==0.1.7 | >=0.1.8 is forbidden.
    And because xgboost-ray==0.1.4 is forbidden (30), xgboost-ray>=0.1.2, <=0.1.4 | ==0.1.5 | ==0.1.6 | ==0.1.7 | >=0.1.8 is forbidden.
    And because there is no version of xgboost-ray in >0.1.4, <0.1.5 | >0.1.5, <0.1.6 | >0.1.6, <0.1.7 | >0.1.7, <0.1.8 and lightgbm-ray ==0.1.0 depends on xgboost-ray >=0.1.2, lightgbm-ray==0.1.0 is forbidden.
    And because lightgbm-ray<0.1.0 | >0.1.0, <0.1.1 | >0.1.1, <0.1.2 | >0.1.2, <0.1.3 | >0.1.3, <0.1.4 | >0.1.4, <0.1.5 | >0.1.5, <0.1.7 | >0.1.7, <0.1.8 | >0.1.8, <0.1.9 | >0.1.9 is forbidden (36), lightgbm-ray<0.1.1 | >0.1.1, <0.1.2 | >0.1.2, <0.1.3 | >0.1.3, <0.1.4 | >0.1.4, <0.1.5 | >0.1.5, <0.1.7 | >0.1.7, <0.1.8 | >0.1.8, <0.1.9 | >0.1.9 is forbidden. (37)
    
    Because xgboost-ray==0.1.19 is forbidden (1) and xgboost-ray==0.1.18 is forbidden (2), xgboost-ray==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.17 is forbidden (4), xgboost-ray==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.16 is forbidden (6), xgboost-ray==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.15 is forbidden (8), xgboost-ray==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.14 is forbidden (10), xgboost-ray==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.13 is forbidden (12), xgboost-ray==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.12 is forbidden (14), xgboost-ray==0.1.12 | ==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.12, <0.1.13 | >0.1.13, <0.1.14 | >0.1.14, <0.1.15 | >0.1.15, <0.1.16 | >0.1.16, <0.1.17 | >0.1.17, <0.1.18 | >0.1.18, <0.1.19 | >0.1.19, xgboost-ray>=0.1.12 is forbidden.
    And because xgboost-ray==0.1.11 is forbidden (16), xgboost-ray==0.1.11 | >=0.1.12 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.11, <0.1.12, xgboost-ray>=0.1.11 is forbidden.
    And because xgboost-ray==0.1.10 is forbidden (18), xgboost-ray==0.1.10 | >=0.1.11 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.10, <0.1.11, xgboost-ray>=0.1.10 is forbidden.
    And because xgboost-ray==0.1.9 is forbidden (20), xgboost-ray==0.1.9 | >=0.1.10 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.9, <0.1.10, xgboost-ray>=0.1.9 is forbidden.
    And because xgboost-ray==0.1.8 is forbidden (22), xgboost-ray==0.1.8 | >=0.1.9 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.8, <0.1.9, xgboost-ray>=0.1.8 is forbidden.
    And because xgboost-ray==0.1.7 is forbidden (24), xgboost-ray==0.1.7 | >=0.1.8 is forbidden.
    And because xgboost-ray==0.1.6 is forbidden (26), xgboost-ray==0.1.6 | ==0.1.7 | >=0.1.8 is forbidden.
    And because xgboost-ray==0.1.5 is forbidden (28), xgboost-ray==0.1.5 | ==0.1.6 | ==0.1.7 | >=0.1.8 is forbidden.
    And because xgboost-ray==0.1.4 is forbidden (30), xgboost-ray==0.1.4 | ==0.1.5 | ==0.1.6 | ==0.1.7 | >=0.1.8 is forbidden.
    And because there is no version of xgboost-ray in >0.1.4, <0.1.5 | >0.1.5, <0.1.6 | >0.1.6, <0.1.7 | >0.1.7, <0.1.8 and lightgbm-ray ==0.1.1 depends on xgboost-ray >=0.1.4, lightgbm-ray==0.1.1 is forbidden.
    And because lightgbm-ray<0.1.1 | >0.1.1, <0.1.2 | >0.1.2, <0.1.3 | >0.1.3, <0.1.4 | >0.1.4, <0.1.5 | >0.1.5, <0.1.7 | >0.1.7, <0.1.8 | >0.1.8, <0.1.9 | >0.1.9 is forbidden (37), lightgbm-ray<0.1.2 | >0.1.2, <0.1.3 | >0.1.3, <0.1.4 | >0.1.4, <0.1.5 | >0.1.5, <0.1.7 | >0.1.7, <0.1.8 | >0.1.8, <0.1.9 | >0.1.9 is forbidden. (38)
    
    Because xgboost-ray==0.1.4 is forbidden (30) and there is no version of xgboost-ray available matching >0.1.4, <0.1.5 | >0.1.5, <0.1.6 | >0.1.6, <0.1.7 | >0.1.7, <0.1.8, xgboost-ray>=0.1.4, <0.1.5 | >0.1.5, <0.1.6 | >0.1.6, <0.1.7 | >0.1.7, <0.1.8 is forbidden.
    And because xgboost-ray==0.1.5 is forbidden (28), xgboost-ray>=0.1.4, <0.1.6 | >0.1.6, <0.1.7 | >0.1.7, <0.1.8 is forbidden.
    And because xgboost-ray==0.1.6 is forbidden (26), xgboost-ray>=0.1.4, <0.1.7 | >0.1.7, <0.1.8 is forbidden.
    And because xgboost-ray==0.1.7 is forbidden (24), xgboost-ray>=0.1.4, <0.1.8 is forbidden.
    And because xgboost-ray==0.1.19 is forbidden (1), xgboost-ray>=0.1.4, <0.1.8 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.18 is forbidden (2), xgboost-ray>=0.1.4, <0.1.8 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.17 is forbidden (4), xgboost-ray>=0.1.4, <0.1.8 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.16 is forbidden (6), xgboost-ray>=0.1.4, <0.1.8 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.15 is forbidden (8), xgboost-ray>=0.1.4, <0.1.8 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.14 is forbidden (10), xgboost-ray>=0.1.4, <0.1.8 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.13 is forbidden (12), xgboost-ray>=0.1.4, <0.1.8 | ==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.12 is forbidden (14), xgboost-ray>=0.1.4, <0.1.8 | ==0.1.12 | ==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.12, <0.1.13 | >0.1.13, <0.1.14 | >0.1.14, <0.1.15 | >0.1.15, <0.1.16 | >0.1.16, <0.1.17 | >0.1.17, <0.1.18 | >0.1.18, <0.1.19 | >0.1.19, xgboost-ray>=0.1.4, <0.1.8 | >=0.1.12 is forbidden.
    And because xgboost-ray==0.1.11 is forbidden (16), xgboost-ray>=0.1.4, <0.1.8 | ==0.1.11 | >=0.1.12 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.11, <0.1.12, xgboost-ray>=0.1.4, <0.1.8 | >=0.1.11 is forbidden.
    And because xgboost-ray==0.1.10 is forbidden (18), xgboost-ray>=0.1.4, <0.1.8 | ==0.1.10 | >=0.1.11 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.10, <0.1.11, xgboost-ray>=0.1.4, <0.1.8 | >=0.1.10 is forbidden.
    And because xgboost-ray==0.1.9 is forbidden (20), xgboost-ray>=0.1.4, <0.1.8 | ==0.1.9 | >=0.1.10 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.9, <0.1.10, xgboost-ray>=0.1.4, <0.1.8 | >=0.1.9 is forbidden.
    And because xgboost-ray==0.1.8 is forbidden (22), xgboost-ray>=0.1.4, <=0.1.8 | >=0.1.9 is forbidden.
    And because there is no version of xgboost-ray in >0.1.8, <0.1.9 and lightgbm-ray ==0.1.2 depends on xgboost-ray >=0.1.4, lightgbm-ray==0.1.2 is forbidden.
    And because lightgbm-ray<0.1.2 | >0.1.2, <0.1.3 | >0.1.3, <0.1.4 | >0.1.4, <0.1.5 | >0.1.5, <0.1.7 | >0.1.7, <0.1.8 | >0.1.8, <0.1.9 | >0.1.9 is forbidden (38), lightgbm-ray<0.1.3 | >0.1.3, <0.1.4 | >0.1.4, <0.1.5 | >0.1.5, <0.1.7 | >0.1.7, <0.1.8 | >0.1.8, <0.1.9 | >0.1.9 is forbidden. (39)
    
    Because xgboost-ray==0.1.8 is forbidden (22) and there is no version of xgboost-ray available matching >0.1.8, <0.1.9, xgboost-ray>=0.1.8, <0.1.9 is forbidden.
    And because xgboost-ray==0.1.19 is forbidden (1), xgboost-ray>=0.1.8, <0.1.9 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.18 is forbidden (2), xgboost-ray>=0.1.8, <0.1.9 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.17 is forbidden (4), xgboost-ray>=0.1.8, <0.1.9 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.16 is forbidden (6), xgboost-ray>=0.1.8, <0.1.9 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.15 is forbidden (8), xgboost-ray>=0.1.8, <0.1.9 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.14 is forbidden (10), xgboost-ray>=0.1.8, <0.1.9 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.13 is forbidden (12), xgboost-ray>=0.1.8, <0.1.9 | ==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.12 is forbidden (14), xgboost-ray>=0.1.8, <0.1.9 | ==0.1.12 | ==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.12, <0.1.13 | >0.1.13, <0.1.14 | >0.1.14, <0.1.15 | >0.1.15, <0.1.16 | >0.1.16, <0.1.17 | >0.1.17, <0.1.18 | >0.1.18, <0.1.19 | >0.1.19, xgboost-ray>=0.1.8, <0.1.9 | >=0.1.12 is forbidden.
    And because xgboost-ray==0.1.11 is forbidden (16), xgboost-ray>=0.1.8, <0.1.9 | ==0.1.11 | >=0.1.12 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.11, <0.1.12, xgboost-ray>=0.1.8, <0.1.9 | >=0.1.11 is forbidden.
    And because xgboost-ray==0.1.10 is forbidden (18), xgboost-ray>=0.1.8, <0.1.9 | ==0.1.10 | >=0.1.11 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.10, <0.1.11, xgboost-ray>=0.1.8, <0.1.9 | >=0.1.10 is forbidden.
    And because xgboost-ray==0.1.9 is forbidden (20), xgboost-ray>=0.1.8, <=0.1.9 | >=0.1.10 is forbidden.
    And because there is no version of xgboost-ray in >0.1.9, <0.1.10 and lightgbm-ray ==0.1.3 depends on xgboost-ray >=0.1.8, lightgbm-ray==0.1.3 is forbidden.
    And because lightgbm-ray<0.1.3 | >0.1.3, <0.1.4 | >0.1.4, <0.1.5 | >0.1.5, <0.1.7 | >0.1.7, <0.1.8 | >0.1.8, <0.1.9 | >0.1.9 is forbidden (39), lightgbm-ray<0.1.4 | >0.1.4, <0.1.5 | >0.1.5, <0.1.7 | >0.1.7, <0.1.8 | >0.1.8, <0.1.9 | >0.1.9 is forbidden. (40)
    
    Because xgboost-ray==0.1.9 is forbidden (20) and there is no version of xgboost-ray available matching >0.1.9, <0.1.10, xgboost-ray>=0.1.9, <0.1.10 is forbidden.
    And because xgboost-ray==0.1.19 is forbidden (1), xgboost-ray>=0.1.9, <0.1.10 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.18 is forbidden (2), xgboost-ray>=0.1.9, <0.1.10 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.17 is forbidden (4), xgboost-ray>=0.1.9, <0.1.10 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.16 is forbidden (6), xgboost-ray>=0.1.9, <0.1.10 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.15 is forbidden (8), xgboost-ray>=0.1.9, <0.1.10 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.14 is forbidden (10), xgboost-ray>=0.1.9, <0.1.10 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.13 is forbidden (12), xgboost-ray>=0.1.9, <0.1.10 | ==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.12 is forbidden (14), xgboost-ray>=0.1.9, <0.1.10 | ==0.1.12 | ==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.12, <0.1.13 | >0.1.13, <0.1.14 | >0.1.14, <0.1.15 | >0.1.15, <0.1.16 | >0.1.16, <0.1.17 | >0.1.17, <0.1.18 | >0.1.18, <0.1.19 | >0.1.19, xgboost-ray>=0.1.9, <0.1.10 | >=0.1.12 is forbidden.
    And because xgboost-ray==0.1.11 is forbidden (16), xgboost-ray>=0.1.9, <0.1.10 | ==0.1.11 | >=0.1.12 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.11, <0.1.12, xgboost-ray>=0.1.9, <0.1.10 | >=0.1.11 is forbidden.
    And because xgboost-ray==0.1.10 is forbidden (18), xgboost-ray>=0.1.9, <=0.1.10 | >=0.1.11 is forbidden.
    And because there is no version of xgboost-ray in >0.1.10, <0.1.11 and lightgbm-ray ==0.1.4 depends on xgboost-ray >=0.1.9, lightgbm-ray==0.1.4 is forbidden.
    And because lightgbm-ray<0.1.4 | >0.1.4, <0.1.5 | >0.1.5, <0.1.7 | >0.1.7, <0.1.8 | >0.1.8, <0.1.9 | >0.1.9 is forbidden (40), lightgbm-ray<0.1.5 | >0.1.5, <0.1.7 | >0.1.7, <0.1.8 | >0.1.8, <0.1.9 | >0.1.9 is forbidden. (41)
    
    Because xgboost-ray==0.1.10 is forbidden (18) and there is no version of xgboost-ray available matching >0.1.10, <0.1.11, xgboost-ray>=0.1.10, <0.1.11 is forbidden.
    And because xgboost-ray==0.1.19 is forbidden (1), xgboost-ray>=0.1.10, <0.1.11 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.18 is forbidden (2), xgboost-ray>=0.1.10, <0.1.11 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.17 is forbidden (4), xgboost-ray>=0.1.10, <0.1.11 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.16 is forbidden (6), xgboost-ray>=0.1.10, <0.1.11 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.15 is forbidden (8), xgboost-ray>=0.1.10, <0.1.11 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.14 is forbidden (10), xgboost-ray>=0.1.10, <0.1.11 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.13 is forbidden (12), xgboost-ray>=0.1.10, <0.1.11 | ==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.12 is forbidden (14), xgboost-ray>=0.1.10, <0.1.11 | ==0.1.12 | ==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because there is no version of xgboost-ray available matching >0.1.12, <0.1.13 | >0.1.13, <0.1.14 | >0.1.14, <0.1.15 | >0.1.15, <0.1.16 | >0.1.16, <0.1.17 | >0.1.17, <0.1.18 | >0.1.18, <0.1.19 | >0.1.19, xgboost-ray>=0.1.10, <0.1.11 | >=0.1.12 is forbidden.
    And because xgboost-ray==0.1.11 is forbidden (16), xgboost-ray>=0.1.10, <=0.1.11 | >=0.1.12 is forbidden.
    And because there is no version of xgboost-ray in >0.1.11, <0.1.12 and lightgbm-ray ==0.1.5 depends on xgboost-ray >=0.1.10, lightgbm-ray==0.1.5 is forbidden.
    And because lightgbm-ray<0.1.5 | >0.1.5, <0.1.7 | >0.1.7, <0.1.8 | >0.1.8, <0.1.9 | >0.1.9 is forbidden (41), lightgbm-ray<0.1.7 | >0.1.7, <0.1.8 | >0.1.8, <0.1.9 | >0.1.9 is forbidden. (42)
    
    Because xgboost-ray==0.1.11 is forbidden (16) and there is no version of xgboost-ray available matching >0.1.11, <0.1.12, xgboost-ray>=0.1.11, <0.1.12 is forbidden.
    And because xgboost-ray==0.1.19 is forbidden (1), xgboost-ray>=0.1.11, <0.1.12 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.18 is forbidden (2), xgboost-ray>=0.1.11, <0.1.12 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.17 is forbidden (4), xgboost-ray>=0.1.11, <0.1.12 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.16 is forbidden (6), xgboost-ray>=0.1.11, <0.1.12 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.15 is forbidden (8), xgboost-ray>=0.1.11, <0.1.12 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.14 is forbidden (10), xgboost-ray>=0.1.11, <0.1.12 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.13 is forbidden (12), xgboost-ray>=0.1.11, <0.1.12 | ==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.12 is forbidden (14), xgboost-ray>=0.1.11, <=0.1.12 | ==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because there is no version of xgboost-ray in >0.1.12, <0.1.13 | >0.1.13, <0.1.14 | >0.1.14, <0.1.15 | >0.1.15, <0.1.16 | >0.1.16, <0.1.17 | >0.1.17, <0.1.18 | >0.1.18, <0.1.19 | >0.1.19 and lightgbm-ray ==0.1.7 depends on xgboost-ray >=0.1.11, lightgbm-ray==0.1.7 is forbidden.
    And because lightgbm-ray<0.1.7 | >0.1.7, <0.1.8 | >0.1.8, <0.1.9 | >0.1.9 is forbidden (42), lightgbm-ray<0.1.8 | >0.1.8, <0.1.9 | >0.1.9 is forbidden. (43)
    
    Because xgboost-ray==0.1.19 is forbidden (1) and xgboost-ray==0.1.18 is forbidden (2), xgboost-ray==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.17 is forbidden (4), xgboost-ray==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.16 is forbidden (6), xgboost-ray==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.15 is forbidden (8), xgboost-ray==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.14 is forbidden (10), xgboost-ray==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.13 is forbidden (12), xgboost-ray==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because xgboost-ray==0.1.12 is forbidden (14), xgboost-ray==0.1.12 | ==0.1.13 | ==0.1.14 | ==0.1.15 | ==0.1.16 | ==0.1.17 | ==0.1.18 | ==0.1.19 is forbidden.
    And because there is no version of xgboost-ray in >0.1.12, <0.1.13 | >0.1.13, <0.1.14 | >0.1.14, <0.1.15 | >0.1.15, <0.1.16 | >0.1.16, <0.1.17 | >0.1.17, <0.1.18 | >0.1.18, <0.1.19 | >0.1.19 and lightgbm-ray ==0.1.8 depends on xgboost-ray >=0.1.12, lightgbm-ray==0.1.8 is forbidden.
    And because lightgbm-ray<0.1.8 | >0.1.8, <0.1.9 | >0.1.9 is forbidden (43), lightgbm-ray<0.1.9 | >0.1.9 is forbidden. (44)
    
    Because xgboost-ray==0.1.12 is forbidden (14) and there is no version of xgboost-ray available matching >0.1.12, <0.1.13 | >0.1.13, <0.1.14 | >0.1.14, <0.1.15 | >0.1.15, <0.1.16 | >0.1.16, <0.1.17 | >0.1.17, <0.1.18 | >0.1.18, <0.1.19 | >0.1.19, xgboost-ray>=0.1.12, <0.1.13 | >0.1.13, <0.1.14 | >0.1.14, <0.1.15 | >0.1.15, <0.1.16 | >0.1.16, <0.1.17 | >0.1.17, <0.1.18 | >0.1.18, <0.1.19 | >0.1.19 is forbidden.
    And because xgboost-ray==0.1.13 is forbidden (12), xgboost-ray>=0.1.12, <0.1.14 | >0.1.14, <0.1.15 | >0.1.15, <0.1.16 | >0.1.16, <0.1.17 | >0.1.17, <0.1.18 | >0.1.18, <0.1.19 | >0.1.19 is forbidden.
    And because xgboost-ray==0.1.14 is forbidden (10), xgboost-ray>=0.1.12, <0.1.15 | >0.1.15, <0.1.16 | >0.1.16, <0.1.17 | >0.1.17, <0.1.18 | >0.1.18, <0.1.19 | >0.1.19 is forbidden.
    And because xgboost-ray==0.1.15 is forbidden (8), xgboost-ray>=0.1.12, <0.1.16 | >0.1.16, <0.1.17 | >0.1.17, <0.1.18 | >0.1.18, <0.1.19 | >0.1.19 is forbidden.
    And because xgboost-ray==0.1.16 is forbidden (6), xgboost-ray>=0.1.12, <0.1.17 | >0.1.17, <0.1.18 | >0.1.18, <0.1.19 | >0.1.19 is forbidden.
    And because xgboost-ray==0.1.17 is forbidden (4), xgboost-ray>=0.1.12, <0.1.18 | >0.1.18, <0.1.19 | >0.1.19 is forbidden.
    And because xgboost-ray==0.1.18 is forbidden (2), xgboost-ray>=0.1.12, <0.1.19 | >0.1.19 is forbidden.
    And because xgboost-ray==0.1.19 is forbidden (1), xgboost-ray>=0.1.12 is forbidden.
    And because lightgbm-ray==0.1.9 depends on xgboost-ray>=0.1.12, lightgbm-ray==0.1.9 is forbidden.
    And because lightgbm-ray<0.1.9 | >0.1.9 is forbidden (44), lightgbm-ray* is forbidden.
    And because root depends on lightgbm-ray, version solving failed.
```

---

_Comment by @konstin on 2023-12-12 15:45_

New bad error message, this used to resolve:
```
$ puffin-dev resolve-cli "transformers[accelerate,agents,audio,codecarbon,deepspeed,deepspeed-testing,dev,dev-tensorflow,dev-torch,flax,flax-speech,ftfy,integrations,ja,modelcreation,onnx,onnxruntime,optuna,quality,ray,retrieval,sagemaker,sentencepiece,sigopt,sklearn,speech,testing,tf,tf-cpu,tf-speech,timm,tokenizers,torch,torch-speech,torch-vision,torchhub,video,vision]"
puffin-dev failed
  Caused by: No solution found when resolving: transformers[accelerate,agents,audio,codecarbon,deepspeed,deepspeed-testing,dev,dev-tensorflow,dev-torch,flax,flax-speech,ftfy,integrations,ja,modelcreation,onnx,onnxruntime,optuna,quality,ray,retrieval,sagemaker,sentencepiece,sigopt,sklearn,speech,testing,tf,tf-cpu,tf-speech,timm,tokenizers,torch,torch-speech,torch-vision,torchhub,video,vision]
  Caused by: Conflicting versions for `tokenizers`: `tokenizers==0.9.3` does not intersect with `tokenizers==0.9.2`
```

---

_Comment by @charliermarsh on 2023-12-12 16:18_

Is it correct that it doesn't resolve though?

---

_Comment by @charliermarsh on 2023-12-12 16:32_

(Checking if Poetry can resolve. pip-compile could not, but failed trying to build a package.)

---

_Comment by @konstin on 2023-12-12 16:33_

In this case yes, but it shouldn't fail at that point (i'll file a separate issue)

---

_Comment by @charliermarsh on 2023-12-12 19:53_

After 3 hours and 13 minutes, Poetry failed with:

```
❯ poetry lock
Updating dependencies
Resolving dependencies... (10929.2s)

Incompatible constraints in requirements of transformers[accelerate,agents,audio,codecarbon,deepspeed,deepspeed-testing,dev,dev-tensorflow,dev-torch,flax,flax-speech,ftfy,integrations,ja,modelcreation,onnx,onnxruntime,optuna,quality,ray,retrieval,sagemaker,sentencepiece,sigopt,sklearn,speech,testing,tf,tf-cpu,tf-speech,timm,tokenizers,torch,torch-speech,torch-vision,torchhub,video,vision] (3.5.1):
tokenizers (==0.9.3)
tokenizers (==0.9.2) ; extra == "dev" or extra == "tokenizers"
```

---

_Comment by @charliermarsh on 2024-02-26 21:10_

Some of these are now fixed -- should we close this in favor of more targeted issues?

---

_Closed by @charliermarsh on 2024-02-26 21:12_

---
