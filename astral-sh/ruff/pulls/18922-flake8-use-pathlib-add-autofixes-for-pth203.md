```yaml
number: 18922
title: "[`flake8-use-pathlib`] Add autofixes for `PTH203`, `PTH204`, `PTH205`"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: feat/autofixes
created_at: 2025-06-24T19:40:03Z
updated_at: 2025-07-08T05:03:31Z
url: https://github.com/astral-sh/ruff/pull/18922
synced_at: 2026-01-10T18:33:12Z
```

# [`flake8-use-pathlib`] Add autofixes for `PTH203`, `PTH204`, `PTH205`

---

_Pull request opened by @chirizxc on 2025-06-24 19:40_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
Part of #2331 | [#18763](https://github.com/astral-sh/ruff/pull/18763#issuecomment-2988340436)
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
update snapshots
<!-- How was it tested? -->


---

_Converted to draft by @chirizxc on 2025-06-24 19:40_

---

_Comment by @chirizxc on 2025-06-24 20:01_

idk why `cargo nextest run pth205` doesn't go through properly after these names
![изображение](https://github.com/user-attachments/assets/d5f9f9fb-766c-408a-828a-6d0559ef8f65)


---

_Comment by @MichaReiser on 2025-06-25 08:07_

You'll need to update the snapshots (they assert the expected output) using `cargo insta review` (https://insta.rs/docs/cli/)

---

_Comment by @chirizxc on 2025-06-25 08:09_

> You'll need to update the snapshots (they assert the expected output) usi

i did, but the problem is that they don't update correctly

---

_Comment by @MichaReiser on 2025-06-25 08:36_

> i did, but the problem is that they don't update correctly

Can you say more about why they don't update correctly? I did

```
cargo nextest run -p ruff_linter --no-fail-fast
cargo insta review
```

and the snapshots were updated correctly

---

_Comment by @chirizxc on 2025-06-25 08:44_

> cargo insta review

![изображение](https://github.com/user-attachments/assets/e2021aa9-1c86-426d-92e4-8d5722699e66)


---

_Comment by @chirizxc on 2025-06-25 08:45_

i applied the changes from past snapshots, but for some reason they didn't overwrite now, very strange

---

_Comment by @MichaReiser on 2025-06-25 08:50_

Did you add them with `git add <path to snapshot>` or `git add .`?

---

_Comment by @chirizxc on 2025-06-25 08:51_

> Did you add them with `git add <path to snapshot>` or `git add .`?

yes

---

_Comment by @MichaReiser on 2025-06-25 08:53_

Can you run `git status` and `git diff` and share the output

---

_Comment by @chirizxc on 2025-06-25 08:55_

```diff
❯ git status                                                                                                                                          
On branch feat/autofixes                                                                                                                              
Your branch is up to date with 'origin/feat/autofixes'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH203_PTH203.py.snap
        new file:   crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__preview__PTH203_PTH203.py.snap
        new file:   crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__preview__PTH204_PTH204.py.snap
        new file:   crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__preview__PTH205_PTH205.py.snap

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH204_PTH204.py.snap
        modified:   crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH205_PTH205.py.
       ```

---

_Comment by @chirizxc on 2025-06-25 08:55_

```diff
❯ git diff                                                                                                                                            
diff --git a/crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH204_PTH204.py.snap b/crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH204_PTH204.py.snap
index b137cbd77..6d3b40def 100644
--- a/crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH204_PTH204.py.snap
+++ b/crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH204_PTH204.py.snap
@@ -1,51 +1,357 @@                                                                                                                                    
 ---
 source: crates/ruff_linter/src/rules/flake8_use_pathlib/mod.rs
-snapshot_kind: text                                                                                                                                  
 ---
-PTH204.py:6:1: PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`                                                                
-  |                                                                                                                                                  
-6 | os.path.getmtime("filename")                                                                                                                     
-  | ^^^^^^^^^^^^^^^^ PTH204                                                                                                                          
-7 | os.path.getmtime(b"filename")                                                                                                                    
-8 | os.path.getmtime(Path("filename"))                                                                                                               
-  |                                                                                                                                                  
+PTH204.py:11:1: PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`                                                                 
+   |                                                                                                                                                 
+11 | os.path.getmtime("filename")                                                                                                                    
+   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^ PTH202                                                                                                             
+12 | os.path.getmtime(b"filename")                                                                                                                   
+13 | os.path.getmtime(Path("filename"))                                                                                                              
+   |                                                                                                                                                 
:...skipping...
diff --git a/crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH204_PTH204.py.snap b/crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH204_PTH204.py.snap
index b137cbd77..6d3b40def 100644
--- a/crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH204_PTH204.py.snap
+++ b/crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH204_PTH204.py.snap
@@ -1,51 +1,357 @@
 ---
 source: crates/ruff_linter/src/rules/flake8_use_pathlib/mod.rs
-snapshot_kind: text
 ---
-PTH204.py:6:1: PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
-  |
-6 | os.path.getmtime("filename")
-  | ^^^^^^^^^^^^^^^^ PTH204
-7 | os.path.getmtime(b"filename")
-8 | os.path.getmtime(Path("filename"))
-  |
+PTH204.py:11:1: PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`
+   |
+11 | os.path.getmtime("filename")
+   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^ PTH202
+12 | os.path.getmtime(b"filename")
+13 | os.path.getmtime(Path("filename"))
+   |
+   = help: Replace with `Path(...).stat().st_size`
+
+PTH204.py:12:1: PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`
+   |
+11 | os.path.getmtime("filename")
 ESCOD
diff --git a/crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH204_PTH204.py.snap b/crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH204_PTH204.py.snap
index b137cbd77..6d3b40def 100644
--- a/crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH204_PTH204.py.snap
+++ b/crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH204_PTH204.py.snap
index b137cbd77..6d3b40def 100644
--- a/crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH204_PTH204.py.snap
+++ b/crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH204_PTH204.py.snap
@@ -1,51 +1,357 @@                                                                                                                                    
 ---
 source: crates/ruff_linter/src/rules/flake8_use_pathlib/mod.rs
-snapshot_kind: text                                                                                                                                  
 ---
-PTH204.py:6:1: PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`                                                                
-  |                                                                                                                                                  
-6 | os.path.getmtime("filename")                                                                                                                     
-  | ^^^^^^^^^^^^^^^^ PTH204                                                                                                                          
-7 | os.path.getmtime(b"filename")                                                                                                                    
-8 | os.path.getmtime(Path("filename"))                                                                                                               
-  |                                                                                                                                                  
+PTH204.py:11:1: PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`                                                                 
+   |                                                                                                                                                 
+11 | os.path.getmtime("filename")                                                                                                                    
+   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^ PTH202                                                                                                             
+12 | os.path.getmtime(b"filename")                                                                                                                   
+13 | os.path.getmtime(Path("filename"))                                                                                                              
+   |                                                                                                                                                 
+   = help: Replace with `Path(...).stat().st_size`                                                                                                   
+                                                                                                                                                     
+PTH204.py:12:1: PTH202 `os.path.getsize` should be replaced by `Path.stat().st_size`                                                                 
+   |                                                                                                                                                 
+11 | os.path.getmtime("filename")                                                                                                                    
+12 | os.path.getmtime(b"filename")                                                                                                                   
+   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ PTH202     
```

---

_Comment by @MichaReiser on 2025-06-25 08:57_

The output suggests that you still have uncommited local changes. You have to stage them using `git add .`, then commit and push them.

---

_Comment by @github-actions[bot] on 2025-06-25 18:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +28 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +12 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/83348cda3aa95df1157bb09f7d88bdd574f4a5cb/airflow-core/src/airflow/dag_processing/manager.py#L931'>airflow-core/src/airflow/dag_processing/manager.py:931:38:</a> PTH204 [*] `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
- <a href='https://github.com/apache/airflow/blob/83348cda3aa95df1157bb09f7d88bdd574f4a5cb/airflow-core/src/airflow/dag_processing/manager.py#L931'>airflow-core/src/airflow/dag_processing/manager.py:931:38:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/apache/airflow/blob/83348cda3aa95df1157bb09f7d88bdd574f4a5cb/airflow-core/src/airflow/models/dagbag.py#L318'>airflow-core/src/airflow/models/dagbag.py:318:64:</a> PTH204 [*] `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
- <a href='https://github.com/apache/airflow/blob/83348cda3aa95df1157bb09f7d88bdd574f4a5cb/airflow-core/src/airflow/models/dagbag.py#L318'>airflow-core/src/airflow/models/dagbag.py:318:64:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/apache/airflow/blob/83348cda3aa95df1157bb09f7d88bdd574f4a5cb/devel-common/src/sphinx_exts/docs_build/fetch_inventories.py#L92'>devel-common/src/sphinx_exts/docs_build/fetch_inventories.py:92:71:</a> PTH204 [*] `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
- <a href='https://github.com/apache/airflow/blob/83348cda3aa95df1157bb09f7d88bdd574f4a5cb/devel-common/src/sphinx_exts/docs_build/fetch_inventories.py#L92'>devel-common/src/sphinx_exts/docs_build/fetch_inventories.py:92:71:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/apache/airflow/blob/83348cda3aa95df1157bb09f7d88bdd574f4a5cb/providers/standard/src/airflow/providers/standard/sensors/filesystem.py#L132'>providers/standard/src/airflow/providers/standard/sensors/filesystem.py:132:60:</a> PTH204 [*] `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
- <a href='https://github.com/apache/airflow/blob/83348cda3aa95df1157bb09f7d88bdd574f4a5cb/providers/standard/src/airflow/providers/standard/sensors/filesystem.py#L132'>providers/standard/src/airflow/providers/standard/sensors/filesystem.py:132:60:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/apache/airflow/blob/83348cda3aa95df1157bb09f7d88bdd574f4a5cb/providers/standard/src/airflow/providers/standard/triggers/file.py#L124'>providers/standard/src/airflow/providers/standard/triggers/file.py:124:30:</a> PTH204 [*] `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
- <a href='https://github.com/apache/airflow/blob/83348cda3aa95df1157bb09f7d88bdd574f4a5cb/providers/standard/src/airflow/providers/standard/triggers/file.py#L124'>providers/standard/src/airflow/providers/standard/triggers/file.py:124:30:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/apache/airflow/blob/83348cda3aa95df1157bb09f7d88bdd574f4a5cb/providers/standard/src/airflow/providers/standard/triggers/file.py#L77'>providers/standard/src/airflow/providers/standard/triggers/file.py:77:34:</a> PTH204 [*] `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
- <a href='https://github.com/apache/airflow/blob/83348cda3aa95df1157bb09f7d88bdd574f4a5cb/providers/standard/src/airflow/providers/standard/triggers/file.py#L77'>providers/standard/src/airflow/providers/standard/triggers/file.py:77:34:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +4 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_gallery.py#L125'>src/bokeh/sphinxext/bokeh_gallery.py:125:26:</a> PTH204 [*] `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_gallery.py#L125'>src/bokeh/sphinxext/bokeh_gallery.py:125:26:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_gallery.py#L146'>src/bokeh/sphinxext/bokeh_gallery.py:146:41:</a> PTH204 [*] `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_gallery.py#L146'>src/bokeh/sphinxext/bokeh_gallery.py:146:41:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +12 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/a3e7ef8e71929154691361b36e2d06c57fc87270/scripts/lib/zulip_tools.py#L359'>scripts/lib/zulip_tools.py:359:12:</a> PTH205 [*] `os.path.getctime` should be replaced by `Path.stat().st_ctime`
- <a href='https://github.com/zulip/zulip/blob/a3e7ef8e71929154691361b36e2d06c57fc87270/scripts/lib/zulip_tools.py#L359'>scripts/lib/zulip_tools.py:359:12:</a> PTH205 `os.path.getctime` should be replaced by `Path.stat().st_ctime`
+ <a href='https://github.com/zulip/zulip/blob/a3e7ef8e71929154691361b36e2d06c57fc87270/zerver/data_import/mattermost.py#L362'>zerver/data_import/mattermost.py:362:24:</a> PTH204 [*] `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
- <a href='https://github.com/zulip/zulip/blob/a3e7ef8e71929154691361b36e2d06c57fc87270/zerver/data_import/mattermost.py#L362'>zerver/data_import/mattermost.py:362:24:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/zulip/zulip/blob/a3e7ef8e71929154691361b36e2d06c57fc87270/zerver/lib/compatibility.py#L30'>zerver/lib/compatibility.py:30:49:</a> PTH204 [*] `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
- <a href='https://github.com/zulip/zulip/blob/a3e7ef8e71929154691361b36e2d06c57fc87270/zerver/lib/compatibility.py#L30'>zerver/lib/compatibility.py:30:49:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/zulip/zulip/blob/a3e7ef8e71929154691361b36e2d06c57fc87270/zerver/lib/test_fixtures.py#L329'>zerver/lib/test_fixtures.py:329:33:</a> PTH204 [*] `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
- <a href='https://github.com/zulip/zulip/blob/a3e7ef8e71929154691361b36e2d06c57fc87270/zerver/lib/test_fixtures.py#L329'>zerver/lib/test_fixtures.py:329:33:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/zulip/zulip/blob/a3e7ef8e71929154691361b36e2d06c57fc87270/zerver/lib/test_fixtures.py#L357'>zerver/lib/test_fixtures.py:357:33:</a> PTH204 [*] `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
- <a href='https://github.com/zulip/zulip/blob/a3e7ef8e71929154691361b36e2d06c57fc87270/zerver/lib/test_fixtures.py#L357'>zerver/lib/test_fixtures.py:357:33:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
+ <a href='https://github.com/zulip/zulip/blob/a3e7ef8e71929154691361b36e2d06c57fc87270/zerver/lib/upload/local.py#L134'>zerver/lib/upload/local.py:134:43:</a> PTH204 [*] `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
- <a href='https://github.com/zulip/zulip/blob/a3e7ef8e71929154691361b36e2d06c57fc87270/zerver/lib/upload/local.py#L134'>zerver/lib/upload/local.py:134:43:</a> PTH204 `os.path.getmtime` should be replaced by `Path.stat().st_mtime`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PTH204 | 26 | 0 | 0 | 26 | 0 |
| PTH205 | 2 | 0 | 0 | 2 | 0 |

</p>
</details>




---

_Comment by @chirizxc on 2025-06-25 18:42_

tests on github runners also passed, i don't really understand why, maybe it's because i did something wrong in adding the tests `crates/ruff_linter/src/rules/flake8_use_pathlib/mod.rs`

---

_Comment by @chirizxc on 2025-06-25 18:46_

oh, sorry, i found my stupid mistake :(

---

_Marked ready for review by @chirizxc on 2025-06-25 18:59_

---

_Label `fixes` added by @MichaReiser on 2025-06-26 08:09_

---

_Label `fixes` added by @MichaReiser on 2025-06-26 08:09_

---

_Review requested from @ntBre by @MichaReiser on 2025-06-26 08:09_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_getatime.rs`:115 on 2025-06-26 17:53_

```suggestion
            Ok(
                Fix::applicable_edits(Edit::range_replacement(replacement, range), [import_edit], applicability)
            )
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_getctime.rs`:114 on 2025-06-26 17:53_

Same as above, let's use `applicable_edits`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_getmtime.rs`:114 on 2025-06-26 17:54_

`applicable_edits`

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_use_pathlib/PTH203.py`:1 on 2025-06-26 17:56_

Do we need all of these new tests? The answer could definitely be "yes," but could you explain what new coverage they're adding? I think it would make sense to add at least one case with comments to each rule, but this seems like a lot of new cases for rules I thought would already have good test coverage.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_getctime.rs`:65 on 2025-06-26 17:57_

I feel like there's more that we should be able to factor out here. These three implementations look virtually identical. All that changes is the attribute being checked, right?

---

_@ntBre reviewed on 2025-06-26 18:01_

Thanks! I had some suggestions to factor out common code and maybe strip down new test cases a bit.

---

_@chirizxc reviewed on 2025-06-27 09:35_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_getctime.rs`:65 on 2025-06-27 09:35_

yes, i put the implementation in a `helper.rs`, but not sure about the name of the function, what do you think?

---

_Review comment by @chirizxc on `crates/ruff_linter/resources/test/fixtures/flake8_use_pathlib/PTH203.py`:1 on 2025-06-27 09:49_

I think we can cut the tests down to:
```python
import os
import pathlib
from os.path import getatime
from pathlib import Path

file = __file__

os.path.getatime(file)
os.path.getatime("filename")
os.path.getatime(Path("filename"))

os.path.getatime(filename="filename")
os.path.getatime(filename=Path("filename"))

getatime("filename")
getatime(Path("filename"))

os.path.getatime(
    "filename", # comment
)

os.path.getatime(
    # comment
    "filename"
    ,
    # comment
)

os.path.getatime( # comment
    Path(__file__)
    # comment
) # comment

getatime( # comment
    "filename")

getatime( # comment
    b"filename",
    #comment
)

os.path.getatime("file" + "name")

getatime \
 \
 \
        ( # comment
        "filename",
    )

getatime(Path("filename").resolve())

os.path.getatime(pathlib.Path("filename"))
```


---

_@chirizxc reviewed on 2025-06-27 09:49_

---

_@chirizxc reviewed on 2025-06-27 09:54_

---

_Review comment by @chirizxc on `crates/ruff_linter/resources/test/fixtures/flake8_use_pathlib/PTH203.py`:1 on 2025-06-27 09:54_

new tests are needed to check unsafe fix when there are comments, check filename, and check that `pathlib.Path(pathlib.Path("test"))` becomes `pathlib.Path("test")` too

---

_@chirizxc reviewed on 2025-06-27 09:55_

---

_Review comment by @chirizxc on `crates/ruff_linter/resources/test/fixtures/flake8_use_pathlib/PTH203.py`:1 on 2025-06-27 09:55_

![изображение](https://github.com/user-attachments/assets/bf095932-72b8-46bc-8b96-13ab80a200a5)


---

_Comment by @chirizxc on 2025-06-27 10:04_

looks like windows runner down

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_use_pathlib/PTH203.py`:1 on 2025-07-02 13:40_

This would be my suggestion for this file:

```python
import os.path, pathlib
from pathlib import Path
from os.path import getatime

os.path.getatime("filename")
os.path.getatime(b"filename")
os.path.getatime(Path("filename"))


getatime("filename")
getatime(b"filename")
getatime(Path("filename"))


file = __file__

os.path.getatime(file)
os.path.getatime(filename="filename")
os.path.getatime(filename=Path("filename"))

os.path.getatime(  # comment 1
    # comment 2
    "filename"  # comment 3
    # comment 4
    ,  # comment 5
    # comment 6
)  # comment 7

os.path.getatime("file" + "name")

getatime(Path("filename").resolve())

os.path.getatime(pathlib.Path("filename"))
```

It avoids a diff for the existing test cases, which makes it easier to review, and it condenses the various comment cases into a single case with basically all of the possible comment locations.

Since we're using the same helper method for the other two rules, I think we can probably just leave their tests alone.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:53 on 2025-07-02 13:48_

nit: we could check this in the `if fix_enabled` branch.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_use_pathlib/PTH203.py`:1 on 2025-07-02 13:59_

We may also want a test like this:

```python
getatime(Path("dir") / "file.txt")
```

I tried locally, and I think it works out because we wrap it in `Path` again, otherwise we'd need parentheses.

---

_@ntBre reviewed on 2025-07-02 14:00_

Thanks! Just a couple more suggestions, mostly around test organization.

---

_@chirizxc reviewed on 2025-07-02 14:33_

---

_Review comment by @chirizxc on `crates/ruff_linter/resources/test/fixtures/flake8_use_pathlib/PTH203.py`:1 on 2025-07-02 14:33_

> We may also want a test like this:
> 
> ```python
> getatime(Path("dir") / "file.txt")
> ```
> 
> I tried locally, and I think it works out because we wrap it in `Path` again, otherwise we'd need parentheses.

yea it should work, if we remove `Path()` here we would get `TypeError: unsupported operand type(s) for /: ‘str’ and ‘str’`.


---

_Label `preview` added by @ntBre on 2025-07-04 19:28_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH203_PTH203.py.snap`:9 on 2025-07-04 19:36_

I think we have the wrong range selected here. I'd prefer to keep the old one.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_use_pathlib/PTH204.py`:13 on 2025-07-04 19:38_

While fixing the range, can you go ahead and revert the changes to these files? Just to clear them from the diff.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:36 on 2025-07-04 19:45_

I think this is a slight simplification to the signature and the qualified name check:


```suggestion
pub(crate) fn check_os_path_get_calls(
    checker: &Checker,
    call: &ExprCall,
    fn_name: &str,
    attr: &str,
    fix_enabled: bool,
    violation: impl Violation,
) {
    if checker
        .semantic()
        .resolve_qualified_name(&call.func)
        .is_some_and(|qualified_name| qualified_name.segments() != &["os", "path", fn_name])
    {
        return;
    }
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:49 on 2025-07-04 19:45_

I think this should fix the snapshot ranges:

```suggestion
    let mut diagnostic = checker.report_diagnostic(violation, call.func.range());
```

---

_@ntBre reviewed on 2025-07-04 19:50_

Thanks! Just a couple more nits and a larger, but easy to fix, concern about the diagnostic range that I didn't notice before.

---

_@chirizxc reviewed on 2025-07-04 20:09_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:36 on 2025-07-04 20:09_

thanks

---

_Comment by @chirizxc on 2025-07-04 20:09_

done! waiting CI

---

_Comment by @codspeed-hq[bot] on 2025-07-04 20:18_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/chirizxc%3Afeat%2Fautofixes?runnerMode=Instrumentation)

### Merging #18922 will **not alter performance**

<sub>Comparing <code>chirizxc:feat/autofixes</code> (de70bda) with <code>main</code> (44f2f77)</sub>



### Summary

`✅ 40` untouched benchmarks  





---

_Comment by @ntBre on 2025-07-04 20:27_

Uh, I think something very weird just happened with CI. Both the ecosystem report and benchmarks are showing very unreasonable results. I'll try rerunning it. I definitely don't think your changes caused this.

---

_@ntBre reviewed on 2025-07-04 20:31_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__PTH202_PTH202.py.snap`:7 on 2025-07-04 20:31_

Ahh, I see. I missed this on https://github.com/astral-sh/ruff/pull/18763. The shorter range is how PTH202 used to be too, so this is perfect.

---

_@ntBre approved on 2025-07-04 20:33_

Thanks! We'll get CI to pass somehow and then this is good to go. I may try closing and reopening the PR to fully re-trigger it if re-running through the actions interface doesn't work.

---

_Closed by @ntBre on 2025-07-04 20:36_

---

_Reopened by @ntBre on 2025-07-04 20:36_

---

_Comment by @ntBre on 2025-07-04 21:19_

Hmm, rerunning didn't seem to help. Do you want to try merging `main` into your branch? Just in case it's a real change from the past couple of days.

---

_@ntBre reviewed on 2025-07-05 05:06_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:27 on 2025-07-05 05:06_

Well this is a bit embarrassing, but I just realized that my suggestion _did_ break your code, and CI was correct. This should be `is_none_or` to preserve the original behavior.

```suggestion
        .is_none_or(|qualified_name| qualified_name.segments() != ["os", "path", fn_name])
```

---

_Comment by @chirizxc on 2025-07-05 10:17_

oh, it seems to working, thank you!

---

_Review requested from @ntBre by @MichaReiser on 2025-07-07 16:39_

---

_@ntBre approved on 2025-07-07 20:55_

Great, thanks again! And sorry for the bad suggestion

---

_Merged by @ntBre on 2025-07-07 20:56_

---

_Closed by @ntBre on 2025-07-07 20:56_

---

_Branch deleted on 2025-07-08 05:03_

---
