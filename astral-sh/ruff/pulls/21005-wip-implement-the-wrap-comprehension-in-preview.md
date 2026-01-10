```yaml
number: 21005
title: "[WIP] Implement the `wrap_comprehension_in` preview style from Black"
type: pull_request
state: closed
author: ntBre
labels:
  - formatter
  - preview
assignees: []
draft: true
base: main
head: brent/wrap-comprehension-in
created_at: 2025-10-20T21:27:32Z
updated_at: 2025-10-24T13:09:15Z
url: https://github.com/astral-sh/ruff/pull/21005
synced_at: 2026-01-10T17:34:34Z
```

# [WIP] Implement the `wrap_comprehension_in` preview style from Black

---

_Pull request opened by @ntBre on 2025-10-20 21:27_

This is a rough draft with a naive fix. We still disagree with Black on some formatting, for example this snippet from our docstring formatting tests:

```py
def by_first_letter_of_column_values(self, col: str) -> list[pl.DataFrame]:
    return [
        self._df.filter(pl.col(col).str.starts_with(c))
        for c in sorted(set(df.select(pl.col(col).str.slice(0, 1)).to_series()))
    ]
```

Black reuses the parentheses from the `sorted` call instead of adding new parentheses around the whole thing, which seems preferable.

**Black**:

```py
def by_first_letter_of_column_values(self, col: str) -> list[pl.DataFrame]:
    return [
        self._df.filter(pl.col(col).str.starts_with(c))
        for c in sorted(
            set(df.select(pl.col(col).str.slice(0, 1)).to_series())
        )
    ]
```

**This PR**:
```py
def by_first_letter_of_column_values(self, col: str) -> list[pl.DataFrame]:
    return [
        self._df.filter(pl.col(col).str.starts_with(c))
        for c in (
            sorted(set(df.select(pl.col(col).str.slice(0, 1)).to_series()))
        )
    ]
```

I can't quite tell if I'm having trouble here because this is tricky to implement in Ruff as Dylan mentioned [here](https://github.com/astral-sh/ruff/issues/20482#issuecomment-3340449328), or if I'm still just unfamiliar with the formatter.

Summary
--

This PR implements the `wrap_comprehension_in` style added in
https://github.com/psf/black/pull/4699. This wraps `in` clauses in
comprehensions if they get too long. Using some examples from the upstream
issue, this code:

```py
[a for graph_path_expression in refined_constraint.condition_as_predicate.variables]

[
    a
    for graph_path_expression
    in refined_constraint.condition_as_predicate.variables
]
```

is currently formatted to:

```py
[
    a
    for graph_path_expression in refined_constraint.condition_as_predicate.variables
]

[
    a
    for graph_path_expression in refined_constraint.condition_as_predicate.variables
]
```

even if the second line of the comprehension exceeds the configured line length.

In preview, black will now break these lines by parenthesizing the expression
following `in`:

```py
[
    a
    for graph_path_expression in (
        refined_constraint.condition_as_predicate.variables
    )
]

[
    a
    for graph_path_expression in (
        refined_constraint.condition_as_predicate.variables
    )
]
```

I actually kind of like the alternative formatting mentioned on the original
Black issue and in our #12870 which would be more like:

```py
[
    a
    for graph_path_expression
	in refined_constraint.condition_as_predicate.variables
]
```

but I think I'm in the minority there.

Test Plan
--

Existing Black compatibility tests showing fewer differences


---

_Comment by @github-actions[bot] on 2025-10-20 21:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+449 -544 lines in 95 files in 24 projects; 31 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+21 -25 lines across 5 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/policies/ted_policy.py#L1009'>rasa/core/policies/ted_policy.py~L1009</a>
```diff
         entity_tag_specs = [
             EntityTagSpec(
                 tag_name=tag_spec["tag_name"],
-                ids_to_tags={
-                    int(key): value for key, value in tag_spec["ids_to_tags"].items()
-                },
-                tags_to_ids={
-                    key: int(value) for key, value in tag_spec["tags_to_ids"].items()
-                },
+                ids_to_tags={int(key): value for key, value in tag_spec[
+                        "ids_to_tags"
+                    ].items()},
+                tags_to_ids={key: int(value) for key, value in tag_spec[
+                        "tags_to_ids"
+                    ].items()},
                 num_tags=tag_spec["num_tags"],
             )
             for tag_spec in entity_tag_specs
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/classifiers/diet_classifier.py#L1195'>rasa/nlu/classifiers/diet_classifier.py~L1195</a>
```diff
         entity_tag_specs = [
             EntityTagSpec(
                 tag_name=tag_spec["tag_name"],
-                ids_to_tags={
-                    int(key): value for key, value in tag_spec["ids_to_tags"].items()
-                },
-                tags_to_ids={
-                    key: int(value) for key, value in tag_spec["tags_to_ids"].items()
-                },
+                ids_to_tags={int(key): value for key, value in tag_spec[
+                        "ids_to_tags"
+                    ].items()},
+                tags_to_ids={key: int(value) for key, value in tag_spec[
+                        "tags_to_ids"
+                    ].items()},
                 num_tags=tag_spec["num_tags"],
             )
             for tag_spec in entity_tag_specs
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/domain.py#L1672'>rasa/shared/core/domain.py~L1672</a>
```diff
 
         def get_duplicates(my_items: Iterable[Any]) -> List[Any]:
             """Returns a list of duplicate items in my_items."""
-            return [
-                item
-                for item, count in collections.Counter(my_items).items()
-                if count > 1
-            ]
+            return [item for item, count in collections.Counter(
+                    my_items
+                ).items() if count > 1]
 
         def check_mappings(
             intent_properties: Dict[Text, Dict[Text, Union[bool, List]]],
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/generator.py#L141'>rasa/shared/core/generator.py~L141</a>
```diff
 
     @staticmethod
     def _unfreeze_states(frozen_states: Deque[FrozenState]) -> List[State]:
-        return [
-            {key: dict(value) for key, value in dict(frozen_state).items()}
-            for frozen_state in frozen_states
-        ]
+        return [{key: dict(value) for key, value in dict(
+                    frozen_state
+                ).items()} for frozen_state in frozen_states]
 
     def past_states(
         self,
```
<a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/test_broker.py#L163'>tests/core/test_broker.py~L163</a>
```diff
         actual.publish(e.as_dict())
 
     with actual.session_scope() as session:
-        events_types = [
-            json.loads(event.data)["event"]
-            for event in session.query(actual.SQLBrokerEvent).all()
-        ]
+        events_types = [json.loads(event.data)["event"] for event in session.query(
+                actual.SQLBrokerEvent
+            ).all()]
 
     assert events_types == ["user", "slot", "restart"]
 
```

</p>
</details>
<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+6 -9 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/Snowflake-Labs/snowcli/blob/8ddec6c828bca5c9e4105e4bc6ffdcf94b3fe4cf/src/snowflake/cli/_app/snow_connector.py#L123'>src/snowflake/cli/_app/snow_connector.py~L123</a>
```diff
 
     connection_parameters = {}
     if connection_name:
-        connection_parameters = {
-            _resolve_alias(k): v
-            for k, v in get_connection_dict(connection_name).items()
-        }
+        connection_parameters = {_resolve_alias(k): v for k, v in get_connection_dict(
+                connection_name
+            ).items()}
 
     elif temporary_connection:
         connection_parameters = {}  # we will apply overrides in next step
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/8ddec6c828bca5c9e4105e4bc6ffdcf94b3fe4cf/src/snowflake/cli/api/config.py#L111'>src/snowflake/cli/api/config.py~L111</a>
```diff
         return cls(**known_settings, _other_settings=other_settings)
 
     def to_dict_of_known_non_empty_values(self) -> dict:
-        return {
-            k: v
-            for k, v in asdict(self).items()
-            if k != "_other_settings" and v is not None
-        }
+        return {k: v for k, v in asdict(
+                self
+            ).items() if k != "_other_settings" and v is not None}
 
     def _non_empty_other_values(self) -> dict:
         return {k: v for k, v in self._other_settings.items() if v is not None}
```

</p>
</details>
<details><summary><a href="https://github.com/alteryx/featuretools">alteryx/featuretools</a> (+9 -11 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/alteryx/featuretools/blob/938a0f6ccb98eaf21eca89830a25be2358a75db7/featuretools/feature_base/feature_base.py#L612'>featuretools/feature_base/feature_base.py~L612</a>
```diff
         return self._name_from_base(self.base_features[0].get_name())
 
     def generate_names(self):
-        return [
-            self._name_from_base(base_name)
-            for base_name in self.base_features[0].get_feature_names()
-        ]
+        return [self._name_from_base(base_name) for base_name in self.base_features[
+                0
+            ].get_feature_names()]
 
     def get_arguments(self):
         _is_forward, relationship = self.relationship_path[0]
```
<a href='https://github.com/alteryx/featuretools/blob/938a0f6ccb98eaf21eca89830a25be2358a75db7/featuretools/synthesis/deep_feature_synthesis.py#L607'>featuretools/synthesis/deep_feature_synthesis.py~L607</a>
```diff
                 return True
             return False
 
-        for feat in [
-            f for f in all_features[dataframe.ww.name].values() if is_valid_feature(f)
-        ]:
+        for feat in [f for f in all_features[
+                dataframe.ww.name
+            ].values() if is_valid_feature(f)]:
             # Get interesting_values from the EntitySet that was passed, which
             # is assumed to be the most recent version of the EntitySet.
             # Features can contain a stale EntitySet reference without
```
<a href='https://github.com/alteryx/featuretools/blob/938a0f6ccb98eaf21eca89830a25be2358a75db7/featuretools/synthesis/deep_feature_synthesis.py#L921'>featuretools/synthesis/deep_feature_synthesis.py~L921</a>
```diff
             return [feature]
 
         # Build the complete list of features prior to processing
-        selected_features = [
-            expand_features(feature)
-            for feature in all_features[dataframe.ww.name].values()
-        ]
+        selected_features = [expand_features(feature) for feature in all_features[
+                dataframe.ww.name
+            ].values()]
         selected_features = functools.reduce(operator.iconcat, selected_features, [])
 
         column_schemas = column_schemas if column_schemas else set()
```

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+37 -42 lines across 7 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/apache/airflow/blob/1c62f7541a6efa365c62a0af68047bc2373827a0/airflow-core/src/airflow/serialization/serialized_objects.py#L2426'>airflow-core/src/airflow/serialization/serialized_objects.py~L2426</a>
```diff
         param_to_attr = {
             "description": "_description",
         }
-        return {
-            param_to_attr.get(k, k): v.default
-            for k, v in signature(DAG.__init__).parameters.items()
-            if v.default is not v.empty
-        }
+        return {param_to_attr.get(k, k): v.default for k, v in signature(
+                DAG.__init__
+            ).parameters.items() if v.default is not v.empty}
 
     _CONSTRUCTOR_PARAMS = __get_constructor_defaults.__func__()  # type: ignore
     del __get_constructor_defaults
```
<a href='https://github.com/apache/airflow/blob/1c62f7541a6efa365c62a0af68047bc2373827a0/airflow-core/tests/unit/always/test_secrets_local_filesystem.py#L432'>airflow-core/tests/unit/always/test_secrets_local_filesystem.py~L432</a>
```diff
     def test_yaml_extension_parsers_return_same_result(self, file_content):
         with mock_local_file(file_content):
             conn_uri_by_conn_id_yaml = {
-                conn_id: conn.get_uri()
-                for conn_id, conn in local_filesystem.load_connections_dict("a.yaml").items()
+                conn_id: conn.get_uri() for conn_id, conn in local_filesystem.load_connections_dict(
+                    "a.yaml"
+                ).items()
             }
             conn_uri_by_conn_id_yml = {
-                conn_id: conn.get_uri()
-                for conn_id, conn in local_filesystem.load_connections_dict("a.yml").items()
+                conn_id: conn.get_uri() for conn_id, conn in local_filesystem.load_connections_dict(
+                    "a.yml"
+                ).items()
             }
             assert conn_uri_by_conn_id_yaml == conn_uri_by_conn_id_yml
 
```
<a href='https://github.com/apache/airflow/blob/1c62f7541a6efa365c62a0af68047bc2373827a0/airflow-core/tests/unit/jobs/test_scheduler_job.py#L2206'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L2206</a>
```diff
         assert [x.queued_dttm for x in tis] == [None, None]
 
         _queue_tasks(tis=tis)
-        log_events = [
-            x.event for x in session.scalars(select(Log).where(Log.run_id == run_id).order_by(Log.id)).all()
-        ]
+        log_events = [x.event for x in session.scalars(
+                select(Log).where(Log.run_id == run_id).order_by(Log.id)
+            ).all()]
         assert log_events == [
             "stuck in queued reschedule",
             "stuck in queued reschedule",
```
<a href='https://github.com/apache/airflow/blob/1c62f7541a6efa365c62a0af68047bc2373827a0/airflow-core/tests/unit/jobs/test_scheduler_job.py#L2217'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L2217</a>
```diff
         with _loader_mock(mock_executors):
             scheduler._handle_tasks_stuck_in_queued()
 
-        log_events = [
-            x.event for x in session.scalars(select(Log).where(Log.run_id == run_id).order_by(Log.id)).all()
-        ]
+        log_events = [x.event for x in session.scalars(
+                select(Log).where(Log.run_id == run_id).order_by(Log.id)
+            ).all()]
         assert log_events == [
             "stuck in queued reschedule",
             "stuck in queued reschedule",
```
<a href='https://github.com/apache/airflow/blob/1c62f7541a6efa365c62a0af68047bc2373827a0/airflow-core/tests/unit/jobs/test_scheduler_job.py#L2233'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L2233</a>
```diff
 
         with _loader_mock(mock_executors):
             scheduler._handle_tasks_stuck_in_queued()
-        log_events = [
-            x.event for x in session.scalars(select(Log).where(Log.run_id == run_id).order_by(Log.id)).all()
-        ]
+        log_events = [x.event for x in session.scalars(
+                select(Log).where(Log.run_id == run_id).order_by(Log.id)
+            ).all()]
         assert log_events == [
             "stuck in queued reschedule",
             "stuck in queued reschedule",
```
<a href='https://github.com/apache/airflow/blob/1c62f7541a6efa365c62a0af68047bc2373827a0/airflow-core/tests/unit/jobs/test_scheduler_job.py#L2297'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L2297</a>
```diff
         assert [x.queued_dttm for x in tis] == [None, None]
 
         _queue_tasks(tis=tis)
-        log_events = [
-            x.event for x in session.scalars(select(Log).where(Log.run_id == run_id).order_by(Log.id)).all()
-        ]
+        log_events = [x.event for x in session.scalars(
+                select(Log).where(Log.run_id == run_id).order_by(Log.id)
+            ).all()]
         assert log_events == [
             "stuck in queued reschedule",
             "stuck in queued reschedule",
```
<a href='https://github.com/apache/airflow/blob/1c62f7541a6efa365c62a0af68047bc2373827a0/airflow-core/tests/unit/jobs/test_scheduler_job.py#L2308'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L2308</a>
```diff
         with _loader_mock(mock_executors):
             scheduler._handle_tasks_stuck_in_queued()
 
-        log_events = [
-            x.event for x in session.scalars(select(Log).where(Log.run_id == run_id).order_by(Log.id)).all()
-        ]
+        log_events = [x.event for x in session.scalars(
+                select(Log).where(Log.run_id == run_id).order_by(Log.id)
+            ).all()]
         assert log_events == [
             "stuck in queued reschedule",
             "stuck in queued reschedule",
```
<a href='https://github.com/apache/airflow/blob/1c62f7541a6efa365c62a0af68047bc2373827a0/airflow-core/tests/unit/jobs/test_scheduler_job.py#L2329'>airflow-core/tests/unit/jobs/test_scheduler_job.py~L2329</a>
```diff
                 scheduler._handle_tasks_stuck_in_queued()
             tis = dr.get_task_instances(session=session)
 
-        log_events = [
-            x.event for x in session.scalars(select(Log).where(Log.run_id == run_id).order_by(Log.id)).all()
-        ]
+        log_events = [x.event for x in session.scalars(
+                select(Log).where(Log.run_id == run_id).order_by(Log.id)
+            ).all()]
         assert log_events == [
             "stuck in queued reschedule",
             "stuck in queued reschedule",
```
<a href='https://github.com/apache/airflow/blob/1c62f7541a6efa365c62a0af68047bc2373827a0/airflow-core/tests/unit/models/test_dag.py#L3239'>airflow-core/tests/unit/models/test_dag.py~L3239</a>
```diff
             t1 = my_teardown()
             s1 >> w1 >> t1
             s1 >> t1
-        assert {
-            x.task_id
-            for x in dag.partial_subset(
+        assert {x.task_id for x in dag.partial_subset(
                 "my_setup", include_upstream=upstream, include_downstream=downstream
-            ).tasks
-        } == expected
+            ).tasks} == expected
 
     def test_get_flat_relative_ids_two_tasks_diff_setup_teardowns_deeper(self):
         with DAG(dag_id="test_dag", schedule=None, start_date=pendulum.now()) as dag:
```
<a href='https://github.com/apache/airflow/blob/1c62f7541a6efa365c62a0af68047bc2373827a0/devel-common/src/docs/utils/conf_constants.py#L68'>devel-common/src/docs/utils/conf_constants.py~L68</a>
```diff
 
 
 def get_rst_epilogue(package_version: str, airflow_core: bool) -> str:
-    return "\n".join(
-        f".. |{key}| replace:: {replace}"
-        for key, replace in get_global_substitutions(package_version, airflow_core).items()
-    )
+    return "\n".join(f".. |{key}| replace:: {replace}" for key, replace in get_global_substitutions(
+            package_version, airflow_core
+        ).items())
 
 
 SMARTQUOTES_EXCLUDES = {"builders": ["man", "text", "spelling"]}
```
<a href='https://github.com/apache/airflow/blob/1c62f7541a6efa365c62a0af68047bc2373827a0/providers/fab/src/airflow/providers/fab/auth_manager/security_manager/override.py#L2415'>providers/fab/src/airflow/providers/fab/auth_manager/security_manager/override.py~L2415</a>
```diff
 
     def _get_all_roles_with_permissions(self) -> dict[str, Role]:
         """Return a dict with a key of role name and value of role with early loaded permissions."""
-        return {
-            r.name: r
-            for r in self.session.scalars(
+        return {r.name: r for r in self.session.scalars(
                 select(self.role_model).options(joinedload(self.role_model.permissions))
-            ).unique()
-        }
+            ).unique()}
 
     def _get_all_non_dag_permissions(self) -> dict[tuple[str, str], Permission]:
         """
```
<a href='https://github.com/apache/airflow/blob/1c62f7541a6efa365c62a0af68047bc2373827a0/providers/google/src/airflow/providers/google/cloud/openlineage/mixins.py#L473'>providers/google/src/airflow/providers/google/cloud/openlineage/mixins.py~L473</a>
```diff
         """Extract column names from a dataset's schema."""
         return [
             f.name
-            for f in dataset.facets.get("schema", SchemaDatasetFacet(fields=[])).fields  # type: ignore[union-attr]
+            for f in (
+                dataset.facets.get("schema", SchemaDatasetFacet(fields=[])).fields  # type: ignore[union-attr]
+            )
             if dataset.facets
         ]
 
```

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+71 -62 lines across 12 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/apache/superset/blob/f165785003eeb51603b3be64e9303b332a53c7e7/superset/commands/dashboard/importers/v1/utils.py#L82'>superset/commands/dashboard/importers/v1/utils.py~L82</a>
```diff
         # in filter_scopes the key is the chart ID as a string; we need to update
         # them to be the new ID as a string:
         metadata["filter_scopes"] = {
-            str(id_map[int(old_id)]): columns
-            for old_id, columns in metadata["filter_scopes"].items()
-            if int(old_id) in id_map
+            str(id_map[int(old_id)]): columns for old_id, columns in metadata[
+                "filter_scopes"
+            ].items() if int(old_id) in id_map
         }
 
         # now update columns to use new IDs:
```
<a href='https://github.com/apache/superset/blob/f165785003eeb51603b3be64e9303b332a53c7e7/superset/commands/dashboard/importers/v1/utils.py#L98'>superset/commands/dashboard/importers/v1/utils.py~L98</a>
```diff
 
     if "expanded_slices" in metadata:
         metadata["expanded_slices"] = {
-            str(id_map[int(old_id)]): value
-            for old_id, value in metadata["expanded_slices"].items()
+            str(id_map[int(old_id)]): value for old_id, value in metadata[
+                "expanded_slices"
+            ].items()
         }
 
     if "default_filters" in metadata:
```
<a href='https://github.com/apache/superset/blob/f165785003eeb51603b3be64e9303b332a53c7e7/superset/common/query_context_processor.py#L148'>superset/common/query_context_processor.py~L148</a>
```diff
             try:
                 if invalid_columns := [
                     col
-                    for col in get_column_names_from_columns(query_obj.columns)
-                    + get_column_names_from_metrics(query_obj.metrics or [])
+                    for col in get_column_names_from_columns(
+                        query_obj.columns
+                    ) + get_column_names_from_metrics(query_obj.metrics or [])
                     if (
                         col not in self._qc_datasource.column_names
                         and col != DTTM_ALIAS
```
<a href='https://github.com/apache/superset/blob/f165785003eeb51603b3be64e9303b332a53c7e7/superset/daos/tag.py#L318'>superset/daos/tag.py~L318</a>
```diff
         ids = [tag.id for tag in tags]
         return [
             star.tag_id
-            for star in db.session.query(user_favorite_tag_table.c.tag_id)
-            .filter(
-                user_favorite_tag_table.c.tag_id.in_(ids),
-                user_favorite_tag_table.c.user_id == get_user_id(),
+            for star in (
+                db.session.query(user_favorite_tag_table.c.tag_id)
+                .filter(
+                    user_favorite_tag_table.c.tag_id.in_(ids),
+                    user_favorite_tag_table.c.user_id == get_user_id(),
+                )
+                .all()
             )
-            .all()
         ]
 
     @staticmethod
```
<a href='https://github.com/apache/superset/blob/f165785003eeb51603b3be64e9303b332a53c7e7/superset/migrations/shared/native_filters.py#L264'>superset/migrations/shared/native_filters.py~L264</a>
```diff
                                 child["cascadeParentIds"].append(parent["id"])
 
     return sorted(
-        [
-            fltr
-            for key in filter_by_key_and_field
-            for fltr in filter_by_key_and_field[key].values()
-        ],
+        [fltr for key in filter_by_key_and_field for fltr in filter_by_key_and_field[
+                key
+            ].values()],
         key=lambda fltr: fltr["filterType"],
     )
 
```
<a href='https://github.com/apache/superset/blob/f165785003eeb51603b3be64e9303b332a53c7e7/superset/models/helpers.py#L214'>superset/models/helpers.py~L214</a>
```diff
         """Get all (single column and multi column) unique constraints"""
         unique = [
             {c.name for c in u.columns}
-            for u in cls.__table_args__  # type: ignore
+            for u in (
+                cls.__table_args__  # type: ignore
+            )
             if isinstance(u, UniqueConstraint)
         ]
         unique.extend({c.name} for c in cls.__table__.columns if c.unique)  # type: ignore
```
<a href='https://github.com/apache/superset/blob/f165785003eeb51603b3be64e9303b332a53c7e7/superset/models/helpers.py#L251'>superset/models/helpers.py~L251</a>
```diff
 
         schema: dict[str, Any] = {
             column.name: formatter(column)
-            for column in cls.__table__.columns  # type: ignore
+            for column in (
+                cls.__table__.columns  # type: ignore
+            )
             if (column.name in cls.export_fields and column.name not in parent_excludes)
         }
         if recursive:
```
<a href='https://github.com/apache/superset/blob/f165785003eeb51603b3be64e9303b332a53c7e7/superset/models/helpers.py#L403'>superset/models/helpers.py~L403</a>
```diff
                 parent_excludes = {c.name for c in parent_ref.local_columns}
         dict_rep = {
             c.name: getattr(self, c.name)
-            for c in cls.__table__.columns  # type: ignore
+            for c in (
+                cls.__table__.columns  # type: ignore
+            )
             if (
                 c.name in export_fields
                 and c.name not in parent_excludes
```
<a href='https://github.com/apache/superset/blob/f165785003eeb51603b3be64e9303b332a53c7e7/superset/models/helpers.py#L694'>superset/models/helpers.py~L694</a>
```diff
 
     table = target.__table__
     primary_keys = table.primary_key.columns.keys()
-    data = {
-        attr: getattr(target, attr)
-        for attr in list(table.columns.keys()) + (keep_relations or [])
-        if attr not in primary_keys and attr not in ignore
-    }
+    data = {attr: getattr(target, attr) for attr in list(table.columns.keys()) + (
+            keep_relations or []
+        ) if attr not in primary_keys and attr not in ignore}
     data.update(kwargs)
 
     return target.__class__(**data)
```
<a href='https://github.com/apache/superset/blob/f165785003eeb51603b3be64e9303b332a53c7e7/superset/security/manager.py#L633'>superset/security/manager.py~L633</a>
```diff
             and (
                 drillable_columns := {
                     row[0]
-                    for row in self.session.query(TableColumn.column_name)
-                    .filter(TableColumn.table_id == datasource.id)
-                    .filter(TableColumn.groupby)
-                    .all()
+                    for row in (
+                        self.session.query(TableColumn.column_name)
+                        .filter(TableColumn.table_id == datasource.id)
+                        .filter(TableColumn.groupby)
+                        .all()
+                    )
                 }
             )
             and set(dimensions).issubset(drillable_columns)
```
<a href='https://github.com/apache/superset/blob/f165785003eeb51603b3be64e9303b332a53c7e7/superset/views/core.py#L736'>superset/views/core.py~L736</a>
```diff
                 [
                     {
                         "slice_id" if key == "chart_id" else key: value
-                        for key, value in ChartWarmUpCacheCommand(
-                            slc, dashboard_id, extra_filters
+                        for key, value in (
+                            ChartWarmUpCacheCommand(slc, dashboard_id, extra_filters)
+                            .run()
+                            .items()
                         )
-                        .run()
-                        .items()
                     }
                     for slc in slices
                 ],
```
<a href='https://github.com/apache/superset/blob/f165785003eeb51603b3be64e9303b332a53c7e7/superset/views/datasource/views.py#L101'>superset/views/datasource/views.py~L101</a>
```diff
             datasource_dict["owners"], default_to_user=False
         )
 
-        duplicates = [
-            name
-            for name, count in Counter([
+        duplicates = [name for name, count in Counter([
                 col["column_name"] for col in datasource_dict["columns"]
-            ]).items()
-            if count > 1
-        ]
+            ]).items() if count > 1]
         if duplicates:
             return json_error_response(
                 _(
```
<a href='https://github.com/apache/superset/blob/f165785003eeb51603b3be64e9303b332a53c7e7/superset/viz.py#L562'>superset/viz.py~L562</a>
```diff
             try:
                 invalid_columns = [
                     col
-                    for col in get_column_names_from_columns(
-                        query_obj.get("columns") or []
-                    )
-                    + get_column_names_from_columns(query_obj.get("groupby") or [])
-                    + utils.get_column_names_from_metrics(
-                        cast(list[Metric], query_obj.get("metrics") or [])
+                    for col in (
+                        get_column_names_from_columns(query_obj.get("columns") or [])
+                        + get_column_names_from_columns(query_obj.get("groupby") or [])
+                        + utils.get_column_names_from_metrics(
+                            cast(list[Metric], query_obj.get("metrics") or [])
+                        )
                     )
                     if col not in self.datasource.column_names
                 ]
```
<a href='https://github.com/apache/superset/blob/f165785003eeb51603b3be64e9303b332a53c7e7/superset/viz.py#L2762'>superset/viz.py~L2762</a>
```diff
             dims = ()
         if level == -1:
             return [
-                {"name": m, "children": self.nest_procs(procs, 0, (m,))}
-                for m in procs[0].columns
+                {"name": m, "children": self.nest_procs(procs, 0, (m,))} for m in procs[
+                    0
+                ].columns
             ]
         if not level:
             return [
```
<a href='https://github.com/apache/superset/blob/f165785003eeb51603b3be64e9303b332a53c7e7/tests/integration_tests/charts/api_tests.py#L1545'>tests/integration_tests/charts/api_tests.py~L1545</a>
```diff
         admin = self.get_user("admin")
         users_favorite_ids = [
             star.obj_id
-            for star in db.session.query(FavStar.obj_id)
-            .filter(
-                and_(
-                    FavStar.user_id == admin.id,
-                    FavStar.class_name == FavStarClassName.CHART,
+            for star in (
+                db.session.query(FavStar.obj_id)
+                .filter(
+                    and_(
+                        FavStar.user_id == admin.id,
+                        FavStar.class_name == FavStarClassName.CHART,
+                    )
                 )
+                .all()
             )
-            .all()
         ]
 
         assert users_favorite_ids
```
<a href='https://github.com/apache/superset/blob/f165785003eeb51603b3be64e9303b332a53c7e7/tests/integration_tests/dashboards/api_tests.py#L847'>tests/integration_tests/dashboards/api_tests.py~L847</a>
```diff
         admin = self.get_user("admin")
         users_favorite_ids = [
             star.obj_id
-            for star in db.session.query(FavStar.obj_id)
-            .filter(
-                and_(
-                    FavStar.user_id == admin.id,
-                    FavStar.class_name == FavStarClassName.DASHBOARD,
+            for star in (
+                db.session.query(FavStar.obj_id)
+                .filter(
+                    and_(
+                        FavStar.user_id == admin.id,
+                        FavStar.class_name == FavStarClassName.DASHBOARD,
+                    )
                 )
+                .all()
             )
-            .all()
         ]
 
         assert users_favorite_ids
```
<a href='https://github.com/apache/superset/blob/f165785003eeb51603b3be64e9303b332a53c7e7/tests/integration_tests/datasets/api_tests.py#L440'>tests/integration_tests/datasets/api_tests.py~L440</a>
```diff
             },
         }
         if response["result"]["database"]["backend"] not in ("presto", "hive"):
-            assert {
-                k: v for k, v in response["result"].items() if k in expected_result
-            } == expected_result
+            assert {k: v for k, v in response[
+                    "result"
+                ].items() if k in expected_result} == expected_result
         assert len(response["result"]["columns"]) == 3
         assert len(response["result"]["metrics"]) == 2
 
```

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+6 -14 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Document_Optimize.py#L770'>crazy_functions/Document_Optimize.py~L770</a>
```diff
 
         # 过滤支持的文件格式
         file_paths = [
-            f
-            for f in file_paths
-            if any(
-                f.lower().endswith(ext)
-                for ext in list(processor.paper_extractor.SUPPORTED_EXTENSIONS)
-                + [".json", ".csv", ".xlsx", ".xls"]
-            )
+            f for f in file_paths if any(f.lower().endswith(ext) for ext in list(
+                    processor.paper_extractor.SUPPORTED_EXTENSIONS
+                ) + [".json", ".csv", ".xlsx", ".xls"])
         ]
 
     if not file_paths:
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/paper_fns/reduce_aigc.py#L977'>crazy_functions/paper_fns/reduce_aigc.py~L977</a>
```diff
 
         # 过滤支持的文件格式
         file_paths = [
-            f
-            for f in file_paths
-            if any(
-                f.lower().endswith(ext)
-                for ext in list(processor.paper_extractor.SUPPORTED_EXTENSIONS)
-                + [".json", ".csv", ".xlsx", ".xls"]
-            )
+            f for f in file_paths if any(f.lower().endswith(ext) for ext in list(
+                    processor.paper_extractor.SUPPORTED_EXTENSIONS
+                ) + [".json", ".csv", ".xlsx", ".xls"])
         ]
 
     if not file_paths:
```

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+12 -13 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/freedomofpress/securedrop/blob/9d136902700bfccfd64225a97cbe3770c2cd1c6a/securedrop/models.py#L874'>securedrop/models.py~L874</a>
```diff
 
         # For seen indicators, we need to make sure one doesn't already exist
         # otherwise it'll hit a unique key conflict
-        already_seen_files = {
-            file.file_id for file in SeenFile.query.filter_by(journalist_id=deleted.id).all()
-        }
+        already_seen_files = {file.file_id for file in SeenFile.query.filter_by(
+                journalist_id=deleted.id
+            ).all()}
         for file in SeenFile.query.filter_by(journalist_id=self.id).all():
             if file.file_id in already_seen_files:
                 db.session.delete(file)
```
<a href='https://github.com/freedomofpress/securedrop/blob/9d136902700bfccfd64225a97cbe3770c2cd1c6a/securedrop/models.py#L884'>securedrop/models.py~L884</a>
```diff
                 file.journalist_id = deleted.id
                 db.session.add(file)
 
-        already_seen_messages = {
-            message.message_id
-            for message in SeenMessage.query.filter_by(journalist_id=deleted.id).all()
-        }
+        already_seen_messages = {message.message_id for message in SeenMessage.query.filter_by(
+                journalist_id=deleted.id
+            ).all()}
         for message in SeenMessage.query.filter_by(journalist_id=self.id).all():
             if message.message_id in already_seen_messages:
                 db.session.delete(message)
```
<a href='https://github.com/freedomofpress/securedrop/blob/9d136902700bfccfd64225a97cbe3770c2cd1c6a/securedrop/models.py#L895'>securedrop/models.py~L895</a>
```diff
                 message.journalist_id = deleted.id
                 db.session.add(message)
 
-        already_seen_replies = {
-            reply.reply_id for reply in SeenReply.query.filter_by(journalist_id=deleted.id).all()
-        }
+        already_seen_replies = {reply.reply_id for reply in SeenReply.query.filter_by(
+                journalist_id=deleted.id
+            ).all()}
         for reply in SeenReply.query.filter_by(journalist_id=self.id).all():
             if reply.reply_id in already_seen_replies:
                 db.session.delete(reply)
```
<a href='https://github.com/freedomofpress/securedrop/blob/9d136902700bfccfd64225a97cbe3770c2cd1c6a/securedrop/tests/test_journalist_api.py#L432'>securedrop/tests/test_journalist_api.py~L432</a>
```diff
             submission["filename"] for submission in response.json["submissions"]
         ]
 
-        expected_submissions = [
-            submission.filename for submission in test_submissions["source"].submissions
-        ]
+        expected_submissions = [submission.filename for submission in test_submissions[
+                "source"
+            ].submissions]
         assert observed_submissions == expected_submissions
 
 
```

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+5 -9 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/ibis-project/ibis/blob/f1c888b6f299d5dae794643464500d038af0994c/ibis/backends/pyspark/__init__.py#L389'>ibis/backends/pyspark/__init__.py~L389</a>
```diff
         table_loc = self._to_sqlglot_table(database)
         catalog, db = self._to_catalog_db_tuple(table_loc)
         with self._active_catalog(catalog):
-            tables = [
-                row.tableName
-                for row in self._session.sql(
+            tables = [row.tableName for row in self._session.sql(
                     f"SHOW TABLES IN {db or self.current_database}"
-                ).collect()
-            ]
+                ).collect()]
         return self._filter_with_like(tables, like)
 
     def _wrap_udf_to_return_pandas(self, func, output_dtype):
```
<a href='https://github.com/ibis-project/ibis/blob/f1c888b6f299d5dae794643464500d038af0994c/ibis/backends/sql/datatypes.py#L1387'>ibis/backends/sql/datatypes.py~L1387</a>
```diff
     dialect = "athena"
 
 
-TYPE_MAPPERS: dict[str, SqlglotType] = {
-    mapper.dialect: mapper
-    for mapper in set(get_subclasses(SqlglotType)) - {SqlglotType, BigQueryUDFType}
-}
+TYPE_MAPPERS: dict[str, SqlglotType] = {mapper.dialect: mapper for mapper in set(
+        get_subclasses(SqlglotType)
+    ) - {SqlglotType, BigQueryUDFType}}
```

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+21 -32 lines across 5 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/langchain-ai/langchain/blob/9f470d297f0177b562c42a8a2f60a6f36ea48324/libs/core/langchain_core/messages/block_translators/openai.py#L726'>libs/core/langchain_core/messages/block_translators/openai.py~L726</a>
```diff
                 if "action" in block and isinstance(block["action"], dict):
                     if "sources" in block["action"]:
                         sources = block["action"]["sources"]
-                    web_search_call["args"] = {
-                        k: v for k, v in block["action"].items() if k != "sources"
-                    }
+                    web_search_call["args"] = {k: v for k, v in block[
+                            "action"
+                        ].items() if k != "sources"}
                 for key in block:
                     if key not in ("type", "id", "action", "status", "index"):
                         web_search_call[key] = block[key]
```
<a href='https://github.com/langchain-ai/langchain/blob/9f470d297f0177b562c42a8a2f60a6f36ea48324/libs/core/langchain_core/messages/utils.py#L1261'>libs/core/langchain_core/messages/utils.py~L1261</a>
```diff
                             f"{missing}. Full content block:\n\n{block}"
                         )
                         raise ValueError(err)
-                    if not any(
-                        tool_call["id"] == block["id"]
-                        for tool_call in cast("AIMessage", message).tool_calls
-                    ):
+                    if not any(tool_call["id"] == block["id"] for tool_call in cast(
+                            "AIMessage", message
+                        ).tool_calls):
                         oai_msg["tool_calls"] = oai_msg.get("tool_calls", [])
                         oai_msg["tool_calls"].append({
                             "type": "function",
```
<a href='https://github.com/langchain-ai/langchain/blob/9f470d297f0177b562c42a8a2f60a6f36ea48324/libs/core/langchain_core/runnables/base.py#L599'>libs/core/langchain_core/runnables/base.py~L599</a>
```diff
         # Import locally to prevent circular import
         from langchain_core.prompts.base import BasePromptTemplate  # noqa: PLC0415
 
-        return [
-            node.data
-            for node in self.get_graph(config=config).nodes.values()
-            if isinstance(node.data, BasePromptTemplate)
-        ]
+        return [node.data for node in self.get_graph(
+                config=config
+            ).nodes.values() if isinstance(node.data, BasePromptTemplate)]
 
     def __or__(
         self,
```
<a href='https://github.com/langchain-ai/langchain/blob/9f470d297f0177b562c42a8a2f60a6f36ea48324/libs/langchain/langchain_classic/chat_models/base.py#L604'>libs/langchain/langchain_classic/chat_models/base.py~L604</a>
```diff
 
     def _model_params(self, config: RunnableConfig | None) -> dict:
         config = ensure_config(config)
-        model_params = {
-            k.removeprefix(self._config_prefix): v
-            for k, v in config.get("configurable", {}).items()
-            if k.startswith(self._config_prefix)
-        }
+        model_params = {k.removeprefix(self._config_prefix): v for k, v in config.get(
+                "configurable", {}
+            ).items() if k.startswith(self._config_prefix)}
         if self._configurable_fields != "any":
             model_params = {
                 k: v for k, v in model_params.items() if k in self._configurable_fields
```
<a href='https://github.com/langchain-ai/langchain/blob/9f470d297f0177b562c42a8a2f60a6f36ea48324/libs/langchain/langchain_classic/chat_models/base.py#L624'>libs/langchain/langchain_classic/chat_models/base.py~L624</a>
```diff
         config = RunnableConfig(**(config or {}), **cast("RunnableConfig", kwargs))
         model_params = self._model_params(config)
         remaining_config = {k: v for k, v in config.items() if k != "configurable"}
-        remaining_config["configurable"] = {
-            k: v
-            for k, v in config.get("configurable", {}).items()
-            if k.removeprefix(self._config_prefix) not in model_params
-        }
+        remaining_config["configurable"] = {k: v for k, v in config.get(
+                "configurable", {}
+            ).items() if k.removeprefix(self._config_prefix) not in model_params}
         queued_declarative_operations = list(self._queued_declarative_operations)
         if remaining_config:
             queued_declarative_operations.append(
```
<a href='https://github.com/langchain-ai/langchain/blob/9f470d297f0177b562c42a8a2f60a6f36ea48324/libs/langchain_v1/langchain/chat_models/base.py#L559'>libs/langchain_v1/langchain/chat_models/base.py~L559</a>
```diff
 
     def _model_params(self, config: RunnableConfig | None) -> dict:
         config = ensure_config(config)
-        model_params = {
-            _remove_prefix(k, self._config_prefix): v
-            for k, v in config.get("configurable", {}).items()
-            if k.startswith(self._config_prefix)
-        }
+        model_params = {_remove_prefix(k, self._config_prefix): v for k, v in config.get(
+                "configurable", {}
+            ).items() if k.startswith(self._config_prefix)}
         if self._configurable_fields != "any":
             model_params = {k: v for k, v in model_params.items() if k in self._configurable_fields}
         return model_params
```
<a href='https://github.com/langchain-ai/langchain/blob/9f470d297f0177b562c42a8a2f60a6f36ea48324/libs/langchain_v1/langchain/chat_models/base.py#L577'>libs/langchain_v1/langchain/chat_models/base.py~L577</a>
```diff
         config = RunnableConfig(**(config or {}), **cast("RunnableConfig", kwargs))
         model_params = self._model_params(config)
         remaining_config = {k: v for k, v in config.items() if k != "configurable"}
-        remaining_config["configurable"] = {
-            k: v
-            for k, v in config.get("configurable", {}).items()
-            if _remove_prefix(k, self._config_prefix) not in model_params
-        }
+        remaining_config["configurable"] = {k: v for k, v in config.get(
+                "configurable", {}
+            ).items() if _remove_prefix(k, self._config_prefix) not in model_params}
         queued_declarative_operations = list(self._queued_declarative_operations)
         if remaining_config:
             queued_declarative_operations.append(
```

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+14 -20 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/latchbio/latch/blob/e0e83673bb418f03d6e221acb4ae7ae829d8f8e9/src/latch/utils.py#L128'>src/latch/utils.py~L128</a>
```diff
             name=x["displayName"],
             default=x["accountId"] == default_account,
         )
-        for x in owned_teams
-        + member_teams
-        + (
-            [res["teamInfoByAccountId"]]
-            if res["teamInfoByAccountId"] is not None
-            else []
+        for x in (
+            owned_teams
+            + member_teams
+            + (
+                [res["teamInfoByAccountId"]]
+                if res["teamInfoByAccountId"] is not None
+                else []
+            )
+            + owned_org_teams
+            + member_org_teams
         )
-        + owned_org_teams
-        + member_org_teams
     }
 
     return teams
```
<a href='https://github.com/latchbio/latch/blob/e0e83673bb418f03d6e221acb4ae7ae829d8f8e9/src/latch_cli/snakemake/serialize.py#L255'>src/latch_cli/snakemake/serialize.py~L255</a>
```diff
     )
     admin_lp = get_serializable_launch_plan(lp, settings, registrable_entity_cache)
 
-    registrable_entities = [
-        x.to_flyte_idl()
-        for x in list(
+    registrable_entities = [x.to_flyte_idl() for x in list(
             filter(should_register_with_admin, list(registrable_entity_cache.values()))
-        )
-        + [admin_lp]
-    ]
+        ) + [admin_lp]]
     for idx, entity in enumerate(registrable_entities):
         cur = spec_dir
 
```
<a href='https://github.com/latchbio/latch/blob/e0e83673bb418f03d6e221acb4ae7ae829d8f8e9/src/latch_cli/snakemake/serialize.py#L308'>src/latch_cli/snakemake/serialize.py~L308</a>
```diff
     )
     admin_lp = get_serializable_launch_plan(lp, settings, registrable_entity_cache)
 
-    registrable_entities = [
-        x.to_flyte_idl()
-        for x in list(
+    registrable_entities = [x.to_flyte_idl() for x in list(
             filter(should_register_with_admin, list(registrable_entity_cache.values()))
-        )
-        + [admin_lp]
-    ]
+        ) + [admin_lp]]
 
     click.secho("\nSerializing workflow entities", bold=True)
     persist_registrable_entities(registrable_entities, output_dir)
```

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+3 -5 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/lnbits/lnbits/blob/bd07a319abf1368723649d99add449607fcb2342/lnbits/core/views/extension_api.py#L456'>lnbits/core/views/extension_api.py~L456</a>
```diff
     user: User = Depends(check_user_exists),
 ) -> list[Extension]:
     user_extensions_ids = [ue.extension for ue in await get_user_extensions(user.id)]
-    return [
-        ext
-        for ext in await get_valid_extensions(False)
-        if ext.code in user_extensions_ids
-    ]
+    return [ext for ext in await get_valid_extensions(
+            False
+        ) if ext.code in user_extensions_ids]
 
 
 @extension_router.delete(
```

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+43 -43 lines across 6 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/mlflow/mlflow/blob/45a42428e175fa01250d22600d43ade1f25e2bc1/mlflow/dspy/util.py#L79'>mlflow/dspy/util.py~L79</a>
```diff
 
         lm = dspy.settings.lm
 
-        lm_attributes = {
-            key: value
-            for key, value in getattr(lm, "kwargs", {}).items()
-            if key not in {"api_key", "api_base"}
-        }
+        lm_attributes = {key: value for key, value in getattr(
+                lm, "kwargs", {}
+            ).items() if key not in {"api_key", "api_base"}}
 
         for attr in ["model", "model_type", "cache", "temperature", "max_tokens"]:
             value = getattr(lm, attr, None)
```
<a href='https://github.com/mlflow/mlflow/blob/45a42428e175fa01250d22600d43ade1f25e2bc1/mlflow/store/tracking/sqlalchemy_store.py#L1801'>mlflow/store/tracking/sqlalchemy_store.py~L1801</a>
```diff
     ) -> list[LoggedModelOutput]:
         return [
             LoggedModelOutput(model_id=output.destination_id, step=output.step)
-            for output in session.query(SqlInput)
-            .filter(
-                SqlInput.source_type == "RUN_OUTPUT",
-                SqlInput.source_id == run_id,
-                SqlInput.destination_type == "MODEL_OUTPUT",
+            for output in (
+                session.query(SqlInput)
+                .filter(
+                    SqlInput.source_type == "RUN_OUTPUT",
+                    SqlInput.source_id == run_id,
+                    SqlInput.destination_type == "MODEL_OUTPUT",
+                )
+                .all()
             )
-            .all()
         ]
 
     #######################################################################################
```
<a href='https://github.com/mlflow/mlflow/blob/45a42428e175fa01250d22600d43ade1f25e2bc1/mlflow/store/tracking/sqlalchemy_store.py#L2050'>mlflow/store/tracking/sqlalchemy_store.py~L2050</a>
```diff
             # First, get all scorer_ids for this experiment
             scorer_ids = [
                 scorer.scorer_id
-                for scorer in session.query(SqlScorer.scorer_id)
-                .filter(SqlScorer.experiment_id == experiment.experiment_id)
-                .all()
+                for scorer in (
+                    session.query(SqlScorer.scorer_id)
+                    .filter(SqlScorer.experiment_id == experiment.experiment_id)
+                    .all()
+                )
             ]
 
             if not scorer_ids:
```
<a href='https://github.com/mlflow/mlflow/blob/45a42428e175fa01250d22600d43ade1f25e2bc1/mlflow/types/schema.py#L399'>mlflow/types/schema.py~L399</a>
```diff
             not isinstance(prop, dict) for prop in kwargs["properties"].values()
         ):
             raise MlflowException("Expected properties to be a dictionary of Property JSON")
-        return cls([
-            Property.from_json_dict(**{name: prop}) for name, prop in kwargs["properties"].items()
-        ])
+        return cls([Property.from_json_dict(**{name: prop}) for name, prop in kwargs[
+                "properties"
+            ].items()])
 
     def _merge(self, other: BaseType) -> Object:
         """
```
<a href='https://github.com/mlflow/mlflow/blob/45a42428e175fa01250d22600d43ade1f25e2bc1/tests/metrics/genai/test_genai_metrics.py#L160'>tests/metrics/genai/test_genai_metrics.py~L160</a>
```diff
 
 
 def test_make_genai_metric_correct_response(custom_metric):
-    assert [
-        param.name for param in inspect.signature(custom_metric.eval_fn).parameters.values()
-    ] == ["predictions", "metrics", "inputs", "targets"]
+    assert [param.name for param in inspect.signature(
+            custom_metric.eval_fn
+        ).parameters.values()] == ["predictions", "metrics", "inputs", "targets"]
 
     with mock.patch.object(
         model_utils,
```
<a href='https://github.com/mlflow/mlflow/blob/45a42428e175fa01250d22600d43ade1f25e2bc1/tests/metrics/genai/test_genai_metrics.py#L275'>tests/metrics/genai/test_genai_metrics.py~L275</a>
```diff
         ],
     )
 
-    assert [
-        param.name for param in inspect.signature(custom_metric.eval_fn).parameters.values()
-    ] == ["predictions", "metrics", "inputs", "targets"]
+    assert [param.name for param in inspect.signature(
+            custom_metric.eval_fn
+        ).parameters.values()] == ["predictions", "metrics", "inputs", "targets"]
 
     with mock.patch.object(
         model_utils,
```
<a href='https://github.com/mlflow/mlflow/blob/45a42428e175fa01250d22600d43ade1f25e2bc1/tests/pyfunc/test_pyfunc_model_with_type_hints.py#L339'>tests/pyfunc/test_pyfunc_model_with_type_hints.py~L339</a>
```diff
         df = spark.createDataFrame(pd.DataFrame({"input": input_example}), schema=schema)
     df = df.withColumn("response", udf("input"))
     pdf = df.toPandas()
-    assert [
-        x.asDict(recursive=True) if isinstance(x, Row) else x for x in pdf["response"].tolist()
-    ] == input_example
+    assert [x.asDict(recursive=True) if isinstance(x, Row) else x for x in pdf[
+            "response"
+        ].tolist()] == input_example
 
 
 def test_pyfunc_model_with_no_op_type_hint_pass_signature_works():
```
<a href='https://github.com/mlflow/mlflow/blob/45a42428e175fa01250d22600d43ade1f25e2bc1/tests/store/artifact/test_presigned_url_artifact_repo.py#L118'>tests/store/artifact/test_presigned_url_artifact_repo.py~L118</a>
```diff
     remote_path = json.loads(kwargs["json_body"])["path"]
     return CreateDownloadUrlResponse(
         url=_make_presigned_url(remote_path),
-        headers=[
-            HttpHeader(name=header, value=val) for header, val in _make_headers(remote_path).items()
-        ],
+        headers=[HttpHeader(name=header, value=val) for header, val in _make_headers(
+                remote_path
+            ).items()],
     )
 
 
```
<a href='https://github.com/mlflow/mlflow/blob/45a42428e175fa01250d22600d43ade1f25e2bc1/tests/store/artifact/test_presigned_url_artifact_repo.py#L163'>tests/store/artifact/test_presigned_url_artifact_repo.py~L163</a>
```diff
     remote_path = json.loads(kwargs["json_body"])["path"]
     return CreateUploadUrlResponse(
         url=_make_presigned_url(remote_path),
-        headers=[
-            HttpHeader(name=header, value=val) for header, val in _make_headers(remote_path).items()
-        ],
+        headers=[HttpHeader(name=header, value=val) for header, val in _make_headers(
+                remote_path
+            ).items()],
     )
 
 
```
<a href='https://github.com/mlflow/mlflow/blob/45a42428e175fa01250d22600d43ade1f25e2bc1/tests/store/artifact/test_presigned_url_artifact_repo.py#L208'>tests/store/artifact/test_presigned_url_artifact_repo.py~L208</a>
```diff
             f"{PRESIGNED_URL_ARTIFACT_REPOSITORY}.PresignedUrlArtifactRepository._get_download_presigned_url_and_headers",
             return_value=CreateDownloadUrlResponse(
                 url=_make_presigned_url(remote_file_path),
-                headers=[
-                    HttpHeader(name=k, value=v) for k, v in _make_headers(remote_file_path).items()
-                ],
+                headers=[HttpHeader(name=k, value=v) for k, v in _make_headers(
+                        remote_file_path
+                    ).items()],
             ),
         ) as mock_request,
         mock.patch(
```
<a href='https://github.com/mlflow/mlflow/blob/45a42428e175fa01250d22600d43ade1f25e2bc1/tests/store/artifact/test_presigned_url_artifact_repo.py#L253'>tests/store/artifact/test_presigned_url_artifact_repo.py~L253</a>
```diff
     total_remote_path = f"{artifact_path}/{os.path.basename(local_file)}"
     creds = ArtifactCredentialInfo(
         signed_uri=_make_presigned_url(total_remote_path),
-        headers=[
-            ArtifactCredentialInfo.HttpHeader(name=k, value=v)
-            for k, v in _make_headers(total_remote_path).items()
-        ],
+        headers=[ArtifactCredentialInfo.HttpHeader(name=k, value=v) for k, v in _make_headers(
+                total_remote_path
+            ).items()],
     )
     with (
         mock.patch(
```
<a href='https://github.com/mlflow/mlflow/blob/45a42428e175fa01250d22600d43ade1f25e2bc1/tests/store/artifact/test_presigned_url_artifact_repo.py#L295'>tests/store/artifact/test_presigned_url_artifact_repo.py~L295</a>
```diff
     ):
         cred_info = ArtifactCredentialInfo(
             signed_uri=_make_presigned_url(remote_file_path),
-            headers=[
-                ArtifactCredentialInfo.HttpHeader(name=k, value=v)
-                for k, v in _make_headers(remote_file_path).items()
-            ],
+            headers=[ArtifactCredentialInfo.HttpHeader(name=k, value=v) for k, v in _make_headers(
+                    remote_file_path
+                ).items()],
         )
         artifact_repo._upload_to_cloud(cred_info, local_file, "some/irrelevant/path")
         mock_cloud.assert_called_once_with(
```

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+3 -7 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pandas-dev/pandas/blob/e9e1b32f14eccf429d2de298b7aa488cf6dcc5df/pandas/core/reshape/melt.py#L548'>pandas/core/reshape/melt.py~L548</a>
```diff
     If we have many columns, we could also use a regex to find our
     stubnames and pass that list on to wide_to_long
 
-    >>> stubnames = sorted(
-    ...     set([
-    ...         match[0]
-    ...         for match in df.columns.str.findall(r"[A-B]\(.*\)").values
-    ...         if match != []
-    ...     ])
-    ... )
+    >>> stubnames = sorted(set([match[0] for match in df.columns.str.findall(
+    ...             r"[A-B]\(.*\)"
+    ...         ).values if match != []]))
     >>> list(stubnames)
     ['A(weekly)', 'B(weekly)']
 
```

</p>
</details>
<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (+31 -28 lines across 4 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/prefecthq/prefect/blob/a3241cd06b5dfd838f4b854f11e9a5193776951a/src/integrations/prefect-kubernetes/tests/test_worker.py#L351'>src/integrations/prefect-kubernetes/tests/test_worker.py~L351</a>
```diff
                                     "env": [
                                         *[
                                             {"name": k, "value": v}
-                                            for k, v in get_current_settings()
-                                            .to_environment_variables(
-                                                exclude_unset=True
+                                            for k, v in (
+                                                get_current_settings()
+                                                .to_environment_variables(
+                                                    exclude_unset=True
+                                                )
+                                                .items()
                                             )
-                                            .items()
                                         ],
                                         {
                                             "name": "PREFECT__FLOW_RUN_ID",
```
<a href='https://github.com/prefecthq/prefect/blob/a3241cd06b5dfd838f4b854f11e9a5193776951a/src/integrations/prefect-kubernetes/tests/test_worker.py#L667'>src/integrations/prefect-kubernetes/tests/test_worker.py~L667</a>
```diff
                                     "env": [
                                         *[
                                             {"name": k, "value": v}
-                                            for k, v in get_current_settings()
-                                            .to_environment_variables(
-                                                exclude_unset=True
+                                            for k, v in (
+                                                get_current_settings()
+                                                .to_environment_variables(
+                                                    exclude_unset=True
+                                                )
+                                                .items()
                                             )
-                                            .items()
                                         ],
                                         {
                                             "name": "PREFECT__FLOW_RUN_ID",
```
<a href='https://github.com/prefecthq/prefect/blob/a3241cd06b5dfd838f4b854f11e9a5193776951a/src/integrations/prefect-kubernetes/tests/test_worker.py#L861'>src/integrations/prefect-kubernetes/tests/test_worker.py~L861</a>
```diff
                                     "env": [
                                         *[
                                             {"name": k, "value": v}
-                                            for k, v in get_current_settings()
-                                            .to_environment_variables(
-                                                exclude_unset=True
+                                            for k, v in (
+                                                get_current_settings()
+                                                .to_environment_variables(
+                                                    exclude_unset=True
+                                                )
+                                                .items()
                                             )
-                                            .items()
                                         ],
                                         {
                                             "name": "PREFECT__FLOW_RUN_ID",
```
<a href='https://github.com/prefecthq/prefect/blob/a3241cd06b5dfd838f4b854f11e9a5193776951a/src/integrations/prefect-kubernetes/tests/test_worker.py#L1179'>src/integrations/prefect-kubernetes/tests/test_worker.py~L1179</a>
```diff
                                     "env": [
                                         *[
                                             {"name": k, "value": v}
-                                            for k, v in get_current_settings()
-                                            .to_environment_variables(
-                                                exclude_unset=True
+                                            for k, v in (
+                                                get_current_settings()
+                                                .to_environment_variables(
+                                                    exclude_unset=True
+                                                )
+                                                .items()
                                             )
-                                            .items()
                                         ],
                                         {
                                             "name": "PREFECT__FLOW_RUN_ID",
```
<a href='https://github.com/prefecthq/prefect/blob/a3241cd06b5dfd838f4b854f11e9a5193776951a/src/prefect/utilities/collections.py#L569'>src/prefect/utilities/collections.py~L569</a>
```diff
     """
     if not isinstance(obj, dict):
         return obj
-    return {
-        key: remove_nested_keys(keys_to_remove, value)
-        for key, value in cast(NestedDict[HashableT, VT], obj).items()
-        if key not in keys_to_remove
-    }
+    return {key: remove_nested_keys(keys_to_remove, value) for key, value in cast(
+            NestedDict[HashableT, VT], obj
+        ).items() if key not in keys_to_remove}
 
 
 @overload
```
<a href='https://github.com/prefecthq/prefect/blob/a3241cd06b5dfd838f4b854f11e9a5193776951a/tests/server/services/test_scheduler.py#L106'>tests/server/services/test_scheduler.py~L106</a>
```diff
     deployment_with_active_schedules: schemas.core.Deployment,
 ):
     active_schedules = [
-        s.schedule
-        for s in await models.deployments.read_deployment_schedules(
+        s.schedule for s in await models.deployments.read_deployment_schedules(
             session=session,
             deployment_id=deployment_with_active_schedules.id,
             deployment_schedule_filter=schemas.filters.DeploymentScheduleFilter(
```
<a href='https://github.com/prefecthq/prefect/blob/a3241cd06b5dfd838f4b854f11e9a5193776951a/tests/test_settings.py#L644'>tests/test_settings.py~L644</a>
```diff
     @pytest.mark.usefixtures("disable_hosted_api_server")
     def test_settings_to_environment_includes_all_settings_with_non_null_values(self):
         settings = Settings()
-        expected_names = {
-            s.name
-            for s in _get_settings_fields(Settings).values()
-            if s.value() is not None
-        }
+        expected_names = {s.name for s in _get_settings_fields(
+                Settings
+            ).values() if s.value() is not None}
         for name, metadata in SUPPORTED_SETTINGS.items():
             if metadata.get("legacy") and name in expected_names:
                 expected_names.remove(name)
```

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (+3 -3 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/python/mypy/blob/b266dd1a238e867e6b135c54664a76dae5a00fcb/mypy/semanal.py#L1922'>mypy/semanal.py~L1922</a>
```diff
         self.check_type_alias_bases(bases)
 
         for tvd in tvar_defs:
-            if isinstance(tvd, TypeVarType) and any(
-                has_placeholder(t) for t in [tvd.upper_bound] + tvd.values
-            ):
+            if isinstance(tvd, TypeVarType) and any(has_placeholder(t) for t in [
+                    tvd.upper_bound
+                ] + tvd.values):
                 # Some type variable bounds or values are not ready, we need
                 # to re-analyze this class.
                 self.defer()
```

</p>
</details>
<details><summary><a href="https://github.com/qdrant/qdrant-client">qdrant/qdrant-client</a> (+6 -9 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/qdrant/qdrant-client/blob/981117a506b549e9dcd51cd633d4a6162df3b05f/qdrant_client/conversions/conversion.py#L78'>qdrant_client/conversions/conversion.py~L78</a>
```diff
     if "structValue" in value_:
         if "fields" not in value_["structValue"]:
             return {}
-        return dict(
-            (key, value_to_json(val))
-            for key, val in value_["structVa...*[Comment body truncated]*

---

_Label `formatter` added by @ntBre on 2025-10-20 22:48_

---

_Label `preview` added by @ntBre on 2025-10-20 22:48_

---

_@MichaReiser reviewed on 2025-10-21 06:33_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/comprehension.rs`:221 on 2025-10-21 06:33_

You probably want has_own_parentheses

---

_Comment by @MichaReiser on 2025-10-21 06:38_

I think the comment here is relevant for your work 

https://github.com/astral-sh/ruff/blob/c9dff5c7d5bcab2a83f1c3b4a1a80af230ece541/crates/ruff_python_formatter/src/expression/mod.rs#L386-L397

I'm also not sure if we should implement this preview style, as @dylwil3 pointed out in https://github.com/astral-sh/ruff/issues/20482#issuecomment-3340449328 because it might fall into the same category as https://github.com/astral-sh/ruff/issues/12856 where Black now starts introducing otherwise unnecessary parentheses. Are there alternative formattings that we could use that avoid the need for inserting parentheses?

---

_Comment by @MichaReiser on 2025-10-21 06:41_

Reading through the issue, the main concern seems to be that very long attribute chains aren't split. That makes me wonder if the proper fix instead is to split attribute expressions in parenthesized expressions. I suspect this would be a bigger change and we probably want to give attribute chains a very low split priority. Or we could decide to only indent the content rather than adding an extra pair of parentheses?

---

_Comment by @ntBre on 2025-10-21 19:48_

Thanks for the pointers here and in our 1:1! The draft is in a slightly better state now, at least in terms of the tests and the ecosystem results. However, I'm now splitting expressions like this too eagerly on the `in`:

```py
unformatted: File would be reformatted
 --> /tmp/tmp.h5wCjfHytw/try.py:1:1
2 | async def api_get_user_extensions(
3 |     user: User = Depends(check_user_exists),
4 | ) -> list[Extension]:
  - 
  -     user_extensions_ids = [
  -         ue.extension for ue in await get_user_extensions(user.id)
  -     ]
  -     return [
  -         ext
  -         for ext in await get_valid_extensions(False)
  -         if ext.code in user_extensions_ids
  -     ]
5 +     user_extensions_ids = [ue.extension for ue in await get_user_extensions(user.id)]
6 +     return [ext for ext in await get_valid_extensions(
7 +             False
8 +         ) if ext.code in user_extensions_ids]
```

Similar cases make up all of the ecosystem results I've looked at.

Is there an easy way to avoid that? It does seem to be something with `can_omit_optional_parentheses` right where you linked me on Discord. The split priority you mentioned above sounded perfect, but I didn't turn up anything with grep.

I will probably move on to another preview style as you and Dylan said, unless this is looking promising. It seems like there are other designs worth exploring in the future anyway.

---

_Comment by @MichaReiser on 2025-10-22 06:31_

> Thanks for the pointers here and in our 1:1! The draft is in a slightly better state now, at least in terms of the tests and the ecosystem results. However, I'm now splitting expressions like this too eagerly on the in:

This makes me wonder if you even want `maybe_parenthesize_expression`. Can you formalize the desired split and parenthesizing behavior?

* Should lines split from left to right? (We parenthesize the `in` before parenthesizing the if)
* Should lines split from right to left (We parenthesize the `if` part before parenthesizing the `in` part?
* Does parenthesizing the `in` part always expand the comprehension (`[` and `]` are on their own line)?
* Where should newlines be added when the `in` part gets parenthesized or splits over multiple lines (as you can see with `await`)
* ...

---

_Comment by @ntBre on 2025-10-23 19:55_

This doesn't feel very formal, so I don't think it fully answers your questions, but my understanding based on the black tests are that this preview style should only come into effect after all of the usual splits have been done and the `in` clause is still too long. For the example above, black leaves the input alone:

```py
@extension_router.get("")
async def api_get_user_extensions(
    user: User = Depends(check_user_exists),
) -> list[Extension]:

    user_extensions_ids = [
        ue.extension for ue in await get_user_extensions(user.id)
    ]
    return [
        ext
        for ext in await get_valid_extensions(False)
        if ext.code in user_extensions_ids
    ]
```

If I extend the `await` expression enough, it eventually breaks like this on both stable and preview:

```py
@extension_router.get("")
async def api_get_user_extensions(
    user: User = Depends(check_user_exists),
) -> list[Extension]:

    user_extensions_ids = [
        ue.extension for ue in await get_user_extensions(user.id)
    ]
    return [
        ext
        for ext in await get_valid_extensions(
            Falseeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee
        )
        if ext.code in user_extensions_ids
    ]
```

which also makes sense to me.

This part is a bit confusing to me, but if I remove the `await` and the `call`, no split happens with the new preview style:

```py
@extension_router.get("")
async def api_get_user_extensions(
    user: User = Depends(check_user_exists),
) -> list[Extension]:

    user_extensions_ids = [
        ue.extension for ue in await get_user_extensions(user.id)
    ]
    return [
        ext
        for ext in aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
    ]
```

If I make that an attribute expression instead (replacing an `a` with a `.` to avoid changing the length), it does split:

```py
@extension_router.get("")
async def api_get_user_extensions(
    user: User = Depends(check_user_exists),
) -> list[Extension]:

    user_extensions_ids = [
        ue.extension for ue in await get_user_extensions(user.id)
    ]
    return [
        ext
        for ext in (
            aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa.aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
        )
    ]
```

I get that same splitting even with a following `if` clause, as I would expect.

Black also explicitly tests that they _don't_ parenthesize the `in` clause if it's still too long [here](https://github.com/psf/black/blob/6122359d2029f24bcac9fd34c930ff78672ae840/tests/data/cases/preview_wrap_comprehension_in.py#L117-L127).

So at least from these tests, it seems that the comprehension always gets fully expanded like the stable behavior, and then if the for-in part is still too long, and it's a specific type of expression like an attribute chain, it can be parenthesized and split across multiple lines.

Does that help at all? Maybe I need to take a look at `normalize_invisible_parens` and the rest of the Black implementation too.

---

_Comment by @MichaReiser on 2025-10-24 07:47_

Thanks for explaining. What I understand is:

* We want a right-to-left layout where the `in` part breaks before the `for element in`
* We only want to break the `for element in` over multiple lines if breaking the `in` on its own isn't enough. But, if we do so, we don't want to break the `in` unless it's necessary

> If I make that an attribute expression instead (replacing an a with a . to avoid changing the length), it does split:

But you changed more than just from identifier to attribute. The attribute expression is shorter. I think this is just

> Black also explicitly tests that they don't parenthesize the in clause if it's still too long [here](https://github.com/psf/black/blob/6122359d2029f24bcac9fd34c930ff78672ae840/tests/data/cases/preview_wrap_comprehension_in.py#L117-L127).

We did implement this behavior in a few places but it's fairly tricky and I'm honestly not sure if it's worth it. Some users find it confusing (it goes against: predictable formatting), but it does avoid parentheses in some places, like assignment, where I think it makes sense.

As discussed with Dylan. I'm not convinced that this always improves formatting. Have you considered alternative formattings that don't require inserting parentheses but result in the same improvement? 


Either way. I haven't seen this come up. I suggest deprioritizing it. We have other formatter issues that have been requested more frequently (I don't remember that this ever came up)

---

_Comment by @ntBre on 2025-10-24 13:09_

> Either way. I haven't seen this come up. I suggest deprioritizing it. We have other formatter issues that have been requested more frequently (I don't remember that this ever came up)

Sounds good, thanks for talking through it. I still learned a lot!

> Have you considered alternative formattings that don't require inserting parentheses but result in the same improvement?

I kind of liked this formatting mentioned on the upstream issue, but that's about as much as I've considered it.

```
[
    a
    for graph_path_expression
	in refined_constraint.condition_as_predicate.variables
]
```

---

_Closed by @ntBre on 2025-10-24 13:09_

---
