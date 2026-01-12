```yaml
number: 10950
title: "`ruff server`: Resolve configuration for each document individually"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/per-document-config
created_at: 2024-04-15T08:43:23Z
updated_at: 2024-04-16T21:39:39Z
url: https://github.com/astral-sh/ruff/pull/10950
synced_at: 2026-01-12T15:55:33Z
```

# `ruff server`: Resolve configuration for each document individually

---

_@snowsignal_

## Summary

Configuration is no longer the property of a workspace but rather of individual documents. Just like the Ruff CLI, each document is configured based on the 'nearest' project configuration. See [the Ruff documentation](https://docs.astral.sh/ruff/configuration/#config-file-discovery) for more details.

To reduce the amount of times we resolve configuration for a file, we have an index for each workspace that stores a reference-counted pointer to a configuration for a given folder. If another file in the same folder is opened, the configuration is simply re-used rather than us re-resolving it.

## Guide for reviewing

The first commit is just the restructuring work, which adds some noise to the diff. If you want to quickly understand what's actually changed, I recommend looking at the two commits that come after it. f7c073d4412242dfe09fd620371432735ffa0c18 makes configuration a property of `DocumentController`/`DocumentRef`, moving it out of `Workspace`, and it also sets up the `ConfigurationIndex`, though it doesn't implement its key function, `get_or_insert`. In the commit after it, fc35618f17f761de170125f089ce7fc054df356b, we implement `get_or_insert`.

## Test Plan

The best way to test this would be to ensure that the behavior matches the Ruff CLI. Open a project with multiple configuration files (or add them yourself), and then introduce problems in certain files that won't show due to their configuration. Add those same problems to a section of the project where those rules are run. Confirm that the lint rules are run as expected with `ruff check`. Then, open your editor and confirm that the diagnostics shown match the CLI output.

As an example - I have a workspace with two separate folders, `pandas` and `scipy`. I created a `pyproject.toml` file in `pandas/pandas/io` and a `ruff.toml` file in `pandas/pandas/api`. I changed the `select` and `preview` settings in the sub-folder configuration files and confirmed that these were reflected in the diagnostics. I also confirmed that this did not change the diagnostics for the `scipy` folder whatsoever.

---

_Label `server` added by @snowsignal on 2024-04-15 08:43_

---

_Review requested from @MichaReiser by @snowsignal on 2024-04-15 08:43_

---

_Review requested from @dhruvmanila by @snowsignal on 2024-04-15 08:43_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/workspace/configuration.rs`:24 on 2024-04-15 08:55_

This is just an understand question. Could we return a reference here instead of cloning the configuration? 

---

_Comment by @github-actions[bot] on 2024-04-15 08:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/workspace/configuration.rs`:53 on 2024-04-15 09:04_

Calling `find_settings_toml` is probably fine for the LSP but it means that it traverses the entire ancestory chain for each directory once, and parse the settings. That means for projects with a single configuration file that we'll reparse (and store in meory) the same configuration once for every possible directory. 

I think I would prefer if we either:

* Discover all setting files during startup and setup the configuration index. The index only contains entries for directories that contain a configuration. Resolving a configuration is the same as before. Use the `BTreeMap` to find the closest directory that contains a configuration. I think that's my preferred solution because it also "just works" when deleting a configuration. All that is necessary is to remove (or add) the deleted (or added) configuration from the `ConfigurationsIndex` and it isn't necessary to invalidat any sub paths. It may also avoid the need for using an `Arc`.
* Or we change the logic here to iterate over the anchestor in the `ConfigurationIndex` starting at the current folder and see if it contains any configuration. If yes, parse it and set it in the Index. If not, go up the ancestory chain and check the parent directory (after testing that the index doesn't already contains an entry for this path).



---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/workspace/configuration.rs`:16 on 2024-04-15 09:05_

The naming here becomes slightly confusing. I can see why you would name it `Configuration` but we generally  use the term `Settings` for deserialized, merged and resolved "ruff configurations" and only use the term `Configuration` for deserialized configuration files (but not resolved).

---

_@MichaReiser reviewed on 2024-04-15 09:10_

I like the laziness of the current design but I think has the downside (as far as I understand) that we deserialize and store a resolved configuration for each directory. That's unlikely a huge memory issue. It's also unclear to me if the `ConfigurationIndex` gets correctly invalidated when:

* I open a file in a directory that inherits the configuration from the parent directory (the configuration is now stored in the index)
* I close the file
* I change the parent configuration (that reloads the configurations of all paths with open documents)
* I reopen the file (I suspect it reuses the old configuration)

I think there's a similar case where creating a new configuration in a specific directory must correctly propagate to all sub-directories). The easiest fix is to simply prune all sub-directory entries, but maybe we can do better?

I wonder if we should instead traverse the workspace eagerly and discovers all configurations or model the index in a way that it tracks whether a path has its own configuration or inherits from the parent (finding the configuration is then the same as finding the first entry where the configuration is not inherited).

---

_Comment by @dhruvmanila on 2024-04-15 16:54_

It's not uncommon for a language server to spend some time initially to setup the workspace. This could potentially include building up the configuration index as suggested by Micha but I'm ok with the lazy approach as well.

Thank you for providing the test plan. Can you expand it by listing down any/all the scenarios that you've tested against? This way I can make sure to not repeat it and come up with any other possible project structure to test this change against.

---

_Comment by @snowsignal on 2024-04-15 18:45_

> It's also unclear to me if the ConfigurationIndex gets correctly invalidated when:
> - I open a file in a directory that inherits the configuration from the parent directory (the configuration is now stored in the index)
> - I close the file
> - I change the parent configuration (that reloads the configurations of all paths with open documents)
> - I reopen the file (I suspect it reuses the old configuration)

It should be correctly invalidated - any time a configuration file gets changed, we clear the entire configuration index before we start the configuration reload (see [this line](https://github.com/astral-sh/ruff/pull/10950/files#diff-41e088f72fc9e84d5e26c2456d6ecd5e4c108bd438f037eef4c54eff185ea99dR207))

---

_Comment by @snowsignal on 2024-04-15 18:49_

> I wonder if we should instead traverse the workspace eagerly and discovers all configurations or model the index in a way that it tracks whether a path has its own configuration or inherits from the parent (finding the configuration is then the same as finding the first entry where the configuration is not inherited).

> It's not uncommon for a language server to spend some time initially to setup the workspace. This could potentially include building up the configuration index as suggested by Micha but I'm ok with the lazy approach as well.

I'm cool with the eager approach! It would definitely be more efficient.

---

_@snowsignal reviewed on 2024-04-15 18:51_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/workspace/configuration.rs`:24 on 2024-04-15 18:51_

We could - this is mostly a matter of convenience for the caller, since we immediately consume the return value where this is called.

---

_@snowsignal reviewed on 2024-04-15 18:58_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/workspace/configuration.rs`:24 on 2024-04-15 18:58_

I'm not against changing this to return a reference, though.

---

_Comment by @snowsignal on 2024-04-16 02:05_

> Thank you for providing the test plan. Can you expand it by listing down any/all the scenarios that you've tested against? This way I can make sure to not repeat it and come up with any other possible project structure to test this change against.

I've added them to the test plan!

---

_Review requested from @MichaReiser by @snowsignal on 2024-04-16 02:30_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/notifications/did_change_watched_files.rs`:22 on 2024-04-16 06:07_

nit: do we have any other kind of settings? If not, I'd drop the "ruff" word here to make it `reload_settings`

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/code_action.rs`:108 on 2024-04-16 06:07_

nit: same as above to drop the "ruff" here to make it `&snapshot.settings().linter`

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/workspace.rs`:214 on 2024-04-16 06:15_

nit: we could potentially drop `reload` and `clear` from the index and re-create the settings with the same method:

```diff
diff --git a/crates/ruff_server/src/session/workspace.rs b/crates/ruff_server/src/session/workspace.rs
index 10d2b8a56..068236f8c 100644
--- a/crates/ruff_server/src/session/workspace.rs
+++ b/crates/ruff_server/src/session/workspace.rs
@@ -210,7 +210,7 @@ impl OpenDocuments {
     }
 
     fn reload_ruff_settings(&mut self, root: &Path) {
-        self.ruff_settings_index.reload(root);
+        self.ruff_settings_index = RuffSettingsIndex::new(root);
     }
 }
 
diff --git a/crates/ruff_server/src/session/workspace/ruff_settings.rs b/crates/ruff_server/src/session/workspace/ruff_settings.rs
index 6bff2a975..376c850a5 100644
--- a/crates/ruff_server/src/session/workspace/ruff_settings.rs
+++ b/crates/ruff_server/src/session/workspace/ruff_settings.rs
@@ -38,20 +38,9 @@ impl std::fmt::Display for RuffSettings {
 
 impl RuffSettingsIndex {
     pub(super) fn new(path: &Path) -> Self {
-        let mut index = Self {
-            index: BTreeMap::default(),
-            fallback: Arc::default(),
-        };
+        let mut index = BTreeMap::default();
 
-        index.reload(path);
-
-        index
-    }
-
-    pub(super) fn reload(&mut self, root: &Path) {
-        self.clear();
-
-        for directory in WalkDir::new(root)
+        for directory in WalkDir::new(path)
             .into_iter()
             .filter_map(Result::ok)
             .filter(|entry| entry.file_type().is_dir())
@@ -65,7 +54,7 @@ impl RuffSettingsIndex {
                 ) else {
                     continue;
                 };
-                self.index.insert(
+                index.insert(
                     directory,
                     Arc::new(RuffSettings {
                         linter: settings.linter,
@@ -74,6 +63,11 @@ impl RuffSettingsIndex {
                 );
             }
         }
+
+        Self {
+            index,
+            fallback: Arc::default(),
+        }
     }
 
     pub(super) fn get(&self, document_path: &Path) -> &Arc<RuffSettings> {
@@ -90,10 +84,6 @@ impl RuffSettingsIndex {
 
         &self.fallback
     }
-
-    fn clear(&mut self) {
-        self.index.clear();
-    }
 }
 
 struct LSPConfigTransformer;
```

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/workspace/ruff_settings.rs`:79 on 2024-04-16 06:17_

Do we still need the `Arc` if we're returning a reference to `Settings`?

---

_@dhruvmanila reviewed on 2024-04-16 06:31_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/workspace/ruff_settings.rs`:91 on 2024-04-16 07:42_

Nit: Could we insert the fallback into the `index` with a path of `/` or `""`? Or do you prefer the explicit fallback to be able to log a warning if no configuration was found

---

_@MichaReiser approved on 2024-04-16 07:44_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/workspace/ruff_settings.rs`:89 on 2024-04-16 08:20_

I'm not sure if this should be a warning, I think it's fine to put this at info level, as users might just open an editor to write a standalone Python script instead of working in a project.

---

_@dhruvmanila approved on 2024-04-16 08:21_

---

_@snowsignal reviewed on 2024-04-16 17:55_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/workspace/ruff_settings.rs`:79 on 2024-04-16 17:55_

We do, since callers of this method need to create a new `Arc` from its return value. Maybe I should go back to return an owned value to make this more clear.

---

_@snowsignal reviewed on 2024-04-16 17:57_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/workspace/ruff_settings.rs`:91 on 2024-04-16 17:57_

I prefer the explicit fallback so that we can log about it 👍 

---

_Merged by @snowsignal on 2024-04-16 18:15_

---

_Closed by @snowsignal on 2024-04-16 18:15_

---

_Branch deleted on 2024-04-16 18:15_

---

_Added to milestone `Ruff Server: Beta` by @snowsignal on 2024-04-16 21:39_

---
