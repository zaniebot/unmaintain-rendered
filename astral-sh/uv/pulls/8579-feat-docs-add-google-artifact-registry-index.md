```yaml
number: 8579
title: "feat(docs): add Google Artifact Registry index instructions"
type: pull_request
state: merged
author: stegayet
labels:
  - documentation
assignees: []
merged: true
base: main
head: feat/add-google-artifact-registry-documentation
created_at: 2024-10-25T22:21:45Z
updated_at: 2024-10-30T08:31:20Z
url: https://github.com/astral-sh/uv/pull/8579
synced_at: 2026-01-10T12:54:12Z
```

# feat(docs): add Google Artifact Registry index instructions

---

_Pull request opened by @stegayet on 2024-10-25 22:21_

## Summary

This commit adds Google Artifact Registry authentication instructions for both basic HTTP authentication and keyring methods.

## Test Plan

Locally tested using both methods.

---

_@Tsafaras reviewed on 2024-10-26 10:28_

---

_Review comment by @Tsafaras on `docs/guides/integration/alternative-indexes.md`:69 on 2024-10-26 10:28_

What about using a Service Account?

That's how I approach it in a CI environment, at least. Following this [Google guide](https://cloud.google.com/docs/authentication#auth-decision-tree) too.
Not sure if you have a better approach.

---

_Assigned to @zanieb by @zanieb on 2024-10-26 16:09_

---

_Label `documentation` added by @zanieb on 2024-10-26 16:09_

---

_@stegayet reviewed on 2024-10-28 08:17_

---

_Review comment by @stegayet on `docs/guides/integration/alternative-indexes.md`:69 on 2024-10-28 08:17_

I can mention other authentication methods indeed :+1: 

---

_Review comment by @Tsafaras on `docs/guides/integration/alternative-indexes.md`:114 on 2024-10-29 21:17_

Can you please double check that it works for you?
Because it doesn't work for me at all, so I don't know if I'm doing something wrong, or if something's wrong with your guide.

I can successfully make it work _without_ `uv` though, if I use this:
```bash
python -m venv .venv
source .venv/bin/activate
pip install keyring keyrings.google-artifactregistry-auth
```
and adding the AR's repo index to `pip.conf`:
```bash
cat > .venv/pip.conf << EOF
[global]
extra-index-url = https://{LOCATION}-python.pkg.dev/{PROJECT}/{REPOSITORY/simple/
EOF
```

Then I can install packages either from PyPI or from my private AR:

![image](https://github.com/user-attachments/assets/fb1865fe-8b5d-4cef-89a8-dcdb710050fa)

But with `uv`, it doesn't work, no matter what I've tried so far. I can't even install from PyPI...

---

_@Tsafaras reviewed on 2024-10-29 21:17_

---

_@zanieb reviewed on 2024-10-29 22:32_

---

_Review comment by @zanieb on `docs/guides/integration/alternative-indexes.md`:114 on 2024-10-29 22:32_

@Tsafaras are you including the `oauth2accesstoken` username as described there?

---

_@zanieb approved on 2024-10-29 22:45_

Thank you!

---

_Merged by @zanieb on 2024-10-29 22:45_

---

_Closed by @zanieb on 2024-10-29 22:45_

---

_Review comment by @Tsafaras on `docs/guides/integration/alternative-indexes.md`:114 on 2024-10-30 07:22_

Yes, I tried all combinations of the guide and none of them worked for me.
I even tried stuff that's not in the guide, still nothing...

---

_@Tsafaras reviewed on 2024-10-30 07:22_

---

_Branch deleted on 2024-10-30 08:31_

---
