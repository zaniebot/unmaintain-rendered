```yaml
number: 8184
title: Does the Ruff formatter validate the final AST in a similar manner to Black?
type: issue
state: closed
author: max-muoto
labels:
  - question
  - formatter
assignees: []
created_at: 2023-10-24T20:52:36Z
updated_at: 2024-07-06T03:10:17Z
url: https://github.com/astral-sh/ruff/issues/8184
synced_at: 2026-01-10T11:09:50Z
```

# Does the Ruff formatter validate the final AST in a similar manner to Black?

---

_Issue opened by @max-muoto on 2023-10-24 20:52_

We are looking to replace `black` with the Ruff formatter, but one question I had was does the Ruff formatter validate for an AST equivalent to the original? I know this can be disabled in `black` with the `--fast` flag but was curious if this was built into the Ruff formatter by default.

---

_Comment by @charliermarsh on 2023-10-24 20:55_

We don't have this behavior, no. We could consider adding it based on interest.

(Not strictly relevant to the question, but just so there's no confusion for any future readers: all benchmarks were compared against Black with `--fast`.)

---

_Comment by @max-muoto on 2023-10-24 20:56_

Thanks for the info!

---

_Comment by @charliermarsh on 2023-10-24 21:01_

No problem.

For completeness: our test suite consists of a large set of [fixtures](https://github.com/astral-sh/ruff/tree/0236e0751ce0f7230a834f9afe8c747b07faf0ee/crates/ruff_python_formatter/resources/test/fixtures) (including Black's own test fixtures, plus a bunch of our own), and then we also run the formatter over a selection of open source projects on CI too (`twine`, `django`, `transformers`, `typeshed`, `warehouse`, `zulip`, `home-assistant`, `poetry`, `cpython`).

For the fixtures, we validate output against checked-in snapshots. For the ecosystem projects, we're just diffing to assess similarity. But in both cases, we also error if:
- We output syntactically invalid code.
- We fail to format any comments.
- Running the formatter twice results in code changes (i.e., the formatter is unstable).

---

_Label `question` added by @MichaReiser on 2023-10-24 23:34_

---

_Label `formatter` added by @MichaReiser on 2023-10-24 23:34_

---

_Comment by @charliermarsh on 2023-10-25 03:17_

(Will leave this open for now to see if other folks have additional feedback or questions.)

---

_Comment by @konstin on 2023-10-25 16:34_

We also have fuzzing (e.g. https://github.com/astral-sh/ruff/issues/7938) which covers a lot of strange edge cases. In recent runs i've only seen instabilities (the second formatting pass looks different than the first) but no syntax errors.

---

_Comment by @tusharsadhwani on 2023-10-26 04:57_

Validating the final AST can still help against accidental formatting changes that would change runtime behaviour.

A good example would be accidentally formatting `f"{a+ 2=}"` into `f"{a + 2 = }"`, which seems like just a formatting change but actually changes the string produced at runtime. The Python AST for this case contains the entire expression itself in it for the same reason.

---

_Comment by @tusharsadhwani on 2023-10-26 04:59_

FWIW I think it should be opt-in, behind a flag to enable it, and all tests (including ones that run ruff against open source projects) should run with that flag enabled, to catch such cases.

---

_Comment by @MichaReiser on 2023-10-30 09:18_

> Validating the final AST can still help against accidental formatting changes that would change runtime behaviour.

That's true, but it requires that the used AST only captures runtime semantics, which isn't the case for Ruff. For example, ruff stores (or will soon) the parts of each implicit concatenated string as individual nodes because having access to each part is useful when doing static analysis (but irrelevant for an interpreter):

```python
"a" "b"

# joined
"ab"
```

* `"a" "b"`:`StringList["a", "b"]` 
* `"ab"`: `StringList["ab"]`

The runtime value of both expressions is the same, but Ruff uses a different internal representation for each. 

I believe there are other examples around parenthesizing match cases which change the AST structure, but don't result in a semantical change. 

Because of that, I believe using the AST for asserting that the runtime semantics are unchanged isn't a good choice and it prevents Ruff from changing its internal AST structure to capture more information, slowing down the development. 

It would be nice to have another automatic means to validate that the formatter doesn't change program semantics (could be very useful during fuzzing). We could explore integrating and comparing Python's AST as part of fuzzing, but I prefer not to compare Ruff's ASTs as part of `ruff format` for the reasons mentioned above and because re-parsing and comparing the AST causes a significant slowdown.

---

_Comment by @tusharsadhwani on 2023-10-30 09:21_

> We could explore integrating and comparing Python's AST as part of fuzzing

@MichaReiser That is indeed along the lines of what I'm suggesting. Having that option available to end users behind a CLI flag would be a nice to have, but using it for fuzzing during CI seems much more important to me.

---

_Comment by @charliermarsh on 2023-10-30 13:27_

> That's true, but it requires that the used AST only captures runtime semantics, which isn't the case for Ruff. 

In fairness, we do have the `Comparable` AST variant which exists for this purpose and intentionally ignores (e.g.) implicit concatenations. That property should be maintained over time too -- it's meant to capture whether two AST nodes correspond to the same value.

---

_Comment by @MichaReiser on 2023-10-31 01:00_

> In fairness, we do have the Comparable AST variant which exists for this purpose and intentionally ignores (e.g.) implicit concatenations. That property should be maintained over time too -- it's meant to capture whether two AST nodes correspond to the same value.

Oh right. Although it will be interesting to see how we would support this (in a cheap way) if we change our AST structure more fundamentally. 

We're also considering integrating `isort` into the formatter, which would ultimately mean that we have to remove the AST equality check.

---

_Comment by @tusharsadhwani on 2023-11-01 11:07_

Consider having `isort` stuff as a separate step, and do the AST validation before doing import sorting in your tests?

Since import sorting should be a flag that you can disable (`isort` is too opinionated for a lot of people), this would make sense overall too.

---

_Comment by @juftin on 2023-11-08 17:29_

I've been working on introducing the ruff formatter at work but got this question when trying to introduce it as a formatting tool for my organization. 

The `black` formatter's claim that it is safe to use is based on the fact that it [checks the AST before and after and compares the two](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#ast-before-and-after-formatting) is a big reason why a given org may push back on switching from black to ruff - it's a powerful piece of marketing.

> To put things in perspective, the code equivalence check is a feature of Black which other formatters don’t implement at all. It is of crucial importance to us to ensure code behaves the way it did before it got reformatted. We treat this as a feature and there are no plans to relax this in the future.

Implementing a similar AST validation as an optional flag for the ruff formatter would be a great feature that would make switching over a no-brainer for the team - and speaking personally we would be more than willing to pay for the performance hit that it might cause. 

---

_Comment by @SonOfLilit on 2023-11-09 17:14_

If you only want to add it to tests and fuzzing, one easy way to write an AST comparison test would be:

```python
def check_same_ast(path):
    with temp_copy(path) as before:
        with temp_copy(path) as after:
            run_ruff(after)
            run_black(before)
            run_black(after)
            assert run_sha256sum(before) == run_sha256sum(after)
```

---

_Comment by @tusharsadhwani on 2023-11-10 09:37_

@SonOfLilit ruff outputs aren't identical to black though

---

_Comment by @SonOfLilit on 2023-11-11 11:13_

Indeed, they aren't. But black output is idempotent and does the check we
want, so two files will have the same output if and only if they contain
the same AST, so comparing before/after ruff is a cheap way to compare ASTs.

On Fri, Nov 10, 2023, 11:38 Tushar Sadhwani ***@***.***>
wrote:

> @SonOfLilit <https://github.com/SonOfLilit> ruff outputs aren't identical
> to black though
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/ruff/issues/8184#issuecomment-1805389875>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAADU7OIFELZ34GIE6LCLE3YDXYX7AVCNFSM6AAAAAA6OJRBJWVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMYTQMBVGM4DSOBXGU>
> .
> You are receiving this because you were mentioned.Message ID:
> ***@***.***>
>


---

_Comment by @tusharsadhwani on 2023-11-11 11:15_

Ah right, i misread the code. Not a bad idea.

---

_Closed by @charliermarsh on 2023-11-13 17:43_

---

_Comment by @charliermarsh on 2023-11-13 17:48_

We now perform this validation during tests.

Interestingly, in implementing this validation, I learned that Black actually _does_ modify the AST during formatting, and so Ruff does too :) Namely, Black modifies indentation within docstrings, which is reflected in the AST; and Black will also parenthesize long `del` statements (e.g., convert `del a, b` to `del (a, b`)), which doesn't change the semantics but does change the AST.

---

_Comment by @jonasrauber on 2024-07-06 02:28_

If I understand the above correctly, this check is only performed when running ruffs test suite at the moment.
Is there an option to use this at runtime like in black?

---

_Comment by @charliermarsh on 2024-07-06 03:10_

No, we don’t support running this check at runtime.

---
