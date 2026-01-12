```yaml
number: 20535
title: "[`pydoclint`] Fix false positive when Sphinx directives follow Raises section (`DOC502`)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - docstring
  - preview
assignees: []
merged: true
base: main
head: fix-18959
created_at: 2025-09-23T14:33:32Z
updated_at: 2025-11-12T21:37:55Z
url: https://github.com/astral-sh/ruff/pull/20535
synced_at: 2026-01-12T15:57:04Z
```

# [`pydoclint`] Fix false positive when Sphinx directives follow Raises section (`DOC502`)

---

_@danparizher_

## Summary

Fixes #18959


---

_Comment by @github-actions[bot] on 2025-09-23 14:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -8 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/a5eb02d178d34dc72e6415134e8377868da8e4fd/superset/utils/date_parser.py#L173'>superset/utils/date_parser.py:173:5:</a> DOC502 Raised exceptions are not explicitly raised: `Relation to `time_range_lookup``, `- Example`
- <a href='https://github.com/apache/superset/blob/a5eb02d178d34dc72e6415134e8377868da8e4fd/superset/utils/date_parser.py#L203'>superset/utils/date_parser.py:203:5:</a> DOC502 Raised exceptions are not explicitly raised: `Relation to `time_range_lookup``, `- Example`
- <a href='https://github.com/apache/superset/blob/a5eb02d178d34dc72e6415134e8377868da8e4fd/superset/utils/date_parser.py#L234'>superset/utils/date_parser.py:234:5:</a> DOC502 Raised exceptions are not explicitly raised: `Relation to `time_range_lookup``, `- Example`
- <a href='https://github.com/apache/superset/blob/a5eb02d178d34dc72e6415134e8377868da8e4fd/superset/utils/date_parser.py#L279'>superset/utils/date_parser.py:279:5:</a> DOC502 Raised exceptions are not explicitly raised: `Relation to `time_range_lookup``, `- Example`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/d2189367630d9d35b2048dcfc017803ef4635881/libs/langchain/langchain_classic/chat_models/base.py#L79'>libs/langchain/langchain_classic/chat_models/base.py:79:5:</a> DOC502 Raised exceptions are not explicitly raised: `ValueError`, `ImportError`
- <a href='https://github.com/langchain-ai/langchain/blob/d2189367630d9d35b2048dcfc017803ef4635881/libs/langchain/langchain_classic/chat_models/base.py#L79'>libs/langchain/langchain_classic/chat_models/base.py:79:5:</a> DOC502 Raised exceptions are not explicitly raised: `ValueError`, `ImportError`, `o3_mini = init_chat_model("openai`, `claude_sonnet = init_chat_model("anthropic`, `"google_vertexai`, `"what's your name", config={"configurable"`, `config={"configurable"`, `"openai`, `"configurable"`, `"foo_model"`, `"foo_temperature"`, `same way that you would with a normal model`, `class GetWeather(BaseModel)`, `location`, `class GetPopulation(BaseModel)`, `location`, `"Which city is hotter today and which is bigger`, `"Which city is hotter today and which is bigger`, `config={"configurable"`
+ <a href='https://github.com/langchain-ai/langchain/blob/d2189367630d9d35b2048dcfc017803ef4635881/libs/langchain/langchain_classic/embeddings/base.py#L133'>libs/langchain/langchain_classic/embeddings/base.py:133:5:</a> DOC502 Raised exception is not explicitly raised: `ImportError`
- <a href='https://github.com/langchain-ai/langchain/blob/d2189367630d9d35b2048dcfc017803ef4635881/libs/langchain/langchain_classic/embeddings/base.py#L133'>libs/langchain/langchain_classic/embeddings/base.py:133:5:</a> DOC502 Raised exceptions are not explicitly raised: `ImportError`, `model = init_embeddings("openai`, `model = init_embeddings("openai`
+ <a href='https://github.com/langchain-ai/langchain/blob/d2189367630d9d35b2048dcfc017803ef4635881/libs/langchain_v1/langchain/chat_models/base.py#L67'>libs/langchain_v1/langchain/chat_models/base.py:67:5:</a> DOC502 Raised exceptions are not explicitly raised: `ValueError`, `ImportError`
- <a href='https://github.com/langchain-ai/langchain/blob/d2189367630d9d35b2048dcfc017803ef4635881/libs/langchain_v1/langchain/chat_models/base.py#L67'>libs/langchain_v1/langchain/chat_models/base.py:67:5:</a> DOC502 Raised exceptions are not explicitly raised: `ValueError`, `ImportError`, `o3_mini = init_chat_model("openai`, `claude_sonnet = init_chat_model("anthropic`, `gemini_2-5_flash = init_chat_model("google_vertexai`, `configurable_model.invoke("what's your name", config={"configurable"`, `config={"configurable"`, `"openai`, `"configurable"`, `"foo_model"`, `"foo_temperature"`, `same way that you would with a normal model`, `class GetWeather(BaseModel)`, `location`, `class GetPopulation(BaseModel)`, `location`, `"Which city is hotter today and which is bigger`, `"Which city is hotter today and which is bigger`, `config={"configurable"`
+ <a href='https://github.com/langchain-ai/langchain/blob/d2189367630d9d35b2048dcfc017803ef4635881/libs/langchain_v1/langchain/embeddings/base.py#L129'>libs/langchain_v1/langchain/embeddings/base.py:129:5:</a> DOC502 Raised exception is not explicitly raised: `ImportError`
- <a href='https://github.com/langchain-ai/langchain/blob/d2189367630d9d35b2048dcfc017803ef4635881/libs/langchain_v1/langchain/embeddings/base.py#L129'>libs/langchain_v1/langchain/embeddings/base.py:129:5:</a> DOC502 Raised exceptions are not explicitly raised: `ImportError`, `model = init_embeddings("openai`, `model = init_embeddings("openai`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DOC502 | 12 | 4 | 8 | 0 | 0 |

</p>
</details>




---

_Comment by @danparizher on 2025-09-23 14:54_

Note on `ruff-ecosystem` results: a net decrease in violations overall.
- Instances where a “removal/addition” pair appears at the same location.
    - Those paired changes indicate message normalization rather than behavioral regression—the false positives are being eliminated where Sphinx directives follow a Raises section
- Remaining diagnostics have cleaner messages without stray directive/markup fragments.

Net effect is reduced noise and improved precision

---

_Review requested from @ntBre by @MichaReiser on 2025-10-12 06:31_

---

_Label `rule` added by @MichaReiser on 2025-10-12 06:31_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:479 on 2025-10-13 22:03_

What do you think about something more like this for this function?

```rust
fn parse_entries_google(content: &str) -> Vec<QualifiedName<'_>> {
    let mut entries: Vec<QualifiedName> = Vec::new();
    let mut lines = content.lines().peekable();
    let Some(first) = lines.peek() else {
        return entries;
    };
    let indentation = &first[..first.len() - first.trim_start().len()];
    for potential in lines {
        if let Some(entry) = potential.strip_prefix(indentation) {
            if let Some(first_char) = entry.chars().next() {
                if !first_char.is_whitespace() {
                    if let Some(colon_idx) = entry.find(':') {
                        let entry = entry[..colon_idx].trim();
                        if !entry.is_empty() {
                            entries.push(QualifiedName::user_defined(entry));
                        }
                    }
                }
            }
        }
    }
    entries
}
```

This is closer to the `parse_entries_numpy` down below and seems to work on the new test case when I tried it locally.

---

_@ntBre reviewed on 2025-10-13 22:12_

Thanks! I had an idea for a possibly simpler implementation more similar to the numpy version. Let me know what you think. We should keep a close eye on the ecosystem results to make sure they are still improved.

This new result actually looks incorrect to me:

> + [libs/langchain_v1/langchain/agents/react_agent.py:304:9:](https://github.com/langchain-ai/langchain/blob/9c1285cf5b538ebebfffc903621df8a3c5f0b53c/libs/langchain_v1/langchain/agents/react_agent.py#L304) DOC502 Raised exception is not explicitly raised: `MultipleStructuredOutputsError`

Maybe my suggestion will help with that too.

---

_Label `docstring` added by @ntBre on 2025-10-13 22:12_

---

_Label `preview` added by @ntBre on 2025-10-13 22:12_

---

_@danparizher reviewed on 2025-10-15 01:08_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:479 on 2025-10-15 01:08_

This is significantly better, thank you! We will have to see what the ecosystem results will look like.

---

_Review requested from @ntBre by @danparizher on 2025-10-15 01:23_

---

_Comment by @ntBre on 2025-10-15 14:24_

Could you take a look at the ecosystem results? It looks like there are still some issues. I think we may need a combination of the two approaches we've tried, especially breaking early if we find a dedented line like you had initially. We should extract some test cases from the erroneous ecosystem results too.

---

_Comment by @danparizher on 2025-10-15 22:37_

Just to clarify, for the "dedented line" breaking logic, should it:
1. Break immediately when ANY line has less indentation than the first line
2. Break only when a line has NO indentation at all
3. Break when a line is dedented to the same level as the section header

---

_Comment by @ntBre on 2025-10-15 22:46_

I think I was picturing option 1, possibly excluding completely blank lines, assuming those are allowed. I'm looking at 
> + [superset/utils/date_parser.py:173:5:](https://github.com/apache/superset/blob/78907d08cd5a3a53e25050a08328bb3b7770f3c2/superset/utils/date_parser.py#L173) DOC502 Raised exception is not explicitly raised: `- Example`

for example. The `- Example` point under a different heading is being flagged, so I assumed breaking when the indentation decreases would help:

```py
    Returns:
        str: The resulting expression for the start of the specified unit.

    Raises:
        ValueError: If the unit is not one of the valid options.

    Relation to `time_range_lookup`:
        - Handles the "start of" or "beginning of" modifiers in the first regex pattern.
        - Example: "start of this month" → `DATETRUNC(DATETIME('today'), month)`.
    """
```

But that's just an idea.

---

_@ntBre approved on 2025-10-28 18:15_

Thank you! This looks great and shows a very clear improvement in the ecosystem check. 

I'm tempted to go ahead and land this, but I think this could also be a problem for numpy-style docstrings ([example](https://play.ruff.rs/bf640866-ee7a-4f67-96c7-3a36573b735f)). These seem a little trickier since the exceptions are already dedented to the same level as the sphinx directive, assuming I followed the docs correctly. We may just have to check for a leading `..` in that case.

Do you want to try fixing that here too, or would it be better as a separate PR?

---

_Comment by @danparizher on 2025-10-29 21:58_

I'll try to fix that here too, thanks!

---

_Review requested from @ntBre by @danparizher on 2025-10-29 22:20_

---

_@ntBre approved on 2025-11-12 21:35_

Thanks again! 

I just pushed one commit moving the `..` check into the indentation-stripped branch. I don't think this is likely to be common, but I could at least imagine a case like the new test I added where a Sphinx directive was used in the description of an exception, so the outer `break` would have been too eager.

---

_Merged by @ntBre on 2025-11-12 21:37_

---

_Closed by @ntBre on 2025-11-12 21:37_

---
