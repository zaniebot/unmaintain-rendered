```yaml
number: 22744
title: "Stabilize `parenthesize_lambda_bodies`"
type: pull_request
state: open
author: dylwil3
labels:
  - breaking
  - formatter
assignees: []
base: 2026-style
head: stabilize-parenthesize_lambda_bodies
created_at: 2026-01-19T20:50:23Z
updated_at: 2026-01-19T21:08:03Z
url: https://github.com/astral-sh/ruff/pull/22744
synced_at: 2026-01-19T21:33:07Z
```

# Stabilize `parenthesize_lambda_bodies`

---

_@dylwil3_

_No description provided._

---

_Review requested from @MichaReiser by @dylwil3 on 2026-01-19 20:50_

---

_Label `breaking` added by @dylwil3 on 2026-01-19 20:50_

---

_Label `formatter` added by @dylwil3 on 2026-01-19 20:50_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 20:58_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+160 -145 lines in 31 files in 10 projects; 45 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+6 -6 lines across 2 files)</summary>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/extractors/crf_entity_extractor.py#L101'>rasa/nlu/extractors/crf_entity_extractor.py~L101</a>
```diff
         CRFEntityExtractorOptions.SUFFIX1: lambda crf_token: crf_token.text[-1:],
         CRFEntityExtractorOptions.BIAS: lambda _: "bias",
         CRFEntityExtractorOptions.POS: lambda crf_token: crf_token.pos_tag,
-        CRFEntityExtractorOptions.POS2: lambda crf_token: crf_token.pos_tag[:2]
-        if crf_token.pos_tag is not None
-        else None,
+        CRFEntityExtractorOptions.POS2: lambda crf_token: (
+            crf_token.pos_tag[:2] if crf_token.pos_tag is not None else None
+        ),
         CRFEntityExtractorOptions.UPPER: lambda crf_token: crf_token.text.isupper(),
         CRFEntityExtractorOptions.DIGIT: lambda crf_token: crf_token.text.isdigit(),
         CRFEntityExtractorOptions.PATTERN: lambda crf_token: crf_token.pattern,
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py#L86'>rasa/nlu/featurizers/sparse_featurizer/lexical_syntactic_featurizer.py~L86</a>
```diff
         "suffix2": lambda token: token.text[-2:],
         "suffix1": lambda token: token.text[-1:],
         "pos": lambda token: token.data.get(POS_TAG_KEY, None),
-        "pos2": lambda token: token.data.get(POS_TAG_KEY, [])[:2]
-        if POS_TAG_KEY in token.data
-        else None,
+        "pos2": lambda token: (
+            token.data.get(POS_TAG_KEY, [])[:2] if POS_TAG_KEY in token.data else None
+        ),
         "upper": lambda token: token.text.isupper(),
         "digit": lambda token: token.text.isdigit(),
     }
```

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+15 -12 lines across 1 file)</summary>
<p>

<a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/samcli/lib/cli_validation/image_repository_validation.py#L72'>samcli/lib/cli_validation/image_repository_validation.py~L72</a>
```diff
 
             validators = [
                 Validator(
-                    validation_function=lambda: bool(image_repository)
-                    + bool(image_repositories)
-                    + bool(resolve_image_repos)
-                    > 1,
+                    validation_function=lambda: (
+                        bool(image_repository) + bool(image_repositories) + bool(resolve_image_repos) > 1
+                    ),
                     exception=click.BadOptionUsage(
                         option_name="--image-repositories",
                         ctx=ctx,
```
<a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/samcli/lib/cli_validation/image_repository_validation.py#L84'>samcli/lib/cli_validation/image_repository_validation.py~L84</a>
```diff
                     ),
                 ),
                 Validator(
-                    validation_function=lambda: not guided
-                    and not (image_repository or image_repositories or resolve_image_repos)
-                    and required,
+                    validation_function=lambda: (
+                        not guided and not (image_repository or image_repositories or resolve_image_repos) and required
+                    ),
                     exception=click.BadOptionUsage(
                         option_name="--image-repositories",
                         ctx=ctx,
```
<a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/samcli/lib/cli_validation/image_repository_validation.py#L94'>samcli/lib/cli_validation/image_repository_validation.py~L94</a>
```diff
                     ),
                 ),
                 Validator(
-                    validation_function=lambda: not guided
-                    and (
-                        image_repositories
-                        and not resolve_image_repos
-                        and not _is_all_image_funcs_provided(template_file, image_repositories, parameters_overrides)
+                    validation_function=lambda: (
+                        not guided
+                        and (
+                            image_repositories
+                            and not resolve_image_repos
+                            and not _is_all_image_funcs_provided(
+                                template_file, image_repositories, parameters_overrides
+                            )
+                        )
                     ),
                     exception=click.BadOptionUsage(
                         option_name="--image-repositories", ctx=ctx, message=image_repos_error_msg
```

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+30 -26 lines across 3 files)</summary>
<p>

<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/agent_fns/general.py#L83'>crazy_functions/agent_fns/general.py~L83</a>
```diff
             }
             kwargs.update(agent_kwargs)
             agent_handle = agent_cls(**kwargs)
-            agent_handle._print_received_message = (
-                lambda a, b: self.gpt_academic_print_override(agent_kwargs, a, b)
+            agent_handle._print_received_message = lambda a, b: (
+                self.gpt_academic_print_override(agent_kwargs, a, b)
             )
             for d in agent_handle._reply_func_list:
                 if (
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/agent_fns/general.py#L93'>crazy_functions/agent_fns/general.py~L93</a>
```diff
                 ):
                     d["reply_func"] = gpt_academic_generate_oai_reply
             if agent_kwargs["name"] == "user_proxy":
-                agent_handle.get_human_input = (
-                    lambda a: self.gpt_academic_get_human_input(user_proxy, a)
+                agent_handle.get_human_input = lambda a: (
+                    self.gpt_academic_get_human_input(user_proxy, a)
                 )
                 user_proxy = agent_handle
             if agent_kwargs["name"] == "assistant":
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/agent_fns/general.py#L134'>crazy_functions/agent_fns/general.py~L134</a>
```diff
                 kwargs = {"code_execution_config": code_execution_config}
                 kwargs.update(agent_kwargs)
                 agent_handle = agent_cls(**kwargs)
-                agent_handle._print_received_message = (
-                    lambda a, b: self.gpt_academic_print_override(agent_kwargs, a, b)
+                agent_handle._print_received_message = lambda a, b: (
+                    self.gpt_academic_print_override(agent_kwargs, a, b)
                 )
                 agents_instances.append(agent_handle)
                 if agent_kwargs["name"] == "user_proxy":
                     user_proxy = agent_handle
-                    user_proxy.get_human_input = (
-                        lambda a: self.gpt_academic_get_human_input(user_proxy, a)
+                    user_proxy.get_human_input = lambda a: (
+                        self.gpt_academic_get_human_input(user_proxy, a)
                     )
             try:
                 groupchat = autogen.GroupChat(
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/agent_fns/general.py#L150'>crazy_functions/agent_fns/general.py~L150</a>
```diff
                 manager = autogen.GroupChatManager(
                     groupchat=groupchat, **self.define_group_chat_manager_config()
                 )
-                manager._print_received_message = (
-                    lambda a, b: self.gpt_academic_print_override(agent_kwargs, a, b)
+                manager._print_received_message = lambda a, b: (
+                    self.gpt_academic_print_override(agent_kwargs, a, b)
                 )
                 manager.get_human_input = lambda a: self.gpt_academic_get_human_input(
                     manager, a
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/crazy_utils.py#L299'>crazy_functions/crazy_utils.py~L299</a>
```diff
         retry_op = retry_times_at_unknown_error
         exceeded_cnt = 0
         mutable[index][2] = "执行中"
-        detect_timeout = (
-            lambda: len(mutable[index]) >= 2
+        detect_timeout = lambda: (
+            len(mutable[index]) >= 2
             and (time.time() - mutable[index][1]) > watch_dog_patience
         )
         while True:
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/review_fns/paper_processor/paper_llm_ranker.py#L143'>crazy_functions/review_fns/paper_processor/paper_llm_ranker.py~L143</a>
```diff
                     )
                 elif search_criteria.query_type == "review":
                     papers.sort(
-                        key=lambda x: 1
-                        if any(
-                            keyword in (getattr(x, "title", "") or "").lower()
-                            or keyword in (getattr(x, "abstract", "") or "").lower()
-                            for keyword in ["review", "survey", "overview"]
-                        )
-                        else 0,
+                        key=lambda x: (
+                            1
+                            if any(
+                                keyword in (getattr(x, "title", "") or "").lower()
+                                or keyword in (getattr(x, "abstract", "") or "").lower()
+                                for keyword in ["review", "survey", "overview"]
+                            )
+                            else 0
+                        ),
                         reverse=True,
                     )
             return papers[:top_k]
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/review_fns/paper_processor/paper_llm_ranker.py#L164'>crazy_functions/review_fns/paper_processor/paper_llm_ranker.py~L164</a>
```diff
         if search_criteria and search_criteria.query_type == "review":
             papers = sorted(
                 papers,
-                key=lambda x: 1
-                if any(
-                    keyword in (getattr(x, "title", "") or "").lower()
-                    or keyword in (getattr(x, "abstract", "") or "").lower()
-                    for keyword in ["review", "survey", "overview"]
-                )
-                else 0,
+                key=lambda x: (
+                    1
+                    if any(
+                        keyword in (getattr(x, "title", "") or "").lower()
+                        or keyword in (getattr(x, "abstract", "") or "").lower()
+                        for keyword in ["review", "survey", "overview"]
+                    )
+                    else 0
+                ),
                 reverse=True,
             )
 
```

</p>
</details>
<details><summary><a href="https://github.com/qdrant/qdrant-client">qdrant/qdrant-client</a> (+14 -12 lines across 2 files)</summary>
<p>

<a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/tests/congruence_tests/test_common.py#L331'>tests/congruence_tests/test_common.py~L331</a>
```diff
 
     if isinstance(res1, list):
         if is_context_search is True:
-            sorted_1 = sorted(res1, key=lambda x: (x.id))
-            sorted_2 = sorted(res2, key=lambda x: (x.id))
+            sorted_1 = sorted(res1, key=lambda x: x.id)
+            sorted_2 = sorted(res2, key=lambda x: x.id)
             compare_records(sorted_1, sorted_2, abs_tol=1e-5)
         else:
             compare_records(res1, res2)
```
<a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/tests/congruence_tests/test_common.py#L340'>tests/congruence_tests/test_common.py~L340</a>
```diff
         res2, models.QueryResponse
     ):
         if is_context_search is True:
-            sorted_1 = sorted(res1.points, key=lambda x: (x.id))
-            sorted_2 = sorted(res2.points, key=lambda x: (x.id))
+            sorted_1 = sorted(res1.points, key=lambda x: x.id)
+            sorted_2 = sorted(res2.points, key=lambda x: x.id)
             compare_records(sorted_1, sorted_2, abs_tol=1e-5)
         else:
             compare_records(res1.points, res2.points)
```
<a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/tests/congruence_tests/test_delete_points.py#L70'>tests/congruence_tests/test_delete_points.py~L70</a>
```diff
     compare_client_results(
         local_client,
         remote_client,
-        lambda c: c.query_points(
-            COLLECTION_NAME,
-            query=vector,
-            using="sparse-image",
-        ).points,
+        lambda c: (
+            c.query_points(
+                COLLECTION_NAME,
+                query=vector,
+                using="sparse-image",
+            ).points
+        ),
     )
 
     found_ids = [
```
<a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/tests/congruence_tests/test_delete_points.py#L92'>tests/congruence_tests/test_delete_points.py~L92</a>
```diff
     compare_client_results(
         local_client,
         remote_client,
-        lambda c: c.query_points(
-            COLLECTION_NAME, query=vector, using="sparse-image"
-        ).points,
+        lambda c: (
+            c.query_points(COLLECTION_NAME, query=vector, using="sparse-image").points
+        ),
     )
```

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+17 -19 lines across 2 files)</summary>
<p>

<a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/app.py#L2113'>reflex/app.py~L2113</a>
```diff
             )
             # Don't await to avoid blocking disconnect, but handle potential errors
             task.add_done_callback(
-                lambda t: t.exception()
-                and console.error(f"Token cleanup error: {t.exception()}")
+                lambda t: (
+                    t.exception()
+                    and console.error(f"Token cleanup error: {t.exception()}")
+                )
             )
             return task
         return None
```
<a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/reflex.py#L803'>reflex/reflex.py~L803</a>
```diff
         app_name=app_name,
         app_id=app_id,
         export_fn=(
-            lambda zip_dest_dir,
-            api_url,
-            deploy_url,
-            frontend,
-            backend,
-            upload_db,
-            zipping: export_utils.export(
-                zip_dest_dir=zip_dest_dir,
-                api_url=api_url,
-                deploy_url=deploy_url,
-                frontend=frontend,
-                backend=backend,
-                zipping=zipping,
-                loglevel=config.loglevel.subprocess_level(),
-                upload_db_file=upload_db,
-                backend_excluded_dirs=backend_excluded_dirs,
-                prerender_routes=ssr,
+            lambda zip_dest_dir, api_url, deploy_url, frontend, backend, upload_db, zipping: (
+                export_utils.export(
+                    zip_dest_dir=zip_dest_dir,
+                    api_url=api_url,
+                    deploy_url=deploy_url,
+                    frontend=frontend,
+                    backend=backend,
+                    zipping=zipping,
+                    loglevel=config.loglevel.subprocess_level(),
+                    upload_db_file=upload_db,
+                    backend_excluded_dirs=backend_excluded_dirs,
+                    prerender_routes=ssr,
+                )
             )
         ),
         regions=list(region),
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+50 -51 lines across 9 files)</summary>
<p>

<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/chain/evm/decoding/aave/v3/decoder.py#L460'>rotkehlchen/chain/evm/decoding/aave/v3/decoder.py~L460</a>
```diff
                 ordered_events=ordered_events,
                 interest_event_lookup=interest_event_lookup,
                 used_interest_event_ids=used_interest_event_ids,
-                match_fn=lambda primary,
-                secondary: (  # use symbols due to Monerium and its different versions  # noqa: E501
+                match_fn=lambda primary, secondary: (  # use symbols due to Monerium and its different versions  # noqa: E501
                     (
                         underlying_token := get_single_underlying_token(
                             primary.asset.resolve_to_evm_token()
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/chain/evm/decoding/balancer/decoder.py#L51'>rotkehlchen/chain/evm/decoding/balancer/decoder.py~L51</a>
```diff
             self,
             evm_inquirer=evm_inquirer,
             cache_type_to_check_for_freshness=BALANCER_CACHE_TYPE_MAPPING[counterparty],
-            query_data_method=lambda inquirer,
-            cache_type,
-            msg_aggregator,
-            reload_all: query_balancer_data(  # noqa: E501
-                inquirer=inquirer,
-                cache_type=cache_type,
-                protocol=counterparty,
-                msg_aggregator=msg_aggregator,
-                version=BALANCER_VERSION_MAPPING[counterparty],
-                reload_all=reload_all,
+            query_data_method=lambda inquirer, cache_type, msg_aggregator, reload_all: (
+                query_balancer_data(  # noqa: E501
+                    inquirer=inquirer,
+                    cache_type=cache_type,
+                    protocol=counterparty,
+                    msg_aggregator=msg_aggregator,
+                    version=BALANCER_VERSION_MAPPING[counterparty],
+                    reload_all=reload_all,
+                )
             ),
             read_data_from_cache_method=read_fn,
             chain_id=evm_inquirer.chain_id,
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/chain/evm/node_inquirer.py#L928'>rotkehlchen/chain/evm/node_inquirer.py~L928</a>
```diff
                     end_block = min(start_block + blocks_step, until_block)
                     try:
                         new_events = self._try_indexers(
-                            func=lambda indexer,
-                            start=start_block,
-                            end=end_block: indexer.get_logs(  # type: ignore[misc]  # noqa: E501
-                                chain_id=self.chain_id,
-                                contract_address=contract_address,
-                                topics=filter_args.get("topics", []),
-                                from_block=start,
-                                to_block=end,
-                                existing_events=events,
+                            func=lambda indexer, start=start_block, end=end_block: (
+                                indexer.get_logs(  # type: ignore[misc]  # noqa: E501
+                                    chain_id=self.chain_id,
+                                    contract_address=contract_address,
+                                    topics=filter_args.get("topics", []),
+                                    from_block=start,
+                                    to_block=end,
+                                    existing_events=events,
+                                )
                             )
                         )
                     except RemoteError as e:
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/chain/solana/node_inquirer.py#L356'>rotkehlchen/chain/solana/node_inquirer.py~L356</a>
```diff
         signatures = []
         while True:
             response: GetSignaturesForAddressResp = self.query(
-                method=lambda client,
-                _before=before,
-                _until=until: client.get_signatures_for_address(  # type: ignore[misc]  # noqa: E501
-                    account=Pubkey.from_string(address),
-                    limit=SIGNATURES_PAGE_SIZE,
-                    before=_before,
-                    until=_until,
+                method=lambda client, _before=before, _until=until: (
+                    client.get_signatures_for_address(  # type: ignore[misc]  # noqa: E501
+                        account=Pubkey.from_string(address),
+                        limit=SIGNATURES_PAGE_SIZE,
+                        before=_before,
+                        until=_until,
+                    )
                 ),
                 only_archive_nodes=True,
             )
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/data_import/importers/binance.py#L331'>rotkehlchen/data_import/importers/binance.py~L331</a>
```diff
 
         for rows_group in rows_grouped_by_fee.values():
             rows_group.sort(
-                key=lambda x: x["Change"]
-                if same_assets
-                else x["Change"] * price_at_timestamp[x["Coin"]],
+                key=lambda x: (
+                    x["Change"] if same_assets else x["Change"] * price_at_timestamp[x["Coin"]]
+                ),
                 reverse=True,
             )  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/globaldb/handler.py#L1192'>rotkehlchen/globaldb/handler.py~L1192</a>
```diff
                 entry.protocol,
             ),
             token_type="evm",
-            post_insert_callback=lambda: GlobalDBHandler._add_underlying_tokens(
-                write_cursor=write_cursor,
-                parent_token_identifier=entry.identifier,
-                underlying_tokens=entry.underlying_tokens,
-                chain_id=entry.chain_id,
-            )
-            if entry.underlying_tokens is not None
-            else None,
+            post_insert_callback=lambda: (
+                GlobalDBHandler._add_underlying_tokens(
+                    write_cursor=write_cursor,
+                    parent_token_identifier=entry.identifier,
+                    underlying_tokens=entry.underlying_tokens,
+                    chain_id=entry.chain_id,
+                )
+                if entry.underlying_tokens is not None
+                else None
+            ),
         )
 
     @staticmethod
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/rotkehlchen.py#L649'>rotkehlchen/rotkehlchen.py~L649</a>
```diff
                         ]
                     },  # noqa: E501
                 ),
-                extra_check_callback=lambda: cursor.execute(
-                    "SELECT COUNT(*) FROM user_added_solana_tokens"
-                ).fetchone()[0]
-                > 0,  # noqa: E501
+                extra_check_callback=lambda: (
+                    cursor.execute("SELECT COUNT(*) FROM user_added_solana_tokens").fetchone()[0]
+                    > 0
+                ),  # noqa: E501
             )
 
     def _logout(self) -> None:
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/tests/exchanges/test_cryptocom.py#L252'>rotkehlchen/tests/exchanges/test_cryptocom.py~L252</a>
```diff
         ),
         patch(
             target="rotkehlchen.inquirer.Inquirer.find_main_currency_price",
-            side_effect=lambda from_asset: (FVal(112000) if from_asset == A_BTC else ONE),
+            side_effect=lambda from_asset: FVal(112000) if from_asset == A_BTC else ONE,
         ),
     ):
         balances, msg = mock_cryptocom.query_balances()
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/tests/unit/test_solana.py#L313'>rotkehlchen/tests/unit/test_solana.py~L313</a>
```diff
             patch.object(
                 target=solana_manager.node_inquirer,
                 attribute="query",
-                side_effect=lambda method,
-                call_order=None,
-                only_archive_nodes=False,
-                endpoint=expected_endpoint: original_query(  # noqa: E501
-                    method=partial(check_client, method=method, expected_endpoint=endpoint),
-                    call_order=call_order,
-                    only_archive_nodes=only_archive_nodes,
+                side_effect=lambda method, call_order=None, only_archive_nodes=False, endpoint=expected_endpoint: (
+                    original_query(  # noqa: E501
+                        method=partial(check_client, method=method, expected_endpoint=endpoint),
+                        call_order=call_order,
+                        only_archive_nodes=only_archive_nodes,
+                    )
                 ),
             ) as query_mock,
         ):
```

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+9 -8 lines across 6 files)</summary>
<p>

<a href='https://github.com/indico/indico/blob/b84e870869d7ff91189ad797171625c0093e7fc6/indico/modules/events/forms.py#L117'>indico/modules/events/forms.py~L117</a>
```diff
     )
     category = CategoryField(
         _('Category'),
-        [UsedIf(lambda form, _: (form.listing.data or not can_create_unlisted_events(session.user))), DataRequired()],
+        [UsedIf(lambda form, _: form.listing.data or not can_create_unlisted_events(session.user)), DataRequired()],
         require_event_creation_rights=True,
         show_event_creation_warning=True,
     )
```
<a href='https://github.com/indico/indico/blob/b84e870869d7ff91189ad797171625c0093e7fc6/indico/modules/events/layout/forms.py#L134'>indico/modules/events/layout/forms.py~L134</a>
```diff
     theme = SelectField(
         _('Theme'),
         [Optional(), HiddenUnless('use_custom_css', False)],
-        coerce=lambda x: (x or None),
+        coerce=lambda x: x or None,
         description=_(
             'Currently selected theme of the conference page. Click on the Preview button to '
             'preview and select a different one.'
```
<a href='https://github.com/indico/indico/blob/b84e870869d7ff91189ad797171625c0093e7fc6/indico/modules/events/layout/forms.py#L274'>indico/modules/events/layout/forms.py~L274</a>
```diff
 
 
 class CSSSelectionForm(IndicoForm):
-    theme = SelectField(_('Theme'), [Optional()], coerce=lambda x: (x or None))
+    theme = SelectField(_('Theme'), [Optional()], coerce=lambda x: x or None)
 
     def __init__(self, *args, **kwargs):
         event = kwargs.pop('event')
```
<a href='https://github.com/indico/indico/blob/b84e870869d7ff91189ad797171625c0093e7fc6/indico/modules/events/management/forms.py#L69'>indico/modules/events/management/forms.py~L69</a>
```diff
 class EventDataForm(IndicoForm):
     title = StringField(_('Event title'), [DataRequired()])
     description = TextAreaField(_('Description'), widget=TinyMCEWidget(images=True, height=350))
-    url_shortcut = StringField(_('URL shortcut'), filters=[lambda x: (x or None)])
+    url_shortcut = StringField(_('URL shortcut'), filters=[lambda x: x or None])
 
     def __init__(self, *args, event, **kwargs):
         self.event = event
```
<a href='https://github.com/indico/indico/blob/b84e870869d7ff91189ad797171625c0093e7fc6/indico/modules/events/papers/schemas.py#L251'>indico/modules/events/papers/schemas.py~L251</a>
```diff
         lambda paper, ctx: editable_type_settings[EditableType.paper].get(paper.event, 'submission_enabled')
     )
     editing_enabled = Function(
-        lambda paper, ctx: paper.event.has_feature('editing')
-        and 'paper' in editing_settings.get(paper.event, 'editable_types')
+        lambda paper, ctx: (
+            paper.event.has_feature('editing') and 'paper' in editing_settings.get(paper.event, 'editable_types')
+        )
     )
 
 
```
<a href='https://github.com/indico/indico/blob/b84e870869d7ff91189ad797171625c0093e7fc6/indico/modules/events/registration/forms.py#L165'>indico/modules/events/registration/forms.py~L165</a>
```diff
     )
     currency = SelectField(_('Currency'), [DataRequired()], description=_('The currency for new registrations'))
     notification_sender_address = StringField(
-        _('Notification sender address'), [IndicoEmail()], filters=[lambda x: (x or None)]
+        _('Notification sender address'), [IndicoEmail()], filters=[lambda x: x or None]
     )
     message_pending = TextAreaField(_('Message for pending registrations'))
     message_unpaid = TextAreaField(_('Message for unpaid registrations'))
```
<a href='https://github.com/indico/indico/blob/b84e870869d7ff91189ad797171625c0093e7fc6/indico/web/flask/templating_test.py#L133'>indico/web/flask/templating_test.py~L133</a>
```diff
 
 def test_template_hooks_markup():
     def _make_tpl_hook(name=''):
-        return lambda: (f'&test{name}@{current_plugin.name}' if current_plugin else f'&test{name}')
+        return lambda: f'&test{name}@{current_plugin.name}' if current_plugin else f'&test{name}'
 
     with (
         _register_template_hook_cleanup('test-hook', _make_tpl_hook(1)),
```

</p>
</details>
<details><summary><a href="https://github.com/zanieb/huggingface-notebooks">zanieb/huggingface-notebooks</a> (+1 -1 lines across 1 file)</summary>
<p>

<a href='https://github.com/zanieb/huggingface-notebooks/blob/68cd6fa1a2831c5189f85257c13d691cb76292db/course/fr/chapter5/section6_tf.ipynb#L58'>course/fr/chapter5/section6_tf.ipynb~L58</a>
```diff
    "outputs": [],
    "source": [
     "issues_dataset = issues_dataset.filter(\n",
-    "    lambda x: (x[\"is_pull_request\"] == False and len(x[\"comments\"]) > 0)\n",
+    "    lambda x: x[\"is_pull_request\"] == False and len(x[\"comments\"]) > 0\n",
     ")\n",
     "issues_dataset"
    ]
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (+14 -8 lines across 4 files)</summary>
<p>
<pre>ruff format --no-preview --exclude examples/mcp/databricks_mcp_cookbook.ipynb,examples/chatgpt/gpt_actions_library/gpt_action_google_drive.ipynb,examples/chatgpt/gpt_actions_library/gpt_action_redshift.ipynb,examples/chatgpt/gpt_actions_library/gpt_action_salesforce.ipynb,</pre>
</p>
<p>

<a href='https://github.com/openai/openai-cookbook/blob/857eb0b0a3419b7868ea768f1de3ee906cf176e4/examples/Search_reranking_with_cross-encoders.ipynb#L538'>examples/Search_reranking_with_cross-encoders.ipynb~L538</a>
```diff
     "output_df[\"probability\"] = output_df[\"logprobs\"].apply(exp)\n",
     "# Reorder based on likelihood of being Yes\n",
     "output_df[\"yes_probability\"] = output_df.apply(\n",
-    "    lambda x: x[\"probability\"] * -1 + 1\n",
-    "    if x[\"prediction\"] == \"No\"\n",
-    "    else x[\"probability\"],\n",
+    "    lambda x: (\n",
+    "        x[\"probability\"] * -1 + 1 if x[\"prediction\"] == \"No\" else x[\"probability\"]\n",
+    "    ),\n",
     "    axis=1,\n",
     ")\n",
     "output_df.head()"
```
<a href='https://github.com/openai/openai-cookbook/blob/857eb0b0a3419b7868ea768f1de3ee906cf176e4/examples/completions_usage_api.ipynb#L1532'>examples/completions_usage_api.ipynb~L1532</a>
```diff
     "            plt.pie(\n",
     "                other_projects[\"num_model_requests\"],\n",
     "                labels=other_projects[\"project_id\"],\n",
-    "                autopct=lambda p: f\"{p:.1f}%\\n({int(p * other_total_requests / 100):,})\",\n",
+    "                autopct=lambda p: (\n",
+    "                    f\"{p:.1f}%\\n({int(p * other_total_requests / 100):,})\"\n",
+    "                ),\n",
     "                startangle=140,\n",
     "                textprops={\"fontsize\": 10},\n",
     "            )\n",
```
<a href='https://github.com/openai/openai-cookbook/blob/857eb0b0a3419b7868ea768f1de3ee906cf176e4/examples/vector_databases/pinecone/Using_vision_modality_for_RAG_with_Pinecone.ipynb#L574'>examples/vector_databases/pinecone/Using_vision_modality_for_RAG_with_Pinecone.ipynb~L574</a>
```diff
    "source": [
     "# Add a column to flag pages with visual content\n",
     "df[\"Visual_Input_Processed\"] = df[\"PageText\"].apply(\n",
-    "    lambda x: \"Y\"\n",
-    "    if \"DESCRIPTION OF THE IMAGE OR CHART\" in x or \"TRANSCRIPTION OF THE TABLE\" in x\n",
-    "    else \"N\"\n",
+    "    lambda x: (\n",
+    "        \"Y\"\n",
+    "        if \"DESCRIPTION OF THE IMAGE OR CHART\" in x or \"TRANSCRIPTION OF THE TABLE\" in x\n",
+    "        else \"N\"\n",
+    "    )\n",
     ")\n",
     "\n",
     "\n",
```
<a href='https://github.com/openai/openai-cookbook/blob/857eb0b0a3419b7868ea768f1de3ee906cf176e4/examples/vector_databases/redis/redis-hybrid-query-examples.ipynb#L356'>examples/vector_databases/redis/redis-hybrid-query-examples.ipynb~L356</a>
```diff
    ],
    "source": [
     "df[\"product_text\"] = df.apply(\n",
-    "    lambda row: f\"name {row['productDisplayName']} category {row['masterCategory']} subcategory {row['subCategory']} color {row['baseColour']} gender {row['gender']}\".lower(),\n",
+    "    lambda row: (\n",
+    "        f\"name {row['productDisplayName']} category {row['masterCategory']} subcategory {row['subCategory']} color {row['baseColour']} gender {row['gender']}\".lower()\n",
+    "    ),\n",
     "    axis=1,\n",
     ")\n",
     "df.rename({\"id\": \"product_id\"}, inplace=True, axis=1)\n",
```

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+4 -2 lines across 1 file)</summary>
<p>

<a href='https://github.com/python-trio/trio/blob/01243945fc3d46d4483acf486177389e59fdc6b6/src/trio/_core/_tests/test_run.py#L1387'>src/trio/_core/_tests/test_run.py~L1387</a>
```diff
         pytest.RaisesExc(
             ValueError,
             match="^Unique Text$",
-            check=lambda e: isinstance(e.__context__, IndexError)
-            and isinstance(e.__context__.__context__, KeyError),
+            check=lambda e: (
+                isinstance(e.__context__, IndexError)
+                and isinstance(e.__context__.__context__, KeyError)
+            ),
         ),
     ):
         async with _core.open_nursery() as nursery:
```

</p>
</details>

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Comment by @dylwil3 on 2026-01-19 21:08_

@ntBre  - we don't run the black compatibility tests against our preview rules, so it looks like some syntax errors slipped past the original implementation.

---
