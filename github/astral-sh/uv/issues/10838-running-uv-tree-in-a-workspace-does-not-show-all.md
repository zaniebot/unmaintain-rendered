---
number: 10838
title: "Running `uv tree` in a workspace does not show all packages as top level entries"
type: issue
state: open
author: scrooks
labels:
  - question
assignees: []
created_at: 2025-01-22T02:07:38Z
updated_at: 2025-01-22T02:10:21Z
url: https://github.com/astral-sh/uv/issues/10838
synced_at: 2026-01-07T13:12:18-06:00
---

# Running `uv tree` in a workspace does not show all packages as top level entries

---

_Issue opened by @scrooks on 2025-01-22 02:07_

### Question

Not posting this as a bug because I'm not sure.

I have a monorepo workspace containing many packages.  I want to run `uv tree` and see _every_ package as a top-level entry in the tree. (I'm actually trying to parse that output to use with another tool.) Instead, I strangely (in my opinion) only see 3 of them as top level.  Is this because some packages are consumed by others and it was decided that that is "good enough" for the tree view?  Or is it for some other reason?  It's not really a true tree representation this way.  Why are those three called out for top level?

Here's the workspace pyproject.toml:

```toml
[project]
name = "cda-modular-summaries"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = []

[dependency-groups]
test = [
    "pytest>=8",
    "pytest-clarity>=0",
    "pytest-cov>=0",
]
manage = [
    "tox>0",
    "uv>=0.5"
]
dev = [
    { include-group = "manage" },
    { include-group = "test" }
]

[tool.uv]
#index-strategy = "first-index"

[tool.uv.sources]
cda-cli = { workspace = true }
cda-csmodel = { workspace = true }
cda-dyndoc2txt = { workspace = true }
cda-facade-access = { workspace = true }
cda-fhir-data-processing = { workspace = true }
cda-ir-model = { workspace = true }
cda-notes = { workspace = true }
cda-openaiproxy = { workspace = true }
cda-pipeline = { workspace = true }
cda-query-layer = { workspace = true }
cda-sectionizer = { workspace = true }
cda-semantic-index = { workspace = true }
cda-summary-gen-commons = { workspace = true }
cda-summary-generator-top = { workspace = true }
cda-summary-modules = { workspace = true }
cda-summary-store = { workspace = true }
cda-utils = { workspace = true }

# Only an old version of semantic-object-model exists on oracle_release for some reason.
semantic-object-model = { index = "oracle_dev" }

[tool.uv.workspace]
members = ["python/cda_*"]

[[tool.uv.index]]
name = "oracle_release"
url = "https://artifactory.oci.oraclecorp.com/api/pypi/global-release-pypi/simple"

[[tool.uv.index]]
name = "oracle_dev"
url = "https://artifactory.oci.oraclecorp.com/api/pypi/global-dev-pypi/simple"

#[[tool.uv.index]]
#name = "pypi"
#url = "https://pypi.org/simple"
#default = true

[tool.3plt]
# Add any dependencies that don't need to be tracked in 3PLT.  Typically, this
#should only be packages created at Oracle.
untracked-dependencies = [
    "oci",
    "semantic-data-service-python-client",
    "semantic-object-model"
]

# Add non-Python dependencies that should be tracked in 3PLT.  This may be items used
# in containers, tools, etc. They may optionally have a version
# indicator by appending `=<version>`.  For example, `foobar=3.2.5`.  Everything after
# the `=` will be included in the 3PLT.yaml file.
other-top-level-dependencies = []
other-dev-dependencies = []
```

Output of `uv tree`

```
cda-cli v1.1.1
├── cda-dyndoc2txt v0.0.2
│   ├── beautifulsoup4 v4.12.3
│   │   └── soupsieve v2.6
│   └── lxml v5.3.0
├── cda-facade-access v0.2.3
│   ├── cda-semantic-index v1.1.0
│   │   ├── cda-utils v1.2.2
│   │   │   ├── oci v2.143.0
│   │   │   │   ├── certifi v2024.12.14
│   │   │   │   ├── circuitbreaker v2.0.0
│   │   │   │   ├── cryptography v44.0.0
│   │   │   │   │   └── cffi v1.17.1
│   │   │   │   │       └── pycparser v2.22
│   │   │   │   ├── pyopenssl v24.3.0
│   │   │   │   │   └── cryptography v44.0.0 (*)
│   │   │   │   ├── python-dateutil v2.9.0.post0
│   │   │   │   │   └── six v1.17.0
│   │   │   │   └── pytz v2024.2
│   │   │   ├── packaging v24.2
│   │   │   ├── pydantic v2.10.5
│   │   │   │   ├── annotated-types v0.7.0
│   │   │   │   ├── pydantic-core v2.27.2
│   │   │   │   │   └── typing-extensions v4.12.2
│   │   │   │   ├── typing-extensions v4.12.2
│   │   │   │   └── email-validator v2.2.0 (extra: email)
│   │   │   │       ├── dnspython v2.7.0
│   │   │   │       └── idna v3.10
│   │   │   ├── pyyaml v6.0.2
│   │   │   ├── toml v0.10.2
│   │   │   ├── pytest v8.3.4 (group: dev)
│   │   │   │   ├── iniconfig v2.0.0
│   │   │   │   ├── packaging v24.2
│   │   │   │   └── pluggy v1.5.0
│   │   │   ├── pytest-clarity v1.0.1 (group: dev)
│   │   │   │   ├── pprintpp v0.4.0
│   │   │   │   ├── pytest v8.3.4 (*)
│   │   │   │   └── rich v13.9.4
│   │   │   │       ├── markdown-it-py v3.0.0
│   │   │   │       │   └── mdurl v0.1.2
│   │   │   │       └── pygments v2.19.1
│   │   │   ├── pytest-cov v6.0.0 (group: dev)
│   │   │   │   ├── coverage[toml] v7.6.10
│   │   │   │   └── pytest v8.3.4 (*)
│   │   │   └── uv v0.5.22 (group: dev)
│   │   ├── oci v2.143.0 (*)
│   │   ├── protobuf v4.23.4
│   │   ├── semantic-data-service-python-client v0.2.399
│   │   │   └── oci v2.143.0 (*)
│   │   ├── semantic-object-model v0.2.505
│   │   │   ├── protobuf v4.23.4
│   │   │   └── wheel v0.45.1
│   │   ├── pytest v8.3.4 (group: dev) (*)
│   │   ├── pytest-clarity v1.0.1 (group: dev) (*)
│   │   ├── pytest-cov v6.0.0 (group: dev) (*)
│   │   ├── setuptools v75.8.0 (group: dev)
│   │   └── uv v0.5.22 (group: dev)
│   ├── cda-utils v1.2.2 (*)
│   ├── json-stream v2.3.3
│   │   └── json-stream-rs-tokenizer v0.4.27
│   ├── oci v2.143.0 (*)
│   ├── pandas v2.2.3
│   │   ├── numpy v2.2.2
│   │   ├── python-dateutil v2.9.0.post0 (*)
│   │   ├── pytz v2024.2
│   │   └── tzdata v2025.1
│   ├── pydantic v2.10.5 (*)
│   ├── requests v2.32.3
│   │   ├── certifi v2024.12.14
│   │   ├── charset-normalizer v3.4.1
│   │   ├── idna v3.10
│   │   └── urllib3 v2.3.0
│   ├── semantic-data-service-python-client v0.2.399 (*)
│   └── semantic-object-model v0.2.505 (*)
├── cda-fhir-data-processing v0.1.30
│   ├── cda-dyndoc2txt v0.0.2 (*)
│   ├── cda-facade-access v0.2.3 (*)
│   ├── cda-ir-model v1.1.3
│   │   ├── cda-csmodel v1.1.1
│   │   │   ├── cda-utils v1.2.2 (*)
│   │   │   ├── colorama v0.4.6
│   │   │   ├── jsonschema v4.23.0
│   │   │   │   ├── attrs v24.3.0
│   │   │   │   ├── jsonschema-specifications v2024.10.1
│   │   │   │   │   └── referencing v0.36.1
│   │   │   │   │       ├── attrs v24.3.0
│   │   │   │   │       ├── rpds-py v0.22.3
│   │   │   │   │       └── typing-extensions v4.12.2
│   │   │   │   ├── referencing v0.36.1 (*)
│   │   │   │   └── rpds-py v0.22.3
│   │   │   ├── pydantic v2.10.5 (*)
│   │   │   ├── semantic-data-service-python-client v0.2.399 (*)
│   │   │   └── semantic-object-model v0.2.505 (*)
│   │   ├── cda-utils v1.2.2 (*)
│   │   ├── jsonschema v4.23.0 (*)
│   │   ├── pydantic v2.10.5 (*)
│   │   └── semantic-object-model v0.2.505 (*)
│   ├── cda-utils v1.2.2 (*)
│   ├── fhir-resources v7.1.0
│   │   └── pydantic[email] v2.10.5 (*)
│   ├── nameparser v1.1.3
│   ├── pandas v2.2.3 (*)
│   ├── pypdf v5.1.0
│   ├── python-dateutil v2.9.0.post0 (*)
│   └── striprtf v0.0.28
├── cda-ir-model v1.1.3 (*)
├── cda-notes v0.1.0
│   ├── cda-csmodel v1.1.1 (*)
│   ├── cda-ir-model v1.1.3 (*)
│   ├── cda-openaiproxy v0.1.2
│   │   ├── openai v1.59.9
│   │   │   ├── anyio v4.8.0
│   │   │   │   ├── idna v3.10
│   │   │   │   ├── sniffio v1.3.1
│   │   │   │   └── typing-extensions v4.12.2
│   │   │   ├── distro v1.9.0
│   │   │   ├── httpx v0.28.1
│   │   │   │   ├── anyio v4.8.0 (*)
│   │   │   │   ├── certifi v2024.12.14
│   │   │   │   ├── httpcore v1.0.7
│   │   │   │   │   ├── certifi v2024.12.14
│   │   │   │   │   └── h11 v0.14.0
│   │   │   │   └── idna v3.10
│   │   │   ├── jiter v0.8.2
│   │   │   ├── pydantic v2.10.5 (*)
│   │   │   ├── sniffio v1.3.1
│   │   │   ├── tqdm v4.67.1
│   │   │   └── typing-extensions v4.12.2
│   │   └── python-dotenv v1.0.1
│   ├── cda-query-layer v0.6.0
│   │   ├── cda-facade-access v0.2.3 (*)
│   │   ├── cda-fhir-data-processing v0.1.30 (*)
│   │   ├── cda-ir-model v1.1.3 (*)
│   │   ├── cda-utils v1.2.2 (*)
│   │   ├── pydantic v2.10.5 (*)
│   │   └── python-dotenv v1.0.1
│   ├── cda-sectionizer v0.0.3
│   ├── cda-summary-gen-commons v0.2.0
│   │   ├── cda-ir-model v1.1.3 (*)
│   │   ├── cda-summary-generator-top v1.0.3
│   │   │   ├── asyncio v3.4.3
│   │   │   ├── cda-facade-access v0.2.3 (*)
│   │   │   ├── cda-fhir-data-processing v0.1.30 (*)
│   │   │   ├── cda-ir-model v1.1.3 (*)
│   │   │   ├── cda-openaiproxy v0.1.2 (*)
│   │   │   ├── cda-pipeline v0.1.14
│   │   │   │   ├── cda-openaiproxy v0.1.2 (*)
│   │   │   │   ├── cda-utils v1.2.2 (*)
│   │   │   │   ├── pydantic v2.10.5 (*)
│   │   │   │   └── pyyaml v6.0.2
│   │   │   ├── cda-semantic-index v1.1.0 (*)
│   │   │   ├── cda-summary-gen-commons v0.2.0 (*)
│   │   │   ├── cda-summary-store v1.0.0
│   │   │   │   ├── cda-csmodel v1.1.1 (*)
│   │   │   │   ├── cda-ir-model v1.1.3 (*)
│   │   │   │   ├── cda-semantic-index v1.1.0 (*)
│   │   │   │   ├── cda-utils v1.2.2 (*)
│   │   │   │   ├── semantic-data-service-python-client v0.2.399 (*)
│   │   │   │   ├── semantic-object-model v0.2.505 (*)
│   │   │   │   ├── pytest v8.3.4 (group: dev) (*)
│   │   │   │   ├── pytest-clarity v1.0.1 (group: dev) (*)
│   │   │   │   ├── pytest-cov v6.0.0 (group: dev) (*)
│   │   │   │   └── uv v0.5.22 (group: dev)
│   │   │   ├── cda-utils v1.2.2 (*)
│   │   │   ├── colorama v0.4.6
│   │   │   ├── fhir-resources v7.1.0 (*)
│   │   │   ├── nltk v3.9.1
│   │   │   │   ├── click v8.1.8
│   │   │   │   ├── joblib v1.4.2
│   │   │   │   ├── regex v2024.11.6
│   │   │   │   └── tqdm v4.67.1
│   │   │   ├── numpy v2.2.2
│   │   │   ├── openai v1.59.9 (*)
│   │   │   ├── pandas v2.2.3 (*)
│   │   │   ├── protobuf v4.23.4
│   │   │   ├── pydantic v2.10.5 (*)
│   │   │   ├── python-dotenv v1.0.1
│   │   │   ├── scikit-learn v1.6.1
│   │   │   │   ├── joblib v1.4.2
│   │   │   │   ├── numpy v2.2.2
│   │   │   │   ├── scipy v1.15.1
│   │   │   │   │   └── numpy v2.2.2
│   │   │   │   └── threadpoolctl v3.5.0
│   │   │   └── semantic-object-model v0.2.505 (*)
│   │   ├── cda-utils v1.2.2 (*)
│   │   ├── colorama v0.4.6
│   │   ├── json-stream v2.3.3 (*)
│   │   ├── nltk v3.9.1 (*)
│   │   ├── pydantic v2.10.5 (*)
│   │   ├── python-dotenv v1.0.1
│   │   └── scikit-learn v1.6.1 (*)
│   ├── cda-summary-generator-top v1.0.3 (*)
│   ├── cda-utils v1.2.2 (*)
│   ├── pydantic v2.10.5 (*)
│   └── pyyaml v6.0.2
├── cda-query-layer v0.6.0 (*)
├── cda-sectionizer v0.0.3
├── cda-semantic-index v1.1.0 (*)
├── cda-summary-generator-top v1.0.3 (*)
├── cda-summary-modules v0.2.2
│   ├── cda-dyndoc2txt v0.0.2 (*)
│   ├── cda-facade-access v0.2.3 (*)
│   ├── cda-fhir-data-processing v0.1.30 (*)
│   ├── cda-openaiproxy v0.1.2 (*)
│   ├── cda-pipeline v0.1.14 (*)
│   ├── cda-query-layer v0.6.0 (*)
│   ├── cda-sectionizer v0.0.3
│   ├── cda-semantic-index v1.1.0 (*)
│   ├── cda-summary-gen-commons v0.2.0 (*)
│   ├── cda-summary-generator-top v1.0.3 (*)
│   ├── cda-summary-store v1.0.0 (*)
│   ├── cda-utils v1.2.2 (*)
│   └── pydantic v2.10.5 (*)
├── cda-summary-store v1.0.0 (*)
├── cda-utils v1.2.2 (*)
├── requests v2.32.3 (*)
└── sentence-transformers v2.7.0
    ├── huggingface-hub v0.27.1
    │   ├── filelock v3.17.0
    │   ├── fsspec v2024.12.0
    │   ├── packaging v24.2
    │   ├── pyyaml v6.0.2
    │   ├── requests v2.32.3 (*)
    │   ├── tqdm v4.67.1
    │   └── typing-extensions v4.12.2
    ├── numpy v2.2.2
    ├── pillow v11.1.0
    ├── scikit-learn v1.6.1 (*)
    ├── scipy v1.15.1 (*)
    ├── torch v2.5.1
    │   ├── filelock v3.17.0
    │   ├── fsspec v2024.12.0
    │   ├── jinja2 v3.1.5
    │   │   └── markupsafe v3.0.2
    │   ├── networkx v3.4.2
    │   ├── sympy v1.13.1
    │   │   └── mpmath v1.3.0
    │   └── typing-extensions v4.12.2
    ├── tqdm v4.67.1
    └── transformers v4.48.1
        ├── filelock v3.17.0
        ├── huggingface-hub v0.27.1 (*)
        ├── numpy v2.2.2
        ├── packaging v24.2
        ├── pyyaml v6.0.2
        ├── regex v2024.11.6
        ├── requests v2.32.3 (*)
        ├── safetensors v0.5.2
        ├── tokenizers v0.21.0
        │   └── huggingface-hub v0.27.1 (*)
        └── tqdm v4.67.1
cda-html-and-txt-differ v1.0.1
cda-modular-summaries v0.1.0
├── pytest v8.3.4 (group: dev) (*)
├── pytest-clarity v1.0.1 (group: dev) (*)
├── pytest-cov v6.0.0 (group: dev) (*)
├── tox v4.24.1 (group: dev)
│   ├── cachetools v5.5.1
│   ├── chardet v5.2.0
│   ├── colorama v0.4.6
│   ├── filelock v3.17.0
│   ├── packaging v24.2
│   ├── platformdirs v4.3.6
│   ├── pluggy v1.5.0
│   ├── pyproject-api v1.9.0
│   │   └── packaging v24.2
│   └── virtualenv v20.29.1
│       ├── distlib v0.3.9
│       ├── filelock v3.17.0
│       └── platformdirs v4.3.6
└── uv v0.5.22 (group: dev)
(*) Package tree already displayed
```

### Platform

MacOSA 15.2 Darwin 24.2.0 arm64

### Version

uv 0.5.22 (4574ced37 2025-01-21)

---

_Label `question` added by @scrooks on 2025-01-22 02:07_

---

_Comment by @charliermarsh on 2025-01-22 02:10_

Those three packages are at the top-level because nothing depends on them. They are root nodes in the dependency graph.

---
