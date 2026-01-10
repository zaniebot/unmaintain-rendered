```yaml
number: 14251
title: GCP Artifact Registry download URLs must have /simple path
type: pull_request
state: merged
author: pasunboneleve
labels:
  - documentation
assignees: []
merged: true
base: main
head: dmvianna/docs/gcp-repository-url-must-be-simple
created_at: 2025-06-25T04:27:08Z
updated_at: 2025-06-25T20:41:52Z
url: https://github.com/astral-sh/uv/pull/14251
synced_at: 2026-01-10T11:10:43Z
```

# GCP Artifact Registry download URLs must have /simple path

---

_Pull request opened by @pasunboneleve on 2025-06-25 04:27_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The current **uv** [docs](https://docs.astral.sh/uv/guides/integration/alternative-indexes/#google-artifact-registry) for **Google Artifact Registry** integration state that download URLs are expressed as

```toml
url = "https://<REGION>-python.pkg.dev/<PROJECT>/<REPOSITORY>"
```

I just found, by following the documentation, that this is incomplete. Instead, they should be 

```toml
url = "https://<REGION>-python.pkg.dev/<PROJECT>/<REPOSITORY>/simple/"
```
in order to work. The trailing `/` is optional, but I also included it to conform to the standard used in the sections referring to other cloud providers within this same page.

This PR makes the necessary change to the documentation.

## Test Plan

1. [Create](https://cloud.google.com/artifact-registry/docs/python/store-python#create) a GCP Artifact Registry repository.
2. Add it to a **Python** project using

```bash
export ARTIFACT_REGISTRY_PULL_TOKEN=$(
    gcloud auth print-access-token --project=<PROJECT>
)

export UV_INDEX_BUILD_USERNAME=oauth2accesstoken
export UV_INDEX_BUILD_PASSWORD="$ARTIFACT_REGISTRY_PULL_TOKEN
```

and

```toml
[[tool.uv.index]]
name = "BUILD"
url = "https://<REGION>-python.pkg.dev/<PROJECT>/<REPOSITORY>/simple/"
```

3. Assuming packages were added to that repository using `uv publish`, pull one of these using `uv add <package>`.

---

_Review requested from @jtfmumm by @konstin on 2025-06-25 07:36_

---

_Label `documentation` added by @konstin on 2025-06-25 07:36_

---

_@konstin reviewed on 2025-06-25 07:37_

---

_Review comment by @konstin on `docs/guides/integration/alternative-indexes.md`:223 on 2025-06-25 07:37_

Have you check the trailing slash for the publish URL? The rules for it are different, as we don't redirect POST requests

---

_@pasunboneleve reviewed on 2025-06-25 10:22_

---

_Review comment by @pasunboneleve on `docs/guides/integration/alternative-indexes.md`:223 on 2025-06-25 10:22_

I just deleted my package from Artifact Repository and published again. Worked as expected.

---

_@jtfmumm approved on 2025-06-25 15:20_

---

_Comment by @jtfmumm on 2025-06-25 15:28_

Thanks for catching this!

---

_Merged by @jtfmumm on 2025-06-25 15:35_

---

_Closed by @jtfmumm on 2025-06-25 15:35_

---

_Branch deleted on 2025-06-25 20:41_

---
