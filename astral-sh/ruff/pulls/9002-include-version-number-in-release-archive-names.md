```yaml
number: 9002
title: Include version number in release archive names
type: pull_request
state: merged
author: tobbez
labels:
  - release
assignees: []
merged: true
base: main
head: release-archive-versions
created_at: 2023-12-05T00:07:50Z
updated_at: 2023-12-05T19:42:22Z
url: https://github.com/astral-sh/ruff/pull/9002
synced_at: 2026-01-10T23:40:55Z
```

# Include version number in release archive names

---

_Pull request opened by @tobbez on 2023-12-05 00:07_

## Summary

Add a release's version number to the names of archives containing binaries that are attached to that GitHub release.

This makes it possible for users to easily tell archives from different downloaded releases apart.

See also: #8961

## Test Plan

The workflow was tested in my fork. The example release can be found at: [https://github.com/tobbez/ruff/releases/tag/v0.1.7](https://github.com/tobbez/ruff/releases/tag/v0.1.7).

To allow the workflow run to succeed in the fork while testing, I had to use a small commit to prevent interaction with external services (ghcr, PyPI, and the ruff-pre-commit repository):

```diff
diff --git a/.github/workflows/release.yaml b/.github/workflows/release.yaml
index 86eac6ebc..56b9fa908 100644
--- a/.github/workflows/release.yaml
+++ b/.github/workflows/release.yaml
@@ -463,10 +463,12 @@ jobs:
       id-token: write
     steps:
       - uses: actions/download-artifact@v3
+        if: false
         with:
           name: wheels
           path: wheels
       - name: Publish to PyPi
+        if: false
         uses: pypa/gh-action-pypi-publish@release/v1
         with:
           skip-existing: true
@@ -517,6 +519,7 @@ jobs:
           tag_name: v${{ inputs.tag }}
 
   docker-publish:
+    if: false
     # This action doesn't need to wait on any other task, it's easy to re-tag if something failed and we're validating
     # the tag here also
     name: Push Docker image ghcr.io/astral-sh/ruff
@@ -575,6 +578,7 @@ jobs:
   # After the release has been published, we update downstream repositories
   # This is separate because if this fails the release is still fine, we just need to do some manual workflow triggers
   update-dependents:
+    if: false
     name: Update dependents
     runs-on: ubuntu-latest
     needs: publish-release
```

Those workflow jobs are however not modified by this PR, so they should not be affected.


---

_Comment by @charliermarsh on 2023-12-05 02:46_

This looks reasonable to me and seems to match the scheme that @BurntSushi uses in ripgrep. I really appreciate that you went through the effort of _actually_ testing this, and have included the example release link in the PR.

---

_Label `release` added by @charliermarsh on 2023-12-05 02:46_

---

_@charliermarsh reviewed on 2023-12-05 02:47_

---

_Review comment by @charliermarsh on `.github/workflows/release.yaml`:89 on 2023-12-05 02:47_

Above, tag says `"The version to tag, without the leading 'v'. If omitted, will initiate a dry run (no uploads)."` Do you know if that's still true? Can it be omitted? And, if it is omitted, what happens here?

---

_Comment by @BurntSushi on 2023-12-05 02:53_

One note to consider here is whether there are any docs anywhere that walk someone through downloading and extracting a release archive. Those might need to be updated to reflect the name change.

---

_@tobbez reviewed on 2023-12-05 10:13_

---

_Review comment by @tobbez on `.github/workflows/release.yaml`:89 on 2023-12-05 10:13_

It can still be omitted, in which case `${{ inputs.tag }}` expands to an empty string. That specific example would result in `ruff--x86_64-apple-darwin.tar.gz` in dry-run mode. You can see an example in [this workflow run](https://github.com/tobbez/ruff/actions/runs/7093701040).

---

_Comment by @tobbez on 2023-12-05 10:13_

> One note to consider here is whether there are any docs anywhere that walk someone through downloading and extracting a release archive. Those might need to be updated to reflect the name change.

The [installation instructions](https://github.com/astral-sh/ruff/blob/main/docs/installation.md) don't mention these archives at all, and a grep for `zip` and `tar.gz` in the repository found nothing relevant.

---

_Review comment by @charliermarsh on `.github/workflows/release.yaml`:89 on 2023-12-05 19:42_

It'd be nice to omit the second dash, but that's just a debugging workflow so not critical.

---

_@charliermarsh reviewed on 2023-12-05 19:42_

---

_Merged by @charliermarsh on 2023-12-05 19:42_

---

_Closed by @charliermarsh on 2023-12-05 19:42_

---

_Comment by @charliermarsh on 2023-12-05 19:42_

Thanks @tobbez! Great PR.

---
