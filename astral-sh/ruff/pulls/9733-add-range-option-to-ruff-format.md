```yaml
number: 9733
title: "Add `--range` option to `ruff format`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
assignees: []
merged: true
base: main
head: range-formatting-cli
created_at: 2024-01-31T11:44:02Z
updated_at: 2024-02-05T19:34:03Z
url: https://github.com/astral-sh/ruff/pull/9733
synced_at: 2026-01-12T15:55:30Z
```

# Add `--range` option to `ruff format`

---

_@MichaReiser_

## Summary

~~This PR adds the `--range-start=<CHAR OFFSET>` and `--range-end=<CHAR_OFFSET>` options to the `format` command.~~

This PR adds the new `--range=<start>-<end>` option to the `format` command where `<start>` and `<end>` are specified as `line:column` (1 based).

The new options allow users only to format a selected range rather than the entire document. The main use case is to enable range formatting in IDEs. 

Closes #7233 

## Design Decisions 

* Range formatting is only supported when formatting a single file, which I expect to be the main use case.
* The CLI only supports a single range. We can explore supporting multiple ranges in the future. The main challenge is that overlapping ranges invalidate the offset of whichever range gets formatted last (starting from the back helps but doesn't prevent it). This is especially a problem if the formatter has to extend the formatted range. 
* ~~The range is specified in character offsets. The alternatives I considered are:~~
  * ~~line numbers similar to Black but being able to specify the range exactly can help the formatter to narrow the range better~~
  * ~~`line:column` This would be more consistent to our `--output-format=json` where we output row and column numbers and can be easier to determine. However, this is mainly a feature for editors or when integrating Ruff into other tooling where computing a character offset shouldn't be a concern. The main downside of `line:column` is that it is a more complicated value~~ 
* ~~I went with two options instead of one to avoid the need for a custom syntax like `4-5` that user need to figure out~~
* See the discussion below for why a single `--range` option. TLDR: It gives us a way to define our own DSL to support byte and codepoint offsets in the future. 

## Limitations

The current implementation doesn't support notebooks because it's unclear if the range is relative to the notebook content or the raw notebook.
I decided to not support notebooks for now because the main use case, range formatting in VS Code, doesn't require notebook support because it only formats the closest cell.

## Test Plan

* Added CLI tests
* I used the debug build to develop and test the LSP range formatting functionality


---

_Review requested from @charliermarsh by @MichaReiser on 2024-01-31 11:45_

---

_Review requested from @zanieb by @MichaReiser on 2024-01-31 11:45_

---

_Label `formatter` added by @MichaReiser on 2024-01-31 11:45_

---

_Comment by @github-actions[bot] on 2024-01-31 12:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 2 project errors)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff format --no-preview --exclude tests/roots/test-pycode/cp_1251_coded.py</pre>
</p>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
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
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>




---

_Comment by @charliermarsh on 2024-01-31 15:54_

Was "byte offset" an option, or is the LSP not able to provide that?

---

_Comment by @MichaReiser on 2024-01-31 16:59_

> Was "byte offset" an option, or is the LSP not able to provide that?

I considered it but decided against it because I want to provide users with a safe interface without thinking about encodings. Byte offsets would mean that we, in addition to out of bound indices, would also need to error on inputs that fall between character boundaries. 

---

_Comment by @BurntSushi on 2024-01-31 17:19_

I would still bias toward byte offsets I think. Or perhaps even better, provide a way for a user to enter either byte offsets or character offsets. Worrying about encoding is a good point, but I imagine the vast majority of all Python source files are UTF-8. (Is it possible for Python source files to be something other than UTF-8? I'm actually not sure.)

Separately from that, could we use the word "codepoint" instead of "character" here? The former has a more concrete and unambiguous definition. The downside I suppose is that "character" is probably a more accessible term. Still, using the word "codepoint" will be an extra clear sign-post that the input is _not_ byte offsets.

---

_Comment by @MichaReiser on 2024-01-31 17:35_

@BurntSushi what's your reasoning for biasing towards byte offsets? The only reason we use byte offsets internally is because they're convenient (and fast) to slice strings.

I don't see performance being a key motivator for this use case. Lexing, parsing, and all the IO are so dominant that the offset conversion won't matter.

Allowing different formats is intriguing but I think I would than either allow row:col or code point offsets. 

---

_Comment by @BurntSushi on 2024-01-31 17:59_

Ah no, it's definitely not about performance. I suppose it's more about what is easy for users to actually get. Users are unlikely to be counting characters or bytes to figure out the inputs to these flags. So they'll probably get them from elsewhere. And I feel like usually what you get are byte offsets, especially when dealing with files on Unix operating systems. For example, grep has a `-b/--byte-offset` flag but no corresponding character offset flag. Byte offsets are just in general easier to deal with because they don't require knowledge of encodings when you treat everything as conventionally UTF-8. (Which is the prevailing strategy on Unix. Files are just bytes and most happen to be UTF-8.)

This is also why I suggested offering multiple ways to provide the range. Even if you switched to byte offsets, in the case of a user with char offsets, converting to byte offsets will be pretty annoying. Similarly, if you have byte offsets but need to provide char offsets, that's annoying too. For the same reason, I'd also advocate supporting line/column inputs too (grep provides them as well).

I'm not sure what the typical use case for these flags are though, and I'm sure that would have an impact on what we accept.

---

_Comment by @MichaReiser on 2024-01-31 18:23_

@BurntSushi the tooling support is an interesting consideration, although I don't really know what the use cases are for using range formatting over the CLI other than from an editor integration. And even there, I would advocate using the LSP instead that supports UTF32, UTF16, and UTF8 offsets. 

For today, I don't think I want to support multiple encodings because we aren't aware of any use case. However, it would be nice if the design supported different encodings: 

* One option is to use different syntaxes, e.g. `row:col` and `codepoint`. However, this doesn't really support UTF8 vs codepoint. We could extend it further with `cell:row:col` for notebook support
* separate --range-encoding option that can either be bytes, codepoints where `row:col` would be changed to `row:bytes` when used with `bytes`. The benefit of this approach is that we don't need to ship it today. 

I could see us do both to allow the most flexibility but I think it's something we can defer until we know of actual use cases needing a different encoding (and they cant use the LSP). 

---

_Comment by @BurntSushi on 2024-01-31 18:26_

Yeah if you don't anticipate this being used by users directly and instead only with editor integrations, then I absolutely defer to you and whatever is most convenient in that context. It might be worth calling that out in the docs too.

---

_Comment by @zanieb on 2024-01-31 19:32_

If we don't expect users to call this directly we should hide it from the CLI help menus

---

_Comment by @MichaReiser on 2024-01-31 21:10_

> If we don't expect users to call this directly we should hide it from the CLI help menus

I'm not convinced that hiding options solves the problem. It is a public API as soon as we add it, even if undocumented. That's why I prefer documenting the behaviour even if I would prefer not having to expose it at all.

@BurntSushi it's not that I'm not anticipating other use cases. It's just that I want to focus on the use case at hand. What's important to me is that the design allows us to support other potential use cases in the future, without having to redesign all options. That's why your feedback is very valuable and we should explore alternative options more if you aren't convinced that the one that I outlined are sufficient.

---

_Review comment by @charliermarsh on `crates/ruff/src/args.rs`:452 on 2024-01-31 21:43_

What if we used `range_start_char` and `range_end_char`? Would that help future-proof the API?

---

_Review comment by @charliermarsh on `crates/ruff/src/commands/format.rs`:270 on 2024-01-31 21:45_

I might consider `let cache = cache.filter(|_| range.is_none())` to make sure I didn't miss any usages of `cache` below.

---

_@charliermarsh approved on 2024-01-31 21:45_

---

_@BurntSushi reviewed on 2024-01-31 22:08_

---

_Review comment by @BurntSushi on `crates/ruff/src/args.rs`:452 on 2024-01-31 22:08_

What about `range_start_codepoint` instead? (Since I think "character" is ambiguous here.) It's a little long, but it's probably better to start longer and come up with shorter names later if new use cases arise.

---

_Comment by @BurntSushi on 2024-01-31 22:15_

> > If we don't expect users to call this directly we should hide it from the CLI help menus
> 
> I'm not convinced that hiding options solves the problem. It is a public API as soon as we add it, even if undocumented. That's why I prefer documenting the behaviour even if I would prefer not having to expose it at all.

Yeah I'd rather it be in `--help` too. Otherwise if someone finds themselves debugging editor integrations (or whatever) and don't see the flags in `--help`, I could imagine that being pretty disorienting.

> @BurntSushi it's not that I'm not anticipating other use cases. It's just that I want to focus on the use case at hand. What's important to me is that the design allows us to support other potential use cases in the future, without having to redesign all options. That's why your feedback is very valuable and we should explore alternative options more if you aren't convinced that the one that I outlined are sufficient.

Probably "sufficient" isn't the right word. It's not that codepoint offsets won't work. They will. It's a sound approach. I'm mostly just making the argument that, in my experience, byte offsets tend to be easier to come by.

If we're just looking for a path forward here that doesn't requiring potentially redesigning everything, then @charliermarsh's idea seems okay. To be clear, I agree with you that these flags will probably only really be used by editor integrations and not by end users directly. So in that sense, being more flexible in what we accept is maybe not so important.

If you asked me what my ideal design was _and_ we had a good reason to believe these flags might be used outside of editor integrations, then I think I'd add one flag called `--range` with its own little DSL for specifying ranges. e.g., `--range 'codepoint(start-end)'` or `--range 'bytes(start-end)'` or `--range 'lines(start-end)'` and do things that way. But that's a fair bit of work. And if we start with `--range-start-codepoint/--range-end-codepoint`, then we can always add a hypothetical `--range` flag later if there's user demand.

---

_Comment by @dhruvmanila on 2024-02-01 05:43_

For reference,

- `black` supports line ranges (`--line-ranges`)
- `stylua` supports byte offsets (`--range-start`, `--range-end`)
- `prettier` supports character offsets (`--range-start`, `--range-end`)
- `clang-format` supports both line ranges and byte offsets (`--offset` with `--length`, `--lines`)

---

_Comment by @MichaReiser on 2024-02-01 13:48_

I'll reply in more depth but one thing to consider is that powershell's `IndexOf` method make it easy to find the index of a substring. Getting the UTF8 byte offset is more involved (also true in Python where you need to encode the string in UTF8 first). 

---

_Comment by @MichaReiser on 2024-02-02 09:31_

I'm leaning toward changing the input to `line:column` because:

* It's the most convenient for users: Open your editor and you can see the numbers right there
* It's what we get in the LSP 
* it allows us to later add our own DSL for providing codepoint or byte offsets. 
* Using byte offsets over code point offsets favors UNIX users because getting the byte offset seems easier on UNIX but is more involved on Windows. Using code points is true the other way

The downside of this is that it may require users to convert from a byte offset to line/column number if they want to use this feature in an automated way. I think I'm fine with this as a compromise for now because some tools provide line/column number output and converting a byte offset to a line number can be done using `wc` or `String-Content` (the range would be slightly larger but than providing exact ranges but that's probably neglectable).

The only remaining question is if it should be single or multiple arguments. I think I'll go with a single argument because supporting a custom DSL where you specify the range type is more awkward with multiple options.

---

_Renamed from "Add `--range-start` and `--range-end` options to `ruff format`" to "Add `--range` option to `ruff format`" by @MichaReiser on 2024-02-02 16:48_

---

_@MichaReiser reviewed on 2024-02-02 16:51_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:452 on 2024-02-02 16:51_

Resolved, because it's now a single option

---

_@MichaReiser reviewed on 2024-02-02 16:52_

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:270 on 2024-02-02 16:52_

Excellent point

---

_Review requested from @BurntSushi by @MichaReiser on 2024-02-02 16:55_

---

_Review comment by @BurntSushi on `crates/ruff/tests/format.rs`:1812 on 2024-02-02 16:59_

I'd suggest adding a test with some Python source that contains non-ASCII Unicode codepoints here.

---

_Review comment by @BurntSushi on `docs/configuration.md`:657 on 2024-02-02 17:02_

Hmmm should this say more about what `<RANGE>` is? (I know this is the auto-generated docs. It looks like it's generated from a Rust doc comment? And that comment appears to have more details...)

---

_Review comment by @BurntSushi on `crates/ruff/src/args.rs`:452 on 2024-02-02 17:05_

Is column a byte offset, codepoint offset or a grapheme cluster offset? :-)

(I think the "most correct" thing is a grapheme cluster offset... But this is another thorny question about what inputs will be produced. And this could likely come up, e.g., with emoji in strings. If an emoji has multiple codepoints in it and column numbers are counted by codepoint by Ruff but by grapheme cluster in the editor, that could lead to a mismatch.)

---

_@BurntSushi reviewed on 2024-02-02 17:06_

---

_@MichaReiser reviewed on 2024-02-02 17:18_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:452 on 2024-02-02 17:18_

I'll extend the documentation but I'll go with codepoint because that's what we get from VS Code. 

---

_Review comment by @MichaReiser on `docs/configuration.md`:657 on 2024-02-02 17:19_

I suspect it only takes the first paragraph?

---

_Review comment by @MichaReiser on `docs/configuration.md`:657 on 2024-02-02 17:46_

Okay, I have no idea what it does. It also removes all terminating `.`.. ugh

---

_@MichaReiser reviewed on 2024-02-02 17:46_

---

_@MichaReiser reviewed on 2024-02-04 15:56_

---

_Review comment by @MichaReiser on `docs/configuration.md`:657 on 2024-02-04 15:56_

The script takes the short help only. I reworded the documentation so that the short help already provides some details on `<RANGE>`

---

_Comment by @codspeed-hq[bot] on 2024-02-04 16:20_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/range-formatting-cli)

### Merging #9733 will **improve performances by 47.49%**

<sub>Comparing <code>range-formatting-cli</code> (ccc71db) with <code>main</code> (4f7fb56)</sub>



### Summary

`⚡ 10` improvements
`✅ 20` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `range-formatting-cli` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 24 ms | 20.7 ms | +15.63% |
| ⚡ | `linter/all-rules[pydantic/types.py]` | 49.6 ms | 44 ms | +12.7% |
| ⚡ | `linter/all-with-preview-rules[numpy/ctypeslib.py]` | 34.6 ms | 23.4 ms | +47.49% |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 3.3 ms | 3.1 ms | +7.83% |
| ⚡ | `linter/all-rules[large/dataset.py]` | 108.2 ms | 93.4 ms | +15.81% |
| ⚡ | `linter/all-with-preview-rules[unicode/pypinyin.py]` | 13.3 ms | 11.8 ms | +11.91% |
| ⚡ | `linter/all-rules[unicode/pypinyin.py]` | 12.5 ms | 10.8 ms | +14.94% |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 3 ms | 2.8 ms | +7.68% |
| ⚡ | `linter/all-with-preview-rules[large/dataset.py]` | 121.6 ms | 105.1 ms | +15.71% |
| ⚡ | `linter/all-with-preview-rules[pydantic/types.py]` | 63.8 ms | 51.4 ms | +24.29% |


---

_@T-256 reviewed on 2024-02-04 16:49_

---

_Review comment by @T-256 on `crates/ruff/src/args.rs`:452 on 2024-02-04 16:49_

```suggestion
    /// The `<RANGE>` uses the format `<start_line>:<start_column>-<end_line>:<end_column>`.
```

---

_@MichaReiser reviewed on 2024-02-04 17:12_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:452 on 2024-02-04 17:12_

Thx

---

_Comment by @charliermarsh on 2024-02-04 17:41_

What’s the scoop with the benchmarks?

---

_Comment by @MichaReiser on 2024-02-05 02:40_

> What’s the scoop with the benchmarks?

It's most likely that I need to rebase my changes

---

_Merged by @MichaReiser on 2024-02-05 19:21_

---

_Closed by @MichaReiser on 2024-02-05 19:21_

---

_Branch deleted on 2024-02-05 19:21_

---
