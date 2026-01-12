```yaml
number: 13539
title: Normalize implicit concatenated f-string quotes per part
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: micha/remove-string-literal-implicit-fstring-kind
created_at: 2024-09-27T15:11:56Z
updated_at: 2024-10-08T10:05:12Z
url: https://github.com/astral-sh/ruff/pull/13539
synced_at: 2026-01-12T15:55:44Z
```

# Normalize implicit concatenated f-string quotes per part

---

_@MichaReiser_

This PR fixes a bug where ruff decided on whether the quotes for an implicit concatenated string can be changed for the entire expression instead of deciding on the part-level. 

**Input**
```python
_ = (
    'This string should change its quotes to double quotes'
    f'This string uses double quotes in an expression {"woah"}'
    f'This f-string does not use any quotes.'
)
```

**Stable**
```python
_ = (
    'This string should change its quotes to double quotes'
    f'This string uses double quotes in an expression {"woah"}'
    f'This f-string does not use any quotes.'
)
```


Notice how the formatter doesn't change any quotes only because one f-string has an expression containing double quotes. 

**Preview**
```python
_ = (
    "This string should change its quotes to double quotes"
    f'This string uses double quotes in an expression {"woah"}'
    f"This f-string does not use any quotes."
)
```

This matches black and how Ruff normalizes quotes for regular string literals (the decision is made per literal and for the entire implicit concatenated string expression)



---

_Review requested from @dhruvmanila by @MichaReiser on 2024-09-27 15:12_

---

_Label `internal` added by @MichaReiser on 2024-09-27 15:12_

---

_Comment by @github-actions[bot] on 2024-09-27 15:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+93 -93 lines in 36 files in 3 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+3 -3 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/langchain-ai/langchain/blob/16f5fdb38b307ee3f3b5065f671b28c90661b698/templates/neo4j-semantic-ollama/neo4j_semantic_ollama/recommendation_tool.py#L77'>templates/neo4j-semantic-ollama/neo4j_semantic_ollama/recommendation_tool.py~L77</a>
```diff
         response = graph.query(recommendation_query_db_history, params)
         try:
             return (
-                'Recommended movies are: '
+                "Recommended movies are: "
                 f'{f"###Movie {nl}".join([el["movie"] for el in response])}'
             )
         except Exception:
```
<a href='https://github.com/langchain-ai/langchain/blob/16f5fdb38b307ee3f3b5065f671b28c90661b698/templates/neo4j-semantic-ollama/neo4j_semantic_ollama/recommendation_tool.py#L87'>templates/neo4j-semantic-ollama/neo4j_semantic_ollama/recommendation_tool.py~L87</a>
```diff
         response = graph.query(recommendation_query_genre, params)
         try:
             return (
-                'Recommended movies are: '
+                "Recommended movies are: "
                 f'{f"###Movie {nl}".join([el["movie"] for el in response])}'
             )
         except Exception:
```
<a href='https://github.com/langchain-ai/langchain/blob/16f5fdb38b307ee3f3b5065f671b28c90661b698/templates/neo4j-semantic-ollama/neo4j_semantic_ollama/recommendation_tool.py#L101'>templates/neo4j-semantic-ollama/neo4j_semantic_ollama/recommendation_tool.py~L101</a>
```diff
     response = graph.query(query, params)
     try:
         return (
-            'Recommended movies are: '
+            "Recommended movies are: "
             f'{f"###Movie {nl}".join([el["movie"] for el in response])}'
         )
     except Exception:
```

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pandas-dev/pandas/blob/5126dcaf88167ff869db874be40a520bb86a27ed/scripts/validate_docstrings.py#L369'>scripts/validate_docstrings.py~L369</a>
```diff
         for err_code in actual_failures - expected_failures:
             sys.stdout.write(
                 f'{prefix}{res["file"]}:{res["file_line"]}:'
-                f'{err_code}:{func_name}:{error_messages[err_code]}\n'
+                f"{err_code}:{func_name}:{error_messages[err_code]}\n"
             )
             exit_status += 1
         for err_code in ignore_errors.get(func_name, set()) - actual_failures:
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+89 -89 lines across 34 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/api/rest.py#L445'>rotkehlchen/api/rest.py~L445</a>
```diff
             return
 
         log.error(
-            f'{task_str} dies with exception: {greenlet.exception}.\n'
-            f'Exception Name: {greenlet.exc_info[0]}\n'
-            f'Exception Info: {greenlet.exc_info[1]}\n'
+            f"{task_str} dies with exception: {greenlet.exception}.\n"
+            f"Exception Name: {greenlet.exc_info[0]}\n"
+            f"Exception Info: {greenlet.exc_info[1]}\n"
             f'Traceback:\n {"".join(traceback.format_tb(greenlet.exc_info[2]))}',
         )
         # also write an error for the task result if it's not the main greenlet
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/api/rest.py#L1479'>rotkehlchen/api/rest.py~L1479</a>
```diff
         if identifiers is not None:
             return api_response(
                 result=wrap_in_fail_result(
-                    f'Failed to add {asset.asset_type!s} {asset.name} '
+                    f"Failed to add {asset.asset_type!s} {asset.name} "
                     f'since it already exists. Existing ids: {",".join(identifiers)}'
                 ),
                 status_code=HTTPStatus.CONFLICT,
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/api/rest.py#L3231'>rotkehlchen/api/rest.py~L3231</a>
```diff
     ) -> dict[str, Any]:
         """Return the current price of the assets in the target asset currency."""
         log.debug(
-            f'Querying the current {target_asset.identifier} price of these assets: '
+            f"Querying the current {target_asset.identifier} price of these assets: "
             f'{", ".join([asset.identifier for asset in assets])}',
         )
         # Type is list instead of tuple here because you can't serialize a tuple
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/api/rest.py#L3338'>rotkehlchen/api/rest.py~L3338</a>
```diff
         asset currency.
         """
         log.debug(
-            f'Querying the historical {target_asset.identifier} price of these assets: '
+            f"Querying the historical {target_asset.identifier} price of these assets: "
             f'{", ".join(f"{asset.identifier} at {ts}" for asset, ts in assets_timestamp)}',
             assets_timestamp=assets_timestamp,
         )
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/api/v1/fields.py#L532'>rotkehlchen/api/v1/fields.py~L532</a>
```diff
 
         if self.limit_to is not None and chain_id not in self.limit_to:
             raise ValidationError(
-                f'Given chain_id {value} is not one of '
+                f"Given chain_id {value} is not one of "
                 f'{",".join([str(x) for x in self.limit_to])} as needed by the endpoint',
             )
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/api/v1/fields.py#L572'>rotkehlchen/api/v1/fields.py~L572</a>
```diff
 
         if self.limit_to is not None and chain not in self.limit_to:
             raise ValidationError(
-                f'Given chain {value} is not one of '
+                f"Given chain {value} is not one of "
                 f'{",".join([str(x) for x in self.limit_to])} as needed by the endpoint',
             )
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/api/v1/fields.py#L807'>rotkehlchen/api/v1/fields.py~L807</a>
```diff
 
         if self.limit_to is not None and location not in self.limit_to:
             raise ValidationError(
-                f'Given location {value} is not one of '
+                f"Given location {value} is not one of "
                 f'{",".join([str(x) for x in self.limit_to])} as needed by the endpoint',
             )
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/api/v1/fields.py#L929'>rotkehlchen/api/v1/fields.py~L929</a>
```diff
                 and not any(value.filename.endswith(x) for x in self.allowed_extensions)
             ):  # noqa: E501
                 raise ValidationError(
-                    f'Given file {value.filename} does not end in any of '
+                    f"Given file {value.filename} does not end in any of "
                     f'{",".join(self.allowed_extensions)}',
                 )
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/api/v1/fields.py#L949'>rotkehlchen/api/v1/fields.py~L949</a>
```diff
             path.suffix == x for x in self.allowed_extensions
         ):  # noqa: E501
             raise ValidationError(
-                f'Given file {path} does not end in any of '
+                f"Given file {path} does not end in any of "
                 f'{",".join(self.allowed_extensions)}',
             )
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/api/v1/schemas.py#L466'>rotkehlchen/api/v1/schemas.py~L466</a>
```diff
             data["order_by_attributes"]
         ).issubset(valid_ordering_attr):
             error_msg = (
-                f'order_by_attributes for trades can not be '
+                f"order_by_attributes for trades can not be "
                 f'{",".join(set(data["order_by_attributes"]) - valid_ordering_attr)}'
             )
             raise ValidationError(
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/api/v1/schemas.py#L695'>rotkehlchen/api/v1/schemas.py~L695</a>
```diff
             data["order_by_attributes"]
         ).issubset(valid_ordering_attr):
             error_msg = (
-                f'order_by_attributes for history event data can not be '
+                f"order_by_attributes for history event data can not be "
                 f'{",".join(set(data["order_by_attributes"]) - valid_ordering_attr)}'
             )
             raise ValidationError(
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/api/v1/schemas.py#L985'>rotkehlchen/api/v1/schemas.py~L985</a>
```diff
             data["order_by_attributes"]
         ).issubset(valid_ordering_attr):
             error_msg = (
-                f'order_by_attributes for asset movements can not be '
+                f"order_by_attributes for asset movements can not be "
                 f'{",".join(set(data["order_by_attributes"]) - valid_ordering_attr)}'
             )
             raise ValidationError(
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/api/v1/schemas.py#L1179'>rotkehlchen/api/v1/schemas.py~L1179</a>
```diff
         raise ValidationError(
             f'Invalid current price oracles in: {", ".join(oracle_names)}. '
             f'Supported oracles are: {", ".join(supported_oracle_names)}. '
-            f'Check there are no repeated ones.',
+            f"Check there are no repeated ones.",
         )
 
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/api/v1/schemas.py#L1195'>rotkehlchen/api/v1/schemas.py~L1195</a>
```diff
         raise ValidationError(
             f'Invalid historical price oracles in: {", ".join(oracle_names)}. '
             f'Supported oracles are: {", ".join(supported_oracle_names)}. '
-            f'Check there are no repeated ones.',
+            f"Check there are no repeated ones.",
         )
 
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/api/v1/schemas.py#L1685'>rotkehlchen/api/v1/schemas.py~L1685</a>
```diff
             data["order_by_attributes"]
         ).issubset(valid_ordering_attr):
             error_msg = (
-                f'order_by_attributes for accounting report data can not be '
+                f"order_by_attributes for accounting report data can not be "
                 f'{",".join(set(data["order_by_attributes"]) - valid_ordering_attr)}'
             )
             raise ValidationError(
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/api/v1/schemas.py#L2758'>rotkehlchen/api/v1/schemas.py~L2758</a>
```diff
             data["order_by_attributes"]
         ).issubset(valid_ordering_attr):
             error_msg = (
-                f'order_by_attributes for eth2 daily stats can not be '
+                f"order_by_attributes for eth2 daily stats can not be "
                 f'{",".join(set(data["order_by_attributes"]) - valid_ordering_attr)}'
             )
             raise ValidationError(
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/api/v1/schemas.py#L3081'>rotkehlchen/api/v1/schemas.py~L3081</a>
```diff
         ):  # noqa: E501
             raise ValidationError(
                 f'timestamp provided {data["timestamp"]} is not the same as the '
-                f'one for the entries provided.',
+                f"one for the entries provided.",
             )
 
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/chain/aggregator.py#L780'>rotkehlchen/chain/aggregator.py~L780</a>
```diff
         unknown_accounts = set(accounts).difference(self.accounts.get(blockchain))
         if len(unknown_accounts) != 0:
             raise InputError(
-                f'Tried to remove unknown {blockchain.value} '
+                f"Tried to remove unknown {blockchain.value} "
                 f'accounts {",".join(unknown_accounts)}',
             )
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/chain/ethereum/abi.py#L89'>rotkehlchen/chain/ethereum/abi.py~L89</a>
```diff
     duplicate_names = set(log_topic_names).intersection(log_data_names)
     if duplicate_names:
         raise DeserializationError(
-            f'The following argument names are duplicated '
+            f"The following argument names are duplicated "
             f"between event inputs: '{', '.join(duplicate_names)}'",
         )
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/chain/ethereum/modules/nft/nfts.py#L293'>rotkehlchen/chain/ethereum/modules/nft/nfts.py~L293</a>
```diff
         with self.db.user_write() as write_cursor:
             # Remove NFTs that the user no longer owns from the DB cache
             write_cursor.execute(
-                f'DELETE FROM nfts WHERE owner_address IN '
+                f"DELETE FROM nfts WHERE owner_address IN "
                 f'({",".join("?" * len(addresses))}) AND identifier NOT IN '
                 f'({",".join("?" * len(fresh_nfts_identifiers))})',
                 tuple(addresses) + tuple(fresh_nfts_identifiers),
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/chain/evm/decoding/curve/curve_cache.py#L406'>rotkehlchen/chain/evm/decoding/curve/curve_cache.py~L406</a>
```diff
             raise RemoteError(f"Curve pool data {api_pool_data} are missing key {e}") from e
         except DeserializationError as e:
             log.error(
-                f'Could not deserialize evm address while decoding curve pool '
+                f"Could not deserialize evm address while decoding curve pool "
                 f'{api_pool_data["address"]} information from curve api: {e}',
             )
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/chain/substrate/manager.py#L110'>rotkehlchen/chain/substrate/manager.py~L110</a>
```diff
             return result
 
         raise RemoteError(
-            f'{manager.chain} request failed after trying the following nodes: '
+            f"{manager.chain} request failed after trying the following nodes: "
             f'{", ".join(requested_nodes)}',
         )
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/db/dbhandler.py#L1242'>rotkehlchen/db/dbhandler.py~L1242</a>
```diff
         """
         # Assure all are there
         accounts_number = write_cursor.execute(
-            f'SELECT COUNT(*) from blockchain_accounts WHERE blockchain = ? '
+            f"SELECT COUNT(*) from blockchain_accounts WHERE blockchain = ? "
             f'AND account IN ({",".join("?" * len(accounts))})',
             (blockchain.value, *accounts),
         ).fetchone()[0]
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/db/dbhandler.py#L2313'>rotkehlchen/db/dbhandler.py~L2313</a>
```diff
         ]
         for hashes_chunk in get_chunks(hashes_to_remove, n=1000):  # limit num of hashes in a query
             write_cursor.execute(  # delete transactions themselves
-                f'DELETE FROM zksynclite_transactions WHERE tx_hash IN '
+                f"DELETE FROM zksynclite_transactions WHERE tx_hash IN "
                 f'({",".join("?" * len(hashes_chunk))})',
                 hashes_chunk,
             )
             write_cursor.execute(
-                f'DELETE FROM history_events WHERE identifier IN (SELECT H.identifier '
-                f'FROM history_events H INNER JOIN evm_events_info E '
-                f'ON H.identifier=E.identifier AND E.tx_hash IN '
+                f"DELETE FROM history_events WHERE identifier IN (SELECT H.identifier "
+                f"FROM history_events H INNER JOIN evm_events_info E "
+                f"ON H.identifier=E.identifier AND E.tx_hash IN "
                 f'({", ".join(["?"] * len(hashes_chunk))}) AND H.location=?)',
                 hashes_chunk + [Location.ZKSYNC_LITE.serialize_for_db()],
             )
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/db/dbhandler.py#L3152'>rotkehlchen/db/dbhandler.py~L3152</a>
```diff
 
         if len(unknown_tags) != 0:
             raise TagConstraintError(
-                f'When {action} {data_type}, unknown tags '
+                f"When {action} {data_type}, unknown tags "
                 f'{", ".join(unknown_tags)} were found',
             )
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/db/dbhandler.py#L3806'>rotkehlchen/db/dbhandler.py~L3806</a>
```diff
         result = {}
         with self.conn.read_ctx() as cursor:
             cursor.execute(
-                f'SELECT identifier, name, collection_name, image_url FROM nfts WHERE '
+                f"SELECT identifier, name, collection_name, image_url FROM nfts WHERE "
                 f'identifier IN ({",".join("?" * len(identifiers))})',
                 identifiers,
             )
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/db/eth2.py#L221'>rotkehlchen/db/eth2.py~L221</a>
```diff
         """
         questionmarks = "?" * len(addresses)
         cursor.execute(
-            f'SELECT S.validator_index FROM eth_staking_events_info S LEFT JOIN '
-            f'history_events H on S.identifier=H.identifier WHERE H.location_label IN '
+            f"SELECT S.validator_index FROM eth_staking_events_info S LEFT JOIN "
+            f"history_events H on S.identifier=H.identifier WHERE H.location_label IN "
             f'({",".join(questionmarks)})',
             addresses,
         )
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/db/eth2.py#L353'>rotkehlchen/db/eth2.py~L353</a>
```diff
         with self.db.user_write() as cursor:
             # Delete from the validators table. This should also delete from daily_staking_details
             cursor.execute(
-                f'DELETE FROM eth2_validators WHERE validator_index IN '
+                f"DELETE FROM eth2_validators WHERE validator_index IN "
                 f'({",".join(question_marks)})',
                 validator_indices,
             )
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/db/eth2.py#L366'>rotkehlchen/db/eth2.py~L366</a>
```diff
             # Delete from the events table, all staking events except for deposits.
             # We keep deposits since they are associated with the address and are EVM transactions
             cursor.execute(
-                f'DELETE FROM history_events WHERE identifier in (SELECT S.identifier '
-                f'FROM eth_staking_events_info S WHERE S.validator_index IN '
+                f"DELETE FROM history_events WHERE identifier in (SELECT S.identifier "
+                f"FROM eth_staking_events_info S WHERE S.validator_index IN "
                 f'({",".join(question_marks)})) AND entry_type != ?',
                 (*validator_indices, HistoryBaseEntryType.ETH_DEPOSIT_EVENT.serialize_for_db()),
             )
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/db/unresolved_conflicts.py#L111'>rotkehlchen/db/unresolved_conflicts.py~L111</a>
```diff
 
             # also query the linked rules information
             cursor.execute(
-                'SELECT accounting_rule, property_name, setting_name FROM '
+                "SELECT accounting_rule, property_name, setting_name FROM "
                 f'linked_rules_properties WHERE accounting_rule IN ({",".join("?" * len(id_to_data))})',  # noqa: E501
                 list(id_to_data.keys()),
             )
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/db/updates.py#L277'>rotkehlchen/db/updates.py~L277</a>
```diff
             if single_contract_data["abi"] not in remote_id_to_local_id:
                 self.msg_aggregator.add_error(
                     f'ABI with id {single_contract_data["abi"]} was missing in a contracts '
-                    f'update. Please report it to the rotki team.',
+                    f"update. Please report it to the rotki team.",
                 )
                 continue
             new_contracts_data.append((
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/db/updates.py#L310'>rotkehlchen/db/updates.py~L310</a>
```diff
                 self.msg_aggregator.add_error(
                     f'Could not deserialize address {raw_entry["address"]} or blockchain '
                     f'{raw_entry["blockchain"]} that was seen in a global addressbook update. '
-                    f'Please report it to the rotki team. {e!s}',
+                    f"Please report it to the rotki team. {e!s}",
                 )
                 continue
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/exchanges/binance.py#L141'>rotkehlchen/exchanges/binance.py~L141</a>
```diff
     rate = deserialize_price(binance_trade["price"])
     if binance_trade["symbol"] not in binance_symbols_to_pair:
         raise DeserializationError(
-            f'Error reading a {location!s} trade. Could not find '
+            f"Error reading a {location!s} trade. Could not find "
             f'{binance_trade["symbol"]} in binance_symbols_to_pair',
         )
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/exchanges/binance.py#L524'>rotkehlchen/exchanges/binance.py~L524</a>
```diff
             except DeserializationError:
                 log.error(
                     f'Found {self.name} asset with non-string type {type(entry["asset"])}. '
-                    f'Ignoring its balance query.',
+                    f"Ignoring its balance query.",
                 )
                 continue
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/exchanges/binance.py#L959'>rotkehlchen/exchanges/binance.py~L959</a>
```diff
                     continue
                 except DeserializationError:
                     log.error(
-                        f'Found {self.name} asset with non-string type '
+                        f"Found {self.name} asset with non-string type "
                         f'{type(entry["asset"])}. Ignoring its futures balance query.',
                     )
                     continue
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/exchanges/binance.py#L1039'>rotkehlchen/exchanges/binance.py~L1039</a>
```diff
                     continue
                 except DeserializationError:
                     log.error(
-                        f'Found {self.name} asset with non-string type '
+                        f"Found {self.name} asset with non-string type "
                         f'{type(entry["asset"])}. Ignoring its margined futures balance query.',
                     )
                     continue
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/exchanges/bitfinex.py#L395'>rotkehlchen/exchanges/bitfinex.py~L395</a>
```diff
 
             if raw_result[timestamp_index] > options["end"]:
                 log.debug(
-                    f'Unexpected result requesting {self.name} {case}. '
-                    f'Result timestamp {raw_result[timestamp_index]} is greater than '
+                    f"Unexpected result requesting {self.name} {case}. "
+                    f"Result timestamp {raw_result[timestamp_index]} is greater than "
                     f'end filter {options["end"]}. Stop requesting.',
                     raw_result=raw_result,
                 )
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/exchanges/bitstamp.py#L474'>rotkehlchen/exchanges/bitstamp.py~L474</a>
```diff
         payload_string = urlencode(call_options)
         content_type = "" if payload_string == "" else "application/x-www-form-urlencoded"
         message = (
-            'BITSTAMP '
-            f'{self.api_key}'
-            f'{method.upper()}'
+            "BITSTAMP "
+            f"{self.api_key}"
+            f"{method.upper()}"
             f'{request_url.replace("https://", "")}'
-            f'{query_params}'
-            f'{content_type}'
-            f'{nonce}'
-            f'{timestamp}'
-            'v2'
-            f'{payload_string}'
+            f"{query_params}"
+            f"{content_type}"
+            f"{nonce}"
+            f"{timestamp}"
+            "v2"
+            f"{payload_string}"
         )
         signature = hmac.new(
             self.secret,
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/exchanges/coinbase.py#L355'>rotkehlchen/exchanges/coinbase.py~L355</a>
```diff
 
             if not isinstance(account_data["id"], str):
                 log.error(
-                    f'Found coinbase account entry with a non string id: '
+                    f"Found coinbase account entry with a non string id: "
                     f'{account_data["id"]}. Skipping it. ',
                 )
                 continue
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/exchanges/coinbase.py#L933'>rotkehlchen/exchanges/coinbase.py~L933</a>
```diff
                     if asset != asset_from_coinbase(fee_asset, time=timestamp):
                         # If not we set ZERO fee and ignore
                         log.error(
-                            f'In a coinbase withdrawal of {asset.identifier} the fee'
+                            f"In a coinbase withdrawal of {asset.identifier} the fee"
                             f'is denoted in {raw_fee["currency"]}',
                         )
                     else:
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/exchanges/kraken.py#L503'>rotkehlchen/exchanges/kraken.py~L503</a>
```diff
 
             if count != response["count"]:
                 log.error(
-                    f'Kraken unexpected response while querying endpoint for period. '
+                    f"Kraken unexpected response while querying endpoint for period. "
                     f'Original count was {count} and response returned {response["count"]}',
                 )
                 with_errors = True
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/externalapis/coingecko.py#L840'>rotkehlchen/externalapis/coingecko.py~L840</a>
```diff
                 if entry["id"] in data:
                     log.warning(
                         f'Found duplicate coingecko identifier {entry["id"]} when querying '
-                        f'the list of coingecko assets. Ignoring...',
+                        f"the list of coingecko assets. Ignoring...",
                     )
                     continue
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/externalapis/cryptocompare.py#L283'>rotkehlchen/externalapis/cryptocompare.py~L283</a>
```diff
         can_query = got_cached_data or not rate_limited
         log.debug(
             f'{"Will" if can_query else "Will not"} query '
-            f'Cryptocompare history for {from_asset.identifier} -> '
-            f'{to_asset.identifier} @ {timestamp}. Cached data: {got_cached_data}'
-            f' rate_limited in last {seconds} seconds: {rate_limited}',
+            f"Cryptocompare history for {from_asset.identifier} -> "
+            f"{to_asset.identifier} @ {timestamp}. Cached data: {got_cached_data}"
+            f" rate_limited in last {seconds} seconds: {rate_limited}",
         )
         return can_query
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/externalapis/etherscan.py#L557'>rotkehlchen/externalapis/etherscan.py~L557</a>
```diff
                 except DeserializationError as e:
                     log.error(
                         f"Failed to read transaction timestamp {entry['hash']} from {self.chain} "
-                        f'etherscan for {account} in the range {from_ts} to {to_ts}. {e!s}',
+                        f"etherscan for {account} in the range {from_ts} to {to_ts}. {e!s}",
                     )
                     continue
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/externalapis/etherscan.py#L570'>rotkehlchen/externalapis/etherscan.py~L570</a>
```diff
                 except DeserializationError as e:
                     log.error(
                         f"Failed to read transaction hash {entry['hash']} from {self.chain} "
-                        f'etherscan for {account} in the range {from_ts} to {to_ts}. {e!s}',
+                        f"etherscan for {account} in the range {from_ts} to {to_ts}. {e!s}",
                     )
                     continue
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/externalapis/gnosispay.py#L225'>rotkehlchen/externalapis/gnosispay.py~L225</a>
```diff
         """Return the gnosis pay data matching the given DB data"""
         with self.database.conn.read_ctx() as cursor:
             cursor.execute(
-                f'SELECT tx_hash, timestamp, merchant_name, merchant_city, country, mcc, '
-                f'transaction_symbol, transaction_amount, billing_symbol, billing_amount, '
-                f'reversal_symbol, reversal_amount, reversal_tx_hash '
+                f"SELECT tx_hash, timestamp, merchant_name, merchant_city, country, mcc, "
+                f"transaction_symbol, transaction_amount, billing_symbol, billing_amount, "
+                f"reversal_symbol, reversal_amount, reversal_tx_hash "
                 f'{", identifier " if with_identifier else ""}'
-                f'FROM gnosispay_data WHERE {wherestatement}',
+                f"FROM gnosispay_data WHERE {wherestatement}",
                 bindings,
             )
             if (result := cursor.fetchone()) is None:
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/externalapis/opensea.py#L295'>rotkehlchen/externalapis/opensea.py~L295</a>
```diff
                 else:  # should not happen. That means collections endpoint doesnt return anything
                     raise DeserializationError(
                         f'Could not find collection {entry["collection"]} in opensea collections '
-                        f'endpoint',
+                        f"endpoint",
                     )
 
             last_price_in_usd = last_price_in_eth * eth_usd_price
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/globaldb/assets_management.py#L40'>rotkehlchen/globaldb/assets_management.py~L40</a>
```diff
 
     if int(data["version"]) not in ASSETS_FILE_IMPORT_ACCEPTED_GLOBALDB_VERSIONS:
         raise InputError(
-            f'Provided file is for a different version of rotki. GlobalDB File version: '
+            f"Provided file is for a different version of rotki. GlobalDB File version: "
             f'{data["version"]} Accepted GlobalDB version by rotki: {ASSETS_FILE_IMPORT_ACCEPTED_GLOBALDB_VERSIONS}',  # noqa: E501
         )
     if data["assets"] is None:
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/globaldb/handler.py#L445'>rotkehlchen/globaldb/handler.py~L445</a>
```diff
             # results before hand, in the range from 10 to 100 and this guarantees that the size of
             # the query is small.
             underlying_tokens_query = (
-                f'SELECT parent_token_entry, address, token_kind, weight FROM '
-                f'underlying_tokens_list LEFT JOIN evm_tokens ON '
-                f'underlying_tokens_list.identifier=evm_tokens.identifier '
+                f"SELECT parent_token_entry, address, token_kind, weight FROM "
+                f"underlying_tokens_list LEFT JOIN evm_tokens ON "
+                f"underlying_tokens_list.identifier=evm_tokens.identifier "
                 f'WHERE parent_token_entry IN ({",".join(["?"] * len(assets_info))})'
             )
             # populate all underlying tokens
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/globaldb/handler.py#L1210'>rotkehlchen/globaldb/handler.py~L1210</a>
```diff
                 )
         except sqlite3.IntegrityError as e:
             log.error(
-                f'One of the following asset ids caused a DB IntegrityError ({e!s}): '
+                f"One of the following asset ids caused a DB IntegrityError ({e!s}): "
                 f'{",".join([x.identifier for x in assets])}',
             )  # should not ever happen but need to handle with informative log if it does
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/globaldb/upgrades/v2_v3.py#L356'>rotkehlchen/globaldb/upgrades/v2_v3.py~L356</a>
```diff
         "KSM",
     )
     cursor.execute(
-        f'SELECT from_asset, to_asset, source_type, timestamp, price FROM '
+        f"SELECT from_asset, to_asset, source_type, timestamp, price FROM "
         f'price_history WHERE (source_type=="A" OR from_asset IN ({",".join(["?"] * len(assets))}))',  # noqa: E501
         assets,
     )
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/greenlets/manager.py#L76'>rotkehlchen/greenlets/manager.py~L76</a>
```diff
             return
 
         msg = (
-            f'{first_line}.\n'
-            f'Exception Name: {greenlet.exc_info[0]}\nException Info: {greenlet.exc_info[1]}'
+            f"{first_line}.\n"
+            f"Exception Name: {greenlet.exc_info[0]}\nException Info: {greenlet.exc_info[1]}"
             f'\nTraceback:\n {"".join(traceback.format_tb(greenlet.exc_info[2]))}'
         )
         log.error(msg)
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/history/manager.py#L268'>rotkehlchen/history/manager.py~L268</a>
```diff
 
             if len(exchange_names) != 0:
                 self.msg_aggregator.add_error(
-                    f'Failed to query some events from {location.name} exchanges '
+                    f"Failed to query some events from {location.name} exchanges "
                     f'{",".join(exchange_names)}',
                 )
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/serialization/schemas.py#L230'>rotkehlchen/serialization/schemas.py~L230</a>
```diff
             if weight_sum > ONE:
                 raise ValidationError(
                     f'The sum of underlying token weights for {data["address"]} '
-                    f'is {weight_sum * 100} and exceeds 100%',
+                    f"is {weight_sum * 100} and exceeds 100%",
                 )
             if weight_sum < ONE:
                 raise ValidationError(
                     f'The sum of underlying token weights for {data["address"]} '
-                    f'is {weight_sum * 100} and does not add up to 100%',
+                    f"is {weight_sum * 100} and does not add up to 100%",
                 )
 
     @post_load
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/tasks/manager.py#L1100'>rotkehlchen/tasks/manager.py~L1100</a>
```diff
         current_greenlets = len(self.greenlet_manager.greenlets) + len(self.api_task_greenlets)
         not_proceed = current_greenlets >= self.max_tasks_num
         log.debug(
-            f'At task scheduling. Current greenlets: {current_greenlets} '
-            f'Max greenlets: {self.max_tasks_num}. '
+            f"At task scheduling. Current greenlets: {current_greenlets} "
+            f"Max greenlets: {self.max_tasks_num}. "
             f'{"Will not schedule" if not_proceed else "Will schedule"}.',
         )
         if not_proceed:
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/tests/exchanges/test_coinbase.py#L1193'>rotkehlchen/tests/exchanges/test_coinbase.py~L1193</a>
```diff
             test_warnings.warn(
                 UserWarning(
                     f'Found unknown asset {e.identifier} with symbol {coin["id"]} in Coinbase. '
-                    f'Support for it has to be added',
+                    f"Support for it has to be added",
                 )
             )
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py#L148'>rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py~L148</a>
```diff
             identifiers[0]
             for cursor in (old_db_cursor, packaged_db_cursor)
             for identifiers in cursor.execute(
-                f'SELECT identifier FROM evm_tokens WHERE '
+                f"SELECT identifier FROM evm_tokens WHERE "
                 f'protocol IN ({",".join(["?"] * len(IGNORED_PROTOCOLS))}) OR '
-                f'address in (SELECT value FROM general_cache)',
+                f"address in (SELECT value FROM general_cache)",
                 list(IGNORED_PROTOCOLS),
             ).fetchall()
         }
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/tests/unit/test_data_updates.py#L730'>rotkehlchen/tests/unit/test_data_updates.py~L730</a>
```diff
     ):
         result = cursor.execute(  # additions are not present already
             f"SELECT {'COUNT(*)' if after_upgrade is False else 'local_id'} "
-            'FROM location_asset_mappings WHERE location IS ? AND exchange_symbol IS ?',
+            "FROM location_asset_mappings WHERE location IS ? AND exchange_symbol IS ?",
             (
                 None
                 if addition["location"] is None
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/tests/utils/ethereum.py#L261'>rotkehlchen/tests/utils/ethereum.py~L261</a>
```diff
         names = [str(x) for idx, x in enumerate(connect_at_start) if not connected[idx]]
         log.warning(
             f'Did not connect to nodes: {",".join(names)} due to '
-            f'timeout of {NODE_CONNECTION_TIMEOUT}. Connected to {connected}',
+            f"timeout of {NODE_CONNECTION_TIMEOUT}. Connected to {connected}",
         )
 
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/tests/utils/substrate.py#L98'>rotkehlchen/tests/utils/substrate.py~L98</a>
```diff
     not_connected_nodes = set(node_names) - connected_nodes
     if not_connected_nodes:
         log.info(
-            f'Substrate {chain} tests failed to connect to nodes: '
+            f"Substrate {chain} tests failed to connect to nodes: "
             f'{",".join([str(node) for node in not_connected_nodes])} ',
         )
 
```
<a href='https://github.com/rotki/rotki/blob/e0081893a774795406bac3510ff24a68aded2ea2/rotkehlchen/tests/utils/substrate.py#L124'>rotkehlchen/tests/utils/substrate.py~L124</a>
```diff
     except gevent.Timeout:
         not_connected_nodes = all_nodes - connected
         log.info(
-            f'{substrate_manager.chain} manager failed to connect to '
+            f"{substrate_manager.chain} manager failed to connect to "
             f'nodes: {",".join([str(node) for node in not_connected_nodes])} '
-            f'due to timeout of {NODE_CONNECTION_TIMEOUT}',
+            f"due to timeout of {NODE_CONNECTION_TIMEOUT}",
         )
```

</p>
</details>




---

_Comment by @MichaReiser on 2024-09-27 15:36_

Uff, I might have to gate this behind preview :( I'm not sure why this isn't local to f-string formatting only. 

Black uses the same layout as this PR proposes.

---

_Comment by @MichaReiser on 2024-09-27 15:38_

I believe this is related to https://github.com/astral-sh/ruff/issues/13237 because the formatter always preserves the quotes if the expression contains a quote anywhere (even inside a string). This seems odd to me

@dhruvmanila could you help me understand why this is in place?

---

_Comment by @MichaReiser on 2024-09-27 15:49_

This might actually be a bug in ruff. For example, it fails to normalise the quotes for

```python
"aaaa" f'bbbbb{'c'}b' 
```

Where black handles this successfully. 

---

_Comment by @dhruvmanila on 2024-10-01 12:20_

I'm looking into this...

---

_Comment by @dhruvmanila on 2024-10-01 12:31_

> This might actually be a bug in ruff. For example, it fails to normalise the quotes for

Yeah, this seems like a bug and I think it's the same as #13237 

---

_Comment by @dhruvmanila on 2024-10-01 12:41_

> @dhruvmanila could you help me understand why this is in place?

Based on the previous comment (https://github.com/astral-sh/ruff/pull/9058#discussion_r1425727018), I think it was so that the quoting effect remains same throughout the f-string but I'm realizing now that it doesn't matter. For example, I'm guessing that my thought process was to keep this formatted as single quote for both string and f-string (`'foo' f'bar {"x"}'`) even though we can change the string quotes.

---

_Renamed from "refactor: Remove `StringLiteralKind::InImplicitConcatenatedFString` kind" to "Normalize string literal quotes in implicit concatenated f-strings containing a quote character" by @MichaReiser on 2024-10-07 13:34_

---

_Renamed from "Normalize string literal quotes in implicit concatenated f-strings containing a quote character" to "Normalize implicit concatenated f-string quotes per part" by @MichaReiser on 2024-10-07 14:14_

---

_@MichaReiser reviewed on 2024-10-07 14:16_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/preview.rs`:23 on 2024-10-07 14:16_

I created a new preview style for now in case we push out the f-string stabilization. We can merge this change with the f-string preview style if we decide to ship both changes.

---

_Marked ready for review by @MichaReiser on 2024-10-07 14:38_

---

_Label `internal` removed by @MichaReiser on 2024-10-07 14:38_

---

_Label `formatter` added by @MichaReiser on 2024-10-07 14:38_

---

_Label `preview` added by @MichaReiser on 2024-10-07 14:38_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/generate.py`:38 on 2024-10-08 04:15_

You might want to update this comment or maybe generalize it to state that formatting for these nodes are handled by another node.

---

_@dhruvmanila approved on 2024-10-08 04:21_

Looks good, thanks for fixing this

---

_Merged by @MichaReiser on 2024-10-08 09:59_

---

_Closed by @MichaReiser on 2024-10-08 09:59_

---

_Branch deleted on 2024-10-08 09:59_

---
