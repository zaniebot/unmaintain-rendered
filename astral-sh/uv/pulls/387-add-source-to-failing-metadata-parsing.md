```yaml
number: 387
title: Add source to failing metadata parsing
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: improve-metadata-error
created_at: 2023-11-10T14:13:18Z
updated_at: 2023-11-10T18:33:51Z
url: https://github.com/astral-sh/uv/pull/387
synced_at: 2026-01-12T16:03:55Z
```

# Add source to failing metadata parsing

---

_@konstin_

Before:
```
cargo run --bin puffin-dev -q -- resolve-cli "transformers[accelerate, agents, all, audio, codecarbon, deepspeed, deepspeed-testing, dev, dev-tensorflow, dev-torch, docs, docs_specific, flax, flax-speech, ftfy, integrations, ja, modelcreation, onnx, onnxruntime, optuna, quality, ray, retrieval, sagemaker, sentencepiece, serving, sigopt, sklearn, speech, testing, tf, tf-cpu, tf-speech, timm, tokenizers, torch, torch-speech, torch-vision, torchhub, video, vision]"
puffin-dev failed
  Caused by: No solution found when resolving: transformers[accelerate,agents,all,audio,codecarbon,deepspeed,deepspeed-testing,dev,dev-tensorflow,dev-torch,docs,docs-specific,flax,flax-speech,ftfy,integrations,ja,modelcreation,onnx,onnxruntime,optuna,quality,ray,retrieval,sagemaker,sentencepiece,serving,sigopt,sklearn,speech,testing,tf,tf-cpu,tf-speech,timm,tokenizers,torch,torch-speech,torch-vision,torchhub,video,vision]
  Caused by: Not a valid package or extra name: ".none". Names must start and end with a letter or digit and may only contain -, _, ., and alphanumeric characters
```
After:
```
cargo run --bin puffin-dev -q -- resolve-cli "transformers[accelerate, agents, all, audio, codecarbon, deepspeed, deepspeed-testing, dev, dev-tensorflow, dev-torch, docs, docs_specific, flax, flax-speech, ftfy, integrations, ja, modelcreation, onnx, onnxruntime, optuna, quality, ray, retrieval, sagemaker, sentencepiece, serving, sigopt, sklearn, speech, testing, tf, tf-cpu, tf-speech, timm, tokenizers, torch, torch-speech, torch-vision, torchhub, video, vision]"
puffin-dev failed
  Caused by: No solution found when resolving: transformers[accelerate,agents,all,audio,codecarbon,deepspeed,deepspeed-testing,dev,dev-tensorflow,dev-torch,docs,docs-specific,flax,flax-speech,ftfy,integrations,ja,modelcreation,onnx,onnxruntime,optuna,quality,ray,retrieval,sagemaker,sentencepiece,serving,sigopt,sklearn,speech,testing,tf,tf-cpu,tf-speech,timm,tokenizers,torch,torch-speech,torch-vision,torchhub,video,vision]
  Caused by: Couldn't parse metadata in fastapi-0.10.1-py3-none-any.whl (https://files.pythonhosted.org/packages/6d/b8/97ac91cb7c8f661a810334bde310ed2b2885cce6d2baab1a50b0c7a17d83/fastapi-0.10.1-py3-none-any.whl)
  Caused by: Not a valid package or extra name: ".none". Names must start and end with a letter or digit and may only contain -, _, ., and alphanumeric characters
```

---

_@konstin reviewed on 2023-11-10 14:13_

---

_Review comment by @konstin on `builder.dockerfile`:10 on 2023-11-10 14:13_

Sneaking this in because it's required for transformers

---

_@charliermarsh reviewed on 2023-11-10 14:17_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/client.rs`:215 on 2023-11-10 14:17_

What is the intent of this underscore? And why is it necessary to clone now?

---

_@konstin reviewed on 2023-11-10 14:20_

---

_Review comment by @konstin on `crates/puffin-client/src/client.rs`:215 on 2023-11-10 14:20_

The borrow checker complains otherwise since the async block is move and we want use `url` later again

---

_@konstin reviewed on 2023-11-10 14:21_

---

_Review comment by @konstin on `crates/puffin-client/src/client.rs`:215 on 2023-11-10 14:21_

Made the async block non-move

---

_@charliermarsh approved on 2023-11-10 14:33_

---

_Comment by @charliermarsh on 2023-11-10 18:30_

(Triggering merge to avoid further conflicts.)

---

_Merged by @charliermarsh on 2023-11-10 18:33_

---

_Closed by @charliermarsh on 2023-11-10 18:33_

---

_Branch deleted on 2023-11-10 18:33_

---
