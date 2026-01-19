```yaml
number: 22745
title: "Stabilize `fluent_layout_split_first_call`"
type: pull_request
state: open
author: dylwil3
labels:
  - breaking
  - formatter
assignees: []
base: 2026-style
head: stabilize-fluent_layout_split_first_call
created_at: 2026-01-19T20:57:07Z
updated_at: 2026-01-19T21:06:32Z
url: https://github.com/astral-sh/ruff/pull/22745
synced_at: 2026-01-19T21:33:07Z
```

# Stabilize `fluent_layout_split_first_call`

---

_@dylwil3_

_No description provided._

---

_Review requested from @MichaReiser by @dylwil3 on 2026-01-19 20:57_

---

_Label `breaking` added by @dylwil3 on 2026-01-19 20:57_

---

_Label `formatter` added by @dylwil3 on 2026-01-19 20:57_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 21:06_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+800 -420 lines in 202 files in 16 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+30 -15 lines across 6 files)</summary>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/cli/scaffold.py#L54'>rasa/cli/scaffold.py~L54</a>
```diff
     print_success("Finished creating project structure.")
 
     should_train = (
-        questionary.confirm("Do you want to train an initial model? 💪🏽")
+        questionary
+        .confirm("Do you want to train an initial model? 💪🏽")
         .skip_if(args.no_prompt, default=True)
         .ask()
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/cli/scaffold.py#L83'>rasa/cli/scaffold.py~L83</a>
```diff
     import questionary
 
     should_run = (
-        questionary.confirm(
+        questionary
+        .confirm(
             "Do you want to speak to the trained assistant on the command line? 🤖"
         )
         .skip_if(args.no_prompt, default=False)
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/cli/scaffold.py#L206'>rasa/cli/scaffold.py~L206</a>
```diff
         path = args.init_dir
     else:
         path = (
-            questionary.text(
+            questionary
+            .text(
                 "Please enter a path where the project will be "
                 "created [default: current directory]"
             )
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/test.py#L500'>rasa/core/test.py~L500</a>
```diff
     # the response selector contains a "default" key
     if RESPONSE_SELECTOR_DEFAULT_INTENT in response_selector:
         full_retrieval_intent = (
-            response_selector.get(RESPONSE_SELECTOR_DEFAULT_INTENT, {})
+            response_selector
+            .get(RESPONSE_SELECTOR_DEFAULT_INTENT, {})
             .get(RESPONSE, {})
             .get(INTENT_RESPONSE_KEY)
         )
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/test.py#L508'>rasa/core/test.py~L508</a>
```diff
 
     # if specified, the response selector contains the base intent as key
     full_retrieval_intent = (
-        response_selector.get(base_intent, {})
+        response_selector
+        .get(base_intent, {})
         .get(RESPONSE, {})
         .get(INTENT_RESPONSE_KEY)
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/tracker_store.py#L1015'>rasa/core/tracker_store.py~L1015</a>
```diff
 
     if is_postgresql_url(engine.url):
         query = sa.exists(
-            sa.select([(sa.text("schema_name"))])
+            sa
+            .select([(sa.text("schema_name"))])
             .select_from(sa.text("information_schema.schemata"))
             .where(sa.text(f"schema_name = '{schema_name}'"))
         )
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/tracker_store.py#L1201'>rasa/core/tracker_store.py~L1201</a>
```diff
         conn = engine.connect()
 
         matching_rows = (
-            conn.execution_options(isolation_level="AUTOCOMMIT")
+            conn
+            .execution_options(isolation_level="AUTOCOMMIT")
             .execute(
                 sa.text(
                     "SELECT 1 FROM pg_catalog.pg_database "
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/tracker_store.py#L1300'>rasa/core/tracker_store.py~L1300</a>
```diff
         """
         # Subquery to find the timestamp of the latest `SessionStarted` event
         session_start_sub_query = (
-            session.query(sa.func.max(self.SQLEvent.timestamp).label("session_start"))
+            session
+            .query(sa.func.max(self.SQLEvent.timestamp).label("session_start"))
             .filter(
                 self.SQLEvent.sender_id == sender_id,
                 self.SQLEvent.type_name == SessionStarted.type_name,
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/utils/io.py#L347'>rasa/shared/utils/io.py~L347</a>
```diff
     if _is_ascii(content):
         # Required to make sure emojis are correctly parsed
         content = (
-            content.encode("utf-8")
+            content
+            .encode("utf-8")
             .decode("raw_unicode_escape")
             .encode("utf-16", "surrogatepass")
             .decode("utf-16")
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/classifiers/test_diet_classifier.py#L874'>tests/nlu/classifiers/test_diet_classifier.py~L874</a>
```diff
                             # expected to change can be compared directly
                             assert (
                                 feature_signature.units
-                                == old_signature.get(attribute)
+                                == old_signature
+                                .get(attribute)
                                 .get(feature_type)[index]
                                 .units
                             )
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/selectors/test_selectors.py#L195'>tests/nlu/selectors/test_selectors.py~L195</a>
```diff
         classified_message.get("response_selector").get("all_retrieval_intents")
     ) == ["chitchat"]
     assert (
-        classified_message.get("response_selector")
+        classified_message
+        .get("response_selector")
         .get("default")
         .get("response")
         .get("intent_response_key")
     ) is not None
     assert (
-        classified_message.get("response_selector")
+        classified_message
+        .get("response_selector")
         .get("default")
         .get("response")
         .get("utter_action")
     ) is not None
     assert (
-        classified_message.get("response_selector")
+        classified_message
+        .get("response_selector")
         .get("default")
         .get("response")
         .get("responses")
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/selectors/test_selectors.py#L637'>tests/nlu/selectors/test_selectors.py~L637</a>
```diff
     unfeaturized_message = Message(data={TEXT: message_text})
     classified_message = response_selector.process([unfeaturized_message])[0]
     output = (
-        classified_message.get(RESPONSE_SELECTOR_PROPERTY_NAME)
+        classified_message
+        .get(RESPONSE_SELECTOR_PROPERTY_NAME)
         .get(RESPONSE_SELECTOR_DEFAULT_INTENT)
         .get(RESPONSE_SELECTOR_PREDICTION_KEY)
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/tests/nlu/selectors/test_selectors.py#L798'>tests/nlu/selectors/test_selectors.py~L798</a>
```diff
                             # expected to change can be compared directly
                             assert (
                                 feature_signature.units
-                                == old_signature.get(attribute)
+                                == old_signature
+                                .get(attribute)
                                 .get(feature_type)[index]
                                 .units
                             )
```

</p>
</details>
<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+10 -5 lines across 1 file)</summary>
<p>

<a href='https://github.com/Snowflake-Labs/snowcli/blob/78174c60f5ef5ae8459e2e900c51d50f045ec549/tests/app/test_telemetry.py#L52'>tests/app/test_telemetry.py~L52</a>
```diff
     assert result.exit_code == 0, result.output
     # The method is called with a TelemetryData type, so we cast it to dict for simpler comparison
     usage_command_event = (
-        mock_conn.return_value._telemetry.try_add_log_to_batch.call_args_list[  # noqa: SLF001
+        mock_conn.return_value._telemetry.try_add_log_to_batch
+        .call_args_list[  # noqa: SLF001
             0
         ]
         .args[0]
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/78174c60f5ef5ae8459e2e900c51d50f045ec549/tests/app/test_telemetry.py#L109'>tests/app/test_telemetry.py~L109</a>
```diff
     assert result.exit_code == 0, result.output
     # The method is called with a TelemetryData type, so we cast it to dict for simpler comparison
     usage_command_event = (
-        mock_conn.return_value._telemetry.try_add_log_to_batch.call_args_list[  # noqa: SLF001
+        mock_conn.return_value._telemetry.try_add_log_to_batch
+        .call_args_list[  # noqa: SLF001
             0
         ]
         .args[0]
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/78174c60f5ef5ae8459e2e900c51d50f045ec549/tests/app/test_telemetry.py#L140'>tests/app/test_telemetry.py~L140</a>
```diff
 
     # The method is called with a TelemetryData type, so we cast it to dict for simpler comparison
     result_command_event = (
-        mock_conn.return_value._telemetry.try_add_log_to_batch.call_args_list[  # noqa: SLF001
+        mock_conn.return_value._telemetry.try_add_log_to_batch
+        .call_args_list[  # noqa: SLF001
             1
         ]
         .args[0]
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/78174c60f5ef5ae8459e2e900c51d50f045ec549/tests/app/test_telemetry.py#L181'>tests/app/test_telemetry.py~L181</a>
```diff
 
     # The method is called with a TelemetryData type, so we cast it to dict for simpler comparison
     result_command_event = (
-        mock_conn.return_value._telemetry.try_add_log_to_batch.call_args_list[  # noqa: SLF001
+        mock_conn.return_value._telemetry.try_add_log_to_batch
+        .call_args_list[  # noqa: SLF001
             1
         ]
         .args[0]
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/78174c60f5ef5ae8459e2e900c51d50f045ec549/tests/app/test_telemetry.py#L228'>tests/app/test_telemetry.py~L228</a>
```diff
     assert result.exit_code == 0, result.output
 
     usage_command_event = (
-        mock_connect.return_value._telemetry.try_add_log_to_batch.call_args_list[  # noqa: SLF001
+        mock_connect.return_value._telemetry.try_add_log_to_batch
+        .call_args_list[  # noqa: SLF001
             0
         ]
         .args[0]
```

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+28 -14 lines across 12 files)</summary>
<p>

<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Arxiv_Downloader.py#L36'>crazy_functions/Arxiv_Downloader.py~L36</a>
```diff
     os.makedirs(download_dir, exist_ok=True)
 
     title_str = (
-        title.replace("?", "？")
+        title
+        .replace("?", "？")
         .replace(":", "：")
         .replace('"', "“")
         .replace("\n", "")
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Arxiv_Downloader.py#L56'>crazy_functions/Arxiv_Downloader.py~L56</a>
```diff
 
     x = "%s  %s %s.bib" % (paper_id, other_info["year"], other_info["authors"])
     x = (
-        x.replace("?", "？")
+        x
+        .replace("?", "？")
         .replace(":", "：")
         .replace('"', "“")
         .replace("\n", "")
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Audio_Assistant.py#L22'>crazy_functions/Audio_Assistant.py~L22</a>
```diff
                 continue
             else:
                 history.append(
-                    q.strip('<div class="markdown-body">')
+                    q
+                    .strip('<div class="markdown-body">')
                     .strip("</div>")
                     .strip("<p>")
                     .strip("</p>")
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Internet_GPT.py#L242'>crazy_functions/Internet_GPT.py~L242</a>
```diff
         response.encoding = response.apparent_encoding
     result = response.text
     result = (
-        result.replace("\\[", "[")
+        result
+        .replace("\\[", "[")
         .replace("\\]", "]")
         .replace("\\(", "(")
         .replace("\\)", ")")
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/SourceCode_Analyse.py#L626'>crazy_functions/SourceCode_Analyse.py~L626</a>
```diff
     pattern_except_suffix += ["zip", "rar", "7z", "tar", "gz"]  # 避免解析压缩文件
     # 将要忽略匹配的文件名(例如: ^README.md)
     pattern_except_name = [
-        _.lstrip(" ^*,")
+        _
+        .lstrip(" ^*,")
         .rstrip(" ,")
         .replace(".", r"\.")  # 移除左边通配符，移除右侧逗号，转义点号
         for _ in txt_pattern.split(" ")  # 以空格分割
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/diagram_fns/file_tree.py#L28'>crazy_functions/diagram_fns/file_tree.py~L28</a>
```diff
             suf = ""
         comment = comment[: self.comment_maxlen_show]
         comment = (
-            comment.replace('"', "")
+            comment
+            .replace('"', "")
             .replace("`", "")
             .replace("\n", "")
             .replace("`", "")
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/doc_fns/read_fns/excel_reader.py#L133'>crazy_functions/doc_fns/read_fns/excel_reader.py~L133</a>
```diff
                 return "\n".join(rows)
             else:
                 flat_values = (
-                    chunk.astype(str)
+                    chunk
+                    .astype(str)
                     .replace({"nan": "", "None": "", "NaN": ""})
                     .values.flatten()
                 )
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/latex_fns/latex_actions.py#L241'>crazy_functions/latex_fns/latex_actions.py~L241</a>
```diff
             title, abstract = find_title_and_abs(txt)
             if title is not None:
                 self.title = (
-                    title.replace("\n", " ")
+                    title
+                    .replace("\n", " ")
                     .replace("\\\\", " ")
                     .replace("  ", "")
                     .replace("  ", "")
                 )
             if abstract is not None:
                 self.abstract = (
-                    abstract.replace("\n", " ")
+                    abstract
+                    .replace("\n", " ")
                     .replace("\\\\", " ")
                     .replace("  ", "")
                     .replace("  ", "")
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/request_llms/bridge_chatglm.py#L41'>request_llms/bridge_chatglm.py~L41</a>
```diff
                 ).float()
             else:
                 chatglm_model = (
-                    AutoModel.from_pretrained(_model_name_, trust_remote_code=True)
+                    AutoModel
+                    .from_pretrained(_model_name_, trust_remote_code=True)
                     .half()
                     .cuda()
                 )
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/request_llms/bridge_chatglm4.py#L33'>request_llms/bridge_chatglm4.py~L33</a>
```diff
             model_path, trust_remote_code=True
         )
         chatglm_model = (
-            AutoModelForCausalLM.from_pretrained(
+            AutoModelForCausalLM
+            .from_pretrained(
                 model_path,
                 torch_dtype=torch.bfloat16,
                 low_cpu_mem_usage=True,
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/request_llms/bridge_internlm.py#L65'>request_llms/bridge_internlm.py~L65</a>
```diff
                     ).to(torch.bfloat16)
                 else:
                     model = (
-                        AutoModelForCausalLM.from_pretrained(
+                        AutoModelForCausalLM
+                        .from_pretrained(
                             "internlm/internlm-chat-7b", trust_remote_code=True
                         )
                         .to(torch.bfloat16)
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/shared_utils/char_visual_effect.py#L11'>shared_utils/char_visual_effect.py~L11</a>
```diff
 
 def scrolling_visual_effect(text, scroller_max_len):
     text = (
-        text.replace("\n", "")
+        text
+        .replace("\n", "")
         .replace("`", ".")
         .replace(" ", ".")
         .replace("<br/>", ".....")
```
<a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/shared_utils/fastapi_stream_server.py#L657'>shared_utils/fastapi_stream_server.py~L657</a>
```diff
                             "The response does not contain the expected future input format."
                         )
                     result = (
-                        result.split("<future_input>")[-1]
+                        result
+                        .split("<future_input>")[-1]
                         .split("</future_input>")[0]
                         .strip()
                     )
```

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+18 -9 lines across 5 files)</summary>
<p>

<a href='https://github.com/freedomofpress/securedrop/blob/b226cf79cf88caf0a499760b6b14d9b285e28760/securedrop/journalist_app/main.py#L69'>securedrop/journalist_app/main.py~L69</a>
```diff
         # unstarred sources below, and the unread counts added to
         # their result sets as an extra column.
         unread_stmt = (
-            db.session.query(Submission.source_id, func.count("*").label("num_unread"))
+            db.session
+            .query(Submission.source_id, func.count("*").label("num_unread"))
             .filter_by(seen_files=None, seen_messages=None)
             .group_by(Submission.source_id)
             .subquery()
```
<a href='https://github.com/freedomofpress/securedrop/blob/b226cf79cf88caf0a499760b6b14d9b285e28760/securedrop/journalist_app/main.py#L78'>securedrop/journalist_app/main.py~L78</a>
```diff
         # Query for starred sources, along with their unread
         # submission counts.
         starred = (
-            db.session.query(Source, unread_stmt.c.num_unread)
+            db.session
+            .query(Source, unread_stmt.c.num_unread)
             .filter_by(pending=False, deleted_at=None)
             .filter(Source.last_updated.isnot(None))
             .filter(SourceStar.starred.is_(True))
```
<a href='https://github.com/freedomofpress/securedrop/blob/b226cf79cf88caf0a499760b6b14d9b285e28760/securedrop/journalist_app/main.py#L98'>securedrop/journalist_app/main.py~L98</a>
```diff
         # Query for sources without stars, along with their unread
         # submission counts.
         unstarred = (
-            db.session.query(Source, unread_stmt.c.num_unread)
+            db.session
+            .query(Source, unread_stmt.c.num_unread)
             .filter_by(pending=False, deleted_at=None)
             .filter(Source.last_updated.isnot(None))
             .filter(~Source.star.has(SourceStar.starred.is_(True)))
```
<a href='https://github.com/freedomofpress/securedrop/blob/b226cf79cf88caf0a499760b6b14d9b285e28760/securedrop/journalist_app/main.py#L229'>securedrop/journalist_app/main.py~L229</a>
```diff
     @view.route("/download_unread/<filesystem_id>")
     def download_unread_filesystem_id(filesystem_id: str) -> werkzeug.Response:
         unseen_submissions = (
-            Submission.query.join(Source)
+            Submission.query
+            .join(Source)
             .filter(Source.deleted_at.is_(None), Source.filesystem_id == filesystem_id)
             .filter(~Submission.seen_files.any(), ~Submission.seen_messages.any())
             .all()
```
<a href='https://github.com/freedomofpress/securedrop/blob/b226cf79cf88caf0a499760b6b14d9b285e28760/securedrop/journalist_app/utils.py#L537'>securedrop/journalist_app/utils.py~L537</a>
```diff
     Download all unseen submissions from all selected sources.
     """
     unseen_submissions = (
-        Submission.query.join(Source)
+        Submission.query
+        .join(Source)
         .filter(Source.deleted_at.is_(None), Source.filesystem_id.in_(cols_selected))
         .filter(~Submission.seen_files.any(), ~Submission.seen_messages.any())
         .all()
```
<a href='https://github.com/freedomofpress/securedrop/blob/b226cf79cf88caf0a499760b6b14d9b285e28760/securedrop/journalist_app/utils.py#L555'>securedrop/journalist_app/utils.py~L555</a>
```diff
     submissions: List[Union[Source, Submission]] = []
     for filesystem_id in cols_selected:
         id = (
-            Source.query.filter(Source.filesystem_id == filesystem_id)
+            Source.query
+            .filter(Source.filesystem_id == filesystem_id)
             .filter_by(deleted_at=None)
             .one()
             .id
```
<a href='https://github.com/freedomofpress/securedrop/blob/b226cf79cf88caf0a499760b6b14d9b285e28760/securedrop/management/submissions.py#L172'>securedrop/management/submissions.py~L172</a>
```diff
 ) -> None:
     with context or app_context():
         something = (
-            db.session.query(Source)
+            db.session
+            .query(Source)
             .filter(Source.last_updated > datetime.datetime.utcnow() - datetime.timedelta(hours=24))
             .count()
             > 0
```
<a href='https://github.com/freedomofpress/securedrop/blob/b226cf79cf88caf0a499760b6b14d9b285e28760/securedrop/source_app/main.py#L347'>securedrop/source_app/main.py~L347</a>
```diff
     @login_required
     def batch_delete(logged_in_source: SourceUser) -> werkzeug.Response:
         replies = (
-            Reply.query.filter(Reply.source_id == logged_in_source.db_record_id)
+            Reply.query
+            .filter(Reply.source_id == logged_in_source.db_record_id)
             .filter(Reply.deleted_by_source == False)
             .all()
         )
```
<a href='https://github.com/freedomofpress/securedrop/blob/b226cf79cf88caf0a499760b6b14d9b285e28760/securedrop/tests/functional/app_navigators/journalist_app_nav.py#L287'>securedrop/tests/functional/app_navigators/journalist_app_nav.py~L287</a>
```diff
         else:
             # We created a totp user
             otp_secret = (
-                self.driver.find_element(By.CSS_SELECTOR, "#shared-secret")
+                self.driver
+                .find_element(By.CSS_SELECTOR, "#shared-secret")
                 .text.strip()
                 .replace(" ", "")
             )
```

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+2 -1 lines across 1 file)</summary>
<p>

<a href='https://github.com/fronzbot/blinkpy/blob/3e673a59ab94e8d11895fc0f160eb3feb445453d/blinkpy/sync_module.py#L360'>blinkpy/sync_module.py~L360</a>
```diff
             last_manifest_id = self._local_storage["last_manifest_id"]
             last_manifest_read = self._local_storage["last_manifest_read"]
             last_read_local = (
-                datetime.datetime.fromisoformat(last_manifest_read)
+                datetime.datetime
+                .fromisoformat(last_manifest_read)
                 .replace(tzinfo=datetime.timezone.utc)
                 .astimezone(tz=None)
             )
```

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+16 -8 lines across 1 file)</summary>
<p>

<a href='https://github.com/lnbits/lnbits/blob/cb3fd566478502a9579389c40e21b8eafba41bad/lnbits/wallets/blink.py#L74'>lnbits/wallets/blink.py~L74</a>
```diff
             payload = {"query": q.balance_query, "variables": {}}
             response = await self._graphql_query(payload)
             wallets = (
-                response.get("data", {})
+                response
+                .get("data", {})
                 .get("me", {})
                 .get("defaultAccount", {})
                 .get("wallets", [])
```
<a href='https://github.com/lnbits/lnbits/blob/cb3fd566478502a9579389c40e21b8eafba41bad/lnbits/wallets/blink.py#L129'>lnbits/wallets/blink.py~L129</a>
```diff
             response = await self._graphql_query(data)
 
             errors = (
-                response.get("data", {})
+                response
+                .get("data", {})
                 .get("lnInvoiceCreateOnBehalfOfRecipient", {})
                 .get("errors", {})
             )
```
<a href='https://github.com/lnbits/lnbits/blob/cb3fd566478502a9579389c40e21b8eafba41bad/lnbits/wallets/blink.py#L138'>lnbits/wallets/blink.py~L138</a>
```diff
                 return InvoiceResponse(ok=False, error_message=error_message)
 
             payment_request = (
-                response.get("data", {})
+                response
+                .get("data", {})
                 .get("lnInvoiceCreateOnBehalfOfRecipient", {})
                 .get("invoice", {})
                 .get("paymentRequest", None)
             )
             checking_id = (
-                response.get("data", {})
+                response
+                .get("data", {})
                 .get("lnInvoiceCreateOnBehalfOfRecipient", {})
                 .get("invoice", {})
                 .get("paymentHash", None)
```
<a href='https://github.com/lnbits/lnbits/blob/cb3fd566478502a9579389c40e21b8eafba41bad/lnbits/wallets/blink.py#L182'>lnbits/wallets/blink.py~L182</a>
```diff
             response = await self._graphql_query(data)
 
             errors = (
-                response.get("data", {})
+                response
+                .get("data", {})
                 .get("lnInvoicePaymentSend", {})
                 .get("errors", {})
             )
```
<a href='https://github.com/lnbits/lnbits/blob/cb3fd566478502a9579389c40e21b8eafba41bad/lnbits/wallets/blink.py#L251'>lnbits/wallets/blink.py~L251</a>
```diff
             if response_data is None:
                 raise ValueError("No data found in response.")
             txs_data = (
-                response_data.get("me", {})
+                response_data
+                .get("me", {})
                 .get("defaultAccount", {})
                 .get("walletById", {})
                 .get("transactionsByPaymentHash", [])
```
<a href='https://github.com/lnbits/lnbits/blob/cb3fd566478502a9579389c40e21b8eafba41bad/lnbits/wallets/blink.py#L299'>lnbits/wallets/blink.py~L299</a>
```diff
                         if resp.get("id") != subscription_id:
                             continue
                         tx = (
-                            resp.get("payload", {})
+                            resp
+                            .get("payload", {})
                             .get("data", {})
                             .get("myUpdates", {})
                             .get("update", {})
```
<a href='https://github.com/lnbits/lnbits/blob/cb3fd566478502a9579389c40e21b8eafba41bad/lnbits/wallets/blink.py#L342'>lnbits/wallets/blink.py~L342</a>
```diff
             }
             response = await self._graphql_query(payload)
             wallets = (
-                response.get("data", {})
+                response
+                .get("data", {})
                 .get("me", {})
                 .get("defaultAccount", {})
                 .get("wallets", [])
```

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (+6 -3 lines across 3 files)</summary>
<p>

<a href='https://github.com/pypa/setuptools/blob/29031718a55e5c7d5bbfc572b84d35d1f1f52aff/setup.py#L24'>setup.py~L24</a>
```diff
 
     _pth_name = 'distutils-precedence'
     _pth_contents = (
-        textwrap.dedent(
+        textwrap
+        .dedent(
             """
         import os
         var = 'SETUPTOOLS_USE_DISTUTILS'
```
<a href='https://github.com/pypa/setuptools/blob/29031718a55e5c7d5bbfc572b84d35d1f1f52aff/setuptools/_normalization.py#L137'>setuptools/_normalization.py~L137</a>
```diff
     # See bdist_wheel.safer_name
     return (
         # Per https://packaging.python.org/en/latest/specifications/name-normalization/#name-normalization
-        re.sub(r"[-_.]+", "-", safe_name(value))
+        re
+        .sub(r"[-_.]+", "-", safe_name(value))
         .lower()
         # Per https://packaging.python.org/en/latest/specifications/binary-distribution-format/#escaping-and-unicode
         .replace("-", "_")
```
<a href='https://github.com/pypa/setuptools/blob/29031718a55e5c7d5bbfc572b84d35d1f1f52aff/setuptools/command/build_ext.py#L354'>setuptools/command/build_ext.py~L354</a>
```diff
         if not self.dry_run:
             with open(stub_file, 'w', encoding="utf-8") as f:
                 content = (
-                    textwrap.dedent(f"""
+                    textwrap
+                    .dedent(f"""
                     def __bootstrap__():
                        global __bootstrap__, __file__, __loader__
                        import sys, os, importlib.resources as irs, importlib.util
```

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (+6 -3 lines across 1 file)</summary>
<p>

<a href='https://github.com/python/mypy/blob/b5587a383fab59231b571751f4955434b43ba381/mypy/test/teststubtest.py#L203'>mypy/test/teststubtest.py~L203</a>
```diff
         with contextlib.redirect_stdout(output), contextlib.redirect_stderr(outerr):
             test_stubs(parse_options([TEST_MODULE_NAME] + options), use_builtins_fixtures=True)
     filtered_output = remove_color_code(
-        output.getvalue()
+        output
+        .getvalue()
         # remove cwd as it's not available from outside
         .replace(os.path.realpath(tmp_dir) + os.sep, "")
         .replace(tmp_dir + os.sep, "")
     )
     filtered_outerr = remove_color_code(
-        outerr.getvalue()
+        outerr
+        .getvalue()
         # remove cwd as it's not available from outside
         .replace(os.path.realpath(tmp_dir) + os.sep, "")
         .replace(tmp_dir + os.sep, "")
```
<a href='https://github.com/python/mypy/blob/b5587a383fab59231b571751f4955434b43ba381/mypy/test/teststubtest.py#L2769'>mypy/test/teststubtest.py~L2769</a>
```diff
                 test_stubs(parse_options([TEST_MODULE_NAME]), use_builtins_fixtures=True)
 
             filtered_output = remove_color_code(
-                output.getvalue()
+                output
+                .getvalue()
                 .replace(os.path.realpath(tmp_dir) + os.sep, "")
                 .replace(tmp_dir + os.sep, "")
             )
```

</p>
</details>
<details><summary><a href="https://github.com/qdrant/qdrant-client">qdrant/qdrant-client</a> (+4 -2 lines across 1 file)</summary>
<p>

<a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/tests/congruence_tests/test_sparse_idf_search.py#L55'>tests/congruence_tests/test_sparse_idf_search.py~L55</a>
```diff
     )
 
     assert (
-        local_client.get_collection(COLLECTION_NAME)
+        local_client
+        .get_collection(COLLECTION_NAME)
         .config.params.sparse_vectors["sparse-text"]
         .modifier
         == models.Modifier.IDF
```
<a href='https://github.com/qdrant/qdrant-client/blob/49fa101696e092a09b9bbf1c3383d03d8f992bcb/tests/congruence_tests/test_sparse_idf_search.py#L81'>tests/congruence_tests/test_sparse_idf_search.py~L81</a>
```diff
     )
 
     assert (
-        local_client.get_collection(COLLECTION_NAME)
+        local_client
+        .get_collection(COLLECTION_NAME)
         .config.params.sparse_vectors["sparse-text"]
         .modifier
         == models.Modifier.NONE
```

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+12 -6 lines across 5 files)</summary>
<p>

<a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/app.py#L2222'>reflex/app.py~L2222</a>
```diff
 
         # Unroll reverse proxy forwarded headers.
         client_ip = (
-            headers.get(
+            headers
+            .get(
                 "x-forwarded-for",
                 client_ip,
             )
```
<a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/compiler/compiler.py#L210'>reflex/compiler/compiler.py~L210</a>
```diff
         raise ValueError(msg)
     if (
         len(
-            stylesheet_full_path.absolute()
+            stylesheet_full_path
+            .absolute()
             .relative_to(assets_app_path.absolute())
             .parts
         )
```
<a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/components/dynamic.py#L119'>reflex/components/dynamic.py~L119</a>
```diff
             if line.startswith("import "):
                 if 'from "$/' in line or 'from "/' in line:
                     module_code_lines[ix] = (
-                        line.replace("import ", "const ", 1)
+                        line
+                        .replace("import ", "const ", 1)
                         .replace(" as ", ": ")
                         .replace(" from ", " = window['__reflex'][", 1)
                         + "]"
```
<a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/components/dynamic.py#L128'>reflex/components/dynamic.py~L128</a>
```diff
                     for lib in libs_in_window:
                         if f'from "{lib}"' in line:
                             module_code_lines[ix] = (
-                                line.replace("import ", "const ", 1)
+                                line
+                                .replace("import ", "const ", 1)
                                 .replace(
                                     f' from "{lib}"', f" = window.__reflex['{lib}']", 1
                                 )
```
<a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/event.py#L1106'>reflex/event.py~L1106</a>
```diff
     get_element_by_id = FunctionStringVar.create("document.getElementById")
 
     return run_script(
-        get_element_by_id.call(elem_id)
+        get_element_by_id
+        .call(elem_id)
         .to(ObjectVar)
         .scrollIntoView.to(FunctionVar)
         .call(align_to_top),
```
<a href='https://github.com/reflex-dev/reflex/blob/2545378b984ea0a278bf642a96084563b0e9d31a/reflex/utils/pyi_generator.py#L576'>reflex/utils/pyi_generator.py~L576</a>
```diff
 
         if isinstance(annotation, str) and annotation.lower().startswith("tuple["):
             inside_of_tuple = (
-                annotation.removeprefix("tuple[")
+                annotation
+                .removeprefix("tuple[")
                 .removeprefix("Tuple[")
                 .removesuffix("]")
             )
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+26 -13 lines across 10 files)</summary>
<p>

<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/balances/historical.py#L173'>rotkehlchen/balances/historical.py~L173</a>
```diff
         data = None
         if df.height != 0:
             result_df = (
-                df.rechunk()
+                df
+                .rechunk()
                 .sort("sort_key")
                 .with_columns(pl.col("delta").cum_sum().alias("amount"))
                 .select(["timestamp", "amount"])
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/balances/historical.py#L243'>rotkehlchen/balances/historical.py~L243</a>
```diff
             # Sum them up in order to reconstruct the running balance for each asset,
             # then keep only the last balance of each day (the end-of-day snapshot).
             pivoted = (
-                df.rechunk()
+                df
+                .rechunk()
                 .sort("sort_key")
                 .with_columns(
                     pl.col("delta").cum_sum().over("asset").alias("balance"),
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/chain/ethereum/airdrops.py#L638'>rotkehlchen/chain/ethereum/airdrops.py~L638</a>
```diff
     airdrop_lazy_dataframe = get_airdrop_data(airdrop_data, protocol_name, data_dir)
 
     for address, amount_raw in (
-        airdrop_lazy_dataframe.filter(
+        airdrop_lazy_dataframe
+        .filter(
             pl.selectors.by_index(0).is_in(addresses),
         )
         .select(
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/chain/evm/decoding/morpho/utils.py#L136'>rotkehlchen/chain/evm/decoding/morpho/utils.py~L136</a>
```diff
     """Query Morpho reward distributor addresses from the api and cache them."""
     try:
         response_data = (
-            requests.get(
+            requests
+            .get(
                 url=f"{MORPHO_REWARDS_API}?chains={chain_id.value}",
                 timeout=CachedSettings().get_timeout_tuple(),
             )
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/db/checks.py#L35'>rotkehlchen/db/checks.py~L35</a>
```diff
         return s
 
     return (
-        WHITESPACE_RE.sub(replacer, text)
+        WHITESPACE_RE
+        .sub(replacer, text)
         .replace(" ", "")
         .replace("\n", "")
         .replace('"', "'")
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/exchanges/okx.py#L200'>rotkehlchen/exchanges/okx.py~L200</a>
```diff
             path += f"?{urlencode(params)}"
 
         datestr = (
-            datetime.datetime.now(tz=datetime.UTC)
+            datetime.datetime
+            .now(tz=datetime.UTC)
             .isoformat(timespec="milliseconds")
             .replace("+00:00", "Z")
         )  # noqa: E501
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/externalapis/google_calendar.py#L245'>rotkehlchen/externalapis/google_calendar.py~L245</a>
```diff
                 # API resource for calendar event
                 # https://developers.google.com/workspace/calendar/api/v3/reference/events#resource
                 events_result = (
-                    service.events()
+                    service
+                    .events()
                     .list(  # pylint: disable=no-member
                         calendarId=calendar_id,
                         pageToken=page_token,
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/tests/api/test_history_events_export.py#L98'>rotkehlchen/tests/api/test_history_events_export.py~L98</a>
```diff
             for attr in base_headers:
                 if attr == "timestamp":
                     timestamp_as_int = (
-                        datetime.strptime(
+                        datetime
+                        .strptime(
                             row["timestamp"],
                             DEFAULT_DATE_DISPLAY_FORMAT,
                         )
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/tests/db/test_db_upgrades.py#L6723'>rotkehlchen/tests/db/test_db_upgrades.py~L6723</a>
```diff
             == result
         )  # noqa: E501
         assert (
-            cursor.execute(  # Verify the unique constraint was updated
+            cursor
+            .execute(  # Verify the unique constraint was updated
                 "SELECT sql FROM sqlite_master WHERE type='table' AND name='history_events'",
             )
             .fetchone()[0]
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/tests/fixtures/google.py#L71'>rotkehlchen/tests/fixtures/google.py~L71</a>
```diff
         page_token = None
         while True:
             response = (
-                self.drive_service.files()
+                self.drive_service
+                .files()
                 .list(  # pylint: disable=no-member
                     spaces="drive",
                     fields="nextPageToken, files(id, name)",
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/tests/fixtures/google.py#L95'>rotkehlchen/tests/fixtures/google.py~L95</a>
```diff
             },
         }
         spreadsheet = (
-            self.sheets_service.spreadsheets()
+            self.sheets_service
+            .spreadsheets()
             .create(  # pylint: disable=no-member
                 body=body,
                 fields="spreadsheetId",
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/tests/fixtures/google.py#L181'>rotkehlchen/tests/fixtures/google.py~L181</a>
```diff
 
     def get_cell_ranges(self, sheet_id: str, range_names: list[str]) -> list[dict[str, Any]]:
         result = (
-            self.sheets_service.spreadsheets()
+            self.sheets_service
+            .spreadsheets()
             .values()
             .batchGet(  # pylint: disable=no-member
                 spreadsheetId=sheet_id,
```
<a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/tests/unit/accounting/test_misc.py#L433'>rotkehlchen/tests/unit/accounting/test_misc.py~L433</a>
```diff
     assert len(rotki.accountant.pots[0].cost_basis.missing_acquisitions) == 0
     assert (
         len(
-            used_acquisitions := rotki.accountant.pots[0]
+            used_acquisitions := rotki.accountant
+            .pots[0]
             .cost_basis.get_events(asset=A_ETH)
             .used_acquisitions
         )
```

</p>
</details>
<details><summary><a href="https://github.com/yandex/ch-backup">yandex/ch-backup</a> (+2 -1 lines across 1 file)</summary>
<p>

<a href='https://github.com/yandex/ch-backup/blob/d1828427a7d41e9643eddf0ca318d587a68fcbfe/tests/integration/modules/zookeeper.py#L20'>tests/integration/modules/zookeeper.py~L20</a>
```diff
     zk = _get_zookeeper_client(context, node)
     zk.start()
     instance_count = (
-        context.conf.get("services", {})
+        context.conf
+        .get("services", {})
         .get("clickhouse", {})
         .get("docker_instances", 1)
     )
```

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+548 -288 lines across 127 files)</summary>
<p>

<a href='https://github.com/indico/indico/blob/b84e870869d7ff91189ad797171625c0093e7fc6/bin/utils/create_module.py#L66'>bin/utils/create_module.py~L66</a>
```diff
 
 def write_header(f):
     f.write(
-        textwrap.dedent("""
+        textwrap
+        .dedent("""
         # This file is part of Indico.
         # Copyright (C) 2002 - {year} CERN
         #
```
<a href='https://github.com/indico/indico/blob/b84e870869d7ff91189ad797171625c0093e7fc6/bin/utils/create_module.py#L87'>bin/utils/create_module.py~L87</a>
```diff
         table_name = table_name[6:]
     f.write('\n\n')
     f.write(
-        textwrap.dedent("""
+        textwrap
+        .dedent("""
         class {cls}(db.Model):
             __tablename__ = '{table}'
             __table_args__ = {{'schema': '{schema}'}}
```
<a href='https://github.com/indico/indico/blob/b84e870869d7ff91189ad797171625c0093e7fc6/bin/utils/storage_checksums.py#L25'>bin/utils/storage_checksums.py~L25</a>
```diff
 
 def make_query(model):
     return (
-        model.query.filter(model.md5 == '', model.storage_file_id.isnot(None))  # noqa: PLC1901
+        model.query
+        .filter(model.md5 == '', model.storage_file_id.isnot(None))  # noqa: PLC1901
         .filter_by(**SPECIAL_FILTERS.get(model, {}))
         .options(lazyload('*'), load_only('storage_backend', 'storage_file_id', 'md5'))
     )
```
<a href='https://github.com/indico/indico/blob/b84e870869d7ff91189ad797171625c0093e7fc6/indico/cli/maintenance.py#L51'>indico/cli/maintenance.py~L51</a>
```diff
     """
     fixed_something = False
     broken = (
-        EventPrincipal.query.join(EventRole, EventRole.id == EventPrincipal.event_role_id)
+        EventPrincipal.query
+        .join(EventRole, EventRole.id == EventPrincipal.event_role_id)
         .filter(EventPrincipal.type == PrincipalType.event_role, EventPrincipal.event_id != EventRole.event_id)
         .all()
     )
```
<a href='https://github.com/indico/indico/blob/b84e870869d7ff91189ad797171625c0093e7fc6/indico/cli/maintenance.py#L59'>indico/cli/maintenance.py~L59</a>
```diff
     fixed_something = fixed_something or bool(broken)
 
     broken = (
-        SessionPrincipal.query.join(Session, Session.id == SessionPrincipal.session_id)
+        SessionPrincipal.query
+        .join(Session, Session.id == SessionPrincipal.session_id)
         .join(EventRole, EventRole.id == SessionPrincipal.event_role_id)
         .filter(SessionPrincipal.type == PrincipalType.event_role, Session.event_id != EventRole.event_id)
         .all()
```
<a href='https://github.com/indico/indico/blob/b84e870869d7ff91189ad797171625c0093e7fc6/indico/cli/maintenance.py#L68'>indico/cli/maintenance.py~L68</a>
```diff
     fixed_something = fixed_something or bool(broken)
 
     broken = (
-        ContributionPrincipal.query.join(Contribution, Contribution.id == ContributionPrincipal.contribution_id)
+        ContributionPrincipal.query
+        .join(Contribution, Contribution.id == ContributionPrincipal.contribution_id)
         .join(EventRole, EventRole.id == ContributionPrincipal.event_role_id)
         .filter(ContributionPrincipal.type == PrincipalType.event_role, Contribution.event_id != EventRole.event_id)
         .all()
```
<a href='https://github.com/indico/indico/blob/b84e870869d7ff91189ad797171625c0093e7fc6/indico/cli/maintenance.py#L77'>indico/cli/maintenance.py~L77</a>
```diff
     fixed_something = fixed_something or bool(broken)
 
     broken = (
-        AttachmentFolderPrincipal.query.join(
-            AttachmentFolder, AttachmentFolder.id == AttachmentFolderPrincipal.folder_id
-        )
+        AttachmentFolderPrincipal.query
+        .join(AttachmentFolder, AttachmentFolder.id == AttachmentFolderPrincipal.folder_id)
         .join(EventRole, EventRole.id == AttachmentFolderPrincipal.event_role_id)
         .filter(
             AttachmentFolderPrincipal.type == PrincipalType.event_role, AttachmentFolder.event_id != EventRole.event_id
```
<a href='https://github.com/indico/indico/blob/b84e870869d7ff91189ad797171625c0093e7fc6/indico/cli/maintenance.py#L90'>indico/cli/maintenance.py~L90</a>
```diff
     fixed_something = fixed_something or bool(broken)
 
     broken = (
-        AttachmentPrincipal.query.join(Attachment, Attachment.id == AttachmentPrincipal.attachment_id)
+        AttachmentPrincipal.query
+        .join(Attachment, Attachment.id == AttachmentPrincipal.attachment_id)
         .join(AttachmentFolder, AttachmentFolder.id == Attachment.folder_id)
         .join(EventRole, EventRole.id == AttachmentPrincipal.event_role_id)
         .filter(AttachmentPrincipal.type == PrincipalType.event_role, AttachmentFolder.event_id != EventRole.event_id)
```
<a href='https://github.com/indico/indico/blob/b84e870869d7ff91189ad797171625c0093e7fc6/indico/cli/user.py#L448'>indico/cli/user.py~L448</a>
```diff
         token.scopes = (token.scopes | set(add_scopes)) - set(del_scopes)
     if new_token_name:
         conflict = (
-            user.query_personal_tokens()
+            user
+            .query_personal_tokens()
             .filter(PersonalToken.id != token.id, db.func.lower(PersonalToken.name) == new_token_name.lower())
             .has_rows()
         )
```
<a href='https://github.com/indico/indico/blob/b84e870869d7ff91189ad797171625c0093e7fc6/indico/core/db/sqlalchemy/attachments.py#L49'>indico/core/db/sqlalchemy/attachments.py~L49</a>
```diff
 
     assert cls.attachment_count is None
     query = (
-        db.select([db.func.count(Attachment.id)])
+        db
+        .select([db.func.count(Attachment.id)])
         .select_from(db.join(Attachment, AttachmentFolder, Attachment.folder_id == AttachmentFolder.id))
         .where(
             db.and_(
```
<a href='https://github.com/indico/indico/blob/b84e870869d7ff91189ad797171625c0093e7fc6/indico/core/db/sqlalchemy/principals.py#L548'>indico/core/db/sqlalchemy/principals.py~L548</a>
```diff
         for entry in query:
             parent = getattr(entry, relationship_attr)
             existing = (
-                cls.query.with_parent(parent, 'acl_entries')
+                cls.query
+                .with_parent(parent, 'acl_entries')
                 .options(noload('user'), noload('local_group'))
                 .filter_by(principal=user)
                 .first()
```
<a href='https://github.com/indico/indico/blob/b84e870869d7ff91189ad797171625c0093e7fc6/indico/core/db/sqlalchemy/util/queries.py#L52'>indico/core/db/sqlalchemy/util/queries.py~L52</a>
```diff
     """Escape a string to be used as a plain string in LIKE."""
     escape_char = '\\'
     return (
-        value.replace(escape_char, escape_char * 2)  # literal escape char needs to be escaped
+        value
+        .replace(escape_char, escape_char * 2)  # literal escape char needs to be escaped
         .replace('%', escape_char + '%')  # we don't

... (truncated 3717 lines) ...



---
