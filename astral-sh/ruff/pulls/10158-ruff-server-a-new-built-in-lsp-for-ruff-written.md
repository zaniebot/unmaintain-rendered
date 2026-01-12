```yaml
number: 10158
title: "`ruff server` - A new built-in LSP for Ruff, written in Rust"
type: pull_request
state: merged
author: snowsignal
labels:
  - preview
assignees: []
merged: true
base: main
head: jane/lsp/server-mvp
created_at: 2024-02-29T01:29:22Z
updated_at: 2024-03-09T05:00:24Z
url: https://github.com/astral-sh/ruff/pull/10158
synced_at: 2026-01-12T15:55:31Z
```

# `ruff server` - A new built-in LSP for Ruff, written in Rust

---

_@snowsignal_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR introduces the `ruff_server` crate and a new `ruff server` command. `ruff_server` is a re-implementation of [`ruff-lsp`](https://github.com/astral-sh/ruff-lsp), written entirely in Rust. It brings significant performance improvements, much tighter integration with Ruff, a foundation for supporting entirely new language server features, and more!

This PR is an early version of `ruff_lsp` that we're calling the **pre-release** version. Anyone is more than welcome to use it and submit bug reports for any issues they encounter - we'll have some documentation on how to set it up with a few common editors, and we'll also provide a pre-release VSCode extension for those interested.

This pre-release version supports:
- **Diagnostics for `.py` files**
- **Quick fixes**
- **Full-file formatting**
- **Range formatting**
- **Multiple workspace folders**
- ~~**Automatic linter/formatter configuration** - taken from any `pyproject.toml` files in the workspace.~~ (this will be implemented in a follow-up pull request)

Many thanks to @MichaReiser for his [proof-of-concept work](https://github.com/astral-sh/ruff/pull/7262), which was important groundwork for making this PR possible.

## Architectural Decisions

I've made an executive choice to go with `lsp-server` as a base framework for the LSP, in favor of `tower-lsp`. There were several reasons for this:

1. I would like to avoid `async` in our implementation. LSPs are mostly computationally bound rather than I/O bound, and `async` adds a lot of complexity to the API, while also making harder to reason about execution order. This leads into the second reason, which is...
2. Any handlers that mutate state should be blocking and run in the event loop, and the state should be lock-free. This is the approach that `rust-analyzer` uses (also with the `lsp-server`/`lsp-types` crates as a framework), and it gives us assurances about data mutation and execution order. `tower-lsp` doesn't support this, which has caused some [issues](https://github.com/ebkalderon/tower-lsp/issues/284) around data races and out-of-order handler execution.
3. In general, I think it makes sense to have tight control over scheduling and the specifics of our implementation, in exchange for a slightly higher up-front cost of writing it ourselves. We'll be able to fine-tune it to our needs and support future LSP features without depending on an upstream maintainer.

## Test Plan

The pre-release of `ruff_server` will have snapshot tests for common document editing scenarios. An expanded test suite is on the roadmap for future version of `ruff_server`.

---

_Marked ready for review by @snowsignal on 2024-03-01 05:17_

---

_Review requested from @MichaReiser by @snowsignal on 2024-03-01 05:17_

---

_Review requested from @charliermarsh by @snowsignal on 2024-03-01 05:23_

---

_Comment by @MichaReiser on 2024-03-01 17:13_

@snowsignal How would you recommend to review this PR? Which parts of the PR are in its final state?  Are there parts that you plan to iterate on in future PRs? 

---

_Review comment by @MichaReiser on `Cargo.toml`:72 on 2024-03-01 17:20_

I was able to remove the `phf` and `libc` dependency from the `ruff_server` crate without getting any compile errors. Can you double check if they're used?

---

_Review comment by @MichaReiser on `Cargo.toml`:59 on 2024-03-01 17:20_

Ohoh, now we go all lowlevel. 

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:500 on 2024-03-01 18:00_

I don't have a strong opinion on the name. One advantage of using `lsp` over `server` is that we could add `ruff server start` and `ruff server stop`  commands in the future if we support a daemon. The LSP spec also has some recommendations about what arguments should be supported that we might not want to have on a generic `server` command. However, we could probably also do this by using `ruff server` with no sub-command to connect to the LSP directly`. 

---

_Review comment by @MichaReiser on `crates/ruff_server/src/edit/document.rs`:25 on 2024-03-01 18:02_

I'm okay with this. The only use case that exists today is to replace the content. So maybe rename the method to `replace` in which case re-computing the `LineIndex` from scratch seems like a good default.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/edit/document.rs`:45 on 2024-03-01 18:03_

I prefer not to group methods but I don't others doing it. What I don't understand is why `modify` isn't part of the `Mutable` api.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/edit/document.rs`:65 on 2024-03-01 18:08_

Can we initialize the line index with `self.line_index`? The index only needs updating when `new_contents` was modified.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/edit/document.rs`:81 on 2024-03-01 18:09_

I believe this unnecessarily computes the line index at least once even when `new_contents` still match `self.content`. 

I think we can simply re-compute the `LineIndex` after every change because we need the new `LineIndex`  when calling `modify_with_manual_index`

---

_Review comment by @MichaReiser on `crates/ruff_server/src/edit/document.rs`:117 on 2024-03-01 18:10_

Nit: Should we create a newtype wrapper for `Version`. It would give the `i32` more meaning (especially if we start storing it in other places)

---

_Review comment by @MichaReiser on `crates/ruff_server/src/edit/range.rs`:8 on 2024-03-01 18:11_

Nit: We could introduce two new extension traits and expose this functionality as methods instead:

```rust
pub(crate) trait RangeExt {
	fn to_text_range(&self, text: &str, index: &LineIndex, encoding: PositionEncoding) -> TextRange{}
}

pub(crate) trait TextRangeExt {
	fn to_range(&self, text: &str, index; &LineIndex, encoding: PositionEncoding) -> types::Range {...}
}
```

It would slightly reduce the number of arguments that need to be passed. 

Another alternative is that we create our own `EditorRange` newtype wrapper around `types::Range` so that we can implement additional methods on it

```rust
#[derive(Copy, Clone, Debug, Eq, PartialEq)]
pub(crate) struct EditorRange(types::Range);

impl EditorRange {
	pub(crate) fn from_text_range(range: TextRange, text: &str, index: &LineIndex, encoding: PositionEncoding) -> Self {
	...
  }

  pub(crate) fn to_text_range(text: &str, index: &LineIndex, encoding: PositionEncoding) -> TextRange {
		...
  }
}
```


---

_Review comment by @MichaReiser on `crates/ruff_server/src/edit/range.rs`:1 on 2024-03-01 18:11_

@BurntSushi would you mind taking a look at these methods as our unicode expert.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/format.rs`:19 on 2024-03-01 18:22_

I would call the method `format_range` and instead use the full qualified name on line 28. I fear that it's otherwise confusing because people need to remember whether it was `format_range` or `range_format` and the answer depends on the crate you're in.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/lint.rs`:45 on 2024-03-01 18:23_

Coud this re-use the computed `LineIndex` from the document?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/lint.rs`:33 on 2024-03-01 18:24_

It would be nice if we could reuse more of the `ruff_linter` crate methods but maybe something to consider to do later.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/lib.rs`:25 on 2024-03-01 18:25_

Can we reuse the linter version here (that's what the CLI uses as well)
```suggestion
    ruff_linter::VERSION
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/lint.rs`:102 on 2024-03-01 18:26_

It would be nice if we wouldn't have to store the fixes in the diagnostics but changing that is probably something we should leave for later.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:103 on 2024-03-01 18:28_

Nit: I would appreciate longer names than `g` and `e`. It improves readability when you don't have type information available, e.g. on GitHub. 

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:103 on 2024-03-01 18:29_

Should we consider more than just the first encoding?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/schedule/thread/priority.rs`:1 on 2024-03-01 18:32_

@BurntSushi I would appreciate it if you could take a look at the scheduling code here. Okay, I'm making assumptions here. I assume you know a good deal about thread scheduling. If not, don't feel required to review.

---

_@MichaReiser reviewed on 2024-03-01 18:33_

This looks very exciting. I haven't managed to get through everything yet. I plan to continue my review on Monday. 

I would appreciate a more in depth explanation of the architecture and why you decided to go with `lsp-server` over `tower-lsp`. Documenting this decision will be useful for the future.

---

_Label `preview` added by @MichaReiser on 2024-03-01 18:34_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/notifications/did_change.rs`:25 on 2024-03-04 13:21_

Do we need to update the document's version when the content is empty to ensure it stays in sync with the IDE?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/notifications/did_change.rs`:21 on 2024-03-04 13:23_

I don't fully understand why using a macro is necessary and it isn't obvious what's happening here (or where `document_url` is coming from). I assume it is because all params have a different shape. How about we define a trait `DocumentUrl` with a single `document_url` method (I don't like the trait name, maybe `Document`?) that we could then pass to the `document_controller`

Or we remove the macro because the implementation is trivial? 

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/notifications/did_open.rs`:28 on 2024-03-04 13:37_

What's the reason that this notification isn't using `        super::define_document_url!(params: &types::DidCloseTextDocumentParams);`?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:30 on 2024-03-04 13:43_

Nit: Can we use `context` here and make it explicit that the serialized fix data is incorrect? I fear that it's otherwise very painful to track down where the json serialization fails. 

Which brings up. Should we use more explicit errors (thiserror?) in combiantion with `anyhow`?



---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/diagnostic.rs`:9 on 2024-03-04 13:45_

Nit: Maybe rename to `DocumentDiagnostic`. `Diagnostic` is a very generic term

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/diagnostic.rs`:23 on 2024-03-04 13:45_

Nit: It seems very common that we need to pass `Document` and `Encoding`. Should we store the encoding on the `Document` or have a type that wrapps `Document` and `encoding`

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/format.rs`:25 on 2024-03-04 13:46_


```suggestion
        let doc_size = snapshot.document().contents().text_len();
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/format.rs`:26 on 2024-03-04 13:48_

Yeah, I think it would be nice to run a small diff. The existing LSP does a very "trivial" diff where it truncates identical lines from the start and end. We should add that at least. Using a proper "diffing" algorithm might help to create smaller edits but has the downside that diffing is exponential. That's why I think the "simple" diffing is enough for now and we can leave it to editors to narrow the edits.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/format_range.rs`:26 on 2024-03-04 13:49_

Would it make sense to use `context` or some other information that lets us know from which request the error is coming from or do we attach this somewhere further up?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests.rs`:15 on 2024-03-04 13:50_

What's the reasoning of making this a `Option<Vec<_>>` over a `Vec<_>` that is either empty (no changes) or contains edits?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/traits.rs`:10 on 2024-03-04 13:52_

Is it possible to send other responses to the server? For example, can an implementation send a notification to the client while it is in the middle of the operation? 

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/traits.rs`:18 on 2024-03-04 13:52_


```suggestion
/// executing. Avoid doing any I/O or long-running computations.
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/traits.rs`:64 on 2024-03-04 13:54_

Nit: I recommend moving this comment to `document_url`. Although, I would prefer to avoid the macro and instead implement the very simple signature manually. 

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/traits.rs`:32 on 2024-03-04 13:56_

The fact that `document_url` is mandatory limits `BackgroundRequest` to requests that have a document. For exmaple, it's not possible to use it for requesting all workflow symbols. 


It seems the `document_url` is used for getting the snapshot. Could we change the signature so that we pass in the entire Global snapshot state and the `snapshot_lense` (or just `snapshot`) slices the content that it needs from the snapshot?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/traits.rs`:90 on 2024-03-04 14:00_

Nit: Could we use `&'static str`?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/traits.rs`:107 on 2024-03-04 14:02_

Should we rename the method to `complete` or similar to decouple it from the request/response concept (that, seemingly, doesn't apply to all `Request`s)

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/traits.rs`:69 on 2024-03-04 14:03_

Same as for `BackgroundTask`: Can we decouple the signature from documents to also support workspace requests?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api.rs`:51 on 2024-03-04 14:08_

This reads very nicely and it's impressive that you can write all these macros! 
Unfortunately, it breaks RustRover completely. It fails to find the `BackgroundRequest::document_url` usage and `unwrap_or_else` is marked as a syntax error. It also took me a while to understand what's going on in the macros and I'm not sure if I fully undertand yet. 

Do you see a way to reduce the need for macros or can we pull out more code out of the macros so that the macros become very thin wrappers?

There's also quiet a lot of complexity in the macro implementations that I find very difficult to read. That's why I think it would be worthwile exploring non-macro designs (I'm okay deriving some repetitive fields with macros but implementing scheduling in macro's sounds scary)

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/client.rs`:18 on 2024-03-04 14:14_

This is the first time that I see `pub(in ..)` 

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/schedule.rs`:25 on 2024-03-04 14:20_

Can you document the reason why we need the larger stack size or remove it until we have a need for it?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/schedule.rs`:21 on 2024-03-04 14:21_

Should we use a different name for it, e.g. `primary`, `mutating`, or whatever its main purpose is?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/types.rs`:3 on 2024-03-04 14:23_

Nit: Rename to `LspSettings` or similar? We already have multiple `Settings` struct in ruff and I often import or navigate to the wrong type.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:27 on 2024-03-04 14:24_

Nit: 
```suggestion
    watch_receiver: Receiver<notify::Result<notify::Event>>,
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:22 on 2024-03-04 14:25_

Can we document the fields in `Session`. They seem important (e.g I don't know what `watch_recv` is)

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:37 on 2024-03-04 14:26_

Should this be `DocumentSnapshot`, considering that it only references a single document. How do you imagine this to grow once we have workspaces and might need snapshoting data other than documents?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:40 on 2024-03-04 14:27_

Nit: Consider renaming to `LspConfiguration` or similar. There's already a handful of `Configuration` structs in ruff and them having the same name can be confusing. 

Also: Should this be `Settings` considering that it stores `Settings` struct?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:77 on 2024-03-04 14:29_

What's the reason that we set-up our own watchers. The LSP specification supports file watching (e.g VS code watches your project for changes and notifices the server)?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:61 on 2024-03-04 14:31_

Nit: I find `Controller` a very generic word that isn't telling me much about what it does (it controls a document somehow?). 

Can we think of a more semantical name of what it represents? How is it different from `Document` and `DocumentRef`? Maybe `OpenDocument`?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:172 on 2024-03-04 14:34_

Nit: `document_snapshot` to make it consistent with `open_document` and to avoid abbreviation. Or you remove the `document` from all methods which I think would read nice:

* `documents.snapshot(my_doc)`
* `documents.controller(my_doc)`
* `documents.open(url, content, version)`
* `documents.close(url, content, version)`

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:261 on 2024-03-04 14:35_

Ruff also support `.ruff.toml` https://docs.astral.sh/ruff/configuration/

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:265 on 2024-03-04 14:35_

Nit: I've most commonly seen `mut` as postfix and not prefix: `self.entry_for_path_mut(path)`

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:321 on 2024-03-04 14:37_

Nit: I would find it helpful if we avoid single letter variables because it is very difficult to tell what the types are when doing a code review. E.g. I have no idea what `u` is. `Url`? 

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:326 on 2024-03-04 14:37_


```suggestion
    fn workspace_for_url_mut(&mut self, url: &Url) -> Option<&mut Workspace> {
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:331 on 2024-03-04 14:43_

Isn't it guaranteed that the longest-match is always the last key given that the keys are stored in sorted order? 

Can we use the `range_mut` api (same for other calls)

```
self.0.range_mut(..path).next_back().map(|(_, w)| w)
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/schedule/thread/pool.rs`:47 on 2024-03-04 14:45_

What's the reason for the increased stack size in worker threads?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/schedule/thread/pool.rs`:53 on 2024-03-04 14:51_

How comfortable do you feel about the threading code and that you understand it well (also applies to priority.rs)

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/schedule/task.rs`:1 on 2024-03-04 14:53_

Does this file (all files in `schedule`) need a license header?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api.rs`:28 on 2024-03-04 15:14_

Nit: Coudl we include the `id` in the returned error?

---

_@MichaReiser reviewed on 2024-03-04 15:20_

As said before. This is excellent work. I haven't reviewed everything in detail, the PR is too large for that. 

I think this work deserves a more in depth explanation of the architecture decisions. I like the simplicity of the handlers and that mutating tasks run on a dedicated thread. However, it comes at the cost that cancellation, the entire scheduling, dispatching, are problems that we now need to solve. That's why I'm interested on hearing your perspective on how to balance these tradeoffs and the complexity of implementing said features (I'm especially interested in cancellation). 

I would find some additional comments useful that explain some concepts, especially around scheduling, how requests/notifications work etc. I can see that you have put a lot of thought into it, let's make sure that other contributors are aware of your deliberate design choices. 

It might be nice if some of the scheduling work could be tested too. Although I fear that this isn't going to be easy.

---

_Review comment by @BurntSushi on `crates/ruff_server/src/edit.rs`:19 on 2024-03-04 17:57_

What does "second choice" mean in this context?

---

_Review comment by @BurntSushi on `crates/ruff_server/src/edit/document.rs`:25 on 2024-03-04 17:59_

Strong agree. Default to encapsulation.

---

_Review comment by @BurntSushi on `crates/ruff_server/src/edit/document.rs`:12 on 2024-03-04 18:01_

Some comments on what these fields are (especially `version`) and any relevant invariants would be helpful.

Also, if the _caller_ needs to care about `version` (and it looks like they might given that they provide it to `Document`'s constructor), then perhaps some public docs on `Document` would be useful.

---

_Review comment by @BurntSushi on `crates/ruff_server/src/edit/document.rs`:45 on 2024-03-04 18:04_

I'm a big fan of grouping methods, but typically in a way that serves a useful documentation/presentation need (beyond necessary groupings due to generics). [For example.](https://docs.rs/regex/latest/regex/struct.Regex.html#impl-Regex-1) I usually wouldn't separate methods by immutable/mutable though, but I don't know enough yet about this type to suggest an alternative.

---

_Review comment by @BurntSushi on `crates/ruff_server/src/edit/range.rs`:15 on 2024-03-04 18:49_

I personally like to avoid `as` casts these days. Since `range.start.line` is a `u32` and I don't think we have any plans to support 16-bit platforms, it should be correct to do `usize::try_from(range.start.line).expect("u32 fits in usize")` here. (Perhaps an extension trait on `lsp_types::Position` to add this method if it's used a lot would be appropriate.)

---

_Review comment by @BurntSushi on `crates/ruff_server/src/edit/range.rs`:77 on 2024-03-04 18:52_

I would just do `u32::try_from(c.len_utf8()).expect("utf8 len always <=4")`. Then you could remove the `SAFETY` comment as it's encoded in the code and `expect` message. And you should be able to get rid of the Clippy `allow`.

---

_Review comment by @BurntSushi on `crates/ruff_server/src/edit/range.rs`:66 on 2024-03-04 18:53_

Can you document the contract of this function? It is doing interesting things!

---

_Review comment by @BurntSushi on `crates/ruff_server/src/edit/range.rs`:67 on 2024-03-04 18:54_

I _think_ this would be more precisely named `utf8_code_unit_offset` or even just `byte_offset`, right?

---

_Review comment by @BurntSushi on `crates/ruff_server/src/edit/range.rs`:66 on 2024-03-04 18:55_

Also, I think "character" is "utf16 code unit offset" right?

---

_Review comment by @BurntSushi on `crates/ruff_server/src/edit/range.rs`:153 on 2024-03-04 18:56_

I think it would be good to have some focused tests for the functions in this module. There are lots of interesting boundary conditions here. Property based tests (`quickcheck` or `proptest`) might work well.

---

_Review comment by @BurntSushi on `crates/ruff_server/src/lib.rs`:7 on 2024-03-04 18:57_

I'd personally drop these sorts of comments. They're pretty redundant with the source itself.

---

_Review comment by @BurntSushi on `crates/ruff_server/src/server.rs`:48 on 2024-03-04 19:00_

This is a very long line. Can you break it up? You can backslash escape newlines to break it up a bit: https://doc.rust-lang.org/rust-by-example/std/str.html#literals-and-escapes

---

_Review comment by @BurntSushi on `crates/ruff_server/src/server/api.rs`:51 on 2024-03-04 19:11_

I somewhat agree. I spent a couple minutes trying to grok them but couldn't manage. I think exploring non-macro designs or a lighter-weight and more transparent macro design might be useful. In particular, it looks like the `select_task!` macro is only being used in a couple places right now, so I'm not sure how much it is buying us. But maybe is intended to be used a lot more in the future.

If a macro design is truly necessary (or rather, is very compelling given the set of trade offs), a mitigation to it is extra docs with lots of examples.

---

_Review comment by @BurntSushi on `crates/ruff_server/src/server/api/traits.rs`:18 on 2024-03-04 19:13_

Can you add more to these docs explaining why?

---

_Review comment by @BurntSushi on `crates/ruff_server/src/server/schedule/thread/pool.rs`:48 on 2024-03-04 19:19_

Do we need unbounded channels here? I'd rather we use bounded channels so that we can benefit from backpressure. (And if we need unbounded channels, we should write some comments explaining why.)

---

_Review comment by @BurntSushi on `crates/ruff_server/src/server/schedule/thread/priority.rs`:1 on 2024-03-04 19:20_

I don't have much special insight here. My prior is that rust-analyzer likely has some institutional knowledge that is encoded here that we'll probably benefit from.

---

_Review comment by @BurntSushi on `crates/ruff_server/src/server/schedule/thread/priority.rs`:1 on 2024-03-04 19:22_

@snowsignal How certain are you that we need (or greatly benefit from) all of this?

---

_@BurntSushi reviewed on 2024-03-04 19:27_

Wow! There is so much here! Really nice work. :-)

Other than the comments I left, I have some holistic feedback too:

1. It feels like there is a lot of interesting documentation that could be written for a lot of your types that would help clarify some of their behavior. I found that the comments that did exist were more on the "inside baseball" side of the spectrum (e.g., the comments about not implementing a trait method or the comments about why a type isn't just a closure) rather than "here is the contract the caller needs to care about." I like the former. They belong. They help contextualize why code is the way it is. But I'd really like to see more of the latter.
2. I found it somewhat difficult to review this PR without a "big picture" of how the code is structure. These are tricky docs to write, I'd suggest the first step in doing so is to pick a target audience. For example, for me, I know very little about LSP, but perhaps the architecture docs should assume that knowledge. But maybe not. I'm not sure.

Overall amazing work. I can't believe how fast you got this built!

---

_Review comment by @snowsignal on `Cargo.toml`:72 on 2024-03-04 22:01_

Thanks for catching `phf`, we can remove that! (it was for an optimization I later took out) We still need `libc` for the QoS wrapper on Apple platforms, though.

---

_Review comment by @snowsignal on `crates/ruff_server/src/edit.rs`:19 on 2024-03-04 22:07_

This was taken from [Micha's proof-of-concept PR](https://github.com/astral-sh/ruff/pull/7262). I believe it means that if we're given a choice between multiple encodings, this should be our second choice after `UTF8`. I haven't implemented logic to prioritize this yet, though.

---

_Review comment by @snowsignal on `crates/ruff_server/src/edit/document.rs`:45 on 2024-03-04 22:09_

I can probably get rid of the grouping here.

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:103 on 2024-03-04 22:12_

We do - I need to implement encoding prioritization here.

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/notifications/did_change.rs`:21 on 2024-03-04 22:20_

This macro was created as a way to avoid the repeated boilerplate of writing a function that returned `&params.text_document.uri` from an arbitrary parameter type in `lsp-types` (a parameter type is a deserialized message payload for requests and notifications). Since these types don't have a shared function to get the `Url`, even though they all follow the same internal layout, it needs to be a concrete function for each parameter type. 

The reason why we need to define this function for each background handler is because we need a generic way to extract the document URL from the parameters of a request/notification, in order to take a session snapshot. Here, though, it's just defined for convenience.

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/notifications/did_change.rs`:25 on 2024-03-04 22:20_

Good catch, we should update the document version here 👍 

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/notifications/did_open.rs`:28 on 2024-03-04 22:24_

We have to de-structure the parameter type anyway to access multiple fields, so defining a boilerplate function to get the document URL wasn't necessary. You're right that it's inconsistent with other handlers, though - maybe I'll just de-structure the parameter type for handlers that don't have `document_url` implemented as a required method.

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests.rs`:15 on 2024-03-04 22:25_

This is the required response type for format requests as defined in `lsp_types`.

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/diagnostic.rs`:23 on 2024-03-04 22:28_

The encoding is a global setting - I think it makes sense to keep it separate from the `Document` type.

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/traits.rs`:10 on 2024-03-04 22:32_

Yes, that's allowed - for example, some long-running requests can post notifications that report task progress. That's what the `notifier` is for, even though we aren't using it yet.

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/traits.rs`:18 on 2024-03-04 22:35_

I wrote "try to" because we already _do_ have some necessary I/O that gets run on the main thread - for example, when resolving configuration for workspace folders that got added.

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/client.rs`:18 on 2024-03-04 22:39_

Hehe, we can probably replace that with `pub(super)` anyway.

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/schedule.rs`:21 on 2024-03-04 22:41_

It's the thread that runs the main loop - would `main_loop_thread` work?

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/schedule/task.rs`:1 on 2024-03-04 22:42_

No, this one I wrote myself.

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/schedule/thread/pool.rs`:47 on 2024-03-04 22:46_

Mainly to override low OS defaults on platforms like Windows and avoid the likelihood of a stack overflow. That being said, we probably don't need a stack size this big - these defaults were taken from `rust-analyzer`.

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/schedule/thread/pool.rs`:53 on 2024-03-04 22:49_

I think I understand it reasonably well, though I could probably use some help with making changes (like the one @BurntSushi suggested about using bounded channels) just so I know I'm on the right track.

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/schedule/thread/pool.rs`:48 on 2024-03-04 22:50_

I can definitely look into using bounded channels here. Could you clarify what you mean by "benefiting from back-pressure?"

---

_Review comment by @snowsignal on `crates/ruff_server/src/session.rs`:61 on 2024-03-04 22:53_

You're right, this could probably be named better. Also these docs need updating because it doesn't have a revision counter anymore 🫠

---

_Review comment by @snowsignal on `crates/ruff_server/src/session.rs`:77 on 2024-03-04 22:56_

Oh, that makes things much simpler! I'll try switching to that.

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/schedule/thread/priority.rs`:1 on 2024-03-04 23:20_

If we're going to have a multi-threaded LSP (which I strongly believe we _should_), we'll need a way to manage thread pool(s), and for something as latency-critical as an LSP I think thread prioritization is a reasonable thing to have too. I'm not as committed to this specific implementation as I am to the general idea of having a scheduler with these capabilities.

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/traits.rs`:18 on 2024-03-04 23:22_

I actually might move the methods on this trait and `Notification` out into standalone functions as part of the work to remove `select_task!`

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api.rs`:51 on 2024-03-04 23:26_

I'll look into removing macros as much as I can from the codebase. Having one for task initialization might be handy, but `select_task!` can definitely be refactored out.

---

_@snowsignal reviewed on 2024-03-04 23:27_

---

_@BurntSushi reviewed on 2024-03-05 12:01_

---

_Review comment by @BurntSushi on `crates/ruff_server/src/edit.rs`:19 on 2024-03-05 12:01_

I think the thing that confused me here is that I understand `UTF8` to be the preferred encoding, but `UTF16` is marked as the default. Yet, the comment on UTF32 says it is the second choice. I think the answer here is maybe some holistic docs on this type explaining when each variant is used. (And I do also wonder whether a `Default` impl for this type is a good idea, but I don't have enough of the surrounding context internalized to have a strong opinion.)

---

_@BurntSushi reviewed on 2024-03-05 12:05_

---

_Review comment by @BurntSushi on `crates/ruff_server/src/server/schedule/thread/pool.rs`:48 on 2024-03-05 12:05_

Yes! I wrote a little bit about it (with links) in the context of `uv` a bit ago: https://github.com/astral-sh/uv/pull/1163#discussion_r1473155361

To be clear, I do not mean to say "never use unbound channels." I mean to say that if we do use them, IMO, we ought to have a specific justification for them. But otherwise default to bounded channels.

---

_@MichaReiser reviewed on 2024-03-05 12:12_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/edit.rs`:19 on 2024-03-05 12:12_

For some context: It's the default because UTF16 is the default according to the LSP specification that all servers must support.

---

_Review comment by @dhruvmanila on `Cargo.toml`:63 on 2024-03-05 17:12_

I would recommend to use the `lsprotocol` library (https://github.com/microsoft/lsprotocol/tree/main/packages/rust/lsprotocol) as it'll always be in sync with the latest protocol. The `ruff-lsp` package also uses the Python version of it.

---

_@dhruvmanila reviewed on 2024-03-05 17:12_

---

_Review comment by @snowsignal on `crates/ruff_server/src/lint.rs`:33 on 2024-03-07 15:20_

I'll explore this in a follow-up!

---

_Review comment by @snowsignal on `crates/ruff_server/src/lint.rs`:102 on 2024-03-07 15:21_

I'll make this a follow-up issue.

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/format_range.rs`:26 on 2024-03-07 15:22_

We attach this further up, inside the task closure.

---

_Review comment by @snowsignal on `Cargo.toml`:63 on 2024-03-07 17:00_

I think we'll have to make this a follow-up issue - this would be a pretty fundamental re-write.

---

_@snowsignal reviewed on 2024-03-07 17:07_

---

_@snowsignal reviewed on 2024-03-07 17:39_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/traits.rs`:32 on 2024-03-07 17:39_

> Could we change the signature so that we pass in the entire Global snapshot state and the snapshot_lense (or just snapshot) slices the content that it needs from the snapshot?

We could, but there isn't a need for that right now. For now, I changed the trait names to be more explicitly about capturing a single document.

---

_@snowsignal reviewed on 2024-03-07 17:54_

---

_Review comment by @snowsignal on `crates/ruff/src/args.rs`:500 on 2024-03-07 17:54_

If we end up supporting a language server _and_ a daemon, we should probably call the daemon something other than 'server', in my opinion.

Here's why I think `server` is a good name for this command:
* It fits our command naming scheme better, since none of our other commands use acronyms
* It's more immediately clear as to what it's doing, especially if someone isn't familiar with the term "LSP". 

---

_Review comment by @MichaReiser on `crates/ruff_server/src/edit/document.rs`:95 on 2024-03-08 09:30_

Can't we take `active_index` here, considering that we carefully updated it in each iteratin?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/edit/range.rs`:153 on 2024-03-08 09:31_

I agree on this @snowsignal do you plan to follow up on this in a separate PR?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/format.rs`:25 on 2024-03-08 09:35_

It might be worth to do a quick check if the code is identical and in that case, return early. 

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/format.rs`:39 on 2024-03-08 09:37_

Nit: I like to make constructor functions methods on the corresponding types: `Replacement::between(unformatted, unformatted_index.line_starts(), ...)`

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/format.rs`:50 on 2024-03-08 09:38_

To use the same terminology as in the `Format::run` function or align with the formatter internal terminology: 

* `unformatted_range` and `formatted_range`
* or `source_range`, `formatted_range`

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/format.rs`:85 on 2024-03-08 09:42_

I think this iterates the document twice if they are identical. We should avoid re-traversing `old_text_line_starts` for the lines we know are identical after having found `old_start` and `new_start.` 

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/format.rs`:142 on 2024-03-08 09:43_

Nit: Some whitespace could help readers to understand the setup / act / assertion flow
```suggestion
        let original = r#"
        aaaa
        bbbb
        cccc
        dddd
        eeee
        "#;
        let original_index = LineIndex::from_source_text(original);

        let new = r#"
        bb
        cccc
        dd
        "#;
        let new_index = LineIndex::from_source_text(new);

        let expected = r#"
        bb
        cccc
        dd
        "#;

        let replacement = find_replacement_range(
            original,
            original_index.line_starts(),
            new,
            new_index.line_starts(),
        );

        let mut test = original.to_string();
        test.replace_range(
            replacement.replace_range.start().to_usize()
                ..replacement.replace_range.end().to_usize(),
            &new[replacement.replacement_text_range],
        );
        
        assert_eq!(expected, &test);
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/notifications/did_change.rs`:21 on 2024-03-08 09:46_

That makes sense. IMO, the macro here seems overkill. The code it generates is minimal and very easy to write by hand. However, it adds significant complexity to readers because they have to open the macro definition or read its documentation to understand what's happening inside. 

That's why I recommend removing the macro and instead implementing the trait function by hand (I'm sure code pilot can autocomplete it for you)

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:177 on 2024-03-08 09:56_

Nit: I would prefer what you said during during the sync demonstration: To make this method explicit rather than in `deref_mut`. I would be surprised to learn that a `deref` function allocates internally, especially considering that the compiler inserts `deref_mut` calls manually

> DerefMut. The compiler will silently insert calls to DerefMut::deref_mut. For this reason, one should be careful about implementing DerefMut and only do so when mutable deref coercion is desirable. See the Deref docs for advice on when this is typically desirable or undesirable.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:153 on 2024-03-08 09:57_

Nit : Some spacing between the methods helps readability
```suggestion
    fn snapshot(&self, url: &Url) -> Option<DocumentRef> {
        Some(self.documents.get(url)?.make_ref())
    }

    fn controller(&mut self, url: &Url) -> Option<&mut DocumentController> {
        self.documents.get_mut(url)
    }

    fn open(&mut self, url: &Url, contents: String, version: DocumentVersion) {
        if self
            .documents
            .insert(url.clone(), DocumentController::new(contents, version))
            .is_some()
        {
            tracing::warn!("Opening document `{url}` that is already open!");
        }
    }
    
    fn close(&mut self, url: &Url) -> crate::Result<()> {
        let Some(_) = self.documents.remove(url) else {
            return Err(anyhow!(
                "Tried to close document `{url}`, which was not open"
            ));
        };
        Ok(())
    }
}
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/schedule.rs`:21 on 2024-03-08 09:57_

Nit: Have you considered renaming `main_loop` to `event_loop`?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/traits.rs`:7 on 2024-03-08 09:59_

Should we rename our `Request` trait to `RequestHandler` or similar to avoid conflicts with `lsp_types`? I'm concerned that it can be confusing when somone accidentally imports the "wrong" request type. Especially because both traits are needed to implement a `Request` trait

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:110 on 2024-03-08 10:01_

Nit: It might be worth to implement `TryFrom` for `&LspEncoding` to avoid the cloning here (I don't know how expensive cloning the LSP encoding is but I don't see a reason why we can't implement `TryFrom` for a reference instead)

---

_@MichaReiser requested changes on 2024-03-08 10:05_

The code looks great. Thanks for addressing all the feedback. 

What's still missing, in my view, is a discussion of the architectural decisions as part of the PR summary. We've covered some of them in our sync meeting but I think it is beneficial to write those down to have a reference for the future. That's the reason why I'm requesting feedback. Once that's done, feel free to merge.

As @BurntSushi pointed out, I think it would be good to have some more conceptual documentation. Maybe the `README` would be a good start. It doesn't have to be extensive but a brief explanation of the core concept involved.

---

_@MichaReiser approved on 2024-03-08 17:08_

---

_Review comment by @snowsignal on `crates/ruff_server/src/edit/range.rs`:153 on 2024-03-09 01:04_

Yep, this will be in a follow-up.

---

_Comment by @github-actions[bot] on 2024-03-09 04:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+13 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/4692686a750b95884e1d5a4f1313614c71c57213/pandas/io/parsers/python_parser.py#L1039'>pandas/io/parsers/python_parser.py:1039:47:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/pandas-dev/pandas/blob/4692686a750b95884e1d5a4f1313614c71c57213/pandas/io/parsers/python_parser.py#L1106'>pandas/io/parsers/python_parser.py:1106:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/4692686a750b95884e1d5a4f1313614c71c57213/pandas/io/parsers/python_parser.py#L1106'>pandas/io/parsers/python_parser.py:1106:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/4692686a750b95884e1d5a4f1313614c71c57213/pandas/io/parsers/python_parser.py#L1208'>pandas/io/parsers/python_parser.py:1208:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/4692686a750b95884e1d5a4f1313614c71c57213/pandas/io/parsers/python_parser.py#L1343'>pandas/io/parsers/python_parser.py:1343:9:</a> PLR6301 Method `_remove_empty_lines` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/4692686a750b95884e1d5a4f1313614c71c57213/pandas/io/parsers/python_parser.py#L1358'>pandas/io/parsers/python_parser.py:1358:40:</a> PLC1901 `v == ""` can be simplified to `not v` as an empty string is falsey
+ <a href='https://github.com/pandas-dev/pandas/blob/4692686a750b95884e1d5a4f1313614c71c57213/pandas/io/parsers/python_parser.py#L391'>pandas/io/parsers/python_parser.py:391:9:</a> PLR0914 Too many local variables (25/15)
+ <a href='https://github.com/pandas-dev/pandas/blob/4692686a750b95884e1d5a4f1313614c71c57213/pandas/io/parsers/python_parser.py#L399'>pandas/io/parsers/python_parser.py:399:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/4692686a750b95884e1d5a4f1313614c71c57213/pandas/io/parsers/python_parser.py#L399'>pandas/io/parsers/python_parser.py:399:9:</a> PLR1702 Too many nested blocks (7 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/4692686a750b95884e1d5a4f1313614c71c57213/pandas/io/parsers/python_parser.py#L449'>pandas/io/parsers/python_parser.py:449:29:</a> PLC1901 `c == ""` can be simplified to `not c` as an empty string is falsey
+ <a href='https://github.com/pandas-dev/pandas/blob/4692686a750b95884e1d5a4f1313614c71c57213/pandas/io/parsers/python_parser.py#L701'>pandas/io/parsers/python_parser.py:701:9:</a> PLR6301 Method `_is_line_empty` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/4692686a750b95884e1d5a4f1313614c71c57213/pandas/io/parsers/python_parser.py#L815'>pandas/io/parsers/python_parser.py:815:37:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/pandas-dev/pandas/blob/4692686a750b95884e1d5a4f1313614c71c57213/pandas/io/parsers/python_parser.py#L863'>pandas/io/parsers/python_parser.py:863:9:</a> PLR6301 Method `_remove_empty_lines` could be a function, class method, or static method
</pre>

</p>
</details>
<details><summary>Changes by rule (6 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1702 | 4 | 4 | 0 | 0 | 0 |
| PLR6301 | 3 | 3 | 0 | 0 | 0 |
| PLR6201 | 2 | 2 | 0 | 0 | 0 |
| PLC1901 | 2 | 2 | 0 | 0 | 0 |
| PLR0917 | 1 | 1 | 0 | 0 | 0 |
| PLR0914 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @snowsignal on 2024-03-09 04:57_

---

_Closed by @snowsignal on 2024-03-09 04:57_

---

_Branch deleted on 2024-03-09 04:57_

---

_@snowsignal reviewed on 2024-03-09 04:58_

---
