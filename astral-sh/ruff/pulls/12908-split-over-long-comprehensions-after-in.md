```yaml
number: 12908
title: "Split over-long comprehensions after `in`"
type: pull_request
state: closed
author: MichaReiser
labels:
  - formatter
  - style
assignees: []
base: main
head: split-comprehension-after-in
created_at: 2024-08-15T16:32:40Z
updated_at: 2024-08-15T16:44:36Z
url: https://github.com/astral-sh/ruff/pull/12908
synced_at: 2026-01-12T15:55:42Z
```

# Split over-long comprehensions after `in`

---

_@MichaReiser_

## Summary

To get some ecosystem results for https://github.com/astral-sh/ruff/issues/12870

## Test Plan

I have no intention of merging this change.


---

_Label `formatter` added by @MichaReiser on 2024-08-15 16:33_

---

_Label `style` added by @MichaReiser on 2024-08-15 16:33_

---

_Comment by @github-actions[bot] on 2024-08-15 16:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+8 -7 lines in 4 files in 3 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (+2 -1 lines across 1 file)</summary>
<p>

<a href='https://github.com/prefecthq/prefect/blob/e11b67b756eacb4a4dc780fe28af473068d09729/src/prefect/server/models/block_schemas.py#L422'>src/prefect/server/models/block_schemas.py~L422</a>
```diff
                 block_schema,
                 _,
                 parent_block_schema_id,
-            ) in block_schemas_with_references
+            )
+            in block_schemas_with_references
             if parent_block_schema_id is None
         ),
         None,
```

</p>
</details>
<details><summary><a href="https://github.com/qdrant/qdrant-client">qdrant/qdrant-client</a> (+2 -4 lines across 1 file)</summary>
<p>

<a href='https://github.com/qdrant/qdrant-client/blob/8054380c26a718d55eb980c35a262e71db0d301a/qdrant_client/local/async_qdrant_local.py#L149'>qdrant_client/local/async_qdrant_local.py~L149</a>
```diff
                     {
                         "collections": {
                             collection_name: to_dict(collection.config)
-                            for (
-                                collection_name,
-                                collection,
-                            ) in self.collections.items()
+                            for (collection_name, collection)
+                            in self.collections.items()
                         },
                         "aliases": self.aliases,
                     }
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+4 -2 lines across 2 files)</summary>
<p>

<a href='https://github.com/rotki/rotki/blob/b65537a9f54a1f5ce833de0d17a4b23239e1f357/rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py#L201'>rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py~L201</a>
```diff
                         protocol,
                         chain,
                         token_kind,
-                    ) in cursor.execute(  # noqa: E501
+                    )  # noqa: E501
+                    in cursor.execute(
                         f"""
                         SELECT A.identifier, A.type, B.address, B.decimals, A.name, C.symbol, C.started, null, C.swapped_for, C.coingecko, C.cryptocompare, B.protocol, B.chain, B.token_kind FROM assets as A JOIN evm_tokens as B
                         ON B.identifier = A.identifier JOIN common_asset_details AS C ON C.identifier = B.identifier WHERE A.type = '{evm_type_db}'
```
<a href='https://github.com/rotki/rotki/blob/b65537a9f54a1f5ce833de0d17a4b23239e1f357/rotkehlchen/tests/unit/test_tasks_manager.py#L1180'>rotkehlchen/tests/unit/test_tasks_manager.py~L1180</a>
```diff
     ens_events = [
         next(
             x
-            for x in get_decoded_events_of_transaction(  # decode ENS registration/renewal event and get the event with the metadata  # noqa: E501
+            for x
+            in get_decoded_events_of_transaction(  # decode ENS registration/renewal event and get the event with the metadata  # noqa: E501
                 evm_inquirer=ethereum_inquirer,
                 database=database,
                 tx_hash=ens_tx_hash,
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+6 -3 lines in 3 files in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (+2 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/prefecthq/prefect/blob/e11b67b756eacb4a4dc780fe28af473068d09729/src/prefect/server/models/block_schemas.py#L422'>src/prefect/server/models/block_schemas.py~L422</a>
```diff
                 block_schema,
                 _,
                 parent_block_schema_id,
-            ) in block_schemas_with_references
+            )
+            in block_schemas_with_references
             if parent_block_schema_id is None
         ),
         None,
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+4 -2 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/rotki/rotki/blob/b65537a9f54a1f5ce833de0d17a4b23239e1f357/rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py#L201'>rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py~L201</a>
```diff
                         protocol,
                         chain,
                         token_kind,
-                    ) in cursor.execute(  # noqa: E501
+                    )  # noqa: E501
+                    in cursor.execute(
                         f"""
                         SELECT A.identifier, A.type, B.address, B.decimals, A.name, C.symbol, C.started, null, C.swapped_for, C.coingecko, C.cryptocompare, B.protocol, B.chain, B.token_kind FROM assets as A JOIN evm_tokens as B
                         ON B.identifier = A.identifier JOIN common_asset_details AS C ON C.identifier = B.identifier WHERE A.type = '{evm_type_db}'
```
<a href='https://github.com/rotki/rotki/blob/b65537a9f54a1f5ce833de0d17a4b23239e1f357/rotkehlchen/tests/unit/test_tasks_manager.py#L1180'>rotkehlchen/tests/unit/test_tasks_manager.py~L1180</a>
```diff
     ens_events = [
         next(
             x
-            for x in get_decoded_events_of_transaction(  # decode ENS registration/renewal event and get the event with the metadata  # noqa: E501
+            for x
+            in get_decoded_events_of_transaction(  # decode ENS registration/renewal event and get the event with the metadata  # noqa: E501
                 evm_inquirer=ethereum_inquirer,
                 database=database,
                 tx_hash=ens_tx_hash,
```

</p>
</details>




---

_Closed by @MichaReiser on 2024-08-15 16:44_

---
