```yaml
number: 20918
title: "[ty] Run file watching tests serial when using nextest"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: configure-nextest-sequential
created_at: 2025-10-16T12:12:42Z
updated_at: 2025-10-16T14:08:39Z
url: https://github.com/astral-sh/ruff/pull/20918
synced_at: 2026-01-12T15:57:12Z
```

# [ty] Run file watching tests serial when using nextest

---

_@MichaReiser_

## Summary

The file watcher tests sometimes flake locally (macOS only). 

I suspect that this is mainly happening because all file watcher test setup native file watchers at once with high concurrency,
which, what i suspect, leads to overwhelming the OS filewatcher. 

This PR adds a custom test group for the file watching tests that enforces that they run serial.

## Test Plan

Verified that nextest recognizes the configuration:

```
cargo nextest --profile ci show-config test-groups
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.16s
group: serial (max threads = 1)
  * override for default profile with filter 'binary(file_watching)':
      ty::file_watching:
          add_search_path
          change_python_version_and_platform
          changed_file
          changed_versions_file
          changes_to_config_file_override
          changes_to_user_configuration
          deleted_file
          directory_deleted
          directory_moved_to_project
          directory_moved_to_trash
          directory_renamed
          hard_links_in_project
          hard_links_to_target_outside_project
          move_file_to_project
          move_file_to_trash
          nested_projects_delete_root
          new_file
          new_file_in_included_out_of_project_directory
          new_files_with_explicit_included_paths
          new_ignored_file
          new_non_project_file
          remove_search_path
          rename_file
          search_path
          submodule_cache_invalidation_after_pyproject_created
          submodule_cache_invalidation_created
          submodule_cache_invalidation_created_then_deleted
          submodule_cache_invalidation_deleted
          unix::changed_metadata
          unix::symlink_inside_project
          unix::symlinked_module_search_path
```

and runs the tests serial


---

_Label `testing` added by @MichaReiser on 2025-10-16 12:12_

---

_Label `ty` added by @MichaReiser on 2025-10-16 12:12_

---

_@sharkdp approved on 2025-10-16 14:04_

---

_Merged by @MichaReiser on 2025-10-16 14:08_

---

_Closed by @MichaReiser on 2025-10-16 14:08_

---

_Branch deleted on 2025-10-16 14:08_

---
