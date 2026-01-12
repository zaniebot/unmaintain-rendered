```yaml
number: 14725
title: "Add new `uv-keyring` crate that vendors the `keyring-rs` crate"
type: pull_request
state: merged
author: jtfmumm
labels:
  - internal
assignees: []
merged: true
base: main
head: jtfm/vendor-keyring
created_at: 2025-07-18T13:56:17Z
updated_at: 2025-08-15T13:57:58Z
url: https://github.com/astral-sh/uv/pull/14725
synced_at: 2026-01-12T16:11:22Z
```

# Add new `uv-keyring` crate that vendors the `keyring-rs` crate

---

_@jtfmumm_

This PR is a first step toward support for storing credentials in the system keyring. The `keyring-rs` crate is the best option for system keyring integration, but the latest version (v4) requires either that Linux users have `libdbus` installed or that it is built with `libdbus` vendored in. This is because v4 depends on [dbus-secret-service](https://github.com/open-source-cooperative/dbus-secret-service), which was created as an alternative to [secret-service](https://github.com/open-source-cooperative/secret-service-rs) so that users are not required to use an async runtime. Since uv does use an async runtime, this is not a good tradeoff for uv.

This PR:
* Vendors `keyring-rs` crate into a new `uv-keyring` workspace crate
* Moves to the async `secret-service` crate that does not require clients on Linux to have `libdbus` on their machines. This includes updating `CredentialsAPI` trait (and implementations) to use async methods.
* Adds `uv-keyring` tests to `cargo test` jobs. For `cargo test | ubuntu`, this meant setting up secret service and priming gnome-keyring as an earlier step.
* Removes iOS code paths
* Patches in @oconnor663 's changes from his [`keyring-rs` PR](https://github.com/open-source-cooperative/keyring-rs/pull/261)
* Applies many clippy-driven updates

---

_Label `internal` added by @jtfmumm on 2025-07-18 13:56_

---

_Marked ready for review by @jtfmumm on 2025-08-06 13:53_

---

_@jtfmumm reviewed on 2025-08-06 14:07_

---

_Review comment by @jtfmumm on `crates/uv-keyring/README.md`:1 on 2025-08-06 14:07_

This is an edited version of the original `keyring-rs` v4 `README`. 

---

_@jtfmumm reviewed on 2025-08-06 14:09_

---

_Review comment by @jtfmumm on `crates/uv-keyring/src/credential.rs`:22 on 2025-08-06 14:09_

Most of these trait methods have been changed to `async` in this PR due to the switch from `dbus-secret-service` to `secret-service`.

---

_@jtfmumm reviewed on 2025-08-06 14:16_

---

_Review comment by @jtfmumm on `crates/uv-keyring/src/lib.rs`:297 on 2025-08-06 14:16_

`async` methods on `Entry` have been updated from the sync versions in `keyring-rs`.

---

_@jtfmumm reviewed on 2025-08-06 14:22_

---

_Review comment by @jtfmumm on `crates/uv-keyring/src/secret_service.rs`:479 on 2025-08-06 14:22_

`find_legacy_items` and `find_unique_legacy_items` replace methods in `keyring-rs` that took a closure and mapped over items. However, with async closures this created lifetime problems. I switched to an approach where we find and return items to the caller, which then applies the function to them directly. The tradeoff is more duplication but I'm not sure if there's a better way.

---

_Review requested from @zanieb by @jtfmumm on 2025-08-11 10:52_

---

_Review requested from @oconnor663 by @jtfmumm on 2025-08-11 10:52_

---

_@oconnor663 reviewed on 2025-08-11 18:53_

---

_Review comment by @oconnor663 on `crates/uv-keyring/src/secret_service.rs`:479 on 2025-08-11 18:53_

John was kind enough to put up [a branch where I can play with this](https://github.com/astral-sh/uv/tree/jtfm/vendor-keyring-async-lifetimes). On that branch, I think I can get things working using the new `AsyncFn` trait like this:

```diff
diff --git i/crates/uv-keyring/src/secret_service.rs w/crates/uv-keyring/src/secret_service.rs
index ef79f5cb1..de4b96e5f 100644
--- i/crates/uv-keyring/src/secret_service.rs
+++ w/crates/uv-keyring/src/secret_service.rs
@@ -456,10 +456,9 @@ impl SsCredential {
     }
 
     /// JACK
-    async fn map_matching_items<F, Fut, T>(&self, f: F, require_unique: bool) -> Result<Vec<T>>
+    async fn map_matching_items<F, T>(&self, f: F, require_unique: bool) -> Result<Vec<T>>
     where
-        F: for<'a> Fn(&'a Item<'_>) -> Fut,
-        Fut: std::future::Future<Output = Result<T>>,
+        F: AsyncFn(&Item) -> Result<T>,
         T: Sized,
     {
         let ss = SecretService::connect(EncryptionType::Dh)
@@ -497,15 +496,14 @@ impl SsCredential {
     }
 
     /// JACK
-    pub async fn map_matching_legacy_items<F, Fut, T>(
+    pub async fn map_matching_legacy_items<F, T>(
         &self,
         ss: &SecretService<'_>,
         f: F,
         require_unique: bool,
     ) -> Result<Vec<T>>
     where
-        F: Fn(&Item) -> Fut,
-        Fut: std::future::Future<Output = Result<T>>,
+        F: AsyncFn(&Item) -> Result<T>,
         T: Sized,
     {
         let collection = ss.get_default_collection().await.map_err(decode_error)?;
```

The difference between these two approaches isn't crystal clear to me, but [this motivating example in the RFC was helpful](https://rust-lang.github.io/rfcs/3668-async-closures.html#inability-to-express-higher-ranked-async-function-signatures):

> This happens because the type for the `Fut` generic parameter is chosen by the caller...and it cannot reference the higher-ranked lifetime `for<'c>` in the `FnMut` trait bound, but the anonymous future produced by calling `do_something` does capture a generic lifetime parameter, and _must_ capture it in order to use the `&str` argument.

---

_@oconnor663 reviewed on 2025-08-11 19:48_

---

_Review comment by @oconnor663 on `crates/uv-keyring/src/macos.rs`:60 on 2025-08-11 19:48_

These underlying platform functions are doing blocking IO, so I lean towards wrapping them with [`spawn_blocking`](https://docs.rs/tokio/latest/tokio/task/fn.spawn_blocking.html). I was kind of on the fence about it, but then if you look at [underlying calls that `set_generic_password makes`](https://github.com/kornelski/rust-security-framework/blob/5256c398b9576857d91e1df719c8cddae91a4fbd/security-framework/src/passwords.rs#L154-L160), and you track down the upstream docs for [`SecItemAdd`](https://developer.apple.com/documentation/security/secitemadd(_:_:)) and [`SecItemUpdate`](https://developer.apple.com/documentation/security/secitemupdate(_:_:)), Apple actually specifically calls this out:

> **Performance considerations:** `SecItemAdd` blocks the calling thread, so it can cause your app’s UI to hang if called from the main thread. Instead, call `SecItemAdd` from a background dispatch queue or `async` function

I think we run Tokio single-threaded, so we might have the same concerns a single-threaded UI framework would here?

---

_@oconnor663 reviewed on 2025-08-11 20:02_

---

_Review comment by @oconnor663 on `crates/uv-keyring/tests/threading.rs`:143 on 2025-08-11 20:02_

I'm not sure exactly what sort of mistakes this test is looking for, but I think wrapping blocking things in `spawn_blocking` will make it more faithful to the original without requiring any changes here?

---

_Review comment by @oconnor663 on `crates/uv-keyring/src/windows.rs`:432 on 2025-08-11 20:30_

Agreed that it's not worth propagating `unsafe` everywhere, since these are module-private functions in vendored code.

---

_@oconnor663 reviewed on 2025-08-11 20:30_

---

_@zanieb reviewed on 2025-08-11 20:34_

---

_Review comment by @zanieb on `crates/uv-keyring/src/macos.rs`:60 on 2025-08-11 20:34_

I'm definitely a big fan of avoiding unawaited blocking IO in async contexts.

---

_@oconnor663 reviewed on 2025-08-13 01:01_

---

_Review comment by @oconnor663 on `crates/uv-keyring/src/secret_service.rs`:322 on 2025-08-13 01:01_

Nice :)

---

_@jtfmumm reviewed on 2025-08-13 10:10_

---

_Review comment by @jtfmumm on `crates/uv-keyring/src/macos.rs`:60 on 2025-08-13 10:10_

Updated for macOS and Windows

---

_Review comment by @oconnor663 on `crates/uv-keyring/src/windows.rs`:298 on 2025-08-13 15:47_

`Send` is in the "prelude", so I don't think it needs to be fully qualified.

---

_Review comment by @oconnor663 on `crates/uv-keyring/src/macos.rs`:174 on 2025-08-13 15:51_

Should this one get the `spawn_blocking` treatment too?

---

_@oconnor663 approved on 2025-08-13 16:15_

LGTM with a couple comments

---

_@jtfmumm reviewed on 2025-08-13 18:44_

---

_Review comment by @jtfmumm on `crates/uv-keyring/src/macos.rs`:174 on 2025-08-13 18:44_

Yeah, added

---

_Merged by @jtfmumm on 2025-08-15 13:57_

---

_Closed by @jtfmumm on 2025-08-15 13:57_

---

_Branch deleted on 2025-08-15 13:57_

---
