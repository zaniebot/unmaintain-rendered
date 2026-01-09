---
number: 14034
title: aws codeartifact works in v0.7.12 but not v0.7.13
type: issue
state: closed
author: kurt-rhee
labels:
  - question
assignees: []
created_at: 2025-06-13T18:05:38Z
updated_at: 2025-06-13T18:38:29Z
url: https://github.com/astral-sh/uv/issues/14034
synced_at: 2026-01-07T13:12:18-06:00
---

# aws codeartifact works in v0.7.12 but not v0.7.13

---

_Issue opened by @kurt-rhee on 2025-06-13 18:05_

### Summary

The following bash script works in `0.7.12` but fails in `0.7.13`

update_core.sh
```
export AWS_CODEARTIFACT_TOKEN="$(
    aws codeartifact get-authorization-token \
    --domain proximal-code-artifact-domain \
    --domain-owner 016997484973 \
    --query authorizationToken \
    --output text
)"

if [ -z "$AWS_CODEARTIFACT_TOKEN" ]; then
    echo "AWS CodeArtifact Token is empty"
    exit 1
fi

LATEST_VERSION=$(aws codeartifact list-package-versions \
    --package core \
    --domain proximal-code-artifact-domain \
    --domain-owner 016997484973 \
    --repository proximal-hub \
    --sort-by PUBLISHED_TIME \
    --format pypi \
    --output text \
    --query 'versions[0].version' \
    )


export UV_INDEX_PROXIMAL_USERNAME=aws
export UV_INDEX_PROXIMAL_PASSWORD="$AWS_CODEARTIFACT_TOKEN"

uv add "core==$LATEST_VERSION" --index proximal

```


pyproject.toml
```
[[tool.uv.index]]
name = "proximal"
url = "https://proximal-code-artifact-domain-016997484973.d.codeartifact.us-east-2.amazonaws.com/pypi/proximal-hub/simple/"
explicit = true

[tool.uv.sources]
core = { index = "proximal" }
```

error message in v0.7.13
```
(api) kurt@Mac api %  ./_scripts/update_core.sh
error: Directory not found for index: file:///Users/kurt/Programs/api/proximal
```



### Platform

Darwin 24.5.0 arm64

### Version

uv 0.7.12 (dc3fd4647 2025-06-06)

### Python version

Python 3.12.8

---

_Label `bug` added by @kurt-rhee on 2025-06-13 18:05_

---

_Comment by @zanieb on 2025-06-13 18:07_

Using `uv add --index <name>` isn't supported, see

- https://github.com/astral-sh/uv/issues/13974
- #13874 

---

_Comment by @zanieb on 2025-06-13 18:08_

(the fact that `uv add` accepted `--index <name>` previously was a bug — sorry the fix broke your script though!)

---

_Label `bug` removed by @zanieb on 2025-06-13 18:08_

---

_Label `question` added by @zanieb on 2025-06-13 18:08_

---

_Comment by @kurt-rhee on 2025-06-13 18:38_

Got it!  Thanks for the links they were helpful.  

---

_Closed by @kurt-rhee on 2025-06-13 18:38_

---
