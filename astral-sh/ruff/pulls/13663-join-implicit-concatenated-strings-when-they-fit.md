```yaml
number: 13663
title: Join implicit concatenated strings when they fit on a line
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: micha/format-implicit-concatenated-strings
created_at: 2024-10-07T14:47:37Z
updated_at: 2024-10-24T09:52:25Z
url: https://github.com/astral-sh/ruff/pull/13663
synced_at: 2026-01-12T15:55:45Z
```

# Join implicit concatenated strings when they fit on a line

---

_@MichaReiser_

## Summary

Implements https://github.com/astral-sh/ruff/issues/9457 for f-strings, regular strings, and byte literals. The implementation doesn't handle raw strings or triple quoted strings, similar to black's string-processing preview style.

Here an example from the ecosystem check. The formatter now joins the implicit concatenated f-string into a single string literal.

```diff
             raise TypeError(
-                "Expected str or callable for parameter 'probe', "
-                f"not '{method.__class__.__name__}'"
+                f"Expected str or callable for parameter 'probe', not '{method.__class__.__name__}'"
             )
```

What makes this change "complicated" is that Ruff has already many special casing for strings:

* Comments at the end of an assignment where the value is a string gets inlined:
  ```python
  aaaaaaaaaaaaa = (
       "long string literal"  # comment
  )

  # instead  of 
  aaaaaaaaaaaaa = (
       "long string literal"
  )  # comment
  ```
* Docstring formatting trims leading and trailing whitespace

In addition, there are many other string-related preview styles, and this implementation must be compatible with the current stable formatting and any other combination of preview styles. Specifically, the implementation must support the stable and preview f-string formatting.

## `assert` formatting

This new style requires a few smaller changes in how expressions containing strings are formatted to mitigate instabilities. However, there's one large change that I'm undecided if we should move forward with it. 

The existing assertion formatting is prone to unstable formatting when the right is an implicit concatenated string that can be joined. The easiest fix was to change the formatting to use `Parenthesize::IfBreaksParenthesized` which we use in other places where we format multiple expressions with the `maybe_parenthesize` layout. However, it has the effect that assertions now break from right to left: We always parenthesize the right before we start splitting the left. This impacts formatting of almost all `assert` statements that are longer than the configured line length and the right is a string because we used to split the assertion and we now parenthesize the message instead. 

Here two examples from the ecosystem checks:

```diff
     with conn.read_ctx() as cursor:  # make sure it wrote in the DB but not the last one
-        assert cursor.execute("SELECT b from a").fetchall() == [
-            (1,),
-            (2,),
-            (3,),
-            (4,),
-        ], "other greenlet should write to the DB"  # noqa: E501
+        assert cursor.execute("SELECT b from a").fetchall() == [(1,), (2,), (3,), (4,)], (
+            "other greenlet should write to the DB"
+        )  # noqa: E501
```

```diff
                         new_id = f"{id}{suffix}{id_suffixes[id]}"
                     resolved_ids[index] = new_id
                     id_suffixes[id] += 1
-        assert len(resolved_ids) == len(
-            set(resolved_ids)
-        ), f"Internal error: {resolved_ids=}"
+        assert len(resolved_ids) == len(set(resolved_ids)), (
+            f"Internal error: {resolved_ids=}"
+        )
         return resolved_ids
 
     def _resolve_ids(self) -> Iterable[str]:
```

You can see how the formatter now keeps the assertion flat (left) and parenthesizes the message. I do find that this overall improves the readability and is also in line with what we do in assignments: We parenthesize the value to avoid splitting the target. 

I have three concerns with the style change:

**1. Black compatibility/Upgrade diff**

IMO, this is the most important one. Our assert formatting has never been compatible with black and this has come up in the past (https://github.com/astral-sh/ruff/issues/13386, https://github.com/astral-sh/ruff/issues/8331, https://github.com/astral-sh/ruff/issues/8388), but there's no clear consensus from users on which style they prefer. This PR would deviate our implementation further from Black's style and it will lead to larger diffs when migrating from Black but it also leads to large diffs when upgrading to the new style.



**2. More parentheses**

Black overall tries to reduce the parentheses. For example, Black prefers

```python
if aaaaaaaaaaaaaa + bbbbbbbbbbbbbbb / len([
    122222,
    33333333333333,
    44444444444444,
    55444,
]):
    ...
```

over 

```python
if (
    aaaaaaaaaaaaaa
    + bbbbbbbbbbbbbbb
    / len([122222, 33333333333333, 44444444444444, 55444])
):
    ...
```

when the left-most or right-most expression has its own parentheses (and some more rules). Black also applies this to assert formatting (`-: Black, +: Ruff`). 

```diff

-            assert isinstance(
-                document, dict
-            ), "document should be of type `dict[str,Any]`. But found: `{}`".format(
-                type(document)
+            assert isinstance(document, dict), (
+                "document should be of type `dict[str,Any]`. But found: `{}`".format(
+                    type(document)
+                )
             )
```

```diff
-        assert isinstance(
-            input, dict
-        ), "The input to RunnablePassthrough.assign() must be a dict."
+        assert isinstance(input, dict), (
+            "The input to RunnablePassthrough.assign() must be a dict."
+        )
 
```

You can see how Black uses the parentheses of the call expression over introducing new parentheses to break the right. 

IMO, this makes assert statements harder to read because it is unclear where the assertion ends and the message start. However, it helps to reduce the overall number of parentheses


**3. assertions that end in parentheses**

There's one cases where the new formatting is arguably worse:

```python
# Black

assert graph.artifacts == [
    GraphArtifact(
        id=expected_top_level_artifact.id,
        created=expected_top_level_artifact.created,
        key=expected_top_level_artifact.key,
        type=expected_top_level_artifact.type,
        data=(
            expected_top_level_artifact.data
            if expected_top_level_artifact.type == "progress"
            else None
        ),
        is_latest=True,
    )
], "Expected artifacts associated with the flow run but not with a task to be included at the roof of the graph."

# Ruff 
assert graph.artifacts == [
    GraphArtifact(
        id=expected_top_level_artifact.id,
        created=expected_top_level_artifact.created,
        key=expected_top_level_artifact.key,
        type=expected_top_level_artifact.type,
        data=expected_top_level_artifact.data
        if expected_top_level_artifact.type == "progress"
        else None,
        is_latest=True,
    )
], (
    "Expected artifacts associated with the flow run but not with a task to be included at the roof of the graph."
)
```

In this example, parenthesizing the message is *useless* because it doesn't help to get more space for the message. The indent requires as much space as the `], (`.

 


The reason I went with this change (and am seeking feedback now) is that:

* It's an extremely easy fix for the instability
* We use break right over left in other places. Therefore, it's arguably more consistent for Ruff
* I do find that it improves formatting in many cases

I do think it would be possible to implement something closer to today's style by using `best_fit` similar to the assignment formatting. And it might not even be that much work (a day or two). So I'm up to tackle this if we conclude that we want to keep closer to Black's style, reduce the upgrade diff, or are concerned about the "bad" cases.




## TODOs

* [x] Review for instabilities when an implicit concatenated string is the right side of an assignment where the target or the annotation is splittable. 
* [x] Review for instabilities with the hug expressions style
* [x] Review implicit concatenated string formatting in `maybe_parenthesize_expression` changes similar to assert stmt (e.g. `YieldExpr`)
* [x] Support the f-string formatting preview style (Big)
* [x] Test with `QuoteStyle` preserve
* [x] In doc-string positions
   ```python
    def in_docstring_position():
        "diffent '" 'quote "are fine"                '
    ```
* [x] Handle strings in `ExprStmt` positions (there's no group)
* [x] Review changes to larger repositories and the ecosystem results
* [x] ~~(stretch) Remove parentheses when collapsing f-strings https://play.ruff.rs/1355c0b0-f452-445a-8ffc-6285e1b0885e~~
* [x] Tests for `can_omit_optional_parentheses` changes.

## Review

* I would appreciate feedback on the assertion style changes.
* Look out if there are more string edge cases that need handling

@AlexWaygood I don't expect you to review the code changes but I highly value your input on the assertion formatting changes. 

## Test Plan

I added plenty of new integration tests and I reviewed the ecosystem changes and did test the formatting on some larger projects. 

Non-truncated ecosystem changes: https://gist.github.com/MichaReiser/c82d6132e577d9e7ee9ae85543904099


---

_Label `formatter` added by @MichaReiser on 2024-10-07 14:47_

---

_Label `preview` added by @MichaReiser on 2024-10-07 14:47_

---

_Comment by @github-actions[bot] on 2024-10-07 14:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+2228 -2609 lines in 575 files in 38 projects; 16 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/DisnakeDev/disnake/blob/a34d0f95b4465f5fcf8bc5b20c25040e155f00e6/disnake/player.py#L550'>disnake/player.py~L550</a>
```diff
             fallback = cls._probe_codec_fallback
         else:
             raise TypeError(
-                "Expected str or callable for parameter 'probe', "
-                f"not '{method.__class__.__name__}'"
+                f"Expected str or callable for parameter 'probe', not '{method.__class__.__name__}'"
             )
 
         codec = bitrate = None
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+59 -73 lines across 32 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/cli/utils.py#L278'>rasa/cli/utils.py~L278</a>
```diff
     # Check if a valid setting for `max_history` was given
     if isinstance(max_history, int) and max_history < 1:
         raise argparse.ArgumentTypeError(
-            f"The value of `--max-history {max_history}` " f"is not a positive integer."
+            f"The value of `--max-history {max_history}` is not a positive integer."
         )
 
     return validator.verify_story_structure(
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/cli/x.py#L165'>rasa/cli/x.py~L165</a>
```diff
         attempts -= 1
 
     rasa.shared.utils.cli.print_error_and_exit(
-        "Could not fetch runtime config from server at '{}'. " "Exiting.".format(
+        "Could not fetch runtime config from server at '{}'. Exiting.".format(
             config_endpoint
         )
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/actions/action.py#L322'>rasa/core/actions/action.py~L322</a>
```diff
         if message is None:
             if not self.silent_fail:
                 logger.error(
-                    "Couldn't create message for response '{}'." "".format(
+                    "Couldn't create message for response '{}'.".format(
                         self.utter_action
                     )
                 )
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/actions/action.py#L470'>rasa/core/actions/action.py~L470</a>
```diff
         else:
             if not self.silent_fail:
                 logger.error(
-                    "Couldn't create message for response action '{}'." "".format(
+                    "Couldn't create message for response action '{}'.".format(
                         self.action_name
                     )
                 )
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/console.py#L194'>rasa/core/channels/console.py~L194</a>
```diff
     exit_text = INTENT_MESSAGE_PREFIX + "stop"
 
     rasa.shared.utils.cli.print_success(
-        "Bot loaded. Type a message and press enter " "(use '{}' to exit): ".format(
+        "Bot loaded. Type a message and press enter (use '{}' to exit): ".format(
             exit_text
         )
     )
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/channels/telegram.py#L97'>rasa/core/channels/telegram.py~L97</a>
```diff
                     reply_markup.add(KeyboardButton(button["title"]))
         else:
             logger.error(
-                "Trying to send text with buttons for unknown " "button type {}".format(
+                "Trying to send text with buttons for unknown button type {}".format(
                     button_type
                 )
             )
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/nlg/callback.py#L81'>rasa/core/nlg/callback.py~L81</a>
```diff
         body = nlg_request_format(utter_action, tracker, output_channel, **kwargs)
 
         logger.debug(
-            "Requesting NLG for {} from {}." "The request body is {}." "".format(
+            "Requesting NLG for {} from {}.The request body is {}.".format(
                 utter_action, self.nlg_endpoint.url, json.dumps(body)
             )
         )
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/policies/policy.py#L250'>rasa/core/policies/policy.py~L250</a>
```diff
         max_training_samples = kwargs.get("max_training_samples")
         if max_training_samples is not None:
             logger.debug(
-                "Limit training data to {} training samples." "".format(
+                "Limit training data to {} training samples.".format(
                     max_training_samples
                 )
             )
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/policies/ted_policy.py#L835'>rasa/core/policies/ted_policy.py~L835</a>
```diff
             # take the last prediction in the sequence
             similarities = outputs["similarities"][:, -1, :]
         else:
-            raise TypeError(
-                "model output for `similarities` " "should be a numpy array"
-            )
+            raise TypeError("model output for `similarities` should be a numpy array")
         if isinstance(outputs["scores"], np.ndarray):
             confidences = outputs["scores"][:, -1, :]
         else:
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/policies/unexpected_intent_policy.py#L612'>rasa/core/policies/unexpected_intent_policy.py~L612</a>
```diff
         if isinstance(output["similarities"], np.ndarray):
             sequence_similarities = output["similarities"][:, -1, :]
         else:
-            raise TypeError(
-                "model output for `similarities` " "should be a numpy array"
-            )
+            raise TypeError("model output for `similarities` should be a numpy array")
 
         # Check for unlikely intent
         last_user_uttered_event = tracker.get_last_event_for(UserUttered)
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/test.py#L772'>rasa/core/test.py~L772</a>
```diff
         ):
             story_dump = YAMLStoryWriter().dumps(partial_tracker.as_story().story_steps)
             error_msg = (
-                f"Model predicted a wrong action. Failed Story: " f"\n\n{story_dump}"
+                f"Model predicted a wrong action. Failed Story: \n\n{story_dump}"
             )
             raise WrongPredictionException(error_msg)
     elif prev_action_unlikely_intent:
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/train.py#L34'>rasa/core/train.py~L34</a>
```diff
             for policy_config in policy_configs:
                 config_name = os.path.splitext(os.path.basename(policy_config))[0]
                 logging.info(
-                    "Starting to train {} round {}/{}" " with {}% exclusion" "".format(
+                    "Starting to train {} round {}/{} with {}% exclusion".format(
                         config_name, current_run, len(exclusion_percentages), percentage
                     )
                 )
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/training/interactive.py#L723'>rasa/core/training/interactive.py~L723</a>
```diff
     # export training data and quit
     questions = questionary.form(
         export_stories=questionary.text(
-            message="Export stories to (if file exists, this "
-            "will append the stories)",
+            message="Export stories to (if file exists, this will append the stories)",
             default=PATHS["stories"],
             validate=io_utils.file_type_validator(
                 rasa.shared.data.YAML_FILE_EXTENSIONS,
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/training/interactive.py#L738'>rasa/core/training/interactive.py~L738</a>
```diff
             default=PATHS["nlu"],
             validate=io_utils.file_type_validator(
                 list(rasa.shared.data.TRAINING_DATA_EXTENSIONS),
-                "Please provide a valid export path for the NLU data, "
-                "e.g. 'nlu.yml'.",
+                "Please provide a valid export path for the NLU data, e.g. 'nlu.yml'.",
             ),
         ),
         export_domain=questionary.text(
-            message="Export domain file to (if file exists, this "
-            "will be overwritten)",
+            message="Export domain file to (if file exists, this will be overwritten)",
             default=PATHS["domain"],
             validate=io_utils.file_type_validator(
                 rasa.shared.data.YAML_FILE_EXTENSIONS,
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/utils.py#L41'>rasa/core/utils.py~L41</a>
```diff
     """
     if use_syslog:
         formatter = logging.Formatter(
-            "%(asctime)s [%(levelname)-5.5s] [%(process)d]" " %(message)s"
+            "%(asctime)s [%(levelname)-5.5s] [%(process)d] %(message)s"
         )
         socktype = SOCK_STREAM if syslog_protocol == TCP_PROTOCOL else SOCK_DGRAM
         syslog_handler = logging.handlers.SysLogHandler(
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/utils.py#L73'>rasa/core/utils.py~L73</a>
```diff
     """
     if hot_idx >= length:
         raise ValueError(
-            "Can't create one hot. Index '{}' is out " "of range (length '{}')".format(
+            "Can't create one hot. Index '{}' is out of range (length '{}')".format(
                 hot_idx, length
             )
         )
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py#L166'>rasa/nlu/featurizers/sparse_featurizer/count_vectors_featurizer.py~L166</a>
```diff
                 )
             if self.stop_words is not None:
                 logger.warning(
-                    "Analyzer is set to character, "
-                    "provided stop words will be ignored."
+                    "Analyzer is set to character, provided stop words will be ignored."
                 )
             if self.max_ngram == 1:
                 logger.warning(
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/server.py#L289'>rasa/server.py~L289</a>
```diff
         raise ErrorResponse(
             HTTPStatus.BAD_REQUEST,
             "BadRequest",
-            "Invalid parameter value for 'include_events'. "
-            "Should be one of {}".format(enum_values),
+            "Invalid parameter value for 'include_events'. Should be one of {}".format(
+                enum_values
+            ),
             {"parameter": "include_events", "in": "query"},
         )
 
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/shared/core/domain.py#L198'>rasa/shared/core/domain.py~L198</a>
```diff
             domain = cls.from_directory(path)
         else:
             raise InvalidDomain(
-                "Failed to load domain specification from '{}'. "
-                "File not found!".format(os.path.abspath(path))
+                "Failed to load domain specification from '{}'. File not found!".format(
+                    os.path.abspath(path)
+                )
             )
 
         return domain
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/shared/core/events.py#L1961'>rasa/shared/core/events.py~L1961</a>
```diff
 
     def __str__(self) -> Text:
         """Returns text representation of event."""
-        return (
-            "ActionExecutionRejected("
-            "action: {}, policy: {}, confidence: {})"
-            "".format(self.action_name, self.policy, self.confidence)
+        return "ActionExecutionRejected(action: {}, policy: {}, confidence: {})".format(
+            self.action_name, self.policy, self.confidence
         )
 
     def __hash__(self) -> int:
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/shared/core/generator.py#L401'>rasa/shared/core/generator.py~L401</a>
```diff
 
             if num_active_trackers:
                 logger.debug(
-                    "Starting {} ... (with {} trackers)" "".format(
+                    "Starting {} ... (with {} trackers)".format(
                         phase_name, num_active_trackers
                     )
                 )
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/shared/core/generator.py#L517'>rasa/shared/core/generator.py~L517</a>
```diff
                     phase = 0
                 else:
                     logger.debug(
-                        "Found {} unused checkpoints " "in current phase." "".format(
+                        "Found {} unused checkpoints in current phase.".format(
                             len(unused_checkpoints)
                         )
                     )
                     logger.debug(
-                        "Found {} active trackers " "for these checkpoints." "".format(
+                        "Found {} active trackers for these checkpoints.".format(
                             num_active_trackers
                         )
                     )
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/shared/core/generator.py#L553'>rasa/shared/core/generator.py~L553</a>
```diff
                 augmented_trackers, self.config.max_number_of_augmented_trackers
             )
             logger.debug(
-                "Subsampled to {} augmented training trackers." "".format(
+                "Subsampled to {} augmented training trackers.".format(
                     len(augmented_trackers)
                 )
             )
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/shared/core/trackers.py#L634'>rasa/shared/core/trackers.py~L634</a>
```diff
         """
         if not isinstance(dialogue, Dialogue):
             raise ValueError(
-                f"story {dialogue} is not of type Dialogue. "
-                f"Have you deserialized it?"
+                f"story {dialogue} is not of type Dialogue. Have you deserialized it?"
             )
 
         self._reset()
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/shared/core/training_data/story_reader/story_reader.py#L83'>rasa/shared/core/training_data/story_reader/story_reader.py~L83</a>
```diff
         )
         if parsed_events is None:
             raise StoryParseError(
-                "Unknown event '{}'. It is Neither an event " "nor an action).".format(
+                "Unknown event '{}'. It is Neither an event nor an action).".format(
                     event_name
                 )
             )
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/shared/core/training_data/story_reader/yaml_story_reader.py#L334'>rasa/shared/core/training_data/story_reader/yaml_story_reader.py~L334</a>
```diff
 
         if not self.domain:
             logger.debug(
-                "Skipped validating if intent is in domain as domain " "is `None`."
+                "Skipped validating if intent is in domain as domain is `None`."
             )
             return
 
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/shared/nlu/training_data/formats/dialogflow.py#L34'>rasa/shared/nlu/training_data/formats/dialogflow.py~L34</a>
```diff
 
         if fformat not in {DIALOGFLOW_INTENT, DIALOGFLOW_ENTITIES}:
             raise ValueError(
-                "fformat must be either {}, or {}" "".format(
+                "fformat must be either {}, or {}".format(
                     DIALOGFLOW_INTENT, DIALOGFLOW_ENTITIES
                 )
             )
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/shared/utils/io.py#L127'>rasa/shared/utils/io.py~L127</a>
```diff
             return f.read()
     except FileNotFoundError:
         raise FileNotFoundException(
-            f"Failed to read file, " f"'{os.path.abspath(filename)}' does not exist."
+            f"Failed to read file, '{os.path.abspath(filename)}' does not exist."
         )
     except UnicodeDecodeError:
         raise FileIOException(
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/shared/utils/io.py#L157'>rasa/shared/utils/io.py~L157</a>
```diff
     """
     if not isinstance(path, str):
         raise ValueError(
-            f"`resource_name` must be a string type. " f"Got `{type(path)}` instead"
+            f"`resource_name` must be a string type. Got `{type(path)}` instead"
         )
 
     if os.path.isfile(path):
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/shared/utils/io.py#L443'>rasa/shared/utils/io.py~L443</a>
```diff
             )
     except FileNotFoundError:
         raise FileNotFoundException(
-            f"Failed to read file, " f"'{os.path.abspath(file_path)}' does not exist."
+            f"Failed to read file, '{os.path.abspath(file_path)}' does not exist."
         )
 
 
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/common.py#L308'>rasa/utils/common.py~L308</a>
```diff
         access_logger.addHandler(file_handler)
     if use_syslog:
         formatter = logging.Formatter(
-            "%(asctime)s [%(levelname)-5.5s] [%(process)d]" " %(message)s"
+            "%(asctime)s [%(levelname)-5.5s] [%(process)d] %(message)s"
         )
         socktype = SOCK_STREAM if syslog_protocol == TCP_PROTOCOL else SOCK_DGRAM
         syslog_handler = logging.handlers.SysLogHandler(
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/endpoints.py#L33'>rasa/utils/endpoints.py~L33</a>
```diff
         return EndpointConfig.from_dict(content[endpoint_type])
     except FileNotFoundError:
         logger.error(
-            "Failed to read endpoint configuration " "from {}. No such file.".format(
+            "Failed to read endpoint configuration from {}. No such file.".format(
                 os.path.abspath(filename)
             )
         )
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/engine/recipes/test_default_recipe.py#L98'>tests/engine/recipes/test_default_recipe.py~L98</a>
```diff
         (
             "data/test_config/config_pretrained_embeddings_mitie.yml",
             "data/graph_schemas/config_pretrained_embeddings_mitie_train_schema.yml",
-            "data/graph_schemas/"
-            "config_pretrained_embeddings_mitie_predict_schema.yml",
+            "data/graph_schemas/config_pretrained_embeddings_mitie_predict_schema.yml",
             TrainingType.BOTH,
             False,
         ),
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/graph_components/validators/test_default_recipe_validator.py#L758'>tests/graph_components/validators/test_default_recipe_validator.py~L758</a>
```diff
     if should_warn:
         with pytest.warns(
             UserWarning,
-            match=(f"'{RulePolicy.__name__}' is not " "included in the model's "),
+            match=(f"'{RulePolicy.__name__}' is not included in the model's "),
         ) as records:
             validator.validate(importer)
     else:
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/graph_components/validators/test_default_recipe_validator.py#L859'>tests/graph_components/validators/test_default_recipe_validator.py~L859</a>
```diff
     num_duplicates: bool,
     priority: int,
 ):
-    assert (
-        len(policy_types) >= priority + num_duplicates
-    ), f"This tests needs at least {priority + num_duplicates} many types."
+    assert len(policy_types) >= priority + num_duplicates, (
+        f"This tests needs at least {priority + num_duplicates} many types."
+    )
 
     # start with a schema where node i has priority i
     nodes = {
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/graph_components/validators/test_default_recipe_validator.py#L968'>tests/graph_components/validators/test_default_recipe_validator.py~L968</a>
```diff
     with pytest.warns(
         UserWarning,
         match=(
-            "Found rule-based training data but no policy "
-            "supporting rule-based data."
+            "Found rule-based training data but no policy supporting rule-based data."
         ),
     ):
         validator.validate(importer)
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/nlu/featurizers/test_count_vectors_featurizer.py#L773'>tests/nlu/featurizers/test_count_vectors_featurizer.py~L773</a>
```diff
 
 
 @pytest.mark.parametrize(
-    "initial_train_text, additional_train_text, " "use_shared_vocab",
+    "initial_train_text, additional_train_text, use_shared_vocab",
     [("am I the coolest person?", "no", True), ("rasa rasa", "sara sara", False)],
 )
 def test_use_shared_vocab_exception(
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/nlu/featurizers/test_regex_featurizer.py#L44'>tests/nlu/featurizers/test_regex_featurizer.py~L44</a>
```diff
 
 
 @pytest.mark.parametrize(
-    "sentence, expected_sequence_features, expected_sentence_features,"
-    "labeled_tokens",
+    "sentence, expected_sequence_features, expected_sentence_features,labeled_tokens",
     [
         (
             "hey how are you today",
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/nlu/featurizers/test_regex_featurizer.py#L219'>tests/nlu/featurizers/test_regex_featurizer.py~L219</a>
```diff
 
 
 @pytest.mark.parametrize(
-    "sentence, expected_sequence_features, expected_sentence_features, "
-    "labeled_tokens",
+    "sentence, expected_sequence_features, expected_sentence_features, labeled_tokens",
     [
         (
             "lemonade and mapo tofu",
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/nlu/featurizers/test_regex_featurizer.py#L383'>tests/nlu/featurizers/test_regex_featurizer.py~L383</a>
```diff
 
 
 @pytest.mark.parametrize(
-    "sentence, expected_sequence_features, expected_sentence_features,"
-    "case_sensitive",
+    "sentence, expected_sequence_features, expected_sentence_features,case_sensitive",
     [
         ("Hey How are you today", [0.0, 0.0, 0.0], [0.0, 0.0, 0.0], True),
         ("Hey How are you today", [0.0, 1.0, 0.0], [0.0, 1.0, 0.0], False),
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/nlu/featurizers/test_spacy_featurizer.py#L131'>tests/nlu/featurizers/test_spacy_featurizer.py~L131</a>
```diff
         vecs = ftr._features_for_doc(doc)
         vecs_capitalized = ftr._features_for_doc(doc_capitalized)
 
-        assert np.allclose(
-            vecs, vecs_capitalized, atol=1e-5
-        ), "Vectors are unequal for texts '{}' and '{}'".format(
-            e.get(TEXT), e.get(TEXT).capitalize()
+        assert np.allclose(vecs, vecs_capitalized, atol=1e-5), (
+            "Vectors are unequal for texts '{}' and '{}'".format(
+                e.get(TEXT), e.get(TEXT).capitalize()
+            )
         )
 
 
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/nlu/test_train.py#L151'>tests/nlu/test_train.py~L151</a>
```diff
             #   publicly available anymore
             #   (see https://github.com/RasaHQ/rasa/issues/6806)
             continue
-        assert (
-            cls.__name__ in all_components
-        ), "`all_components` template is missing component."
+        assert cls.__name__ in all_components, (
+            "`all_components` template is missing component."
+        )
 
 
 @pytest.mark.timeout(600, func_only=True)
```
<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/shared/core/training_data/test_graph.py#L10'>tests/shared/core/training_data/test_graph.py~L10</a>
```diff
     for n in sorted_nodes:
         deps = incoming_edges.get(n, [])
         # checks that all incoming edges are from nodes we have already visited
-        assert all([
-            d in visited or (d, n) in removed_edges for d in deps
-        ]), "Found an incoming edge from a node that wasn't visited yet!"
+        assert all([d in visited or (d, n) in removed_edges for d in deps]), (
+            "Found an incoming edge from a node that wasn't visited yet!"
+        )
         visited.add(n)
 
 
```

</p>
</details>
<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+17 -16 lines across 5 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/Snowflake-Labs/snowcli/blob/b87c8e89815c9892eec46bee6d4ea6fc2c723fa2/src/snowflake/cli/_plugins/nativeapp/artifacts.py#L248'>src/snowflake/cli/_plugins/nativeapp/artifacts.py~L248</a>
```diff
     def __init__(self, *, project_root: Path, deploy_root: Path):
         # If a relative path ends up here, it's a bug in the app and can lead to other
         # subtle bugs as paths would be resolved relative to the current working directory.
-        assert (
-            project_root.is_absolute()
-        ), f"Project root {project_root} must be an absolute path."
-        assert (
-            deploy_root.is_absolute()
-        ), f"Deploy root {deploy_root} must be an absolute path."
+        assert project_root.is_absolute(), (
+            f"Project root {project_root} must be an absolute path."
+        )
+        assert deploy_root.is_absolute(), (
+            f"Deploy root {deploy_root} must be an absolute path."
+        )
 
         self._project_root: Path = resolve_without_follow(project_root)
         self._deploy_root: Path = resolve_without_follow(deploy_root)
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/b87c8e89815c9892eec46bee6d4ea6fc2c723fa2/tests/api/utils/test_templating_functions.py#L309'>tests/api/utils/test_templating_functions.py~L309</a>
```diff
     "input_value, expected_output",
     [
         ("test_value", "test_value"),
-        (" T'EST_Va l.u-e" "", "TEST_Value"),
+        (" T'EST_Va l.u-e", "TEST_Value"),
         ("", "_"),
         ('""', "_"),
         ("_val.ue", "_value"),
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/b87c8e89815c9892eec46bee6d4ea6fc2c723fa2/tests_e2e/test_installation.py#L66'>tests_e2e/test_installation.py~L66</a>
```diff
     assert "Initialized the new project in" in output
     for file in files_to_check:
         expected_generated_file = project_path / file
-        assert expected_generated_file.exists(), f"[{expected_generated_file}] does not exist. It should be generated from templates directory."
+        assert expected_generated_file.exists(), (
+            f"[{expected_generated_file}] does not exist. It should be generated from templates directory."
+        )
 
 
 @pytest.mark.e2e
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/b87c8e89815c9892eec46bee6d4ea6fc2c723fa2/tests_integration/spcs/testing_utils/spcs_services_utils.py#L223'>tests_integration/spcs/testing_utils/spcs_services_utils.py~L223</a>
```diff
 
         describe_result = self._execute_describe(service_name)
         assert describe_result.exit_code == 0, describe_result.output
-        assert (
-            new_container_name not in describe_result.json[0]["spec"]
-        ), f"Container name '{new_container_name}' found in output of DESCRIBE SERVICE before spec has been updated. This is unexpected."
+        assert new_container_name not in describe_result.json[0]["spec"], (
+            f"Container name '{new_container_name}' found in output of DESCRIBE SERVICE before spec has been updated. This is unexpected."
+        )
 
         spec_path = f"{self._setup.test_root_path}/spcs/spec/spec_upgrade.yml"
         upgrade_result = self._setup.runner.invoke_with_connection_json([
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/b87c8e89815c9892eec46bee6d4ea6fc2c723fa2/tests_integration/spcs/testing_utils/spcs_services_utils.py#L244'>tests_integration/spcs/testing_utils/spcs_services_utils.py~L244</a>
```diff
         describe_result = self._execute_describe(service_name)
         assert describe_result.exit_code == 0, describe_result.output
         # do not assert direct equality because the spec field in output of DESCRIBE SERVICE has some extra info
-        assert (
-            new_container_name in describe_result.json[0]["spec"]
-        ), f"Container name '{new_container_name}' from spec_upgrade.yml not found in output of DESCRIBE SERVICE."
+        assert new_container_name in describe_result.json[0]["spec"], (
+            f"Container name '{new_container_name}' from spec_upgrade.yml not found in output of DESCRIBE SERVICE."
+        )
 
     def list_endpoints_should_show_endpoint(self, service_name: str):
         result = self._setup.runner.invoke_with_connection_json([
```
<a href='https://github.com/Snowflake-Labs/snowcli/blob/b87c8e89815c9892eec46bee6d4ea6fc2c723fa2/tests_integration/test_snowpark.py#L622'>tests_integration/test_snowpark.py~L622</a>
```diff
                 "type": "function",
             },
             {
-                "object": f"{database}.{different_schema}.schema_function(name "
-                "string)",
+                "object": f"{database}.{different_schema}.schema_function(name string)",
                 "status": "created",
                 "type": "function",
             },
```

</p>
</details>
<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+2 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/aiven/aiven-client/blob/9bc64ad5dc630690df9e000fa56250367b049bd0/aiven/client/cli.py#L868'>aiven/client/cli.py~L868</a>
```diff
                         types = spec["type"]
                         if isinstance(types, str) and types == "null":
                             print(
-                                "  {title}{description}\n" "     => --remove-option {name}".format(
+                                "  {title}{description}\n     => --remove-option {name}".format(
                                     name=name,
                                     title=spec["title"],
                                     description=description,
```
<a href='https://github.com/aiven/aiven-client/blob/9bc64ad5dc630690df9e000fa56250367b049bd0/aiven/client/cli.py#L879'>aiven/client/cli.py~L879</a>
```diff
                                 types = [types]
                             type_str = " or ".join(t for t in types if t != "null")
                             print(
-                                "  {title}{description}\n" "     => -c {name}=<{type}>  {default}".format(
+                                "  {title}{description}\n     => -c {name}=<{type}>  {default}".format(
                                     name=name,
                                     type=type_str,
                                     default=default_desc,
```

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+49 -62 lines across 8 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py#L1052'>src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py~L1052</a>
```diff
 
         if not self._has_run:
             raise RuntimeError(
-                "The simulation must be run before a results "
-                "dictionary can be created."
+                "The simulation must be run before a results dictionary can be created."
             )
 
         # Determine locations of points in the detector plane using unit
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/src/plasmapy/particles/_parsing.py#L328'>src/plasmapy/particles/_parsing.py~L328</a>
```diff
             element = element_info
         else:
             raise InvalidParticleError(
-                f"The string '{element_info}' does not correspond to "
-                f"a valid element."
+                f"The string '{element_info}' does not correspond to a valid element."
             )
         return element
 
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/src/plasmapy/particles/_parsing.py#L352'>src/plasmapy/particles/_parsing.py~L352</a>
```diff
 
             if isotope not in _isotopes.data_about_isotopes:
                 raise InvalidParticleError(
-                    f"The string '{isotope}' does not correspond to "
-                    f"a valid isotope."
+                    f"The string '{isotope}' does not correspond to a valid isotope."
                 )
 
         else:
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/src/plasmapy/particles/_parsing.py#L434'>src/plasmapy/particles/_parsing.py~L434</a>
```diff
             )
         else:
             warnings.warn(
-                "Redundant charge information for particle "
-                f"'{argument}' with Z = {Z}.",
+                f"Redundant charge information for particle '{argument}' with Z = {Z}.",
                 ParticleWarning,
             )
 
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/src/plasmapy/particles/_parsing.py#L535'>src/plasmapy/particles/_parsing.py~L535</a>
```diff
             )
         else:
             warnings.warn(
-                "Redundant charge information for particle "
-                f"'{argument}' with Z = {Z}.",
+                f"Redundant charge information for particle '{argument}' with Z = {Z}.",
                 ParticleWarning,
             )
 
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/src/plasmapy/particles/atomic.py#L586'>src/plasmapy/particles/atomic.py~L586</a>
```diff
             isotopes_list = known_isotopes_for_element(element)
         except InvalidElementError as ex:
             raise InvalidElementError(
-                "known_isotopes is unable to get "
-                f"isotopes from an input of: {argument}"
+                f"known_isotopes is unable to get isotopes from an input of: {argument}"
             ) from ex
         except InvalidParticleError as ex:
             raise InvalidParticleError("Invalid particle in known_isotopes.") from ex
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/src/plasmapy/particles/ionization_state.py#L98'>src/plasmapy/particles/ionization_state.py~L98</a>
```diff
 
     def __repr__(self) -> str:
         return (
-            f"IonicLevel({self.ionic_symbol!r}, "
-            f"ionic_fraction={self.ionic_fraction})"
+            f"IonicLevel({self.ionic_symbol!r}, ionic_fraction={self.ionic_fraction})"
         )
 
     @property
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/src/plasmapy/particles/ionization_state.py#L477'>src/plasmapy/particles/ionization_state.py~L477</a>
```diff
 
             if len(fractions) != self.atomic_number + 1:
                 raise ParticleError(
-                    "The length of ionic_fractions must be "
-                    f"{self.atomic_number + 1}."
+                    f"The length of ionic_fractions must be {self.atomic_number + 1}."
                 )
 
             if isinstance(fractions, u.Quantity):
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/src/plasmapy/particles/ionization_state_collection.py#L321'>src/plasmapy/particles/ionization_state_collection.py~L321</a>
```diff
         all_nans = np.all(np.isnan(new_fractions))
         if not all_nans and (new_fractions.min() < 0 or new_fractions.max() > 1):
             raise ValueError(
-                f"{errmsg} because the new ionic fractions are not "
-                f"all between 0 and 1."
+                f"{errmsg} because the new ionic fractions are not all between 0 and 1."
             )
 
         normalized = np.isclose(np.sum(new_fractions), 1, rtol=self.tol)
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/tests/diagnostics/test_thomson.py#L191'>tests/diagnostics/test_thomson.py~L191</a>
```diff
     alpha, wavelength, Skw = single_species_collective_spectrum
 
     # Check that alpha is correct
-    assert np.isclose(
-        alpha, 1.801, atol=0.01
-    ), f"Collective case alpha returns {alpha} instead of expected 1.801"
+    assert np.isclose(alpha, 1.801, atol=0.01), (
+        f"Collective case alpha returns {alpha} instead of expected 1.801"
+    )
 
     i_width = width_at_value(wavelength.value, Skw.value, 2e-13)
     e_width = width_at_value(wavelength.value, Skw.value, 0.2e-13)
 
     # Check that the widths of the ion and electron features match expectations
     assert np.isclose(i_width, 0.1599, 1e-3), (
-        "Collective case ion feature "
-        f"width is {i_width}"
-        "instead of expected 0.1599"
+        f"Collective case ion feature width is {i_width}instead of expected 0.1599"
     )
 
     assert np.isclose(e_width, 17.7899, 1e-3), (
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/tests/diagnostics/test_thomson.py#L421'>tests/diagnostics/test_thomson.py~L421</a>
```diff
 
     # Check width
     assert np.isclose(width, 0.17, 1e-2), (
-        f"Multiple ion species case spectrum width is {width} instead of "
-        "expected 0.17"
+        f"Multiple ion species case spectrum width is {width} instead of expected 0.17"
     )
 
     # Check max value
     assert np.isclose(max_skw, 6e-12, 1e-11), (
-        f"Multiple ion species case spectrum max is {max_skw} instead of "
-        "expected 6e-12"
+        f"Multiple ion species case spectrum max is {max_skw} instead of expected 6e-12"
     )
 
     # Check max peak location
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/tests/diagnostics/test_thomson.py#L492'>tests/diagnostics/test_thomson.py~L492</a>
```diff
     alpha, wavelength, Skw = single_species_non_collective_spectrum
 
     # Check that alpha is correct
-    assert np.isclose(
-        alpha, 0.05707, atol=0.01
-    ), f"Non-collective case alpha returns {alpha} instead of expected 0.05707"
+    assert np.isclose(alpha, 0.05707, atol=0.01), (
+        f"Non-collective case alpha returns {alpha} instead of expected 0.05707"
+    )
 
     e_width = width_at_value(wavelength.value, Skw.value, 0.2e-13)
 
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/tests/formulary/collisions/test_collisions_frequencies.py#L781'>tests/formulary/collisions/test_collisions_frequencies.py~L781</a>
```diff
             self.True_electrons, methodVal.si.value, rtol=1e-1, atol=0.0
         )
         errStr = (
-            f"Collision frequency should be {self.True_electrons} and "
-            f"not {methodVal}."
+            f"Collision frequency should be {self.True_electrons} and not {methodVal}."
         )
         assert testTrue, errStr
 
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/tests/formulary/collisions/test_collisions_frequencies.py#L803'>tests/formulary/collisions/test_collisions_frequencies.py~L803</a>
```diff
             self.True_protons, methodVal.si.value, rtol=1e-1, atol=0.0
         )
         errStr = (
-            f"Collision frequency should be {self.True_protons} and "
-            f"not {methodVal}."
+            f"Collision frequency should be {self.True_protons} and not {methodVal}."
         )
         assert testTrue, errStr
 
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/tests/particles/test_ionization_collection.py#L34'>tests/particles/test_ionization_collection.py~L34</a>
```diff
 
     for element, abundance_from_abundances in abundances.items():
         abundance_from_log_abundances = 10 ** log_abundances[element]
-        assert np.isclose(
-            abundance_from_abundances, abundance_from_log_abundances
-        ), "Mismatch between abundances and log_abundances."
+        assert np.isclose(abundance_from_abundances, abundance_from_log_abundances), (
+            "Mismatch between abundances and log_abundances."
+        )
 
 
 def has_attribute(attribute, tests_dict):
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/tests/particles/test_ionization_collection.py#L142'>tests/particles/test_ionization_collection.py~L142</a>
```diff
         assert (
             a == a  # noqa: PLR0124
         ), "IonizationStateCollection doesn't equal itself."
-        assert (
-            a == b
-        ), "IonizationStateCollection instance does not equal identical instance."
+        assert a == b, (
+            "IonizationStateCollection instance does not equal identical instance."
+        )
 
     @pytest.mark.parametrize(
         "test_name",
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/tests/particles/test_ionization_collection.py#L464'>tests/particles/test_ionization_collection.py~L464</a>
```diff
         )
 
     def test_kappa_defaults_to_inf(self) -> None:
-        assert np.isinf(
-            self.instance.kappa
-        ), "kappa does not default to a value of inf."
+        assert np.isinf(self.instance.kappa), (
+            "kappa does not default to a value of inf."
+        )
 
     @pytest.mark.parametrize(
         "uninitialized_attribute", ["number_densities", "ionic_fractions"]
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/tests/particles/test_ionization_collection.py#L474'>tests/particles/test_ionization_collection.py~L474</a>
```diff
     def test_attribute_defaults_to_dict_of_nans(self, uninitialized_attribute) -> None:
         command = f"self.instance.{uninitialized_attribute}"
         default_value = eval(command)  # noqa: S307
-        assert (
-            list(default_value.keys()) == self.elements
-        ), "Incorrect base particle keys."
+        assert list(default_value.keys()) == self.elements, (
+            "Incorrect base particle keys."
+        )
         for element in self.elements:
-            assert (
-                len(default_value[element]) == atomic_number(element) + 1
-            ), f"Incorrect number of ionization levels for {element}."
-            assert np.all(
-                np.isnan(default_value[element])
-            ), f"The values do not default to an array of nans for {element}."
+            assert len(default_value[element]) == atomic_number(element) + 1, (
+                f"Incorrect number of ionization levels for {element}."
+            )
+            assert np.all(np.isnan(default_value[element])), (
+                f"The values do not default to an array of nans for {element}."
+            )
 
     @pytest.mark.parametrize(
         "uninitialized_attribute", ["abundances", "log_abundances"]
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/tests/particles/test_ionization_collection.py#L539'>tests/particles/test_ionization_collection.py~L539</a>
```diff
         self.new_fractions = [0.3, 0.7]
         self.instance["H"] = self.new_fractions
         resulting_fractions = self.instance.ionic_fractions["H"]
-        assert np.allclose(
-            self.new_fractions, resulting_fractions
-        ), "Ionic fractions for H not set using __setitem__."
+        assert np.allclose(self.new_fractions, resulting_fractions), (
+            "Ionic fractions for H not set using __setitem__."
+        )
         assert "He" in self.instance.ionic_fractions, (
             "He is missing in ionic_fractions after __setitem__ was "
             "used to set H ionic fractions."
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/tests/particles/test_ionization_collection.py#L687'>tests/particles/test_ionization_collection.py~L687</a>
```diff
 
         assert u.quantity.allclose(
             self.instance.ionic_fractions[element], valid_ionic_fractions
-        ), "Item assignment of valid number densities did not yield correct ionic fractions."
+        ), (
+            "Item assignment of valid number densities did not yield correct ionic fractions."
+        )
 
         assert u.quantity.allclose(
             self.instance.number_densities[element], valid_number_densities
-        ), "Item assignment of valid number densities did not yield correct number densities."
+        ), (
+            "Item assignment of valid number densities did not yield correct number densities."
+        )
 
     def test_resetting_invalid_densities(self) -> None:
         """
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/tests/particles/test_ionization_collection.py#L782'>tests/particles/test_ionization_collection.py~L782</a>
```diff
         number_densities = self.instances[test_key].number_densities
         for base_particle in self.instances[test_key].base_particles:
             assert not np.any(np.isnan(number_densities[base_particle])), (
-                f"Test {test_key} should have number densities "
-                f"defined, but doesn't."
+                f"Test {test_key} should have number densities defined, but doesn't."
             )
 
     @pytest.mark.parametrize("test_key", ["no_ndens3", "no_ndens4", "no_ndens5"])
```
<a href='https://github.com/PlasmaPy/PlasmaPy/blob/42d7a0b1484ddb1b2edfc1fdf94234223b821147/tests/particles/test_ionization_collection.py#L791'>tests/particles/test_ionization_collection.py~L791</a>
```diff
         number_densities = self.instances[test_key].number_densities
         for base_particle in self.instances[test_key].base_particles:
             assert np.all(np.isnan(number_densities[base_particle])), (
-                f"Test {test_key} should not have number densities "
-                f"defined, but does."
+                f"Test {test_key} should not have number densities defined, but does."
             )
 
     @pytest.mark.parametrize(
```

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+146 -209 lines across 48 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/airflow/jobs/scheduler_job_runner.py#L506'>airflow/jobs/scheduler_job_runner.py~L506</a>
```diff
 
                         if current_task_concurrency >= task_concurrency_limit:
                             self.log.info(
-                                "Not executing %s since the task concurrency for"
-                                " this task has been reached.",
+                                "Not executing %s since the task concurrency for this task has been reached.",
                                 task_instance,
                             )
                             starved_tasks.add((task_instance.dag_id, task_instance.task_id))
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/airflow/www/views.py#L4657'>airflow/www/views.py~L4657</a>
```diff
             if skipped:
                 skipped_repr = ", ".join(repr(k) for k in sorted(skipped))
                 flash(
-                    f"The variables with these keys: {skipped_repr} were skipped "
-                    "because they already exists",
+                    f"The variables with these keys: {skipped_repr} were skipped because they already exists",
                     "warning",
                 )
             self.update_redirect()
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/dev/breeze/src/airflow_breeze/commands/setup_commands.py#L77'>dev/breeze/src/airflow_breeze/commands/setup_commands.py~L77</a>
```diff
 )
 @setup.command(
     name="self-upgrade",
-    help="Self upgrade Breeze. By default it re-installs Breeze "
-    f"from {get_installation_airflow_sources()}."
+    help=f"Self upgrade Breeze. By default it re-installs Breeze from {get_installation_airflow_sources()}."
     if not generating_command_images()
     else "Self upgrade Breeze.",
 )
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/dev/breeze/src/airflow_breeze/commands/setup_commands.py#L174'>dev/breeze/src/airflow_breeze/commands/setup_commands.py~L174</a>
```diff
     get_console().print(f"[info]Used Airflow sources : {get_used_airflow_sources()}[/]\n")
     if get_verbose():
         get_console().print(
-            f"[info]Installation sources config hash : "
-            f"{get_installation_sources_config_metadata_hash()}[/]"
+            f"[info]Installation sources config hash : {get_installation_sources_config_metadata_hash()}[/]"
         )
         get_console().print(
             f"[info]Used sources config hash         : {get_used_sources_setup_metadata_hash()}[/]"
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/dev/breeze/src/airflow_breeze/utils/docker_command_utils.py#L180'>dev/breeze/src/airflow_breeze/utils/docker_command_utils.py~L180</a>
```diff
     )
     if response.returncode != 0:
         get_console().print(
-            "[error]Docker is not running.[/]\n"
-            "[warning]Please make sure Docker is installed and running.[/]"
+            "[error]Docker is not running.[/]\n[warning]Please make sure Docker is installed and running.[/]"
         )
         sys.exit(1)
 
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/dev/breeze/tests/test_selective_checks.py#L90'>dev/breeze/tests/test_selective_checks.py~L90</a>
```diff
                     assert received_value == expected_value, f"Correct value for {expected_key!r}"
                 else:
                     print(
-                        f"\n[red]ERROR: The key '{expected_key}' missing but "
-                        f"it is expected. Expected value:"
+                        f"\n[red]ERROR: The key '{expected_key}' missing but it is expected. Expected value:"
                     )
                     print_in_color(expected_value)
                     print_in_color("\nOutput received:")
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/dev/breeze/tests/test_selective_checks.py#L494'>dev/breeze/tests/test_selective_checks.py~L494</a>
```diff
                     "providers/tests/http/file.py",
                 ),
                 {
-                    "affected-providers-list-as-string": "amazon apache.livy "
-                    "dbt.cloud dingding discord http",
+                    "affected-providers-list-as-string": "amazon apache.livy dbt.cloud dingding discord http",
                     "all-python-versions": "['3.9']",
                     "all-python-versions-list-as-string": "3.9",
                     "python-versions": "['3.9']",
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/docker_tests/test_prod_image.py#L99'>docker_tests/test_prod_image.py~L99</a>
```diff
         packages_installed = set(d["package_name"] for d in providers)
         assert len(packages_installed) != 0
 
-        assert (
-            packages_to_install == packages_installed
-        ), f"List of expected installed packages and image content mismatch. Check {package_file} file."
+        assert packages_to_install == packages_installed, (
+            f"List of expected installed packages and image content mismatch. Check {package_file} file."
+        )
 
     def test_pip_dependencies_conflict(self, default_docker_image):
         try:
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L597'>performance/src/performance_dags/performance_dag/performance_dag_utils.py~L597</a>
```diff
     if env_name in MANDATORY_performance_DAG_VARIABLES:
         if env_name not in performance_dag_conf:
             raise ValueError(
-                f"Mandatory environment variable '{env_name}' "
-                f"is missing from performance dag configuration."
+                f"Mandatory environment variable '{env_name}' is missing from performance dag configuration."
             )
         return performance_dag_conf[env_name]
 
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L417'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py~L417</a>
```diff
                 )
         else:
             raise AirflowException(
-                f"AWS Batch job ({job_id}) description error: exceeded status_retries "
-                f"({self.status_retries})"
+                f"AWS Batch job ({job_id}) description error: exceeded status_retries ({self.status_retries})"
             )
 
     @staticmethod
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/providers/src/airflow/providers/amazon/aws/operators/sagemaker.py#L172'>providers/src/airflow/providers/amazon/aws/operators/sagemaker.py~L172</a>
```diff
         """Raise exception if resource type is not 'model' or 'job'."""
         if resource_type not in ("model", "job"):
             raise AirflowException(
-                "Argument resource_type accepts only 'model' and 'job'. "
-                f"Provided value: '{resource_type}'."
+                f"Argument resource_type accepts only 'model' and 'job'. Provided value: '{resource_type}'."
             )
 
     def _check_if_job_exists(self, job_name: str, describe_func: Callable[[str], Any]) -> bool:
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/providers/src/airflow/providers/amazon/aws/operators/sagemaker.py#L553'>providers/src/airflow/providers/amazon/aws/operators/sagemaker.py~L553</a>
```diff
                 self.operation = "update"
                 sagemaker_operation = self.hook.update_endpoint
                 self.log.warning(
-                    "cannot create already existing endpoint %s, "
-                    "updating it with the given config instead",
+                    "cannot create already existing endpoint %s, updating it with the given config instead",
                     endpoint_info["EndpointName"],
                 )
                 if "Tags" in endpoint_info:
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/providers/src/airflow/providers/celery/executors/default_celery.py#L123'>providers/src/airflow/providers/celery/executors/default_celery.py~L123</a>
```diff
         DEFAULT_CELERY_CONFIG["broker_use_ssl"] = broker_use_ssl
 except AirflowConfigException:
     raise AirflowException(
-        "AirflowConfigException: SSL_ACTIVE is True, "
-        "please ensure SSL_KEY, "
-        "SSL_CERT and SSL_CACERT are set"
+        "AirflowConfigException: SSL_ACTIVE is True, please ensure SSL_KEY, SSL_CERT and SSL_CACERT are set"
     )
 except Exception as e:
     raise AirflowException(
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/providers/src/airflow/providers/common/sql/operators/sql.py#L995'>providers/src/airflow/providers/common/sql/operators/sql.py~L995</a>
```diff
                     test_results[metric] = self.ignore_zero
 
             self.log.info(
-                (
-                    "Current metric for %s: %s\n"
-                    "Past metric for %s: %s\n"
-                    "Ratio for %s: %s\n"
-                    "Threshold: %s\n"
-                ),
+                ("Current metric for %s: %s\nPast metric for %s: %s\nRatio for %s: %s\nThreshold: %s\n"),
                 metric,
                 cur,
                 metric,
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/providers/src/airflow/providers/docker/decorators/docker.py#L141'>providers/src/airflow/providers/docker/decorators/docker.py~L141</a>
```diff
         serializer = serializer or "pickle"
         if serializer not in _SERIALIZERS:
             msg = (
-                f"Unsupported serializer {serializer!r}. "
-                f"Expected one of {', '.join(map(repr, _SERIALIZERS))}"
+                f"Unsupported serializer {serializer!r}. Expected one of {', '.join(map(repr, _SERIALIZERS))}"
             )
             raise AirflowException(msg)
 
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/providers/src/airflow/providers/google/cloud/hooks/bigquery.py#L3722'>providers/src/airflow/providers/google/cloud/hooks/bigquery.py~L3722</a>
```diff
                 test_results[metric] = float(ratios[metric]) < threshold
 
             self.log.info(
-                (
-                    "Current metric for %s: %s\n"
-                    "Past metric for %s: %s\n"
-                    "Ratio for %s: %s\n"
-                    "Threshold: %s\n"
-                ),
+                ("Current metric for %s: %s\nPast metric for %s: %s\nRatio for %s: %s\nThreshold: %s\n"),
                 metric,
                 cur,
                 metric,
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/providers/tests/airbyte/triggers/test_airbyte.py#L210'>providers/tests/airbyte/triggers/test_airbyte.py~L210</a>
```diff
         actual = await generator.asend(None)
         expected = TriggerEvent({
             "status": "error",
-            "message": f"Job run {self.JOB_ID} has not reached a terminal status "
-            f"after {end_time} seconds.",
+            "message": f"Job run {self.JOB_ID} has not reached a terminal status after {end_time} seconds.",
             "job_id": self.JOB_ID,
         })
         assert expected == actual
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/providers/tests/amazon/aws/secrets/test_systems_manager.py#L180'>providers/tests/amazon/aws/secrets/test_systems_manager.py~L180</a>
```diff
         mock_ssm_client.assert_called_once_with(service_name="ssm", use_ssl=False)
 
     @mock.patch(
-        "airflow.providers.amazon.aws.secrets.systems_manager."
-        "SystemsManagerParameterStoreBackend._get_secret"
+        "airflow.providers.amazon.aws.secrets.systems_manager.SystemsManagerParameterStoreBackend._get_secret"
     )
     def test_connection_prefix_none_value(self, mock_get_secret):
         """
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/providers/tests/amazon/aws/secrets/test_systems_manager.py#L197'>providers/tests/amazon/aws/secrets/test_systems_manager.py~L197</a>
```diff
         mock_get_secret.assert_not_called()
 
     @mock.patch(
-        "airflow.providers.amazon.aws.secrets.systems_manager."
-        "SystemsManagerParameterStoreBackend._get_secret"
+        "airflow.providers.amazon.aws.secrets.systems_manager.SystemsManagerParameterStoreBackend._get_secret"
     )
     def test_variable_prefix_none_value(self, mock_get_secret):
         """
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/providers/tests/amazon/aws/secrets/test_systems_manager.py#L214'>providers/tests/amazon/aws/secrets/test_systems_manager.py~L214</a>
```diff
         mock_get_secret.assert_not_called()
 
     @mock.patch(
-        "airflow.providers.amazon.aws.secrets.systems_manager."
-        "SystemsManagerParameterStoreBackend._get_secret"
+        "airflow.providers.amazon.aws.secrets.systems_manager.SystemsManagerParameterStoreBackend._get_secret"
     )
     def test_config_prefix_none_value(self, mock_get_secret):
         """
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/providers/tests/apache/hive/transfers/test_s3_to_hive.py#L170'>providers/tests/apache/hive/transfers/test_s3_to_hive.py~L170</a>
```diff
 
     def test__match_headers(self):
         self.kwargs["field_dict"] = {"Sno": "BIGINT", "Some,Text": "STRING"}
-        assert S3ToHiveOperator(**self.kwargs)._match_headers([
-            "Sno",
-            "Some,Text",
-        ]), "Header row doesn't match expected value"
+        assert S3ToHiveOperator(**self.kwargs)._match_headers(["Sno", "Some,Text"]), (
+            "Header row doesn't match expected value"
+        )
         # Testing with different column order
-        assert not S3ToHiveOperator(**self.kwargs)._match_headers([
-            "Some,Text",
-            "Sno",
-        ]), "Header row doesn't match expected value"
+        assert not S3ToHiveOperator(**self.kwargs)._match_headers(["Some,Text", "Sno"]), (
+            "Header row doesn't match expected value"
+        )
         # Testing with extra column in header
-        assert not S3ToHiveOperator(**self.kwargs)._match_headers([
-            "Sno",
-            "Some,Text",
-            "ExtraColumn",
-        ]), "Header row doesn't match expected value"
+        assert not S3ToHiveOperator(**self.kwargs)._match_headers(["Sno", "Some,Text", "ExtraColumn"]), (
+            "Header row doesn't match expected value"
+        )
 
     def test__delete_top_row_and_compress(self):
         s32hive = S3ToHiveOperator(**self.kwargs)
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/providers/tests/databricks/operators/test_databricks.py#L1983'>providers/tests/databricks/operators/test_databricks.py~L1983</a>
```diff
 
         with pytest.raises(TaskDeferred) as exec_info:
             operator.monitor_databricks_job()
-        assert isinstance(
-            exec_info.value.trigger, DatabricksExecutionTrigger
-        ), "Trigger is not a DatabricksExecutionTrigger"
+        assert isinstance(exec_info.value.trigger, DatabricksExecutionTrigger), (
+            "Trigger is not a DatabricksExecutionTrigger"
+        )
         assert exec_info.value.method_name == "execute_complete"
 
     @mock.patch("airflow.providers.databricks.operators.databricks.DatabricksHook")
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/providers/tests/dbt/cloud/triggers/test_dbt.py#L213'>providers/tests/dbt/cloud/triggers/test_dbt.py~L213</a>
```diff
         actual = await generator.asend(None)
         expected = TriggerEvent({
             "status": "error",
-            "message": f"Job run {self.RUN_ID} has not reached a terminal status "
-            f"after {end_time} seconds.",
+            "message": f"Job run {self.RUN_ID} has not reached a terminal status after {end_time} seconds.",
             "run_id": self.RUN_ID,
         })
         assert expected == actual
```
<a href='https://github.com/apache/airflow/blob/4d54cda4114125bb671b0bfccddc73b646855a2d/providers/tests/docker/operators/test_docker.py#L468'>providers/tests/docker/operators/test_docker.py~L468</a>
```diff
         operator = DockerOperator(
             private_environment={"PRIVATE": "MESSAGE"}, image=TEST_IMAGE, task_id="unittest"
         )
-        assert operator._private_environment == {
-            "PRIVATE": "MESSAGE"
-        }, "To keep this private, it must be an underscored attribute."
+        assert operator._private_environment == {"PRIVATE": "MESSAGE"}, (
+            "To keep this private, it must be an underscored attribute."
+        )
 
     @mock.patch("airflow.providers.docker.operators.docker.StringIO")
     def test_environment_overrides_env_file(self, stringio_mock):
```
<a href='https://github.com/apache/airflow/blob/4d54cda4...*[Comment body truncated]*

---

_Comment by @MichaReiser on 2024-10-16 09:05_

I like what I'm seeing in the diff. I don't like seeing another instability, ugh

---

_Comment by @MichaReiser on 2024-10-16 13:37_

This one seems unfortunate

```diff
         # Written on 06.06.2022 and can be removed after a couple of months if everything goes well
         assert (
             ZERO <= used_amount <= self._acquisitions_heap[0].acquisition_event.remaining_amount
-        ), f"Used amount must be in the interval [0, {self._acquisitions_heap[0].acquisition_event.remaining_amount}] but it was {used_amount} for {asset}"  # noqa: E501
+        ), (
+            f"Used amount must be in the interval [0, {self._acquisitions_heap[0].acquisition_event.remaining_amount}] but it was {used_amount} for {asset}"
+        )  # noqa: E501
 
         self._acquisitions_heap[0].acquisition_event.remaining_amount -= used_amount
         if self._acquisitions_heap[0].acquisition_event.remaining_amount == ZERO:
```

But I think it is related to f-string formatting

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/implicit.rs`:220 on 2024-10-21 13:06_

TODO: This is not correct for f-strings

---

_@MichaReiser reviewed on 2024-10-21 13:06_

---

_Comment by @MichaReiser on 2024-10-22 16:12_

This is mostly done. I plant to do some ecosystem testing tomorrow and do a final self-review before publishing

---

_@MichaReiser reviewed on 2024-10-22 16:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/docstring.rs`:1608 on 2024-10-22 16:13_

TODO: gate behind preview

---

_@MichaReiser reviewed on 2024-10-23 08:34_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:438 on 2024-10-23 08:34_

This is the main change. All other changes are replacing `expression.format().with_options(Parentheses::Never)` with `unparenthesized

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-23 12:01_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-10-23 12:01_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-10-23 12:01_

---

_Marked ready for review by @MichaReiser on 2024-10-23 12:11_

---

_Comment by @AlexWaygood on 2024-10-23 14:01_

> @AlexWaygood I don't expect you to review the code changes but I highly value your input on the assertion formatting changes.

Overall I find these changes _especially_ are a significant improvement to readability.

I tried running this branch on some repos I have clones for locally, and a _lot_ of the changes were related to this, but I found all of them to be improvements. I think there will undoubtedly be some specific patterns where formatting is likely to regress (such as `3. assertions that end in parentheses` in your PR description), but I'm quite strongly in favour of this change overall. I also think the formatting for that specific case would be quite easy for a user to fix manually by splitting the string over multiple lines (but I _don't_ think that's something that Ruff should try to do for the user):

```py
assert graph.artifacts == [
    GraphArtifact(
        id=expected_top_level_artifact.id,
        created=expected_top_level_artifact.created,
        key=expected_top_level_artifact.key,
        type=expected_top_level_artifact.type,
        data=expected_top_level_artifact.data
        if expected_top_level_artifact.type == "progress"
        else None,
        is_latest=True,
    )
], (
    "Expected artifacts associated with the flow run "
    "but not with a task to be included at the roof of the graph."
)
```

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:333 on 2024-10-24 02:03_

(Very helpful.)

---

_@charliermarsh approved on 2024-10-24 02:06_

This is excellent. The code is so clear and easy to read (especially for the formatter). Well done with the comments, clarity, etc.


---

_Comment by @charliermarsh on 2024-10-24 02:07_

I think the `assert` formatting is probably an improvement, though I hear your concerns. This one stuck out to me a bit:

```diff

-            assert isinstance(
-                document, dict
-            ), "document should be of type `dict[str,Any]`. But found: `{}`".format(
-                type(document)
+            assert isinstance(document, dict), (
+                "document should be of type `dict[str,Any]`. But found: `{}`".format(
+                    type(document)
+                )
             )
```

But it's actually _not_ an increase in total lines, and the condition and message are actually more-clearly separated than before.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:367 on 2024-10-24 08:22_

Can you help me understand why shouldn't we concatenate these two strings?

---

_@dhruvmanila approved on 2024-10-24 08:53_

This is really thorough! Thanks for taking the time to add comments throughout the code and especially in tests. It was really easy for me to look at the formatted output directly in test because the comment described the original source code and what to expect.

Regarding the assert formatting, I too think that it's improves readability as it's easier to split the condition and the message mentally.

---

_@MichaReiser reviewed on 2024-10-24 09:10_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:367 on 2024-10-24 09:10_

The important bit here is the magic trailing comma at the end of the list-expression. It always makes the list expand over multiple line. That means, it's guaranteed that the f-string can never fit on a single line. That's why we don't want to apply the implicit concatenated layout.

---

_@dhruvmanila reviewed on 2024-10-24 09:26_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:367 on 2024-10-24 09:26_

Ah right, got it.

---

_@MichaReiser reviewed on 2024-10-24 09:29_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:367 on 2024-10-24 09:29_

But good call out. It helped me discover another bug :)

---

_Comment by @MichaReiser on 2024-10-24 09:52_

Okay. Let's merge this as is and revisit the assert formatting decision depending on the preview style feedback 

---

_Merged by @MichaReiser on 2024-10-24 09:52_

---

_Closed by @MichaReiser on 2024-10-24 09:52_

---

_Branch deleted on 2024-10-24 09:52_

---
