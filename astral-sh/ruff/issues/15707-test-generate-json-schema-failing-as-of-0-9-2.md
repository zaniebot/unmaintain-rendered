```yaml
number: 15707
title: test_generate_json_schema failing as of 0.9.2
type: issue
state: closed
author: WhyNotHugo
labels:
  - testing
assignees: []
created_at: 2025-01-24T03:40:56Z
updated_at: 2025-01-24T04:53:40Z
url: https://github.com/astral-sh/ruff/issues/15707
synced_at: 2026-01-10T11:09:57Z
```

# test_generate_json_schema failing as of 0.9.2

---

_Issue opened by @WhyNotHugo on 2025-01-24 03:40_

### Description

As of 0.9.2, the test_generate_json_schema test fails when building from source. Of note, we build with the default features.

Full build output is here: https://paste.sr.ht/~whynothugo/41004049e3a4f9d2098bec4593a68fd949bd5cd4

The output for this specific test starts at [line 1816](https://paste.sr.ht/~whynothugo/41004049e3a4f9d2098bec4593a68fd949bd5cd4#ruff-0.9.3.log-L1816) and is 4251 lines long.

This error is produced when building the Alpine packages. The build steps can be summarised as:

```sh
# prepare()
# shadow git repo for tests
git init -q

# Avoid downloading a different toolchain on systems with rustup installed.
rm rust-toolchain.toml

cargo fetch --locked

# build()
gpep517 build-wheel \
        --wheel-dir .dist \
        --config-json '{"build-args": "--frozen"}' \
        --output-fd 3 3>&1 >&2

./target/release/ruff generate-shell-completion bash > $pkgname.bash
./target/release/ruff generate-shell-completion fish > $pkgname.fish
./target/release/ruff generate-shell-completion zsh > $pkgname.zsh

# Update ruff.schema.json as the pre-built one is generated
# using the '--all-features' Cargo flag which we don't pass.
cargo dev generate-all

# check()
unset CI_PROJECT_DIR
cargo test --frozen --workspace --exclude ruff_benchmark
```

---

_Comment by @InSyncWithFoo on 2025-01-24 04:16_

[Line 5474](https://paste.sr.ht/~whynothugo/41004049e3a4f9d2098bec4593a68fd949bd5cd4#ruff-0.9.3.log-L5474) is the one in question:

```text
5472 |          "PLW01",
5473 |          "PLW010",
5474 | [32m>        "PLW0101",[0m
5475 |          "PLW0108",
5476 |          "PLW012",
```

`PLW0101` was added in #10801. Its implementation turned out to have an infinite loop bug, which was reported in #15248. #15252 then made it a test-only rule, adding [these attributes](https://github.com/astral-sh/ruff/pull/15252/files#diff-6aee1fd915e8ed29e72aa56560f2b15b8abc4168dc73a8b40afe516b08572a94R369):

```rust
#[cfg(any(feature = "test-rules", test))]  // < !!!
if checker.enabled(Rule::UnreachableCode) {
    /* ... */
}
```

#15278 fixed the issue, but did not revert the attributes.

---

_Comment by @dhruvmanila on 2025-01-24 04:20_

This will be fixed with https://github.com/astral-sh/ruff/pull/15627

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-01-24 04:20_

---

_Label `testing` added by @dhruvmanila on 2025-01-24 04:20_

---

_Comment by @InSyncWithFoo on 2025-01-24 04:25_

@dhruvmanila I don't think so. #15278 fixed `PLW0101`, so the correct thing to do is perhaps to repromote it.

---

_Comment by @dhruvmanila on 2025-01-24 04:28_

I don't think we're sure of promoting it yet, so for now we should just fix the JSON schema generation. cc @dylwil3 who's taking the lead on this specific issue.

---

_Comment by @InSyncWithFoo on 2025-01-24 04:33_

Now that I rechecked it, @dylwil3 [did say](https://github.com/astral-sh/ruff/pull/15278#pullrequestreview-2537389009) that the rule should be kept in test-only realm:

> Added the latest example from the fuzzer, which passes. So let's merge this in and continue to test this rule and improve the CFG implementation.

It has been two weeks since then, though, and there don't seem to be any new issues. Maybe it's time?

---

_Closed by @dhruvmanila on 2025-01-24 04:48_

---

_Closed by @dhruvmanila on 2025-01-24 04:48_

---

_Comment by @WhyNotHugo on 2025-01-24 04:53_

Thanks for the quick fix!

---
