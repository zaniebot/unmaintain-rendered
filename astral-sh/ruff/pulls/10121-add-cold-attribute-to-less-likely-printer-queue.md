```yaml
number: 10121
title: Add cold attribute to less likely printer queue branches
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
  - formatter
assignees: []
merged: true
base: main
head: printer-queue-experiment-with-cold
created_at: 2024-02-25T20:01:04Z
updated_at: 2024-02-26T07:19:41Z
url: https://github.com/astral-sh/ruff/pull/10121
synced_at: 2026-01-12T15:55:31Z
```

# Add cold attribute to less likely printer queue branches

---

_@MichaReiser_

## Summary

The formatter writes most elements to a single `Vec<FormatElement>` except for: 

* Memoized content: It's sometimes necessary to emit the same content but using two different layouts. The formatter memoizes/interns the content's IR to avoid formatting it twice. The internet content stores the elements in its own `Vec`
* `BestFitting` layout: The `BestFitting` IR allows multiple different layouts for the same content. It stores the IR elements of the variants in a dedicated `Vec`

The fact that the Printer must support multiple `Vec`s add complexity to its queue because the queue needs to support a stack of slices where it pops elements from the top slice but takes the next slice if the top slice is empty. 

This PR adds a few `#[cold]` attributes signaling that it's unlikely that the top slice is empty because a) the printer takes elements from the main top-level `Vec` most of the time and b) most slices contain many elements, but have only one end ;)

This gives us a solid 2-3% performance improvement in the printer (I no longer see `FitsQueue::pop` in the top methods when profiling the large dataset)

![image](https://github.com/astral-sh/ruff/assets/1203881/8d793fba-47df-4127-88b1-b35699f50d6a)


@BurntSushi won't believe this

## Test

`cargo test`



---

_Label `performance` added by @MichaReiser on 2024-02-25 20:18_

---

_Label `formatter` added by @MichaReiser on 2024-02-25 20:18_

---

_Comment by @github-actions[bot] on 2024-02-25 20:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>




---

_Marked ready for review by @MichaReiser on 2024-02-25 20:19_

---

_@charliermarsh approved on 2024-02-25 20:58_

That's neat.

---

_@BurntSushi approved on 2024-02-25 23:09_

Wow. Rather impressive. It's very rare to see `#[cold]` matter. :tada: Nice find!

---

_Merged by @MichaReiser on 2024-02-26 07:19_

---

_Closed by @MichaReiser on 2024-02-26 07:19_

---

_Branch deleted on 2024-02-26 07:19_

---
