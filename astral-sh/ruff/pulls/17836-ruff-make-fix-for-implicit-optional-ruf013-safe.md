```yaml
number: 17836
title: "[`ruff`] Make fix for `implicit-optional` (`RUF013`) safe in preview"
type: pull_request
state: closed
author: dylwil3
labels:
  - fixes
  - preview
assignees: []
base: main
head: implicit-optional-safe
created_at: 2025-05-04T22:34:08Z
updated_at: 2025-05-05T10:28:21Z
url: https://github.com/astral-sh/ruff/pull/17836
synced_at: 2026-01-12T15:56:06Z
```

# [`ruff`] Make fix for `implicit-optional` (`RUF013`) safe in preview

---

_@dylwil3_

The fix for [implicit-optional (RUF013)](https://docs.astral.sh/ruff/rules/implicit-optional/#implicit-optional-ruf013), introduced in https://github.com/astral-sh/ruff/pull/4831, was marked as unsafe, but I can't actually see a reason why it would be. So this PR makes the fix safe when `preview` is enabled.

Some potential reasons why it is marked as unsafe:

1. When the PR was opened, there were some false positives - but those are now simply skipped.
2. The fix may have to import `Optional` - but we use `try_set_fix` to abort if that wasn't possible for some reason.
3. One may be worried that the pipe notation is used for the union which could introduce a syntax error - but we are careful to check the target version.

But it's possible I'm missing something here so please let me know!


---

_Label `fixes` added by @dylwil3 on 2025-05-04 22:34_

---

_Label `preview` added by @dylwil3 on 2025-05-04 22:34_

---

_Comment by @github-actions[bot] on 2025-05-04 22:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +62 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/stubs/aio_pika/robust_channel.pyi#L15'>stubs/aio_pika/robust_channel.pyi:15:18:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/stubs/aio_pika/robust_channel.pyi#L15'>stubs/aio_pika/robust_channel.pyi:15:18:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/stubs/aio_pika/robust_channel.pyi#L19'>stubs/aio_pika/robust_channel.pyi:19:20:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/stubs/aio_pika/robust_channel.pyi#L19'>stubs/aio_pika/robust_channel.pyi:19:20:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/stubs/socketio.pyi#L51'>stubs/socketio.pyi:51:64:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/stubs/socketio.pyi#L51'>stubs/socketio.pyi:51:64:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+0 -0 violations, +50 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/crazy_functions/vector_fns/general_file_loader.py#L10'>crazy_functions/vector_fns/general_file_loader.py:10:58:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/crazy_functions/vector_fns/general_file_loader.py#L10'>crazy_functions/vector_fns/general_file_loader.py:10:58:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_all.py#L1538'>request_llms/bridge_all.py:1538:84:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_all.py#L1538'>request_llms/bridge_all.py:1538:84:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_chatgpt.py#L128'>request_llms/bridge_chatgpt.py:128:115:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_chatgpt.py#L128'>request_llms/bridge_chatgpt.py:128:115:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_chatgpt.py#L227'>request_llms/bridge_chatgpt.py:227:84:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_chatgpt.py#L227'>request_llms/bridge_chatgpt.py:227:84:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_cohere.py#L128'>request_llms/bridge_cohere.py:128:84:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_cohere.py#L128'>request_llms/bridge_cohere.py:128:84:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_cohere.py#L71'>request_llms/bridge_cohere.py:71:115:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_cohere.py#L71'>request_llms/bridge_cohere.py:71:115:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_google_gemini.py#L57'>request_llms/bridge_google_gemini.py:57:84:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_google_gemini.py#L57'>request_llms/bridge_google_gemini.py:57:84:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_moonshot.py#L150'>request_llms/bridge_moonshot.py:150:84:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_moonshot.py#L150'>request_llms/bridge_moonshot.py:150:84:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_openrouter.py#L122'>request_llms/bridge_openrouter.py:122:115:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_openrouter.py#L122'>request_llms/bridge_openrouter.py:122:115:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_openrouter.py#L209'>request_llms/bridge_openrouter.py:209:84:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_openrouter.py#L209'>request_llms/bridge_openrouter.py:209:84:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_taichu.py#L43'>request_llms/bridge_taichu.py:43:84:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_taichu.py#L43'>request_llms/bridge_taichu.py:43:84:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_zhipu.py#L48'>request_llms/bridge_zhipu.py:48:84:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/bridge_zhipu.py#L48'>request_llms/bridge_zhipu.py:48:84:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/chatglmoonx.py#L188'>request_llms/chatglmoonx.py:188:37:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/chatglmoonx.py#L188'>request_llms/chatglmoonx.py:188:37:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/com_google.py#L62'>request_llms/com_google.py:62:51:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/com_google.py#L62'>request_llms/com_google.py:62:51:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/edge_gpt_free.py#L443'>request_llms/edge_gpt_free.py:443:18:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/edge_gpt_free.py#L443'>request_llms/edge_gpt_free.py:443:18:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/edge_gpt_free.py#L658'>request_llms/edge_gpt_free.py:658:18:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/edge_gpt_free.py#L658'>request_llms/edge_gpt_free.py:658:18:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/edge_gpt_free.py#L684'>request_llms/edge_gpt_free.py:684:18:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/edge_gpt_free.py#L684'>request_llms/edge_gpt_free.py:684:18:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/embed_models/openai_embed.py#L36'>request_llms/embed_models/openai_embed.py:36:35:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/embed_models/openai_embed.py#L36'>request_llms/embed_models/openai_embed.py:36:35:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/embed_models/openai_embed.py#L42'>request_llms/embed_models/openai_embed.py:42:63:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/embed_models/openai_embed.py#L42'>request_llms/embed_models/openai_embed.py:42:63:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/local_llm_class.py#L266'>request_llms/local_llm_class.py:266:88:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/ee1a9e7cce18eb07a2f8cb592573d6d82475d24c/request_llms/local_llm_class.py#L266'>request_llms/local_llm_class.py:266:88:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
... 10 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/a4d3f6f0de94d0beb98cb478eb031aecf5badce9/stdlib/_sitebuiltins.pyi#L9'>stdlib/_sitebuiltins.pyi:9:30:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/python/typeshed/blob/a4d3f6f0de94d0beb98cb478eb031aecf5badce9/stdlib/_sitebuiltins.pyi#L9'>stdlib/_sitebuiltins.pyi:9:30:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/python/typeshed/blob/a4d3f6f0de94d0beb98cb478eb031aecf5badce9/stdlib/lib2to3/main.pyi#L29'>stdlib/lib2to3/main.pyi:29:19:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/python/typeshed/blob/a4d3f6f0de94d0beb98cb478eb031aecf5badce9/stdlib/lib2to3/main.pyi#L29'>stdlib/lib2to3/main.pyi:29:19:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
- <a href='https://github.com/python/typeshed/blob/a4d3f6f0de94d0beb98cb478eb031aecf5badce9/stdlib/pickle.pyi#L214'>stdlib/pickle.pyi:214:26:</a> RUF013 PEP 484 prohibits implicit `Optional`
+ <a href='https://github.com/python/typeshed/blob/a4d3f6f0de94d0beb98cb478eb031aecf5badce9/stdlib/pickle.pyi#L214'>stdlib/pickle.pyi:214:26:</a> RUF013 [*] PEP 484 prohibits implicit `Optional`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF013 | 62 | 0 | 0 | 62 | 0 |

</p>
</details>




---

_@MichaReiser reviewed on 2025-05-05 05:51_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/preview.rs`:130 on 2025-05-05 05:51_

It would be nice if we can keep the `_postfix`. It helps readers to easily recognize this as a preview check

---

_Comment by @MichaReiser on 2025-05-05 05:54_

I think the reason why the fix is unsafe is because it's a possibility that the default value is wrong (a leftover from changing the signature from `int | None` to `None`. A user has to explicitly express their intention.

---

_Closed by @dylwil3 on 2025-05-05 10:28_

---
