```yaml
number: 22650
title: "[`pyflakes`] mark fix as unsafe (`F541`)"
type: pull_request
state: open
author: leandrobbraga
labels: []
assignees: []
base: main
head: feat/f541-unsafe-fix
created_at: 2026-01-17T13:26:32Z
updated_at: 2026-01-19T09:35:58Z
url: https://github.com/astral-sh/ruff/pull/22650
synced_at: 2026-01-19T10:28:42Z
```

# [`pyflakes`] mark fix as unsafe (`F541`)

---

_@leandrobbraga_

Mark `F541` fix as unsafe because the author intent is unclear. An f-string without placeholders may indicate the author intended to interpolate a variable but forgot the braces (e.g., `f"Hello name!"` instead of `f"Hello {name}!"`).

Closes #22620 

---

_Comment by @astral-sh-bot[bot] on 2026-01-17 13:36_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -858 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+0 -0 violations, +0 -98 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/buildcmd/test_build_samconfig.py#L104'>tests/integration/buildcmd/test_build_samconfig.py:104:13:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/buildcmd/test_build_samconfig.py#L104'>tests/integration/buildcmd/test_build_samconfig.py:104:13:</a> F541 f-string without any placeholders
- <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/buildcmd/test_build_samconfig.py#L106'>tests/integration/buildcmd/test_build_samconfig.py:106:13:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/buildcmd/test_build_samconfig.py#L106'>tests/integration/buildcmd/test_build_samconfig.py:106:13:</a> F541 f-string without any placeholders
- <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/buildcmd/test_build_samconfig.py#L78'>tests/integration/buildcmd/test_build_samconfig.py:78:67:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/buildcmd/test_build_samconfig.py#L78'>tests/integration/buildcmd/test_build_samconfig.py:78:67:</a> F541 f-string without any placeholders
- <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/deploy/test_deploy_command.py#L425'>tests/integration/deploy/test_deploy_command.py:425:17:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/deploy/test_deploy_command.py#L425'>tests/integration/deploy/test_deploy_command.py:425:17:</a> F541 f-string without any placeholders
- <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/deploy/test_deploy_command.py#L458'>tests/integration/deploy/test_deploy_command.py:458:17:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/deploy/test_deploy_command.py#L458'>tests/integration/deploy/test_deploy_command.py:458:17:</a> F541 f-string without any placeholders
... 88 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+0 -0 violations, +0 -382 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L257'>check_proxy.py:257:21:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L257'>check_proxy.py:257:21:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L81'>check_proxy.py:81:17:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L81'>check_proxy.py:81:17:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Academic_Conversation.py#L289'>crazy_functions/Academic_Conversation.py:289:46:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Academic_Conversation.py#L289'>crazy_functions/Academic_Conversation.py:289:46:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Academic_Conversation.py#L289'>crazy_functions/Academic_Conversation.py:289:57:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Academic_Conversation.py#L289'>crazy_functions/Academic_Conversation.py:289:57:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Academic_Conversation.py#L89'>crazy_functions/Academic_Conversation.py:89:24:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Academic_Conversation.py#L89'>crazy_functions/Academic_Conversation.py:89:24:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Arxiv_Downloader.py#L138'>crazy_functions/Arxiv_Downloader.py:138:17:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Arxiv_Downloader.py#L138'>crazy_functions/Arxiv_Downloader.py:138:17:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Arxiv_Downloader.py#L151'>crazy_functions/Arxiv_Downloader.py:151:17:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Arxiv_Downloader.py#L151'>crazy_functions/Arxiv_Downloader.py:151:17:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Audio_Summary.py#L149'>crazy_functions/Audio_Summary.py:149:28:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Audio_Summary.py#L149'>crazy_functions/Audio_Summary.py:149:28:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Conversation_To_File.py#L214'>crazy_functions/Conversation_To_File.py:214:12:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Conversation_To_File.py#L214'>crazy_functions/Conversation_To_File.py:214:12:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Conversation_To_File.py#L270'>crazy_functions/Conversation_To_File.py:270:31:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Conversation_To_File.py#L270'>crazy_functions/Conversation_To_File.py:270:31:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Conversation_To_File.py#L348'>crazy_functions/Conversation_To_File.py:348:25:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Conversation_To_File.py#L348'>crazy_functions/Conversation_To_File.py:348:25:</a> F541 f-string without any placeholders
... 360 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +0 -60 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/ldata/_transfer/node.py#L135'>src/latch/ldata/_transfer/node.py:135:21:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/ldata/_transfer/node.py#L135'>src/latch/ldata/_transfer/node.py:135:21:</a> F541 f-string without any placeholders
- <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/ldata/_transfer/node.py#L147'>src/latch/ldata/_transfer/node.py:147:17:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/ldata/_transfer/node.py#L147'>src/latch/ldata/_transfer/node.py:147:17:</a> F541 f-string without any placeholders
- <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/ldata/_transfer/remote_copy.py#L72'>src/latch/ldata/_transfer/remote_copy.py:72:34:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/ldata/_transfer/remote_copy.py#L72'>src/latch/ldata/_transfer/remote_copy.py:72:34:</a> F541 f-string without any placeholders
- <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/registry/utils.py#L392'>src/latch/registry/utils.py:392:13:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/registry/utils.py#L392'>src/latch/registry/utils.py:392:13:</a> F541 f-string without any placeholders
- <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch_cli/menus.py#L34'>src/latch_cli/menus.py:34:12:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch_cli/menus.py#L34'>src/latch_cli/menus.py:34:12:</a> F541 f-string without any placeholders
... 50 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -0 violations, +0 -276 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/bulk_writer_all_types.py#L239'>examples/bulk_import/bulk_writer_all_types.py:239:11:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/bulk_writer_all_types.py#L239'>examples/bulk_import/bulk_writer_all_types.py:239:11:</a> F541 f-string without any placeholders
- <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/bulk_writer_all_types.py#L292'>examples/bulk_import/bulk_writer_all_types.py:292:11:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/bulk_writer_all_types.py#L292'>examples/bulk_import/bulk_writer_all_types.py:292:11:</a> F541 f-string without any placeholders
- <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_json.py#L178'>examples/bulk_import/example_bulkinsert_json.py:178:11:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_json.py#L178'>examples/bulk_import/example_bulkinsert_json.py:178:11:</a> F541 f-string without any placeholders
- <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_json.py#L192'>examples/bulk_import/example_bulkinsert_json.py:192:15:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_json.py#L192'>examples/bulk_import/example_bulkinsert_json.py:192:15:</a> F541 f-string without any placeholders
- <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_json.py#L212'>examples/bulk_import/example_bulkinsert_json.py:212:11:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_json.py#L212'>examples/bulk_import/example_bulkinsert_json.py:212:11:</a> F541 f-string without any placeholders
- <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_parquet.py#L274'>examples/bulk_import/example_bulkinsert_parquet.py:274:11:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_parquet.py#L274'>examples/bulk_import/example_bulkinsert_parquet.py:274:11:</a> F541 f-string without any placeholders
- <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_parquet.py#L288'>examples/bulk_import/example_bulkinsert_parquet.py:288:15:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_parquet.py#L288'>examples/bulk_import/example_bulkinsert_parquet.py:288:15:</a> F541 f-string without any placeholders
- <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_parquet.py#L308'>examples/bulk_import/example_bulkinsert_parquet.py:308:11:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_parquet.py#L308'>examples/bulk_import/example_bulkinsert_parquet.py:308:11:</a> F541 f-string without any placeholders
... 260 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/qdrant/qdrant-client">qdrant/qdrant-client</a> (+0 -0 violations, +0 -42 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/qdrant_client/async_qdrant_remote.py#L81'>qdrant_client/async_qdrant_remote.py:81:17:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/qdrant_client/async_qdrant_remote.py#L81'>qdrant_client/async_qdrant_remote.py:81:17:</a> F541 f-string without any placeholders
- <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/qdrant_client/qdrant_remote.py#L87'>qdrant_client/qdrant_remote.py:87:17:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/qdrant_client/qdrant_remote.py#L87'>qdrant_client/qdrant_remote.py:87:17:</a> F541 f-string without any placeholders
- <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/qdrant_client/qdrant_remote.py#L88'>qdrant_client/qdrant_remote.py:88:17:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/qdrant_client/qdrant_remote.py#L88'>qdrant_client/qdrant_remote.py:88:17:</a> F541 f-string without any placeholders
- <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/tests/congruence_tests/test_geo_polygon_filter_query.py#L84'>tests/congruence_tests/test_geo_polygon_filter_query.py:84:19:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/tests/congruence_tests/test_geo_polygon_filter_query.py#L84'>tests/congruence_tests/test_geo_polygon_filter_query.py:84:19:</a> F541 f-string without any placeholders
- <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/tests/congruence_tests/test_geo_polygon_filter_query.py#L85'>tests/congruence_tests/test_geo_polygon_filter_query.py:85:19:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/tests/congruence_tests/test_geo_polygon_filter_query.py#L85'>tests/congruence_tests/test_geo_polygon_filter_query.py:85:19:</a> F541 f-string without any placeholders
... 32 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F541 | 858 | 0 | 0 | 0 | 858 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -858 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+0 -0 violations, +0 -98 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/buildcmd/test_build_samconfig.py#L104'>tests/integration/buildcmd/test_build_samconfig.py:104:13:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/buildcmd/test_build_samconfig.py#L104'>tests/integration/buildcmd/test_build_samconfig.py:104:13:</a> F541 f-string without any placeholders
- <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/buildcmd/test_build_samconfig.py#L106'>tests/integration/buildcmd/test_build_samconfig.py:106:13:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/buildcmd/test_build_samconfig.py#L106'>tests/integration/buildcmd/test_build_samconfig.py:106:13:</a> F541 f-string without any placeholders
- <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/buildcmd/test_build_samconfig.py#L78'>tests/integration/buildcmd/test_build_samconfig.py:78:67:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/buildcmd/test_build_samconfig.py#L78'>tests/integration/buildcmd/test_build_samconfig.py:78:67:</a> F541 f-string without any placeholders
- <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/deploy/test_deploy_command.py#L425'>tests/integration/deploy/test_deploy_command.py:425:17:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/deploy/test_deploy_command.py#L425'>tests/integration/deploy/test_deploy_command.py:425:17:</a> F541 f-string without any placeholders
- <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/deploy/test_deploy_command.py#L458'>tests/integration/deploy/test_deploy_command.py:458:17:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/aws/aws-sam-cli/blob/17114dfe9eccc8a231334983793fae7e01829f38/tests/integration/deploy/test_deploy_command.py#L458'>tests/integration/deploy/test_deploy_command.py:458:17:</a> F541 f-string without any placeholders
... 88 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+0 -0 violations, +0 -382 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L257'>check_proxy.py:257:21:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L257'>check_proxy.py:257:21:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L81'>check_proxy.py:81:17:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L81'>check_proxy.py:81:17:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Academic_Conversation.py#L289'>crazy_functions/Academic_Conversation.py:289:46:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Academic_Conversation.py#L289'>crazy_functions/Academic_Conversation.py:289:46:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Academic_Conversation.py#L289'>crazy_functions/Academic_Conversation.py:289:57:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Academic_Conversation.py#L289'>crazy_functions/Academic_Conversation.py:289:57:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Academic_Conversation.py#L89'>crazy_functions/Academic_Conversation.py:89:24:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Academic_Conversation.py#L89'>crazy_functions/Academic_Conversation.py:89:24:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Arxiv_Downloader.py#L138'>crazy_functions/Arxiv_Downloader.py:138:17:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Arxiv_Downloader.py#L138'>crazy_functions/Arxiv_Downloader.py:138:17:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Arxiv_Downloader.py#L151'>crazy_functions/Arxiv_Downloader.py:151:17:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Arxiv_Downloader.py#L151'>crazy_functions/Arxiv_Downloader.py:151:17:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Audio_Summary.py#L149'>crazy_functions/Audio_Summary.py:149:28:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Audio_Summary.py#L149'>crazy_functions/Audio_Summary.py:149:28:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Conversation_To_File.py#L214'>crazy_functions/Conversation_To_File.py:214:12:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Conversation_To_File.py#L214'>crazy_functions/Conversation_To_File.py:214:12:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Conversation_To_File.py#L270'>crazy_functions/Conversation_To_File.py:270:31:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Conversation_To_File.py#L270'>crazy_functions/Conversation_To_File.py:270:31:</a> F541 f-string without any placeholders
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Conversation_To_File.py#L348'>crazy_functions/Conversation_To_File.py:348:25:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Conversation_To_File.py#L348'>crazy_functions/Conversation_To_File.py:348:25:</a> F541 f-string without any placeholders
... 360 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +0 -60 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/ldata/_transfer/node.py#L135'>src/latch/ldata/_transfer/node.py:135:21:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/ldata/_transfer/node.py#L135'>src/latch/ldata/_transfer/node.py:135:21:</a> F541 f-string without any placeholders
- <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/ldata/_transfer/node.py#L147'>src/latch/ldata/_transfer/node.py:147:17:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/ldata/_transfer/node.py#L147'>src/latch/ldata/_transfer/node.py:147:17:</a> F541 f-string without any placeholders
- <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/ldata/_transfer/remote_copy.py#L72'>src/latch/ldata/_transfer/remote_copy.py:72:34:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/ldata/_transfer/remote_copy.py#L72'>src/latch/ldata/_transfer/remote_copy.py:72:34:</a> F541 f-string without any placeholders
- <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/registry/utils.py#L392'>src/latch/registry/utils.py:392:13:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/registry/utils.py#L392'>src/latch/registry/utils.py:392:13:</a> F541 f-string without any placeholders
- <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch_cli/menus.py#L34'>src/latch_cli/menus.py:34:12:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch_cli/menus.py#L34'>src/latch_cli/menus.py:34:12:</a> F541 f-string without any placeholders
... 50 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -0 violations, +0 -276 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/bulk_writer_all_types.py#L239'>examples/bulk_import/bulk_writer_all_types.py:239:11:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/bulk_writer_all_types.py#L239'>examples/bulk_import/bulk_writer_all_types.py:239:11:</a> F541 f-string without any placeholders
- <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/bulk_writer_all_types.py#L292'>examples/bulk_import/bulk_writer_all_types.py:292:11:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/bulk_writer_all_types.py#L292'>examples/bulk_import/bulk_writer_all_types.py:292:11:</a> F541 f-string without any placeholders
- <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_json.py#L178'>examples/bulk_import/example_bulkinsert_json.py:178:11:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_json.py#L178'>examples/bulk_import/example_bulkinsert_json.py:178:11:</a> F541 f-string without any placeholders
- <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_json.py#L192'>examples/bulk_import/example_bulkinsert_json.py:192:15:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_json.py#L192'>examples/bulk_import/example_bulkinsert_json.py:192:15:</a> F541 f-string without any placeholders
- <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_json.py#L212'>examples/bulk_import/example_bulkinsert_json.py:212:11:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_json.py#L212'>examples/bulk_import/example_bulkinsert_json.py:212:11:</a> F541 f-string without any placeholders
- <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_parquet.py#L274'>examples/bulk_import/example_bulkinsert_parquet.py:274:11:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_parquet.py#L274'>examples/bulk_import/example_bulkinsert_parquet.py:274:11:</a> F541 f-string without any placeholders
- <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_parquet.py#L288'>examples/bulk_import/example_bulkinsert_parquet.py:288:15:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_parquet.py#L288'>examples/bulk_import/example_bulkinsert_parquet.py:288:15:</a> F541 f-string without any placeholders
- <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_parquet.py#L308'>examples/bulk_import/example_bulkinsert_parquet.py:308:11:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/milvus-io/pymilvus/blob/262ab27d606884e4730fe100b4ad475b86750bb1/examples/bulk_import/example_bulkinsert_parquet.py#L308'>examples/bulk_import/example_bulkinsert_parquet.py:308:11:</a> F541 f-string without any placeholders
... 260 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/qdrant/qdrant-client">qdrant/qdrant-client</a> (+0 -0 violations, +0 -42 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/qdrant_client/async_qdrant_remote.py#L81'>qdrant_client/async_qdrant_remote.py:81:17:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/qdrant_client/async_qdrant_remote.py#L81'>qdrant_client/async_qdrant_remote.py:81:17:</a> F541 f-string without any placeholders
- <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/qdrant_client/qdrant_remote.py#L87'>qdrant_client/qdrant_remote.py:87:17:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/qdrant_client/qdrant_remote.py#L87'>qdrant_client/qdrant_remote.py:87:17:</a> F541 f-string without any placeholders
- <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/qdrant_client/qdrant_remote.py#L88'>qdrant_client/qdrant_remote.py:88:17:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/qdrant_client/qdrant_remote.py#L88'>qdrant_client/qdrant_remote.py:88:17:</a> F541 f-string without any placeholders
- <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/tests/congruence_tests/test_geo_polygon_filter_query.py#L84'>tests/congruence_tests/test_geo_polygon_filter_query.py:84:19:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/tests/congruence_tests/test_geo_polygon_filter_query.py#L84'>tests/congruence_tests/test_geo_polygon_filter_query.py:84:19:</a> F541 f-string without any placeholders
- <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/tests/congruence_tests/test_geo_polygon_filter_query.py#L85'>tests/congruence_tests/test_geo_polygon_filter_query.py:85:19:</a> F541 [*] f-string without any placeholders
+ <a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/tests/congruence_tests/test_geo_polygon_filter_query.py#L85'>tests/congruence_tests/test_geo_polygon_filter_query.py:85:19:</a> F541 f-string without any placeholders
... 32 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F541 | 858 | 0 | 0 | 0 | 858 |

</p>
</details>





---

_Comment by @MichaReiser on 2026-01-19 09:29_

Sorry that I didn't comment on the issue before you put up the PR but I don't think we should make this change, see https://github.com/astral-sh/ruff/issues/22620#issuecomment-3760168368 

---

_Comment by @leandrobbraga on 2026-01-19 09:35_

> Sorry that I didn't comment on the issue before you put up the PR but I don't think we should make this change, see https://github.com/astral-sh/ruff/issues/22620#issuecomment-3760168368 

That's OK! I thought the decision was already settled. 


---
