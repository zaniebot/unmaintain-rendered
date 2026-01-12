```yaml
number: 9857
title: "Patch `sysconfig` data at install time"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - compatibility
  - no-build
assignees: []
merged: true
base: main
head: charlie/patch-sysconfig
created_at: 2024-12-13T00:40:33Z
updated_at: 2024-12-17T05:12:17Z
url: https://github.com/astral-sh/uv/pull/9857
synced_at: 2026-01-12T16:09:00Z
```

# Patch `sysconfig` data at install time

---

_@charliermarsh_

## Summary

This PR reimplements [`sysconfigpatcher`](https://github.com/bluss/sysconfigpatcher) in Rust and applies it to our Python installations at install-time, ensuring that the `sysconfig` data is more likely to be correct.

For now, we only rewrite prefixes (i.e., any path that starts with `/install` gets rewritten to the correct absolute path for the current machine).

Unlike `sysconfigpatcher`, this PR does not yet do any of the following:

- Patch `pkginfo` files.
- Change `clang` references to `cc`.

A few things that we should do as follow-ups, in my opinion:

1. Rewrite [`AR`](https://github.com/bluss/sysconfigpatcher/blob/c1ebf8ab9274dcde255484d93ce0f1fd1f76a248/src/sysconfigpatcher.py#L61).
2. Remove `-isysroot`, which we already do for newer builds.


---

_Label `no-build` added by @charliermarsh on 2024-12-13 00:40_

---

_Label `no-test` added by @charliermarsh on 2024-12-13 00:40_

---

_Converted to draft by @charliermarsh on 2024-12-13 00:40_

---

_Review requested from @zanieb by @charliermarsh on 2024-12-13 00:57_

---

_Review requested from @konstin by @charliermarsh on 2024-12-13 00:57_

---

_Marked ready for review by @charliermarsh on 2024-12-13 00:57_

---

_Label `no-build` removed by @charliermarsh on 2024-12-13 00:57_

---

_Label `no-test` removed by @charliermarsh on 2024-12-13 00:57_

---

_@charliermarsh reviewed on 2024-12-13 00:58_

---

_Review comment by @charliermarsh on `crates/uv-python/src/sysconfig/parser.rs`:1 on 2024-12-13 00:58_

I initially thought we could just parse this as JSON, but we only started to output sysconfig data as JSON in the most recent PBS release. So it won't apply to existing Pythons.

Instead, this is a small parser that just handles Python dictionaries with the assumption that keys are string and values are (strings or integers). Main complexity is that strings can be implicitly concatenated (and are in practice), but it's not too bad.

---

_@charliermarsh reviewed on 2024-12-13 00:59_

---

_Review comment by @charliermarsh on `crates/uv-python/src/sysconfig/cursor.rs`:1 on 2024-12-13 00:59_

This comes from Ruff (which ported it from rustc).

---

_Comment by @charliermarsh on 2024-12-13 00:59_

\cc @bluss who I also chatted with on Discord

---

_Label `enhancement` added by @charliermarsh on 2024-12-13 00:59_

---

_Label `compatibility` added by @charliermarsh on 2024-12-13 00:59_

---

_Label `no-build` added by @charliermarsh on 2024-12-13 03:13_

---

_Review comment by @zanieb on `crates/uv-python/src/sysconfig/parser.rs`:1 on 2024-12-13 03:57_

Do we need to have @dhruvmanila review this? :)

---

_@zanieb reviewed on 2024-12-13 03:57_

---

_@charliermarsh reviewed on 2024-12-13 04:12_

---

_Review comment by @charliermarsh on `crates/uv-python/src/sysconfig/parser.rs`:1 on 2024-12-13 04:12_

Yeah good idea. @dhruvmanila -- this is applied to a file like the following. We know that it's an object with string keys, and the values must be strings or integers (no f-strings, but they can be implicitly concatenated).

```python
# system configuration generated and used by the sysconfig module
build_time_vars = {'ABIFLAGS': '',
 'AC_APPLE_UNIVERSAL_BUILD': 0,
 'AIX_BUILDDATE': 0,
 'AIX_GENUINE_CPLUSPLUS': 0,
 ...
 'XMLLIBSUBDIRS': 'xml xml/dom xml/etree xml/parsers xml/sax',
 'abs_builddir': '/private/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmp5be2znu_/Python-3.10.16',
 'abs_srcdir': '/private/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmp5be2znu_/Python-3.10.16',
 'datarootdir': '/install/share',
 'exec_prefix': '/install',
 'prefix': '/install',
 'srcdir': '.'}
```

---

_Review comment by @dhruvmanila on `crates/uv-python/src/sysconfig/parser.rs`:110 on 2024-12-13 04:57_

Do we need to handle Python specific whitespaces?

https://github.com/astral-sh/ruff/blob/be4ce1673507fe9033ba0d9bd132b8e94430fff8/crates/ruff_python_trivia/src/whitespace.rs#L38-L46

https://docs.python.org/3/reference/lexical_analysis.html#whitespace-between-tokens

---

_Review comment by @dhruvmanila on `crates/uv-python/src/sysconfig/parser.rs`:109 on 2024-12-13 05:05_

Do we need to always do the lookahead first and then bump the character? Typically, we avoid doing lookaheads and only do it when required. If we bump the character first, it always avoids bumping it for every branch in the loop.

This would look like:
```rust
loop {
	let Some(next) = cursor.bump() else {
		break;
	};

	match next {
		...
	}
}
```

For other parse methods, it would then need to accept the character that's bumped. For strings, you could then utilize the `cursor.previous()` method to add a debug assertion that the previous character was the same as the given quote character.

---

_Review comment by @dhruvmanila on `crates/uv-python/src/sysconfig/parser.rs`:202 on 2024-12-13 05:08_

Same as earlier, do we need to look at Python specific whitespace?

---

_Review comment by @dhruvmanila on `crates/uv-python/src/sysconfig/parser.rs`:189 on 2024-12-13 05:18_

nit: we could utilize a `match` to avoid computing `self.first` for each branch

---

_Review comment by @dhruvmanila on `crates/uv-python/src/sysconfig/parser.rs`:91 on 2024-12-13 05:20_

Is there a chance that the whitespace around `=` might be missing? I think the data is auto-generated so it's quite unlikely.

---

_@dhruvmanila reviewed on 2024-12-13 05:21_

---

_Review comment by @dhruvmanila on `crates/uv-python/src/sysconfig/parser.rs`:1 on 2024-12-13 05:52_

The parser looks good. I think it just has a lot of lookaheads where we could just use `bump` but if this is not in a critical path (which it seems like so), then it shouldn't matter practically.

---

_@dhruvmanila reviewed on 2024-12-13 05:52_

---

_@zanieb reviewed on 2024-12-13 18:23_

---

_Review comment by @zanieb on `crates/uv-python/src/sysconfig/mod.rs`:190 on 2024-12-13 18:23_

I'd include something here that shouldn't be patched.

---

_@zanieb approved on 2024-12-13 18:24_

---

_Merged by @charliermarsh on 2024-12-13 19:36_

---

_Closed by @charliermarsh on 2024-12-13 19:36_

---

_Branch deleted on 2024-12-13 19:36_

---

_@charliermarsh reviewed on 2024-12-13 19:38_

---

_Review comment by @charliermarsh on `crates/uv-python/src/sysconfig/parser.rs`:109 on 2024-12-13 19:38_

I left it as-is but I'll play with changing it.

---

_Review comment by @konstin on `crates/uv-python/src/sysconfig/cursor.rs`:9 on 2024-12-16 12:17_

This looks a lot like https://github.com/typst/unscanny

---

_@konstin reviewed on 2024-12-16 12:17_

---

_@dhruvmanila reviewed on 2024-12-17 05:12_

---

_Review comment by @dhruvmanila on `crates/uv-python/src/sysconfig/cursor.rs`:9 on 2024-12-17 05:12_

That looks interesting!

---
