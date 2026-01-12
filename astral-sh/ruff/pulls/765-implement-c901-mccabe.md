```yaml
number: 765
title: Implement C901 (mccabe)
type: pull_request
state: merged
author: edgarrmondragon
labels: []
assignees: []
merged: true
base: main
head: feat/mccabe
created_at: 2022-11-16T07:34:13Z
updated_at: 2022-11-18T00:17:13Z
url: https://github.com/astral-sh/ruff/pull/765
synced_at: 2026-01-12T05:48:45Z
```

# Implement C901 (mccabe)

---

_Pull request opened by @edgarrmondragon on 2022-11-16 07:34_

Closes https://github.com/charliermarsh/ruff/issues/491

- Implement C901 (mccabe)
- Update contributing guide with settings hashing and flake8-to-ruff guidance


---

_Review comment by @charliermarsh on `src/cli.rs`:96 on 2022-11-16 14:34_

Could we make this `usize` everywhere?

---

_Review comment by @charliermarsh on `src/mccabe/settings.rs`:26 on 2022-11-16 14:35_

I think we should default this to 10 or whatever Flake8's default is, and then use the check enablement to drive toggling on and off (rather than relying on both check enablement _and_ complexity value).

---

_Review comment by @charliermarsh on `flake8_to_ruff/src/converter.rs`:196 on 2022-11-16 14:36_

(Can you also add this to `flake8_to_ruff/src/plugin.rs`?)

---

_Review comment by @charliermarsh on `CONTRIBUTING.md`:106 on 2022-11-16 14:36_

Awesome.

---

_Review comment by @charliermarsh on `src/check_ast.rs`:353 on 2022-11-16 14:37_

I'd vote to nix this "or", and just drive based on `self.settings.enabled`. Wdyt?

---

_Review comment by @charliermarsh on `src/checks_gen.rs`:88 on 2022-11-16 14:37_

It's confusing that both `flake8-comprehensions` and `mccabe` are now enabled by `--select C`, but I guess that's already true in Flake8?

---

_Review comment by @charliermarsh on `src/mccabe/checks.rs`:7 on 2022-11-16 14:39_

I wonder if we should memoize this? E.g., in theory, for nested functions, we're going to first run this check for the outer function (which will include computing complexity for the inner function), then we're going to run it again when we hit the outer function in the AST visitor. For deeply nested functions, that could really add up. Can we reduce the number of re-iterations?

---

_Review comment by @charliermarsh on `src/mccabe/checks.rs`:61 on 2022-11-16 14:39_

Nit: I think we could omit `checker` and return `Option<Check>` (and pass in `max_complexity`).

---

_@charliermarsh reviewed on 2022-11-16 14:39_

This is great!

---

_@edgarrmondragon reviewed on 2022-11-17 18:28_

---

_Review comment by @edgarrmondragon on `src/cli.rs`:96 on 2022-11-17 18:28_

Sure, my hesitance for that came from the default in `mccabe` being `-1`. If we default to something else (e.g. `10`) then `usize` is good.

---

_@edgarrmondragon reviewed on 2022-11-17 18:32_

---

_Review comment by @edgarrmondragon on `src/mccabe/settings.rs`:26 on 2022-11-17 18:32_

flake8' has a similar behavior. Just selecting `C901` doesn't actually trigger the check, `max-complexity` still needs to be set to greater than `-1`:

```console
$ flake8 resources/test/fixtures/C901.py --max-complexity=2 --ignore=F
resources/test/fixtures/C901.py:15:1: C901 'if_elif_else_dead_path' is too complex (3)
resources/test/fixtures/C901.py:24:1: C901 'nested_ifs' is too complex (3)
resources/test/fixtures/C901.py:53:1: C901 'nested_functions' is too complex (3)
resources/test/fixtures/C901.py:61:1: C901 'try_else' is too complex (4)
resources/test/fixtures/C901.py:72:1: C901 'nested_try_finally' is too complex (3)
resources/test/fixtures/C901.py:82:1: C901 'foobar' is too complex (3)

$ flake8 resources/test/fixtures/C901.py --select=C901
```

but I'm all for whatever makes the most sense. 

---

_@edgarrmondragon reviewed on 2022-11-17 18:42_

---

_Review comment by @edgarrmondragon on `src/checks_gen.rs`:88 on 2022-11-17 18:42_

@charliermarsh yeah, this is true in flake8. We could use a different letter too. I know bandit uses `B` but `flake8-bandit` uses `S` to avoid collisions with flake8-bugbear.

---

_@edgarrmondragon reviewed on 2022-11-17 18:48_

---

_Review comment by @edgarrmondragon on `flake8_to_ruff/src/converter.rs`:196 on 2022-11-17 18:48_

Done 7289688

---

_@charliermarsh reviewed on 2022-11-17 19:11_

---

_Review comment by @charliermarsh on `src/mccabe/settings.rs`:26 on 2022-11-17 19:11_

Hmm yeah, I generally want to be compatible with existing tools but I do feel ok deviating on configuration settings. I think it's confusing that `--select=C901` doesn't _actually_ enable the check. I'd vote to use some kind of default and drive enablement purely through `select`.


---

_@charliermarsh reviewed on 2022-11-17 19:11_

---

_Review comment by @charliermarsh on `src/checks_gen.rs`:88 on 2022-11-17 19:11_

I think it's ok, I'd rather be compatible than try to rename check codes.

---

_Comment by @charliermarsh on 2022-11-17 19:47_

@edgarrmondragon - Should I change that `> -1` check and merge?

---

_Comment by @edgarrmondragon on 2022-11-17 19:54_

> @edgarrmondragon - Should I change that `> -1` check and merge?

@charliermarsh I'll set the max complexity default to 10, use `usize` and remove the "or". Should I still look into memoizing `get_complexity_number`?

---

_Review comment by @Stranger6667 on `src/mccabe/checks.rs`:42 on 2022-11-17 20:45_

Would this be clearer?

```rust
let ExcepthandlerKind::ExceptHandler { body, .. } = &handler.node;
complexity += get_complexity_number(body);
```

P.S. I didn't try to compile it though, but as this enum has only one variant, it is an irrefutable pattern (as far as I know), so it should work.

---

_@Stranger6667 reviewed on 2022-11-17 20:45_

---

_Comment by @charliermarsh on 2022-11-17 21:15_

@edgarrmondragon - I think we should try memoizing, but it can come in a separate PR. I'm also happy to take a look at it.

---

_Review comment by @edgarrmondragon on `src/mccabe/checks.rs`:42 on 2022-11-17 22:30_

@Stranger6667 let me try that

---

_@edgarrmondragon reviewed on 2022-11-17 22:30_

---

_Review comment by @edgarrmondragon on `src/mccabe/checks.rs`:42 on 2022-11-17 22:33_

That worked 🙂 f5e8e91b648c56c881d415a0c9a5e0421ed5e41e

---

_@edgarrmondragon reviewed on 2022-11-17 22:33_

---

_Merged by @charliermarsh on 2022-11-17 22:40_

---

_Closed by @charliermarsh on 2022-11-17 22:40_

---

_Comment by @charliermarsh on 2022-11-17 22:41_

Sweet! Thanks @edgarrmondragon!

---

_Comment by @charliermarsh on 2022-11-17 22:41_

(I pushed a tweak at the end just to annotate that examples with the expected complexity.)

---

_@Stranger6667 reviewed on 2022-11-17 22:41_

---

_Review comment by @Stranger6667 on `src/mccabe/checks.rs`:42 on 2022-11-17 22:41_

Awesome! :tada: 

---

_Branch deleted on 2022-11-17 22:44_

---

_Comment by @tgross35 on 2022-11-18 00:17_

Amazing work!

---
