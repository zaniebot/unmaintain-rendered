```yaml
number: 4198
title: "Refactor `Fix` and `Edit` API"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: internal/4182
created_at: 2023-05-03T01:42:17Z
updated_at: 2023-05-08T09:57:04Z
url: https://github.com/astral-sh/ruff/pull/4198
synced_at: 2026-01-12T03:56:38Z
```

# Refactor `Fix` and `Edit` API

---

_Pull request opened by @zanieb on 2023-05-03 01:42_

Part of https://github.com/charliermarsh/ruff/issues/4181
Closes https://github.com/charliermarsh/ruff/issues/4182

Thanks for the well specified issue! This implements many of the noted changes (see commit messages). There are a couple of deviations:

- Removes `Fix::empty`
- Updates `Message::fix` to be `Option<Fix>`
- Updates `MessageHeader::fix` to be `Option<Fix>`
- Does not remove `From<Edit> for Fix` (see [discussion](https://github.com/charliermarsh/ruff/pull/4198#issuecomment-1537473767))



---

_@zanieb reviewed on 2023-05-03 01:44_

---

_Review comment by @zanieb on `crates/ruff/src/autofix/mod.rs`:46 on 2023-05-03 01:44_

I'm guessing there's a more idiomatic way to handle this now that `diagnostic.fix` is an `Option`?

---

_Comment by @zanieb on 2023-05-03 01:46_

The removal of `From<Edit> for Fix` is pretty heavy. It looks like many rule functions passed to `try_set_fix` return `Edit` types. Is the intention to replace the return value of all of those functions with `Fix::unspecified`? e.g. https://github.com/charliermarsh/ruff/blob/cab65b25da8aff6fb9cb539b55345aae64a835f0/crates/ruff/src/rules/pyflakes/fixes.rs#L198

---

_Comment by @github-actions[bot] on 2023-05-03 02:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.9±0.20ms     2.4 MB/sec    1.00     16.8±0.17ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.01ms     4.1 MB/sec    1.00      4.0±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    500.1±1.40µs     5.9 MB/sec    1.01    505.6±6.62µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.04ms     3.6 MB/sec    1.00      7.0±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.05ms     4.9 MB/sec    1.00      8.4±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1792.1±6.07µs     9.3 MB/sec    1.01   1802.9±6.57µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.0±0.65µs    15.1 MB/sec    1.01    198.6±2.94µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.02ms     6.9 MB/sec    1.00      3.7±0.05ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.6±0.01ms     6.2 MB/sec    1.12      7.4±0.11ms     5.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1294.1±6.65µs    12.9 MB/sec    1.08  1396.5±21.32µs    11.9 MB/sec
parser/numpy/globals.py                    1.00   134.5±19.64µs    21.9 MB/sec    1.03    138.6±2.55µs    21.3 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.00ms     9.0 MB/sec    1.09      3.1±0.05ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     17.6±0.20ms     2.3 MB/sec    1.00     17.1±0.21ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.4±0.08ms     3.8 MB/sec    1.00      4.3±0.11ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.04   505.8±13.93µs     5.8 MB/sec    1.00    486.6±6.53µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.4±0.14ms     3.4 MB/sec    1.00      7.2±0.16ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.7±0.12ms     4.7 MB/sec    1.00      8.7±0.12ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1817.6±38.49µs     9.2 MB/sec    1.00  1815.4±21.20µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.3±3.36µs    15.1 MB/sec    1.02    199.9±8.55µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.7 MB/sec    1.01      3.8±0.05ms     6.6 MB/sec
parser/large/dataset.py                    1.01      6.8±0.08ms     6.0 MB/sec    1.00      6.8±0.07ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.02  1305.7±18.54µs    12.8 MB/sec    1.00  1276.6±28.38µs    13.0 MB/sec
parser/numpy/globals.py                    1.03    133.4±2.82µs    22.1 MB/sec    1.00    129.2±2.78µs    22.8 MB/sec
parser/pydantic/types.py                   1.02      2.9±0.03ms     8.8 MB/sec    1.00      2.8±0.04ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/autofix/mod.rs`:46 on 2023-05-03 07:06_

You can write `diagnostic.fix().map(|fix| (diagnostic.kind.rule(), fix))`

---

_Comment by @MichaReiser on 2023-05-03 07:07_

> The removal of `From<Edit> for Fix` is pretty heavy. It looks like many rule functions passed to `try_set_fix` return `Edit` types. Is the intention to replace the return value of all of those functions with `Fix::unspecified`? e.g.
> 
> https://github.com/charliermarsh/ruff/blob/cab65b25da8aff6fb9cb539b55345aae64a835f0/crates/ruff/src/rules/pyflakes/fixes.rs#L198

Yeah, this is unfortunate but necessary because we'll need to explicitly pass the `Applicability` in the future. 

---

_@MichaReiser reviewed on 2023-05-03 07:08_

Thank you for working on this refactor

---

_Review comment by @zanieb on `crates/ruff/src/autofix/mod.rs`:46 on 2023-05-03 14:29_

Wonderful thanks!

---

_@zanieb reviewed on 2023-05-03 14:29_

---

_Comment by @zanieb on 2023-05-03 14:30_

> Yeah, this is unfortunate but necessary because we'll need to explicitly pass the Applicability in the future.

Makes sense, just wanted to check before I really went down the rabbit hole on that one — I'll make those changes next and mark this as ready for review when it's done.

---

_Comment by @zanieb on 2023-05-04 00:12_

@MichaReiser I've encountered a bit of a problem and since the work is rather repetitive I'd like to check in again :)

In many cases, we can just replace `Result<Edit>` with `Result<Fix>` but there are generic functions like `delete_stmt` that return `Result<Edit>` and should probably continue to do so. Since they're used from many rules, I think it does not make sense for it to decide on the applicability of the proposed edits.
https://github.com/charliermarsh/ruff/blob/561f610b0543c28d46916c5760012c342aa8b08f/crates/ruff/src/autofix/actions.rs#LL169C1-L169C1

However there are some cases where this is used in `Diagnostic::try_set_fix`
https://github.com/charliermarsh/ruff/blob/e3bac78ea70667358a87ee090e2734bd9bb87dcb/crates/ruff/src/rules/flake8_pie/rules.rs#L143-L152

In this use, there's not a clear way to provide the specificity without requiring an `Applicability` to be passed to `try_set_fix` or introducing new factory methods like `Fix::try_unspecified` or `Diagnostics::try_set_unspecified_fix`. 

Suggestions?

Overall, this requires some careful editing as there are additional cases where a function is reused and probably should not decide the applicability level. My hopes of replacing these with regular expressions have gone down the drain 😁 Maybe it'd be easier to leave `From<Edit> for Fix` in place for now and update each call site individually?

---

_Comment by @MichaReiser on 2023-05-04 15:50_

> @MichaReiser I've encountered a bit of a problem and since the work is rather repetitive I'd like to check in again :)

I would have been surprised if my proposal just worked :laughing:. Thanks for doing this work so carefully. 

> In many cases, we can just replace `Result<Edit>` with `Result<Fix>` but there are generic functions like `delete_stmt` that return `Result<Edit>` and should probably continue to do so. Since they're used from many rules, I think it does not make sense for it to decide on the applicability of the proposed edits. [`561f610`/crates/ruff/src/autofix/actions.rs#LL169C1-L169C1](https://github.com/charliermarsh/ruff/blob/561f610b0543c28d46916c5760012c342aa8b08f/crates/ruff/src/autofix/actions.rs#LL169C1-L169C1)

I agree, that this should be decided by the caller rather than by `delete_stmt`. 

> 
> However there are some cases where this is used in `Diagnostic::try_set_fix`
> 
> https://github.com/charliermarsh/ruff/blob/e3bac78ea70667358a87ee090e2734bd9bb87dcb/crates/ruff/src/rules/flake8_pie/rules.rs#L143-L152
> 
> In this use, there's not a clear way to provide the specificity without requiring an `Applicability` to be passed to `try_set_fix` or introducing new factory methods like `Fix::try_unspecified` or `Diagnostics::try_set_unspecified_fix`.

In this case, I would create the `Fix` inside of the callback by using the `try` operator. It's a bit more verbose but avoids the need for new factory methods. 

```rust
 diagnostic.try_set_fix(|| { 
	Ok(Fix::unspecified(
		delete_stmt( 
	         pass_stmt, 
	         None, 
	         &[], 
	         checker.locator, 
	         checker.indexer, 
	         checker.stylist, 
	     )?
	))
 }); 
```

> 
> Suggestions?
> 
> Overall, this requires some careful editing as there are additional cases where a function is reused and probably should not decide the applicability level. 

I think it's fine if we get some of them wrong because we have to re-visit all of them anyway when deciding on the fixe's safety. We can use this opportunity to decide whether the function should defer the determination of the fixe's safety to the caller or even change the signature to return an `Edit`.

The main motivation behind changing all of them to `unspecified` is that we can then use the IDE to find all places that still need to be changed.


> My hopes of replacing these with regular expressions have gone down the drain grin Maybe it'd be easier to leave `From<Edit> for Fix` in place for now and update each call site individually?

That's' also be a valid approach that I haven't thought of. Determining the "fixes to migrate" workflow could then be:

* comment out the `From` implementation
* fix all build errors of a single rule, 
* uncomment the `From` implementation except if there are no build errors, then remove the `From` implementation. 
* Submit PR
 
 I'll let you decide. You know the extent of the work best.





---

_Comment by @zanieb on 2023-05-04 16:20_

Of course you can just use a closure :) my lack of experience with Rust is showing. I think that's a reasonable approach if you think it's maintainable. I'll forge onward but if the changes feel hard to review I'll consider the "separate changes" workflow some more.

Thanks for the guidance again!

---

_Comment by @zanieb on 2023-05-04 19:39_

@MichaReiser I am leaning towards separate pull requests for individual rules. Otherwise this is going to take a while longer. It'll be harder to review and I'll need to keep rebasing as old patterns are used on `main`. I also wouldn't be upset if you wanted to take a look — familiarity with the codebase would probably make doing it all at once significantly easier.

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:236 on 2023-05-05 06:24_

`MessageHeader` is a helper struct to deserialize a `Message`. Its fields must match the fields on `Message` or the deserialization will fail if we have a `Message` without a fix. 

That's why we should change the `fix` definition on `MessageHeader` to `Option<Fix>` to match `Message`

```rust 
#[derive(Deserialize)]
struct MessageHeader {
    kind: DiagnosticKind,
    range: TextRange,
    fix: Option<Fix>,
    file_id: usize,
    noqa_row: TextSize,
}
```

---

_@MichaReiser reviewed on 2023-05-05 06:27_

> @MichaReiser I am leaning towards separate pull requests for individual rules. Otherwise this is going to take a while longer. It'll be harder to review and I'll need to keep rebasing as old patterns are used on `main`. I also wouldn't be upset if you wanted to take a look — familiarity with the codebase would probably make doing it all at once significantly easier.

Sounds good to me. We can merge the PR as is and do a couple of follow-up PRs where we migrate a bunch of `.set_fix` calls to use `Fix::unspecified`. That also gives us the option for multiple collaborators to contribute to the refactor, so that you don't have to do all by yourself. 

Thank you

---

_Review comment by @zanieb on `crates/ruff_cli/src/cache.rs`:236 on 2023-05-07 15:41_



Done in https://github.com/charliermarsh/ruff/pull/4198/commits/310fa94f2c36b6d46d059f8168265085262ce29e — do you normally add tests for things like this to ensure it stays up to date?

---

_@zanieb reviewed on 2023-05-07 15:41_

---

_Marked ready for review by @zanieb on 2023-05-07 15:43_

---

_Comment by @zanieb on 2023-05-07 15:45_

Following merge, we can add a comment to #4181 and/or https://github.com/charliermarsh/ruff/issues/4184 covering the workflow for incremental removal of `From<Edit> for Fix`

---

_@MichaReiser reviewed on 2023-05-08 09:56_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:236 on 2023-05-08 09:56_

Thx. We probably should but don't have to as part of this PR. 

---

_@MichaReiser approved on 2023-05-08 09:56_

Thank you, this is awesome.

---

_Merged by @MichaReiser on 2023-05-08 09:57_

---

_Closed by @MichaReiser on 2023-05-08 09:57_

---
