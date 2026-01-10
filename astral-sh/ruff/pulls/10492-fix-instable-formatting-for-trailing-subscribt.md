```yaml
number: 10492
title: Fix instable formatting for trailing subscribt end-of-line comment
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: subscript-trailing-end-of-line-comment-left-bracket
created_at: 2024-03-20T16:48:34Z
updated_at: 2024-03-20T17:12:11Z
url: https://github.com/astral-sh/ruff/pull/10492
synced_at: 2026-01-10T22:47:02Z
```

# Fix instable formatting for trailing subscribt end-of-line comment

---

_Pull request opened by @MichaReiser on 2024-03-20 16:48_

## Summary

This PR fixes an instability where formatting a subscribt 
where the `slice` is not an `ExprSlice` and it has a trailing end-of-line comment after its opening `[` required two formatting passes to be stable.

The fix is to associate the trailing end-of-line comment as dangling comment on `[` to preserve its position, similar to how Ruff does it for other parenthesized expressions. 
This also matches how trailing end-of-line subscript comments are handled when the `slice` is an `ExprSlice`.

Fixes https://github.com/astral-sh/ruff/issues/10355

## Versioning

Shipping this as part of a patch release is fine because:

* It fixes a stability issue
* It doesn't impact already formatted code because Ruff would already have moved to the comment to the end of the line (instead of preserving it)

## Test Plan

Added tests


---

_Review requested from @konstin by @MichaReiser on 2024-03-20 16:48_

---

_Label `bug` added by @MichaReiser on 2024-03-20 16:49_

---

_Label `formatter` added by @MichaReiser on 2024-03-20 16:49_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-03-20 16:52_

---

_@konstin approved on 2024-03-20 16:57_

---

_Comment by @github-actions[bot] on 2024-03-20 16:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+15 -5 lines in 3 files in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+15 -5 lines across 3 files)</summary>
<p>
<pre>ruff format --no-preview --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py</pre>
</p>
<p>

<a href='https://github.com/demisto/content/blob/426215f184c4e7476a38d883cd4d88feee40874b/Packs/ApiModules/Scripts/CoreIRApiModule/CoreIRApiModule_test.py#L770'>Packs/ApiModules/Scripts/CoreIRApiModule/CoreIRApiModule_test.py~L770</a>
```diff
 
     assert (
         "handle_outgoing_issue_closure Closing Remote incident incident_id=None with status resolved_security_testing"
-        in request_data_log.call_args[0][0]  # noqa: E501
+        in request_data_log.call_args[  # noqa: E501
+            0
+        ][0]
     )
 
 
```
<a href='https://github.com/demisto/content/blob/426215f184c4e7476a38d883cd4d88feee40874b/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L241'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py~L241</a>
```diff
                 dist = {k: v * 100 / total for k, v in chosen.items()}
                 dist_total[cluster_number] = {}
                 dist_total[cluster_number]["number_samples"] = sum(
-                    self.clustering.raw_data[self.clustering.model.labels_ == cluster_number].label.isin(  # type: ignore  # type: ignore
+                    self.clustering.raw_data[  # type: ignore
+                        self.clustering.model.labels_ == cluster_number
+                    ].label.isin(  # type: ignore
                         list(chosen.keys())
                     )
                 )  # type: ignore
```
<a href='https://github.com/demisto/content/blob/426215f184c4e7476a38d883cd4d88feee40874b/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L603'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py~L603</a>
```diff
             "pivot": "clusterId:" + str(cluster_number),
             "incidents_ids": [
                 x
-                for x in incidents_df[clustering.model.labels_ == cluster_number].id.values.tolist()  # type: ignore
+                for x in incidents_df[  # type: ignore
+                    clustering.model.labels_ == cluster_number
+                ].id.values.tolist()
             ],  # type: ignore
             "incidents": incidents_df[clustering.model.labels_ == cluster_number][  # type: ignore
                 display_fields + fields_for_clustering_remove_display
```
<a href='https://github.com/demisto/content/blob/426215f184c4e7476a38d883cd4d88feee40874b/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L617'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py~L617</a>
```diff
     d_outliers = {
         "incidents_ids": [
             x
-            for x in incidents_df[clustering.model.labels_ == -1].id.values.tolist()  # type: ignore
+            for x in incidents_df[  # type: ignore
+                clustering.model.labels_ == -1
+            ].id.values.tolist()
         ],  # type: ignore
         "incidents": incidents_df[clustering.model.labels_ == -1][display_fields].to_json(  # type: ignore
             orient="records"
```
<a href='https://github.com/demisto/content/blob/426215f184c4e7476a38d883cd4d88feee40874b/Packs/Orca/Integrations/Orca/Orca.py#L47'>Packs/Orca/Integrations/Orca/Orca.py~L47</a>
```diff
 
     def get_alerts_by_filter(
         self, alert_type: Optional[str] = None, asset_unique_id: Optional[str] = None, limit: int = 1000
-    ) -> Union[List[Dict[str, Any]], str]:  # pylint: disable=E1136 # noqa: E501  # pylint: disable=E1136 # noqa: E125
+    ) -> Union[  # pylint: disable=E1136 # noqa: E501
+        List[Dict[str, Any]], str
+    ]:  # pylint: disable=E1136 # noqa: E125
         demisto.info("get_alerts_by_filter, enter")
 
         url_suffix = "/alerts"
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+15 -5 lines in 3 files in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+15 -5 lines across 3 files)</summary>
<p>
<pre>ruff format --preview --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py</pre>
</p>
<p>

<a href='https://github.com/demisto/content/blob/426215f184c4e7476a38d883cd4d88feee40874b/Packs/ApiModules/Scripts/CoreIRApiModule/CoreIRApiModule_test.py#L768'>Packs/ApiModules/Scripts/CoreIRApiModule/CoreIRApiModule_test.py~L768</a>
```diff
 
     assert (
         "handle_outgoing_issue_closure Closing Remote incident incident_id=None with status resolved_security_testing"
-        in request_data_log.call_args[0][0]  # noqa: E501
+        in request_data_log.call_args[  # noqa: E501
+            0
+        ][0]
     )
 
 
```
<a href='https://github.com/demisto/content/blob/426215f184c4e7476a38d883cd4d88feee40874b/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L241'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py~L241</a>
```diff
                 dist = {k: v * 100 / total for k, v in chosen.items()}
                 dist_total[cluster_number] = {}
                 dist_total[cluster_number]["number_samples"] = sum(
-                    self.clustering.raw_data[self.clustering.model.labels_ == cluster_number].label.isin(  # type: ignore  # type: ignore
+                    self.clustering.raw_data[  # type: ignore
+                        self.clustering.model.labels_ == cluster_number
+                    ].label.isin(  # type: ignore
                         list(chosen.keys())
                     )
                 )  # type: ignore
```
<a href='https://github.com/demisto/content/blob/426215f184c4e7476a38d883cd4d88feee40874b/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L603'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py~L603</a>
```diff
             "pivot": "clusterId:" + str(cluster_number),
             "incidents_ids": [
                 x
-                for x in incidents_df[clustering.model.labels_ == cluster_number].id.values.tolist()  # type: ignore
+                for x in incidents_df[  # type: ignore
+                    clustering.model.labels_ == cluster_number
+                ].id.values.tolist()
             ],  # type: ignore
             "incidents": incidents_df[clustering.model.labels_ == cluster_number][  # type: ignore
                 display_fields + fields_for_clustering_remove_display
```
<a href='https://github.com/demisto/content/blob/426215f184c4e7476a38d883cd4d88feee40874b/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L617'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py~L617</a>
```diff
     d_outliers = {
         "incidents_ids": [
             x
-            for x in incidents_df[clustering.model.labels_ == -1].id.values.tolist()  # type: ignore
+            for x in incidents_df[  # type: ignore
+                clustering.model.labels_ == -1
+            ].id.values.tolist()
         ],  # type: ignore
         "incidents": incidents_df[clustering.model.labels_ == -1][display_fields].to_json(  # type: ignore
             orient="records"
```
<a href='https://github.com/demisto/content/blob/426215f184c4e7476a38d883cd4d88feee40874b/Packs/Orca/Integrations/Orca/Orca.py#L47'>Packs/Orca/Integrations/Orca/Orca.py~L47</a>
```diff
 
     def get_alerts_by_filter(
         self, alert_type: Optional[str] = None, asset_unique_id: Optional[str] = None, limit: int = 1000
-    ) -> Union[List[Dict[str, Any]], str]:  # pylint: disable=E1136 # noqa: E501  # pylint: disable=E1136 # noqa: E125
+    ) -> Union[  # pylint: disable=E1136 # noqa: E501
+        List[Dict[str, Any]], str
+    ]:  # pylint: disable=E1136 # noqa: E125
         demisto.info("get_alerts_by_filter, enter")
 
         url_suffix = "/alerts"
```

</p>
</details>




---

_Comment by @MichaReiser on 2024-03-20 17:09_

Hmm, I was wrong. I need to look into the ecosystem changes. 

Never mind. These are unformatted projects and the ecosystem changes show that the comment position is now preserved

---

_Merged by @MichaReiser on 2024-03-20 17:12_

---

_Closed by @MichaReiser on 2024-03-20 17:12_

---

_Branch deleted on 2024-03-20 17:12_

---
