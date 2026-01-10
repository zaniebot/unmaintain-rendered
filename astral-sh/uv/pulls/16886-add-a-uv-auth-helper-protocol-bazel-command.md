---
number: 16886
title: "Add a `uv auth helper --protocol bazel` command"
type: pull_request
state: closed
author: zsol
labels:
  - enhancement
  - preview
assignees: []
base: main
head: zsol/jj-qpnoslvrymlk
created_at: 2025-11-28T19:35:57Z
updated_at: 2025-12-04T18:59:35Z
url: https://github.com/astral-sh/uv/pull/16886
synced_at: 2026-01-10T01:26:18Z
---

# Add a `uv auth helper --protocol bazel` command

---

_Pull request opened by @zsol on 2025-11-28 19:35_

## Summary

This supports Bazel's credential-helper protocol as described in [this doc](https://github.com/bazelbuild/proposals/blob/main/designs/2022-06-07-bazel-credential-helpers.md).

## Test Plan

Added test cases (including a new macro that supports sending stdin to the spawned command).

## TODOs

- [x] add a `get` subcommand
- [x] actually try it with bazel

---

_@zanieb reviewed on 2025-12-02 17:06_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:6185 on 2025-12-02 17:06_

Can we require an argument for the format? e.g., `--format bazel`?

---

_@zanieb reviewed on 2025-12-02 17:07_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4902 on 2025-12-02 17:07_

I wonder if it should be `uv auth credential-helper` or `uv auth helper`? 🤔 

I know I suggested this, but can you remind me why this isn't just an output format for `uv auth token`?

---

_@zanieb reviewed on 2025-12-02 17:10_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:6185 on 2025-12-02 17:10_

(Is the bazel format versioned?)

---

_@zanieb reviewed on 2025-12-02 17:11_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4902 on 2025-12-02 17:11_

I guess because it takes stdin?

---

_@zanieb reviewed on 2025-12-02 17:12_

---

_Review comment by @zanieb on `crates/uv/src/commands/auth/credential_helper.rs`:123 on 2025-12-02 17:12_

I think this should be implemented as `CredentialResponse::from(credentials)`

---

_@zanieb reviewed on 2025-12-02 17:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/auth/credential_helper.rs`:123 on 2025-12-02 17:13_

You can derive `Default` for `CredentialResponse` and use that for the no-credentials empty header case.

---

_@zanieb reviewed on 2025-12-02 17:14_

---

_Review comment by @zanieb on `crates/uv/src/commands/auth/credential_helper.rs`:100 on 2025-12-02 17:14_

I think I'd probably implement this as `CredentialRequest::from_str` and `CredentialRequest::from_stdin`

---

_@zanieb reviewed on 2025-12-02 17:14_

---

_Review comment by @zanieb on `crates/uv/src/commands/auth/credential_helper.rs`:18 on 2025-12-02 17:14_

I'd name these to be specific to Bazel

---

_Review comment by @zanieb on `crates/uv/src/commands/auth/helper.rs`:102 on 2025-12-02 17:16_

It doesn't need to happen in this pull request, but if we make `request.uri` a method provided by a trait, then we can make this function generic over the format and easily add more formats in the future. I'd leave a TODO on the function.

---

_@zanieb reviewed on 2025-12-02 17:16_

---

_@zsol reviewed on 2025-12-02 17:30_

---

_Review comment by @zsol on `crates/uv/src/commands/auth/credential_helper.rs`:18 on 2025-12-02 17:30_

I was torn on how much I should call out Bazel, but it seems like this is the same credential-helper protocol that git and docker use too (Bazel just happens to implement a subset of it). Do you still think we should name these like that?
I'd maybe rather drop the name Bazel from the doc comment

---

_@zsol reviewed on 2025-12-02 17:35_

---

_Review comment by @zsol on `crates/uv/src/commands/auth/credential_helper.rs`:18 on 2025-12-02 17:35_

Actually nevermind, I just double checked and git uses `url` instead of `uri` as the key in the json payload ... I'll make your suggested changes

---

_@zsol reviewed on 2025-12-02 17:42_

---

_Review comment by @zsol on `crates/uv-cli/src/lib.rs`:4902 on 2025-12-02 17:42_

All of the above work for me, no strong opinions. Although a big difference is that `uv auth token https://foo` fails if a username is not provided, whereas `jq -n '{uri: "https://foo"}' | uv auth credential-helper get` doesn't - it picks the first username available.
Maybe it should fail if there are no usernames specified in the input url but we have multiple usernames logged in? 

---

_@zanieb reviewed on 2025-12-02 17:43_

---

_Review comment by @zanieb on `crates/uv/src/commands/auth/helper.rs`:102 on 2025-12-02 17:43_

(Or an enum with handling for each format in the method via match on self)

---

_@zsol reviewed on 2025-12-02 17:44_

---

_Review comment by @zsol on `crates/uv-cli/src/lib.rs`:6185 on 2025-12-02 17:44_

it's not versioned, and yes we can require an argument

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4902 on 2025-12-02 19:44_

I guess I'm leaning towards `uv auth helper` since it has a different API and should always be machine readable IO.

---

_@zanieb reviewed on 2025-12-02 19:44_

---

_@zanieb reviewed on 2025-12-02 19:45_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4902 on 2025-12-02 19:45_

(`credential-helper` seems vaguely more discoverable since it's a standard term but also feels verbose. I don't have strong feelings)

---

_@zanieb reviewed on 2025-12-02 20:21_

---

_Review comment by @zanieb on `crates/uv/src/commands/auth/credential_helper.rs`:18 on 2025-12-02 20:21_

In the request payload? If that's the case we can just use an alias.

Generally I'm wary of the idea that this is the one protocol to rule them all though.

---

_Marked ready for review by @zsol on 2025-12-03 12:44_

---

_Label `enhancement` added by @konstin on 2025-12-03 14:10_

---

_@zsol reviewed on 2025-12-03 15:34_

---

_Review comment by @zsol on `crates/uv-cli/src/lib.rs`:4902 on 2025-12-03 15:34_

> Maybe it should fail if there are no usernames specified in the input url but we have multiple usernames logged in?

I'll follow up in a separate PR with this, everything else is addressed

---

_Referenced in [astral-sh/pyx-docs#46](../../astral-sh/pyx-docs/pulls/46.md) on 2025-12-03 15:51_

---

_Renamed from "Add a `uv auth credential-helper` command" to "Add a `uv auth helper --protocol bazel` command" by @konstin on 2025-12-04 12:04_

---

_Review comment by @konstin on `crates/uv/tests/it/auth.rs`:1956 on 2025-12-04 12:08_

We should show the actual header here to show it's the correct one (the password is already in cleartext above)

---

_Review comment by @konstin on `crates/uv/tests/it/auth.rs`:2083 on 2025-12-04 12:15_

nit: we can add a test case with `differentuser` that checks that a different user doesn't get any credentials, i.e. we're using the user field.

---

_Review comment by @konstin on `crates/uv/src/commands/auth/helper.rs`:47 on 2025-12-04 12:32_

Can you add a comment that bazel skips over uv as credential helper if we return an empty set of headers?

---

_Review comment by @konstin on `crates/uv/src/commands/auth/helper.rs`:68 on 2025-12-04 12:42_

We should add a log message here indicating that we discard a password if there's any. There shouldn't be, but if someone tries to use it, see it doesn't work and then tries to debug it by manually running `uv auth helper --protocol bazel -v`, we should be helpful and tell them that we're intentionally eating the password.

Not as a user-facing warning (since we're in a machine-readable interface anyway), just as a `warn!` or `debug!` that only gets shown with `-v`.

---

_Review comment by @konstin on `crates/uv/src/commands/auth/helper.rs`:83 on 2025-12-04 12:45_

```suggestion
                    .unwrap_or(url.to_string())
```

---

_Review comment by @konstin on `crates/uv/src/commands/auth/helper.rs`:88 on 2025-12-04 12:48_

We should use the global `client_builder` here

---

_Review comment by @konstin on `crates/uv/src/commands/auth/helper.rs`:100 on 2025-12-04 12:52_

```suggestion
        let token = maybe_token.context("No access token found")?;
```

---

_Review comment by @konstin on `crates/uv/src/commands/auth/helper.rs`:134 on 2025-12-04 12:56_

Should the type be `uri: DisplaySafeUrl`? Then serde does the conversion and an invalid URL is a deserialization error.

---

_Review comment by @konstin on `crates/uv/src/commands/auth/helper.rs`:145 on 2025-12-04 13:01_

To print the response with `--quiet` too:

```suggestion
    writeln!(printer.stdout_important(), "{response}")?;
```

Arguably `printer.stdout()` is more consistent as most other APIs use it. But then again `--quiet` doesn't really fit with a machine-readable command.

---

_@konstin approved on 2025-12-04 13:09_

---

_@zsol reviewed on 2025-12-04 14:38_

---

_Review comment by @zsol on `crates/uv/src/commands/auth/helper.rs`:47 on 2025-12-04 14:38_

I double checked just now and that seems not true. Will change the logic here to always return a header.

---

_@zsol reviewed on 2025-12-04 15:20_

---

_Review comment by @zsol on `crates/uv/src/commands/auth/helper.rs`:88 on 2025-12-04 15:20_

Following up in https://github.com/astral-sh/uv/pull/16979

---

_@zanieb reviewed on 2025-12-04 15:25_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4942 on 2025-12-04 15:25_

We don't call them users to their face :) I'd just omit "by users".

---

_@zanieb reviewed on 2025-12-04 15:29_

---

_Review comment by @zanieb on `crates/uv/src/commands/auth/helper.rs`:101 on 2025-12-04 15:29_

nit: In general, I'd avoid the intermediate variable? I think `maybe_` variables are weird in Rust.

```suggestion
        let token = pyx_store
            .access_token(client.for_host(pyx_store.api()).raw_client(), 0)
            .await
            .context("Authentication failure")?
            .context("No access token found")?;
```

---

_@zanieb reviewed on 2025-12-04 15:31_

---

_Review comment by @zanieb on `crates/uv/src/commands/auth/helper.rs`:129 on 2025-12-04 15:31_

I think I'd emit a preview warning for this command / have a feature flag for it. In general, I'm wary to add new interfaces without going through preview to start so we can make breaking changes if we need to.

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:1469 on 2025-12-04 15:33_

We shouldn't hide all this in the `auth_helper` utility, that really confused me. Just use these explicitly in the tests.

---

_@zanieb reviewed on 2025-12-04 15:33_

---

_@zanieb reviewed on 2025-12-04 15:34_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:6240 on 2025-12-04 15:34_

Is this going to be rendered in the docs for the arg? If so, we should make sure it renders correctly with the broken line.

---

_@zsol reviewed on 2025-12-04 18:00_

---

_Review comment by @zsol on `crates/uv-cli/src/lib.rs`:4902 on 2025-12-04 18:00_

https://github.com/astral-sh/uv/pull/16983

---

_Review comment by @zsol on `crates/uv-cli/src/lib.rs`:6240 on 2025-12-04 18:23_

Checked the output of `cargo doc`, it renders fine!

---

_@zsol reviewed on 2025-12-04 18:23_

---

_Comment by @codspeed-hq[bot] on 2025-12-04 18:34_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/zsol%2Fjj-qpnoslvrymlk?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16886 will **improve performances by 12.08%**

<sub>Comparing <code>zsol/jj-qpnoslvrymlk</code> (4469013) with <code>main</code> (d3cd94e)</sub>



### Summary

`⚡ 1` improvement  
`✅ 5` untouched  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | Simulation | [`` resolve_warm_airflow ``](https://codspeed.io/astral-sh/uv/branches/zsol%2Fjj-qpnoslvrymlk?uri=crates%2Fuv-bench%2Fbenches%2Fuv.rs%3A%3Auv%3A%3Aresolve_warm_airflow%3A%3Aresolve_warm_airflow&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 889 ms | 793.2 ms | +12.08% |


---

_Label `preview` added by @zsol on 2025-12-04 18:34_

---

_Merged by @zsol on 2025-12-04 18:56_

---

_Closed by @zsol on 2025-12-04 18:56_

---

_Branch deleted on 2025-12-04 18:56_

---

_Referenced in [aspect-build/rules_py#721](../../aspect-build/rules_py/issues/721.md) on 2025-12-04 19:10_

---

_Referenced in [Homebrew/homebrew-core#257541](../../Homebrew/homebrew-core/pulls/257541.md) on 2025-12-06 14:23_

---
