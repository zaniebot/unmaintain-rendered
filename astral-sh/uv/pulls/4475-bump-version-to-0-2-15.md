```yaml
number: 4475
title: Bump version to 0.2.15
type: pull_request
state: merged
author: zanieb
labels:
  - releases
assignees: []
merged: true
base: main
head: zb/0215
created_at: 2024-06-24T14:46:24Z
updated_at: 2024-06-24T15:04:10Z
url: https://github.com/astral-sh/uv/pull/4475
synced_at: 2026-01-10T13:48:28Z
```

# Bump version to 0.2.15

---

_Pull request opened by @zanieb on 2024-06-24 14:46_

Releasing 0.2.15 with a few additions over 0.2.14. Motivated by the incorrect tagging of 0.2.14 (#4474).

Generated the changelog with a small patch to Rooster allowing me to force the previous commit to be correct.

```diff
diff --git a/src/rooster/_cli.py b/src/rooster/_cli.py
index 2a4f61b..4ec1299 100644
--- a/src/rooster/_cli.py
+++ b/src/rooster/_cli.py
@@ -38,6 +38,7 @@ def release(
     without_sections: list[str] = typer.Option(
         [], help="Sections to exclude from the changelog"
     ),
+    previous_commit: str = None,
 ):
     """
     Create a new release.
@@ -58,7 +59,11 @@ def release(
         typer.echo("It looks like there are no version tags for this project.")
 
     # Get the commits since the last release
-    changes = list(get_commits_between(config, repo, last_version))
+    changes = list(
+        get_commits_between(
+            config, repo, last_version, force_first_commit=previous_commit
+        )
+    )
     since = "since last release" if last_version else "in the project"
     typer.echo(f"Found {len(changes)} commits {since}.")
 
diff --git a/src/rooster/_git.py b/src/rooster/_git.py
index 597bb88..66bc54e 100644
--- a/src/rooster/_git.py
+++ b/src/rooster/_git.py
@@ -29,12 +29,13 @@ def get_commits_between(
     target: Path,
     first_version: Version | None = None,
     second_version: Version | None = None,
+    force_first_commit: str | None = None,
 ) -> Generator[git.Commit, None, None]:
     """
     Yield all commits between two tags
     """
     repo = git.repository.Repository(target.absolute())
-    first_commit = (
+    first_commit = force_first_commit or (
         repo.lookup_reference(
             TAG_PREFIX + config.version_tag_prefix + str(first_version)
         )
```

---

_Label `release` added by @zanieb on 2024-06-24 14:46_

---

_@charliermarsh approved on 2024-06-24 14:48_

---

_Merged by @zanieb on 2024-06-24 15:04_

---

_Closed by @zanieb on 2024-06-24 15:04_

---

_Branch deleted on 2024-06-24 15:04_

---
