```yaml
number: 9911
title: "[`ruff`] Implement missing `await` for coroutine (`RUF066`)"
type: pull_request
state: open
author: mikeleppane
labels:
  - rule
  - accepted
  - preview
assignees: []
base: main
head: rule(RUF028)/detect-unused-await
created_at: 2024-02-09T13:15:13Z
updated_at: 2025-10-29T15:05:27Z
url: https://github.com/astral-sh/ruff/pull/9911
synced_at: 2026-01-10T16:59:49Z
```

# [`ruff`] Implement missing `await` for coroutine (`RUF066`)

---

_Pull request opened by @mikeleppane on 2024-02-09 13:15_

## Summary

Detect never-awaited [coroutines](https://docs.python.org/3/library/asyncio-dev.html#detect-never-awaited-coroutines) 

This rule only applies to simple scenarios. We need to be quite conservative on how to handle coroutines. Because there are many situations in which the programmer does not want to evaluate/await a coroutine but instead use the actual coroutine object.


Closes https://github.com/astral-sh/ruff/issues/9833

## Test Plan

```bash
cargo test
```

---

_Comment by @github-actions[bot] on 2024-02-09 14:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @codspeed-hq[bot] on 2024-02-09 14:08_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mikeleppane%3Arule(RUF028)%2Fdetect-unused-await?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #9911 will **not alter performance**

<sub>Comparing <code>mikeleppane:rule(RUF028)/detect-unused-await</code> (37bc212) with <code>main</code> (46decd4)</sub>



### Summary

`✅ 30` untouched  
`⏩ 13` skipped[^skipped]  



[^skipped]: 13 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/mikeleppane%3Arule(RUF028)%2Fdetect-unused-await?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @zanieb on 2024-02-09 23:45_

Thanks for contributing!

It looks like the ecosystem checks are showing some false positives e.g. https://github.com/apache/airflow/blob/48bfb1a970f5b47ba1b385ad809b8324923ddf3e/tests/providers/google/cloud/hooks/test_dataflow.py#L1956 — could you look into those?

---

_Comment by @mikeleppane on 2024-02-10 07:51_

> Thanks for contributing!
> 
> It looks like the ecosystem checks are showing some false positives e.g. https://github.com/apache/airflow/blob/48bfb1a970f5b47ba1b385ad809b8324923ddf3e/tests/providers/google/cloud/hooks/test_dataflow.py#L1956 — could you look into those?

Thanks! Good catch. 

---

_Review requested from @zanieb by @zanieb on 2024-02-12 21:10_

---

_Label `accepted` added by @MichaReiser on 2024-04-05 10:27_

---

_Comment by @MichaReiser on 2024-04-05 10:28_

This rule fits into ruff as a suspicious rule. I think we should rename the rule to `never-awaited-coroutines` so that `allow(never-awaited-coroutines)` reads better.

---

_@earonesty reviewed on 2024-04-09 22:16_

---

_Review comment by @earonesty on `crates/ruff_linter/resources/test/fixtures/ruff/RUF028.py`:110 on 2024-04-09 22:16_

is this ok?  another_coro is never awaited.   if you await coro() you will get an unevaluated coro.
 
this works tho:

```
async def test_coroutine_with_sync_return():
    async def another_coro():
        pass
    def coro():
        return another_coro()
    await coro()  # OK
```



---

_Comment by @akselerando on 2024-04-30 12:51_

Hi, any update on when this will be released?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/missing_await_for_coroutine.rs`:135 on 2024-05-01 11:26_

The function docs doesn't match what it actually returns. I think we should either return the `Fix` as mentioned in the docs or update the name to something like `create_await_expr`. I would recommend to go with the former.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/missing_await_for_coroutine.rs`:79 on 2024-05-01 11:32_

The curly braces aren't required for empty structs

```suggestion
    let mut diagnostic = Diagnostic::new(MissingAwaitForCoroutine, call.range);
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/missing_await_for_coroutine.rs`:67 on 2024-05-01 11:33_

I think it might be useful to move the comment as part of `possibly_missing_await`'s documentation.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/missing_await_for_coroutine.rs`:84 on 2024-05-01 11:35_

nit: we could possibly just create an `Insertion` edit instead at the start of the call expression.

```suggestion
    diagnostic.set_fix(Fix::unsafe_edit(Edit::insertion(
        "await ".to_string(),
        call.start(),
    )));
```

which is basically what the generator does as well ;)

https://github.com/astral-sh/ruff/blob/f76a3e850209977ec52ad48f01ff242d287406de/crates/ruff_python_codegen/src/generator.rs#L980-L985

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/missing_await_for_coroutine.rs`:66 on 2024-05-01 11:39_

nit: Do you think it would be helpful for the user to know this limitation? If so, we could possibly provide a "Limitations" section in the rule documentation. For example, refer https://docs.astral.sh/ruff/rules/implicit-optional/#limitations.

---

_@dhruvmanila approved on 2024-05-01 11:41_

Thank you for implementing this rule!

Can either @zanieb or @AlexWaygood look at this? It's good to go from my side but I've mainly looked at the code part of the rule as I'm not an async expert :)

---

_Label `rule` added by @dhruvmanila on 2024-05-01 11:42_

---

_Label `preview` added by @dhruvmanila on 2024-05-01 11:42_

---

_Renamed from "rule(`RUF028`) Implement missing await for coroutine" to "[`ruff`] Implement missing `await` for coroutine (`RUF028`)" by @dhruvmanila on 2024-05-01 11:42_

---

_Comment by @akselerando on 2024-09-19 07:21_

Hi again, when can we expect this to be released?

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-09-19 07:25_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-15 17:55_

---

_Comment by @amyreese on 2025-09-23 03:16_

Rebased onto latest main (now it's `RUF066`) and addressed most review comments.

---

_Renamed from "[`ruff`] Implement missing `await` for coroutine (`RUF028`)" to "[`ruff`] Implement missing `await` for coroutine (`RUF066`)" by @amyreese on 2025-09-23 03:16_

---

_@amyreese reviewed on 2025-09-23 03:18_

---

_Review comment by @amyreese on `crates/ruff_linter/resources/test/fixtures/ruff/RUF028.py`:110 on 2025-09-23 03:18_

This looks like a legimitate miss. Will need to figure out how to catch that case (I'm not sure how it differs from other test cases that catch this).

---

_Review requested from @MichaReiser by @amyreese on 2025-09-23 03:18_

---

_Review requested from @ntBre by @amyreese on 2025-09-23 03:18_

---

_Comment by @MichaReiser on 2025-09-23 08:32_

@amyreese did you make any other changes besides rebasing/renaming the rule (trying to understand where I need to focus my review on :))? Reading your other comment, I assume you reviewed the rule's semantics already.

---

_Comment by @amyreese on 2025-09-23 15:10_

> @amyreese did you make any other changes besides rebasing/renaming the rule (trying to understand where I need to focus my review on :))? Reading your other comment, I assume you reviewed the rule's semantics already.

I made changes to fix/address all of Dhruv's comments, and to update it where internal API's changed. I have no changed functionality though, and the name is the same as it was when it was last updated by the original author. The semantics generally look good to me, though I'm concerned about one of the test cases being "clean" when IMO it should be triggering a diagnostic.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/missing_await_for_coroutine.rs`:47 on 2025-09-23 17:18_

I also think it's worth mentioning that this check only applies to functions defined in the same file as the caller. Calls to functions defined in other files will always be accepted because Ruff can't infer their types. 

This is also my biggest hesitation with merging this rule as it can give users a false sense of safety where they think ruff would catch unawaited co-routines, but it only catches those that are between calls in the same file. 



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/missing_await_for_coroutine.rs`:111 on 2025-09-23 17:19_

What are cases where the `name` wouldn't match the `id` after looking up the binding.

---

_@MichaReiser reviewed on 2025-09-23 17:24_

I still need to look at the test cases and the case we currently miss but I, unfortunately, ran out of time today so this has to wait for tomorrow.

---

_@MichaReiser reviewed on 2025-09-25 11:41_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/RUF028.py`:110 on 2025-09-25 11:41_

Yeah, I think we should catch this

---

_Comment by @MichaReiser on 2025-09-25 11:43_

I think it'd be great if this rule could use Ruff's limited type checker functionality to infer what `Awaitable` bindings are. For example, it would be great if Ruff detects if a parameter annotated with `Awaitable` is never awaited. 

I'm a bit surprised that there are zero ecosystem hits. It makes me wonder if it's because the rule's capabilities are too limited or if there are other reasons.

---

_Comment by @amyreese on 2025-09-30 18:05_

> I think it'd be great if this rule could use Ruff's limited type checker functionality to infer what `Awaitable` bindings are. For example, it would be great if Ruff detects if a parameter annotated with `Awaitable` is never awaited.

Is that something that Ruff can do, or does that require getting type information from ty?  

---

_Comment by @MichaReiser on 2025-10-01 06:48_

> Is that something that Ruff can do, or does that require getting type information from ty?

That's something Ruff can "do" already today. It's a bit manual. See the `TypeChecker` trait and its implementations

https://github.com/astral-sh/ruff/blob/32be31d2f9dce7a5e23b406c5b8a338dc4d21755/crates/ruff_python_semantic/src/analyze/typing.rs#L582-L587

---
