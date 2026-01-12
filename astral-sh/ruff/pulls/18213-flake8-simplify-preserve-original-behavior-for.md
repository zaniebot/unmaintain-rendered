```yaml
number: 18213
title: "[`flake8-simplify`] Preserve original behavior for `except ()` and bare `except` (`SIM105`)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: issue-18209
created_at: 2025-05-20T06:45:03Z
updated_at: 2025-06-23T20:45:38Z
url: https://github.com/astral-sh/ruff/pull/18213
synced_at: 2026-01-12T15:56:14Z
```

# [`flake8-simplify`] Preserve original behavior for `except ()` and bare `except` (`SIM105`)

---

_@VascoSch92_

The PR addresses issue #18209.


---

_Comment by @github-actions[bot] on 2025-05-20 06:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+6 -6 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/b7d3ff1e85a4e535493c5c245375c717293e5f26/superset/migrations/versions/2015-12-04_09-42_1a48a5411020_adding_slug_to_dash.py#L35'>superset/migrations/versions/2015-12-04_09-42_1a48a5411020_adding_slug_to_dash.py:35:5:</a> SIM105 Use `contextlib.suppress(BaseException)` instead of `try`-`except`-`pass`
- <a href='https://github.com/apache/superset/blob/b7d3ff1e85a4e535493c5c245375c717293e5f26/superset/migrations/versions/2015-12-04_09-42_1a48a5411020_adding_slug_to_dash.py#L35'>superset/migrations/versions/2015-12-04_09-42_1a48a5411020_adding_slug_to_dash.py:35:5:</a> SIM105 Use `contextlib.suppress(Exception)` instead of `try`-`except`-`pass`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+5 -5 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/crazy_functions/Latex_Function.py#L76'>crazy_functions/Latex_Function.py:76:5:</a> SIM105 Use `contextlib.suppress(BaseException)` instead of `try`-`except`-`pass`
- <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/crazy_functions/Latex_Function.py#L76'>crazy_functions/Latex_Function.py:76:5:</a> SIM105 Use `contextlib.suppress(Exception)` instead of `try`-`except`-`pass`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/multi_language.py#L363'>multi_language.py:363:9:</a> SIM105 Use `contextlib.suppress(BaseException)` instead of `try`-`except`-`pass`
- <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/multi_language.py#L363'>multi_language.py:363:9:</a> SIM105 Use `contextlib.suppress(Exception)` instead of `try`-`except`-`pass`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/request_llms/com_qwenapi.py#L57'>request_llms/com_qwenapi.py:57:21:</a> SIM105 Use `contextlib.suppress(BaseException)` instead of `try`-`except`-`pass`
- <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/request_llms/com_qwenapi.py#L57'>request_llms/com_qwenapi.py:57:21:</a> SIM105 Use `contextlib.suppress(Exception)` instead of `try`-`except`-`pass`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/request_llms/oai_std_model_template.py#L72'>request_llms/oai_std_model_template.py:72:5:</a> SIM105 Use `contextlib.suppress(BaseException)` instead of `try`-`except`-`pass`
- <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/request_llms/oai_std_model_template.py#L72'>request_llms/oai_std_model_template.py:72:5:</a> SIM105 Use `contextlib.suppress(Exception)` instead of `try`-`except`-`pass`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/toolbox.py#L463'>toolbox.py:463:13:</a> SIM105 Use `contextlib.suppress(BaseException)` instead of `try`-`except`-`pass`
- <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/toolbox.py#L463'>toolbox.py:463:13:</a> SIM105 Use `contextlib.suppress(Exception)` instead of `try`-`except`-`pass`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM105 | 12 | 6 | 6 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+6 -6 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/b7d3ff1e85a4e535493c5c245375c717293e5f26/superset/migrations/versions/2015-12-04_09-42_1a48a5411020_adding_slug_to_dash.py#L35'>superset/migrations/versions/2015-12-04_09-42_1a48a5411020_adding_slug_to_dash.py:35:5:</a> SIM105 Use `contextlib.suppress(BaseException)` instead of `try`-`except`-`pass`
- <a href='https://github.com/apache/superset/blob/b7d3ff1e85a4e535493c5c245375c717293e5f26/superset/migrations/versions/2015-12-04_09-42_1a48a5411020_adding_slug_to_dash.py#L35'>superset/migrations/versions/2015-12-04_09-42_1a48a5411020_adding_slug_to_dash.py:35:5:</a> SIM105 Use `contextlib.suppress(Exception)` instead of `try`-`except`-`pass`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+5 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/crazy_functions/Latex_Function.py#L76'>crazy_functions/Latex_Function.py:76:5:</a> SIM105 Use `contextlib.suppress(BaseException)` instead of `try`-`except`-`pass`
- <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/crazy_functions/Latex_Function.py#L76'>crazy_functions/Latex_Function.py:76:5:</a> SIM105 Use `contextlib.suppress(Exception)` instead of `try`-`except`-`pass`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/multi_language.py#L363'>multi_language.py:363:9:</a> SIM105 Use `contextlib.suppress(BaseException)` instead of `try`-`except`-`pass`
- <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/multi_language.py#L363'>multi_language.py:363:9:</a> SIM105 Use `contextlib.suppress(Exception)` instead of `try`-`except`-`pass`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/request_llms/com_qwenapi.py#L57'>request_llms/com_qwenapi.py:57:21:</a> SIM105 Use `contextlib.suppress(BaseException)` instead of `try`-`except`-`pass`
- <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/request_llms/com_qwenapi.py#L57'>request_llms/com_qwenapi.py:57:21:</a> SIM105 Use `contextlib.suppress(Exception)` instead of `try`-`except`-`pass`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/request_llms/oai_std_model_template.py#L72'>request_llms/oai_std_model_template.py:72:5:</a> SIM105 Use `contextlib.suppress(BaseException)` instead of `try`-`except`-`pass`
- <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/request_llms/oai_std_model_template.py#L72'>request_llms/oai_std_model_template.py:72:5:</a> SIM105 Use `contextlib.suppress(Exception)` instead of `try`-`except`-`pass`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/toolbox.py#L463'>toolbox.py:463:13:</a> SIM105 Use `contextlib.suppress(BaseException)` instead of `try`-`except`-`pass`
- <a href='https://github.com/binary-husky/gpt_academic/blob/be8390739411da67da8b755f9518f52a54b66f49/toolbox.py#L463'>toolbox.py:463:13:</a> SIM105 Use `contextlib.suppress(Exception)` instead of `try`-`except`-`pass`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM105 | 12 | 6 | 6 | 0 | 0 |

</p>
</details>




---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/flake8_simplify/rules/suppressible_exception.rs`:123 on 2025-05-20 13:11_

This won’t work if `BaseException` is shadowed. Here is an example of how to safely insert a builtin symbol:
https://github.com/astral-sh/ruff/blob/b35bf8ae073a47e12a98eea3eb4818d3695ff302/crates/ruff_linter/src/rules/flake8_bugbear/rules/unreliable_callable_check.rs#L78-L82

---

_@dscorbett reviewed on 2025-05-20 13:11_

---

_Comment by @VascoSch92 on 2025-05-21 06:10_

@ntBre I have a problem. How I could import more stuff at the same time? Do you have an example where it was made in another rule?

I'm asking because here I would like to import `subprocess` from `contextlib` and also `BaseException`

---

_Comment by @MichaReiser on 2025-05-21 07:30_

Would this work https://github.com/astral-sh/ruff/blob/9ae698fe30cf3526f0e7ae237b800b3ed19a819f/crates/ruff_linter/src/rules/ruff/rules/quadratic_list_summation.rs#L105-L115

---

_Review requested from @dscorbett by @VascoSch92 on 2025-05-21 16:49_

---

_Comment by @VascoSch92 on 2025-05-21 16:49_

Hey, 
thanks for the help. 

I hope understood what  you were asking ;-) 

---

_@dscorbett reviewed on 2025-05-21 17:50_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/flake8_simplify/rules/suppressible_exception.rs`:150 on 2025-05-21 17:50_

Is this condition correct when `BaseException` is shadowed and caught explicitly?

```python
BaseException = ValueError
try:
    int("a")
except BaseException:
    pass
```

The fixed version should use `suppress(BaseException)`, not `suppress(builtins.BaseException)`.

---

_@VascoSch92 reviewed on 2025-05-22 06:24_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/flake8_simplify/rules/suppressible_exception.rs`:150 on 2025-05-22 06:24_

Good catch. No, it was't. I added a new test case and fix that

---

_Comment by @VascoSch92 on 2025-05-23 15:09_

It should be fine now

---

_Label `bug` added by @ntBre on 2025-05-29 19:19_

---

_Label `fixes` added by @ntBre on 2025-05-29 19:19_

---

_Review requested from @ntBre by @ntBre on 2025-05-29 19:19_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/suppressible_exception.rs`:121 on 2025-06-05 02:15_

I think we could collapse this check like the one below to

```rust
if handler_names.is_empty() && type_.is_none() {
```

because `join` on an empty `Vec` should also give the empty string. Maybe that's getting too clever though.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/suppressible_exception.rs`:150 on 2025-06-05 02:15_

It feels a bit unfortunate to "repeat" this check here when we did a very similar check up above to get `exception` in the first place, but I'm not having any better ideas right now. Do you see any way to combine them?

---

_@ntBre reviewed on 2025-06-05 02:44_

Thanks! The implementation looks reasonable to me, I just had a couple of vague ideas for your consideration.

I'm also kind of on the fence about whether we should offer the fix for the completely empty case. As mentioned in the issue, the fix could obscure a potential problem that would be caught by other lint rules.

I think the empty tuple case is okay because [useless-contextlib-suppress (B022)](https://docs.astral.sh/ruff/rules/useless-contextlib-suppress/#useless-contextlib-suppress-b022) will catch the empty `suppress`, but I'm not seeing any diagnostics on `contextlib.suppress(BaseException)` with all rules selected ([playground](https://play.ruff.rs/e26c04e6-190b-4e88-8342-68fd1c49ccb6)).

If we still offer the fix, we could consider adding another rule similar to [blind-except (BLE001)](https://docs.astral.sh/ruff/rules/blind-except/#blind-except-ble001) that applies to `contextlib.suppress` (or extend one of these existing rules).

What do you think?

---

_Comment by @ntBre on 2025-06-05 02:50_

> If we still offer the fix, we could consider adding another rule similar to [blind-except (BLE001)](https://docs.astral.sh/ruff/rules/blind-except/#blind-except-ble001) that applies to contextlib.suppress (or extend one of these existing rules).

There's a somewhat related request for this in https://github.com/astral-sh/ruff/issues/4923, except it's for `Exception` rather than `BaseException` and [try-except-pass (S110)](https://docs.astral.sh/ruff/rules/try-except-pass/#try-except-pass-s110) instead of BLE001. There are a lot of rules about exceptions!

---

_@VascoSch92 reviewed on 2025-06-09 19:56_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/flake8_simplify/rules/suppressible_exception.rs`:121 on 2025-06-09 19:56_

But then we lose one `else` or?

the complete snippet is 

```rust
 let exception = if handler_names.is_empty() {
        if type_.is_none() {
            // case where there are no handler names provided at all
            "BaseException".to_string()
        } else {
            // case where handler names is an empty tuple
            String::new()
        }
    } else {
        handler_names.join(", ")
    };
```

---

_@VascoSch92 reviewed on 2025-06-09 19:57_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/flake8_simplify/rules/suppressible_exception.rs`:150 on 2025-06-09 19:57_

No I don't see. I tried to re-use as much code as possible thought :-(

---

_Comment by @VascoSch92 on 2025-06-09 19:58_

> Thanks! The implementation looks reasonable to me, I just had a couple of vague ideas for your consideration.
> 
> I'm also kind of on the fence about whether we should offer the fix for the completely empty case. As mentioned in the issue, the fix could obscure a potential problem that would be caught by other lint rules.
> 
> I think the empty tuple case is okay because [useless-contextlib-suppress (B022)](https://docs.astral.sh/ruff/rules/useless-contextlib-suppress/#useless-contextlib-suppress-b022) will catch the empty `suppress`, but I'm not seeing any diagnostics on `contextlib.suppress(BaseException)` with all rules selected ([playground](https://play.ruff.rs/e26c04e6-190b-4e88-8342-68fd1c49ccb6)).
> 
> If we still offer the fix, we could consider adding another rule similar to [blind-except (BLE001)](https://docs.astral.sh/ruff/rules/blind-except/#blind-except-ble001) that applies to `contextlib.suppress` (or extend one of these existing rules).
> 
> What do you think?

I don't have strong opinions :-) let me know which solution you like and I would change the code accordingly

---

_@ntBre reviewed on 2025-06-09 21:04_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/suppressible_exception.rs`:121 on 2025-06-09 21:04_

If I remember correctly, I was suggesting this:

```rust
 let exception = if handler_names.is_empty() && type_.is_none() {
         "BaseException".to_string()
    } else {
        handler_names.join(", ")
    };
```

That does drop the middle `else`, but `String::new()` and `handler_names.join(", ")` are equivalent when `handler_names` is empty, so it's safe to combine the two `else` branches. However, I think this is evidence that your current code is more clear :smile: 

---

_Comment by @ntBre on 2025-06-09 21:06_

I'll have to think a bit more about this. I'm mostly working on the 0.12 release this week, but I'm putting this on my todo list for afterward! Thanks for following up.

---

_@ntBre approved on 2025-06-18 17:21_

Let's move ahead with the fix and open an issue to add or extend a rule to catch `contextlib.suppress(BaseException)`.

I think that means all of the questions are resolved and this is good to go?

---

_Comment by @VascoSch92 on 2025-06-22 09:06_

@ntBre Yes good to go ;-)

---

_Renamed from "[`flake8-simplify`] fix handlers corner cases (`SIM105`)" to "[`flake8-simplify`] Preserve original behavior for `except ()` and bare `except` (`SIM105`)" by @ntBre on 2025-06-23 14:01_

---

_Merged by @ntBre on 2025-06-23 14:01_

---

_Closed by @ntBre on 2025-06-23 14:01_

---

_Branch deleted on 2025-06-23 20:45_

---
