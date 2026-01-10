```yaml
number: 11244
title: "Issue with `uv pip` Handling Multiple Private Index URLs Leading to 403 Errors"
type: issue
state: closed
author: Galdanwing
labels:
  - bug
assignees: []
created_at: 2025-02-05T14:03:20Z
updated_at: 2025-02-08T15:23:32Z
url: https://github.com/astral-sh/uv/issues/11244
synced_at: 2026-01-10T03:50:31Z
```

# Issue with `uv pip` Handling Multiple Private Index URLs Leading to 403 Errors

---

_Issue opened by @Galdanwing on 2025-02-05 14:03_

### Summary

Hello,

I am currently working on a project that requires pulling first-party libraries from multiple private artifact registries. To authenticate with these registries, I generate a JSON-based key in the format `https://_json_key_base64:{actual-key}/{location-of-registry}` and use it in the extra-index-url when installing packages via `uv pip`.

This approach works perfectly when installing from a single index. However, when I attempt to pull from multiple indexes, I encounter persistent 403 errors.

#### Steps to Reproduce:

1. Use `uv pip` to install libraries from multiple private registries:

   ```bash
   uv pip install lib-from-private-registry-1 lib-from-private-registry-2 --index {json-base-64-key-of-private-registry-1-index-url} --index {json-base-64-key-of-private-registry-2-index-url}
   ```

2. This results in the following error:

   ```
   error: Failed to fetch: `https://europe-west4-python.pkg.dev/{location-of-registry-1}`
     Caused by: HTTP status client error (403 Forbidden) for url (https://europe-west4-python.pkg.dev/{location-of-registry-1}/l{lib-from-private-registry-1})
   ```

3. The issue does not occur when using `pip` with the `--extra-index-url` option.

#### Expected Behavior:

When using `uv pip` with multiple `--index` options, libraries from all specified private registries should be installed without resulting in authorization errors.

#### Observed Behavior:

When specifying multiple private registries using `uv pip`, access to the libraries results in an HTTP 403 (Forbidden) error, indicating an issue with authorization.

#### Additional Information:

- **Tool Version:** `uv` 0.5.28 (commit ee2bdc21f, 2025-02-04)
- **Python Version:** 3.13.1 (using virtual environment)

#### Anonymized verbose log
```
uv pip install {lib-1} {lib-2} --index ${registry-1} --index ${registry-2} --no-cache --verbose
DEBUG uv 0.5.28 (ee2bdc21f 2025-02-04)
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.13.1-macos-aarch64-none` at `/Users/me/Repos/project/venv/bin/python3` (active virtual environment)
Using Python 3.13.1 environment at: venv
DEBUG Acquired lock for `venv`
DEBUG At least one requirement is not satisfied: {lib-2}
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13.1
DEBUG Solving with target Python version: >=3.13.1
DEBUG Adding direct dependency: {lib-1}*
DEBUG Adding direct dependency: {lib-2}*
DEBUG No cache entry for: https://europe-west4-python.pkg.dev/{registry-1}/{lib-1}
DEBUG No cache entry for: https://europe-west4-python.pkg.dev/{registry-1}/{lib-2}
DEBUG No cache entry for: https://europe-west4-python.pkg.dev/{registry-2}/{lib-2}
DEBUG Searching for a compatible version of {lib-1} (*)
DEBUG Selecting: {lib-1}==0.5.0 [compatible] ({lib-1}-0.5.0-py3-none-any.whl)
DEBUG No cache entry for: https://europe-west4-python.pkg.dev/{registry-1}/{lib-1}/{lib-1}
DEBUG No cache entry for: https://europe-west4-python.pkg.dev/{registry-2/{lib-2}/{lib-2}
WARN Range requests not supported for {lib-2}; streaming wheel
WARN Range requests not supported for {lib-1}; streaming wheel
DEBUG No cache entry for: https://europe-west4-python.pkg.dev/{registry-1}/{lib-1}
DEBUG No cache entry for: https://europe-west4-python.pkg.dev/{registry-2}/{lib-2}
DEBUG Released lock at `/Users/me/Repos/project/venv/.lock`
error: Failed to fetch: `https://europe-west4-python.pkg.dev/{registry-1}/{lib-1
  Caused by: HTTP status client error (403 Forbidden) for url ({registry-1)
```

### Platform

Darwin 24.3.0 arm64

### Version

uv 0.5.28 (ee2bdc21f 2025-02-04)

### Python version

3.13.1

---

_Label `bug` added by @Galdanwing on 2025-02-05 14:03_

---

_Comment by @zanieb on 2025-02-05 15:15_

I'm guessing this is related to https://github.com/astral-sh/uv/pull/11074 e.g., as described at https://github.com/astral-sh/uv/issues/8565#issuecomment-2437949960

Is there a way you could share more details about the index URLs? Obfuscated is fine, but the structure is meaningful here.

You can also use `RUST_LOG=uv=trace` to get some more logs, which I presume will show us falling back to the "realm cache" for credentials.

---

_Comment by @zanieb on 2025-02-05 15:16_

Where do your registries serve files from relative to the index URL you provide?

---

_Comment by @Galdanwing on 2025-02-06 08:10_

Hey @zanieb  , the registries are both 'Artifact Registry's in GCP.

Once I pulled the service-account json keys from GCP I convert it to a url I can use as extra index url like so:

```
import base64
import json

ARTIFACT_REGISTRY_NAME = "test-artifact-registry"
GCP_PROJECT_ID="..."

with open('/Users/me/file/google-service-account-json.json') as json_file:
    json_key = json.load(json_file)

json_b64 = base64.b64encode(json.dumps(json_key).encode()).decode()
url = f"https://_json_key_base64:{json_b64}@europe-west4-python.pkg.dev/{GCP_PROJECT_ID}/{ARTIFACT_REGISTRY_NAME}/simple/"
```
The second URL would then be generated the same way, except that the of course the ARTIFACT_REGISTRY_NAME but also the GCP_PROJECT_ID would be different. Does that answer your question? :)

In the rust trace it first matches the first package with the first registry, and then indeed it does show: `TRACE Found cached credentials for realm https://europe-west4-python.pkg.dev`which seems wrong, after this I see a request to the second registry with the key from the first one :) 

As to your second comment:
`https://europe-west4-python.pkg.dev/{GCP_PROJECT_ID/{ARTIFACT_REGISTRY_NAME}/{library/{libary-version}.whl`, so this no longer contains the key, and therefore you fall back I guess, like you described in #8565 

I will follow #11074 and if there's anything else you need, I'll try and answer!

---

_Comment by @zanieb on 2025-02-08 01:58_

Ah thanks for the details. #11074 was released in uv 0.5.26 — it looks like there's a bug with a trailing `/`.

https://github.com/astral-sh/uv/pull/11336

---

_Closed by @zanieb on 2025-02-08 15:23_

---

_Closed by @zanieb on 2025-02-08 15:23_

---
