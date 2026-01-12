```yaml
number: 15676
title: "Wrong behavior of `--only-group` and `--extra`"
type: issue
state: closed
author: j-adamczyk
labels:
  - bug
  - good first issue
assignees: []
created_at: 2025-09-04T12:11:29Z
updated_at: 2025-09-11T15:34:50Z
url: https://github.com/astral-sh/uv/issues/15676
synced_at: 2026-01-12T16:02:15Z
```

# Wrong behavior of `--only-group` and `--extra`

---

_@j-adamczyk_

### Summary

I have a pretty standard `pyproject.toml` file:
```
[project]
name = "project-name"
version = "1.0.0"
description = ""
authors = [{ name = "ML team" }]
readme = "README.md"
requires-python = ">=3.12,<3.13"

dependencies = []

# select a CPU or GPU variant of PyTorch
pytorch_variant = {train_model = "pytorch-gpu", inference = "pytorch-cpu"}

[dependency-groups]
dev = [
    "jupyter",
    "pre-commit",
    "ruff"
]

training = [
    "accelerate==1.*",
    "boto3==1.*",
    "pillow==11.*",
    "pydantic-settings==2.*",
    "scikit-learn==1.7.*",
    "tqdm",
    "transformers==4.56.*",
]

jenkins = [
    "boto3==1.*",
    "pyyaml",
    "sagemaker==2.*"
]

inference = [
    "fastapi==0.116.*",
    "opentelemetry-api==1.36.*",
    "opentelemetry-exporter-otlp==1.36.*",
    "opentelemetry-instrumentation-fastapi",
    "opentelemetry-sdk==1.36.*",
    "pandas==2.*",
    "pillow==11.*",
    "pydantic-settings==2.*",
    "scikit-learn==1.7.*",
    "sentry-sdk[fastapi]==2.*",
    "transformers==4.56.*",
    "uvicorn==0.35.*",
]

[project.optional-dependencies]
pytorch-cpu = ["torch==2.8.0"]
pytorch-gpu = ["torch==2.8.0"]

[tool.uv]
conflicts = [
  [
    { extra = "pytorch-cpu" },
    { extra = "pytorch-gpu" }
  ]
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "pytorch-cpu" },
  { index = "pytorch-gpu", extra = "pytorch-gpu" }
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-gpu"
url = "https://download.pytorch.org/whl/cu126"  # sync with PyTorch version
explicit = true
```

When I run `uv sync --only-group inference --extra pytorch-cpu`, my environment does not contain PyTorch at all, and extra is ignored. Result of `uv pip list` is:
```
annotated-types                          0.7.0
anyio                                    4.10.0
asgiref                                  3.9.1
certifi                                  2025.8.3
charset-normalizer                       3.4.3
click                                    8.2.1
fastapi                                  0.116.1
filelock                                 3.19.1
fsspec                                   2025.9.0
googleapis-common-protos                 1.70.0
grpcio                                   1.74.0
h11                                      0.16.0
hf-xet                                   1.1.9
huggingface-hub                          0.34.4
idna                                     3.10
importlib-metadata                       6.11.0
joblib                                   1.5.2
numpy                                    1.26.4
opentelemetry-api                        1.36.0
opentelemetry-exporter-otlp              1.36.0
opentelemetry-exporter-otlp-proto-common 1.36.0
opentelemetry-exporter-otlp-proto-grpc   1.36.0
opentelemetry-exporter-otlp-proto-http   1.36.0
opentelemetry-instrumentation            0.57b0
opentelemetry-instrumentation-asgi       0.57b0
opentelemetry-instrumentation-fastapi    0.57b0
opentelemetry-proto                      1.36.0
opentelemetry-sdk                        1.36.0
opentelemetry-semantic-conventions       0.57b0
opentelemetry-util-http                  0.57b0
packaging                                24.2
pandas                                   2.3.2
pillow                                   11.3.0
protobuf                                 6.31.1
pydantic                                 2.11.7
pydantic-core                            2.33.2
pydantic-settings                        2.10.1
python-dateutil                          2.9.0.post0
python-dotenv                            1.1.1
pytz                                     2025.2
pyyaml                                   6.0.2
regex                                    2025.9.1
requests                                 2.32.5
safetensors                              0.6.2
scikit-learn                             1.7.1
scipy                                    1.16.1
sentry-sdk                               2.36.0
six                                      1.17.0
sniffio                                  1.3.1
starlette                                0.47.3
threadpoolctl                            3.6.0
tokenizers                               0.22.0
tqdm                                     4.67.1
transformers                             4.56.0
typing-extensions                        4.15.0
typing-inspection                        0.4.1
tzdata                                   2025.2
urllib3                                  2.5.0
uvicorn                                  0.35.0
wrapt                                    1.17.3
zipp                                     3.23.0
```

Meanwhile, `uv sync --no-default-groups --group inference --extra pytorch-cpu` is behaving properly:
```
annotated-types                          0.7.0
anyio                                    4.10.0
asgiref                                  3.9.1
certifi                                  2025.8.3
charset-normalizer                       3.4.3
click                                    8.2.1
fastapi                                  0.116.1
filelock                                 3.19.1
fsspec                                   2025.9.0
googleapis-common-protos                 1.70.0
grpcio                                   1.74.0
h11                                      0.16.0
hf-xet                                   1.1.9
huggingface-hub                          0.34.4
idna                                     3.10
importlib-metadata                       6.11.0
jinja2                                   3.1.6
joblib                                   1.5.2
markupsafe                               3.0.2
mpmath                                   1.3.0
networkx                                 3.5
numpy                                    1.26.4
opentelemetry-api                        1.36.0
opentelemetry-exporter-otlp              1.36.0
opentelemetry-exporter-otlp-proto-common 1.36.0
opentelemetry-exporter-otlp-proto-grpc   1.36.0
opentelemetry-exporter-otlp-proto-http   1.36.0
opentelemetry-instrumentation            0.57b0
opentelemetry-instrumentation-asgi       0.57b0
opentelemetry-instrumentation-fastapi    0.57b0
opentelemetry-proto                      1.36.0
opentelemetry-sdk                        1.36.0
opentelemetry-semantic-conventions       0.57b0
opentelemetry-util-http                  0.57b0
packaging                                24.2
pandas                                   2.3.2
pillow                                   11.3.0
protobuf                                 6.31.1
pydantic                                 2.11.7
pydantic-core                            2.33.2
pydantic-settings                        2.10.1
python-dateutil                          2.9.0.post0
python-dotenv                            1.1.1
pytz                                     2025.2
pyyaml                                   6.0.2
regex                                    2025.9.1
requests                                 2.32.5
safetensors                              0.6.2
scikit-learn                             1.7.1
scipy                                    1.16.1
sentry-sdk                               2.36.0
setuptools                               80.9.0
six                                      1.17.0
sniffio                                  1.3.1
starlette                                0.47.3
sympy                                    1.14.0
threadpoolctl                            3.6.0
tokenizers                               0.22.0
torch                                    2.8.0+cpu
tqdm                                     4.67.1
transformers                             4.56.0
typing-extensions                        4.15.0
typing-inspection                        0.4.1
tzdata                                   2025.2
urllib3                                  2.5.0
uvicorn                                  0.35.0
wrapt                                    1.17.3
zipp                                     3.23.0
```

This is a bug, because when I explicitly specify an extra, it should override the `--only-group`. Those are two different things anyway (dependency groups vs optional dependencies).

### Platform

Ubuntu 25.04

### Version

0.8.15

### Python version

3.12.11

---

_Label `bug` added by @j-adamczyk on 2025-09-04 12:11_

---

_Comment by @zanieb on 2025-09-04 12:25_

Hm, `--only-group` excludes the project, hence no extras can be selected. I think you want `--no-default-groups --group inference --extra pytorch-cpu`?

I think we should probably error if `--only-group` and `--extra` are provided.

---

_Comment by @j-adamczyk on 2025-09-04 13:11_

@zanieb yeah, I figured that out after 3 full days of working on this. I absolutely expect specyfing `--extra pytorch-cpu` to work even if `--only-group inference` is specified. Since that means "I want just this dependency group, and this optional dependency". Currently, dependency groups are mixed with optional dependencies in this resolution, even if I explicitly specify that `--extra`.

Error + informative message pointing to `--no-default-groups` + other arguments would be definitely helpful, but I still find this behavior very counterintuitive.

This is extremely hard to find with PyTorch+transformers here, since actually if you try to run it, it throws an error that `libcublas` and other CUDA libraries are note present. This is due to complex dependencies between transformers and PyTorch.

---

_Comment by @charliermarsh on 2025-09-06 02:37_

Yeah, I think this probably shouldn't be allowed. Extras are always additive (so they require the project itself to be installed too). Dependency groups are allowed to be installed without installing the project, and that was an intentional part of the standard design.

---

_Label `good first issue` added by @zanieb on 2025-09-06 11:50_

---

_Comment by @11happy on 2025-09-09 15:49_

Hii @charliermarsh & @zanieb  , Can I work on this ? 

Thank you : )


---

_Closed by @zanieb on 2025-09-11 15:34_

---
