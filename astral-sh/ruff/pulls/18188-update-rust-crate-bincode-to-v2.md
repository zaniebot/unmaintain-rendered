```yaml
number: 18188
title: Update Rust crate bincode to v2
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/bincode-2.x
created_at: 2025-05-19T06:17:49Z
updated_at: 2025-05-19T06:59:35Z
url: https://github.com/astral-sh/ruff/pull/18188
synced_at: 2026-01-12T15:56:14Z
```

# Update Rust crate bincode to v2

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [bincode](https://redirect.github.com/bincode-org/bincode) | workspace.dependencies | major | `1.3.3` -> `2.0.0` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>bincode-org/bincode (bincode)</summary>

### [`v2.0.1`](https://redirect.github.com/bincode-org/bincode/releases/tag/v2.0.1)

[Compare Source](https://redirect.github.com/bincode-org/bincode/compare/v2.0.0...v2.0.1)

#### What's Changed

-   Update unty requirement from 0.0.3 to 0.0.4 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/bincode-org/bincode/pull/753](https://redirect.github.com/bincode-org/bincode/pull/753)
-   Use qualified path for Result::Ok in bincode_derive by [@&#8203;nhaef](https://redirect.github.com/nhaef) in [https://github.com/bincode-org/bincode/pull/757](https://redirect.github.com/bincode-org/bincode/pull/757)
-   Fix issue when a foreign `Err` pollutes scope by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/758](https://redirect.github.com/bincode-org/bincode/pull/758)
-   Derive Debug for Configuration by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/759](https://redirect.github.com/bincode-org/bincode/pull/759)
-   bump version to 2.0.1 by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/760](https://redirect.github.com/bincode-org/bincode/pull/760)

#### New Contributors

-   [@&#8203;nhaef](https://redirect.github.com/nhaef) made their first contribution in [https://github.com/bincode-org/bincode/pull/757](https://redirect.github.com/bincode-org/bincode/pull/757)

**Full Changelog**: https://github.com/bincode-org/bincode/compare/v2.0.0...v2.0.1

### [`v2.0.0`](https://redirect.github.com/bincode-org/bincode/releases/tag/v2.0.0)

[Compare Source](https://redirect.github.com/bincode-org/bincode/compare/v1.3.3...v2.0.0)

Stable! Finally! After 4 years in development! Many changes have made it in since rc.3, including (unfortunately) some last minute breaking changes. But documentation has been cleaned up to a point where we finally feel comfortable committing to things as they are.

If you haven't been following along with the 2.0 changes here is a brief overview

-   Completely rewritten API decoupled from serde
-   `no_std` support
-   Official format specification
-   Default configuration changes
-   Increase MSRV to 1.85.0

#### What's Changed since 1.3.1

-   Fix `WithOtherTrailing` and `WithOtherIntEncoding` by [@&#8203;luben](https://redirect.github.com/luben) in [https://github.com/bincode-org/bincode/pull/342](https://redirect.github.com/bincode-org/bincode/pull/342)
-   Update docs to highlight differences between DefaultOptions and fns by [@&#8203;apgoetz](https://redirect.github.com/apgoetz) in [https://github.com/bincode-org/bincode/pull/373](https://redirect.github.com/bincode-org/bincode/pull/373)
-   Address questions regarding suitability for storage and untrusted inputs by [@&#8203;mbr](https://redirect.github.com/mbr) in [https://github.com/bincode-org/bincode/pull/346](https://redirect.github.com/bincode-org/bincode/pull/346)
-   Fixed a stray comment. by [@&#8203;manuthambi](https://redirect.github.com/manuthambi) in [https://github.com/bincode-org/bincode/pull/360](https://redirect.github.com/bincode-org/bincode/pull/360)
-   update CI to new branching scheme by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/376](https://redirect.github.com/bincode-org/bincode/pull/376)
-   clarify msrv support by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/375](https://redirect.github.com/bincode-org/bincode/pull/375)
-   fix linting ci error by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/377](https://redirect.github.com/bincode-org/bincode/pull/377)
-   prep branch for 2.0 work by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/379](https://redirect.github.com/bincode-org/bincode/pull/379)
-   Update URLs and some cleanups by [@&#8203;atouchet](https://redirect.github.com/atouchet) in [https://github.com/bincode-org/bincode/pull/383](https://redirect.github.com/bincode-org/bincode/pull/383)
-   fix typo by [@&#8203;ehooi](https://redirect.github.com/ehooi) in [https://github.com/bincode-org/bincode/pull/392](https://redirect.github.com/bincode-org/bincode/pull/392)
-   Edit version badge link by [@&#8203;atouchet](https://redirect.github.com/atouchet) in [https://github.com/bincode-org/bincode/pull/389](https://redirect.github.com/bincode-org/bincode/pull/389)
-   Optimize varint parsing by [@&#8203;saethlin](https://redirect.github.com/saethlin) in [https://github.com/bincode-org/bincode/pull/337](https://redirect.github.com/bincode-org/bincode/pull/337)
-   Fix CI on trunk by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/408](https://redirect.github.com/bincode-org/bincode/pull/408)
-   Update logo by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/407](https://redirect.github.com/bincode-org/bincode/pull/407)
-   Make bincode_derive 0 dependencies by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/409](https://redirect.github.com/bincode-org/bincode/pull/409)
-   Config rewrite by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/412](https://redirect.github.com/bincode-org/bincode/pull/412)
-   Reintroduce varint optimizations by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/414](https://redirect.github.com/bincode-org/bincode/pull/414)
-   Feature/deserde by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/413](https://redirect.github.com/bincode-org/bincode/pull/413)
-   Updated readme.md and added a test for the examples by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/417](https://redirect.github.com/bincode-org/bincode/pull/417)
-   Update authors to reflect current code state by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/418](https://redirect.github.com/bincode-org/bincode/pull/418)
-   Add necessary metadata to bincode_derive by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/420](https://redirect.github.com/bincode-org/bincode/pull/420)
-   Replace test-all-features with a manual CI matrix by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/419](https://redirect.github.com/bincode-org/bincode/pull/419)
-   Made the zigzag encoding examples compile and run by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/421](https://redirect.github.com/bincode-org/bincode/pull/421)
-   Fix some typos by [@&#8203;Seppel3210](https://redirect.github.com/Seppel3210) in [https://github.com/bincode-org/bincode/pull/423](https://redirect.github.com/bincode-org/bincode/pull/423)
-   Generate qualified Result type in derive by [@&#8203;andrenth](https://redirect.github.com/andrenth) in [https://github.com/bincode-org/bincode/pull/430](https://redirect.github.com/bincode-org/bincode/pull/430)
-   Fixes for 427 by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/428](https://redirect.github.com/bincode-org/bincode/pull/428)
-   split off BorrowDecode from Decode in bincode_derive by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/432](https://redirect.github.com/bincode-org/bincode/pull/432)
-   functions to enable encoding/decoding serde types by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/422](https://redirect.github.com/bincode-org/bincode/pull/422)
-   Allow serde types to be Decode/Encoded by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/434](https://redirect.github.com/bincode-org/bincode/pull/434)
-   Release 2.0.0-alpha.1 by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/435](https://redirect.github.com/bincode-org/bincode/pull/435)
-   Added Decode/Encode for HashMap\<K, V> by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/438](https://redirect.github.com/bincode-org/bincode/pull/438)
-   Added test case for a borrowed str by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/441](https://redirect.github.com/bincode-org/bincode/pull/441)
-   Fixed clippy warnings by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/447](https://redirect.github.com/bincode-org/bincode/pull/447)
-   Impl BorrowDecode for Option<&\[u8]> and Option<\&str> by [@&#8203;songzhi](https://redirect.github.com/songzhi) in [https://github.com/bincode-org/bincode/pull/446](https://redirect.github.com/bincode-org/bincode/pull/446)
-   Feature/config limit by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/439](https://redirect.github.com/bincode-org/bincode/pull/439)
-   \[Breaking change] Made all `decode_from_slice` also return the number of bytes read by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/445](https://redirect.github.com/bincode-org/bincode/pull/445)
-   Extract virtue by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/443](https://redirect.github.com/bincode-org/bincode/pull/443)
-   Made the CI also check the benchmarks, fixed compile issue in benchmarks by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/449](https://redirect.github.com/bincode-org/bincode/pull/449)
-   Made the derive macros automatically implement the required traits on generic arguments by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/454](https://redirect.github.com/bincode-org/bincode/pull/454)
-   Release v2.0.0-alpha.2 by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/455](https://redirect.github.com/bincode-org/bincode/pull/455)
-   feat: Make `Configuration` functions `const` by [@&#8203;Popog](https://redirect.github.com/Popog) in [https://github.com/bincode-org/bincode/pull/456](https://redirect.github.com/bincode-org/bincode/pull/456)
-   Fix failed varint bench by [@&#8203;ygf11](https://redirect.github.com/ygf11) in [https://github.com/bincode-org/bincode/pull/457](https://redirect.github.com/bincode-org/bincode/pull/457)
-   Updated readme, added a paragraph on why we don't support #\[repr(u8)] by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/461](https://redirect.github.com/bincode-org/bincode/pull/461)
-   Bump virtue 0.0.4 by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/463](https://redirect.github.com/bincode-org/bincode/pull/463)
-   Fixed derive impl on an empty enum by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/462](https://redirect.github.com/bincode-org/bincode/pull/462)
-   Release v2.0.0-beta.0 by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/464](https://redirect.github.com/bincode-org/bincode/pull/464)
-   Fix overflow error when deserializing invalid Duration by [@&#8203;5225225](https://redirect.github.com/5225225) in [https://github.com/bincode-org/bincode/pull/465](https://redirect.github.com/bincode-org/bincode/pull/465)
-   Fix panic with invalid system time by [@&#8203;5225225](https://redirect.github.com/5225225) in [https://github.com/bincode-org/bincode/pull/469](https://redirect.github.com/bincode-org/bincode/pull/469)
-   Add fuzzing harness, try to decode into various types by [@&#8203;5225225](https://redirect.github.com/5225225) in [https://github.com/bincode-org/bincode/pull/468](https://redirect.github.com/bincode-org/bincode/pull/468)
-   Switched Decode and BorrowDecode to take \&mut D by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/470](https://redirect.github.com/bincode-org/bincode/pull/470)
-   Switch Encode to take \&mut E by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/471](https://redirect.github.com/bincode-org/bincode/pull/471)
-   Implemented the newly stabilized CString::from_vec_with_nul method by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/473](https://redirect.github.com/bincode-org/bincode/pull/473)
-   Made SerdeDecoder attempt to allocate memory before complaining by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/475](https://redirect.github.com/bincode-org/bincode/pull/475)
-   Update documentation by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/480](https://redirect.github.com/bincode-org/bincode/pull/480)
-   Moved Configuration::standard() and ::legacy() to the config module by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/481](https://redirect.github.com/bincode-org/bincode/pull/481)
-   Feature/improve serde by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/477](https://redirect.github.com/bincode-org/bincode/pull/477)
-   made the serde functions consistent with the base bincode functions by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/483](https://redirect.github.com/bincode-org/bincode/pull/483)
-   Migration guide by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/482](https://redirect.github.com/bincode-org/bincode/pull/482)
-   Release v2.0.0-beta.1 by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/484](https://redirect.github.com/bincode-org/bincode/pull/484)
-   Run code coverage on all features by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/485](https://redirect.github.com/bincode-org/bincode/pull/485)
-   Added #\[serde(untagged)] to the documentation of attributes that don't work by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/486](https://redirect.github.com/bincode-org/bincode/pull/486)
-   Fixed an error in bincode derive where it would implement the wrong trait if a generic parameter is present by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/487](https://redirect.github.com/bincode-org/bincode/pull/487)
-   Release v2.0.0-beta.2 by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/488](https://redirect.github.com/bincode-org/bincode/pull/488)
-   Added a table to the documentation to pick which functions to use by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/490](https://redirect.github.com/bincode-org/bincode/pull/490)
-   Fix a bunch of typos by [@&#8203;poljar](https://redirect.github.com/poljar) in [https://github.com/bincode-org/bincode/pull/492](https://redirect.github.com/bincode-org/bincode/pull/492)
-   Return an error if a decoded slice length doesn't fit into usize by [@&#8203;poljar](https://redirect.github.com/poljar) in [https://github.com/bincode-org/bincode/pull/491](https://redirect.github.com/bincode-org/bincode/pull/491)
-   Updated to virtue 0.0.6, added #\[bincode(crate = other)] attribute by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/494](https://redirect.github.com/bincode-org/bincode/pull/494)
-   Bumped dependency of virtue to 0.0.7 by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/495](https://redirect.github.com/bincode-org/bincode/pull/495)
-   Bincode 1 compatibility framework by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/489](https://redirect.github.com/bincode-org/bincode/pull/489)
-   Added documentation on how to add compatibility tests by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/497](https://redirect.github.com/bincode-org/bincode/pull/497)
-   Made the compatibility check also include bincode 2 serde, and added comments by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/501](https://redirect.github.com/bincode-org/bincode/pull/501)
-   Fix CString compatibility with bincode v1 by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/502](https://redirect.github.com/bincode-org/bincode/pull/502)
-   Fuzz for compatibility with bincode v1 by [@&#8203;5225225](https://redirect.github.com/5225225) in [https://github.com/bincode-org/bincode/pull/498](https://redirect.github.com/bincode-org/bincode/pull/498)
-   Fix/issue 500 by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/503](https://redirect.github.com/bincode-org/bincode/pull/503)
-   Add Membership test by [@&#8203;ppamorim](https://redirect.github.com/ppamorim) in [https://github.com/bincode-org/bincode/pull/500](https://redirect.github.com/bincode-org/bincode/pull/500)
-   Release v2.0.0-beta.3 by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/505](https://redirect.github.com/bincode-org/bincode/pull/505)
-   Reference implementations for Reader and Writer by [@&#8203;BRA1L0R](https://redirect.github.com/BRA1L0R) in [https://github.com/bincode-org/bincode/pull/507](https://redirect.github.com/bincode-org/bincode/pull/507)
-   Made config::standard() implement .write_fixed_array_header() by default by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/509](https://redirect.github.com/bincode-org/bincode/pull/509)
-   Added HashSet by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/516](https://redirect.github.com/bincode-org/bincode/pull/516)
-   Made the compat fuzzer ignore any LimitExceeded error by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/515](https://redirect.github.com/bincode-org/bincode/pull/515)
-   Release 2.0.0-rc.1 by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/510](https://redirect.github.com/bincode-org/bincode/pull/510)
-   Add zoxide under Bincode in the Wild by [@&#8203;ajeetdsouza](https://redirect.github.com/ajeetdsouza) in [https://github.com/bincode-org/bincode/pull/525](https://redirect.github.com/bincode-org/bincode/pull/525)
-   Made the Cow Encode constraints more permissive by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/524](https://redirect.github.com/bincode-org/bincode/pull/524)
-   Added `additional` to the `UnexpectedEnd` decode error by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/522](https://redirect.github.com/bincode-org/bincode/pull/522)
-   Allow encoding/decoding of HashMap and HashSet with custom hash algorithms by [@&#8203;bronsonp](https://redirect.github.com/bronsonp) in [https://github.com/bincode-org/bincode/pull/529](https://redirect.github.com/bincode-org/bincode/pull/529)
-   Added `std::error::Error::source` by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/530](https://redirect.github.com/bincode-org/bincode/pull/530)
-   Added cross platform tests workflow by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/534](https://redirect.github.com/bincode-org/bincode/pull/534)
-   Fix riscv32 atomics and fix tests on 32-bit platforms by [@&#8203;xobs](https://redirect.github.com/xobs) in [https://github.com/bincode-org/bincode/pull/533](https://redirect.github.com/bincode-org/bincode/pull/533)
-   Fix cross platform tests by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/540](https://redirect.github.com/bincode-org/bincode/pull/540)
-   Switched to weak dependencies by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/538](https://redirect.github.com/bincode-org/bincode/pull/538)
-   Fix tuple struct encoding in serde by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/549](https://redirect.github.com/bincode-org/bincode/pull/549)
-   Rewrite: seperated Decode and BorrowDecode by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/526](https://redirect.github.com/bincode-org/bincode/pull/526)
-   Add impl Encode for \[T], where T: Encode by [@&#8203;cronokirby](https://redirect.github.com/cronokirby) in [https://github.com/bincode-org/bincode/pull/542](https://redirect.github.com/bincode-org/bincode/pull/542)
-   Add impls for Rc<\[T]> and Arc<\[T]> by [@&#8203;maciejhirsz](https://redirect.github.com/maciejhirsz) in [https://github.com/bincode-org/bincode/pull/552](https://redirect.github.com/bincode-org/bincode/pull/552)
-   Shrink `DecodeError` from 48 to 32 bytes on 64-bit arch by [@&#8203;maciejhirsz](https://redirect.github.com/maciejhirsz) in [https://github.com/bincode-org/bincode/pull/553](https://redirect.github.com/bincode-org/bincode/pull/553)
-   Added windows and macos runner by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/554](https://redirect.github.com/bincode-org/bincode/pull/554)
-   Implement `Decode` for `Box<str>` by [@&#8203;SabrinaJewson](https://redirect.github.com/SabrinaJewson) in [https://github.com/bincode-org/bincode/pull/562](https://redirect.github.com/bincode-org/bincode/pull/562)
-   Fixed clippy warning and updated DecodeError by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/574](https://redirect.github.com/bincode-org/bincode/pull/574)
-   Updated test dependencies: uuid and glam by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/576](https://redirect.github.com/bincode-org/bincode/pull/576)
-   Made `peek_read` take `&mut self` by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/572](https://redirect.github.com/bincode-org/bincode/pull/572)
-   Prefixed the E and D generic argument in bincode-derive by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/573](https://redirect.github.com/bincode-org/bincode/pull/573)
-   Implement Default for Configuration by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/575](https://redirect.github.com/bincode-org/bincode/pull/575)
-   Document what the usizes are for ([#&#8203;546](https://redirect.github.com/bincode-org/bincode/issues/546)) in [https://github.com/bincode-org/bincode/pull/577](https://redirect.github.com/bincode-org/bincode/pull/577)
-   Clarify config::legacy() doc to match config::standard() by [@&#8203;trevyn](https://redirect.github.com/trevyn) in [https://github.com/bincode-org/bincode/pull/580](https://redirect.github.com/bincode-org/bincode/pull/580)
-   Implement Encode for tuples with up-to 16 elements. by [@&#8203;gz](https://redirect.github.com/gz) in [https://github.com/bincode-org/bincode/pull/583](https://redirect.github.com/bincode-org/bincode/pull/583)
-   Document configuration generics by [@&#8203;trevyn](https://redirect.github.com/trevyn) in [https://github.com/bincode-org/bincode/pull/581](https://redirect.github.com/bincode-org/bincode/pull/581)
-   Extended BorrowDecode for HashMap to support custom hashers by [@&#8203;Speedy37](https://redirect.github.com/Speedy37) in [https://github.com/bincode-org/bincode/pull/585](https://redirect.github.com/bincode-org/bincode/pull/585)
-   Allow decoding with custom `DeserializeSeed` by [@&#8203;MrGVSV](https://redirect.github.com/MrGVSV) in [https://github.com/bincode-org/bincode/pull/586](https://redirect.github.com/bincode-org/bincode/pull/586)
-   Added `[serde(tag)]` to the list of tags that are known to give issues by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/584](https://redirect.github.com/bincode-org/bincode/pull/584)
-   Release 2.0.0-rc.2 by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/588](https://redirect.github.com/bincode-org/bincode/pull/588)
-   Bump `virtue` to 0.0.9 and add test for [#&#8203;537](https://redirect.github.com/bincode-org/bincode/issues/537) by [@&#8203;trevyn](https://redirect.github.com/trevyn) in [https://github.com/bincode-org/bincode/pull/591](https://redirect.github.com/bincode-org/bincode/pull/591)
-   Encode variant index instead of variant value by [@&#8203;trevyn](https://redirect.github.com/trevyn) in [https://github.com/bincode-org/bincode/pull/593](https://redirect.github.com/bincode-org/bincode/pull/593)
-   Create CODE_OF_CONDUCT.md by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/597](https://redirect.github.com/bincode-org/bincode/pull/597)
-   Move generated files to `target/generated/bincode` by [@&#8203;trevyn](https://redirect.github.com/trevyn) in [https://github.com/bincode-org/bincode/pull/600](https://redirect.github.com/bincode-org/bincode/pull/600)
-   Add DecodeError::Other by [@&#8203;odysa](https://redirect.github.com/odysa) in [https://github.com/bincode-org/bincode/pull/602](https://redirect.github.com/bincode-org/bincode/pull/602)
-   Fixed new clippy lint in rust 1.65.0 by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/603](https://redirect.github.com/bincode-org/bincode/pull/603)
-   Add CIFuzz GitHub Action by [@&#8203;DavidKorczynski](https://redirect.github.com/DavidKorczynski) in [https://github.com/bincode-org/bincode/pull/604](https://redirect.github.com/bincode-org/bincode/pull/604)
-   Fixed new clippy warnings by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/617](https://redirect.github.com/bincode-org/bincode/pull/617)
-   Improved encoding and decoding speed of Vec<u8> by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/619](https://redirect.github.com/bincode-org/bincode/pull/619)
-   Bumped virtue to 0.0.13 by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/626](https://redirect.github.com/bincode-org/bincode/pull/626)
-   Disabled i686-linux-andoid and x86\_64-linux-android CI as they fail for external reasons by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/627](https://redirect.github.com/bincode-org/bincode/pull/627)
-   Made arrays never encode their length by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/625](https://redirect.github.com/bincode-org/bincode/pull/625)
-   Release rc.3 by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/628](https://redirect.github.com/bincode-org/bincode/pull/628)
-   Fix typos in Spec.md enum example by [@&#8203;bigbass1997](https://redirect.github.com/bigbass1997) in [https://github.com/bincode-org/bincode/pull/630](https://redirect.github.com/bincode-org/bincode/pull/630)
-   fix(doc): broken intra link by [@&#8203;elpiel](https://redirect.github.com/elpiel) in [https://github.com/bincode-org/bincode/pull/634](https://redirect.github.com/bincode-org/bincode/pull/634)
-   Added dependabot by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/636](https://redirect.github.com/bincode-org/bincode/pull/636)
-   Bump actions/checkout from 1 to 3 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/bincode-org/bincode/pull/638](https://redirect.github.com/bincode-org/bincode/pull/638)
-   Bump codecov/codecov-action from 2 to 3 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/bincode-org/bincode/pull/639](https://redirect.github.com/bincode-org/bincode/pull/639)
-   Update glam requirement from 0.21 to 0.24 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/bincode-org/bincode/pull/640](https://redirect.github.com/bincode-org/bincode/pull/640)
-   Update criterion requirement from 0.3 to 0.4 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/bincode-org/bincode/pull/637](https://redirect.github.com/bincode-org/bincode/pull/637)
-   Update criterion requirement from 0.4 to 0.5 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/bincode-org/bincode/pull/641](https://redirect.github.com/bincode-org/bincode/pull/641)
-   Update virtue requirement from 0.0.13 to 0.0.14 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/bincode-org/bincode/pull/645](https://redirect.github.com/bincode-org/bincode/pull/645)
-   Bump actions/upload-artifact from 1 to 3 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/bincode-org/bincode/pull/651](https://redirect.github.com/bincode-org/bincode/pull/651)
-   Allow generics in impl_borrow_decode by [@&#8203;dullbananas](https://redirect.github.com/dullbananas) in [https://github.com/bincode-org/bincode/pull/635](https://redirect.github.com/bincode-org/bincode/pull/635)
-   Bump actions/checkout from 3 to 4 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/bincode-org/bincode/pull/660](https://redirect.github.com/bincode-org/bincode/pull/660)
-   Fixed a new clippy warning by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/661](https://redirect.github.com/bincode-org/bincode/pull/661)
-   Reverted 'static constraint on T in Vec<T> and \[T; N] by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/663](https://redirect.github.com/bincode-org/bincode/pull/663)
-   Fix cross compilations by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/664](https://redirect.github.com/bincode-org/bincode/pull/664)
-   Added unty dependency and added type checks by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/667](https://redirect.github.com/bincode-org/bincode/pull/667)
-   Fix inconsistent naming between serde and non-serde functions by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/671](https://redirect.github.com/bincode-org/bincode/pull/671)
-   Update virtue requirement from 0.0.14 to 0.0.15 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/bincode-org/bincode/pull/673](https://redirect.github.com/bincode-org/bincode/pull/673)
-   Compat and BorrowCompat Debug and Display implementations by [@&#8203;aegroto](https://redirect.github.com/aegroto) in [https://github.com/bincode-org/bincode/pull/670](https://redirect.github.com/bincode-org/bincode/pull/670)
-   Add missing test for encode_utf8 by [@&#8203;CXWorks](https://redirect.github.com/CXWorks) in [https://github.com/bincode-org/bincode/pull/683](https://redirect.github.com/bincode-org/bincode/pull/683)
-   Add getters for current configuration values by [@&#8203;shahn](https://redirect.github.com/shahn) in [https://github.com/bincode-org/bincode/pull/681](https://redirect.github.com/bincode-org/bincode/pull/681)
-   Use const functions where possible by [@&#8203;richardpringle](https://redirect.github.com/richardpringle) in [https://github.com/bincode-org/bincode/pull/684](https://redirect.github.com/bincode-org/bincode/pull/684)
-   Implement Encode & Decode for Wrapping<T> types by [@&#8203;mzachar](https://redirect.github.com/mzachar) in [https://github.com/bincode-org/bincode/pull/686](https://redirect.github.com/bincode-org/bincode/pull/686)
-   Fixed broken commit to trunk by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/687](https://redirect.github.com/bincode-org/bincode/pull/687)
-   Update glam requirement from 0.24 to 0.25 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/bincode-org/bincode/pull/688](https://redirect.github.com/bincode-org/bincode/pull/688)
-   Add LICENSE.md to derive/ by [@&#8203;jfsulliv](https://redirect.github.com/jfsulliv) in [https://github.com/bincode-org/bincode/pull/698](https://redirect.github.com/bincode-org/bincode/pull/698)
-   Update virtue requirement from 0.0.15 to 0.0.16 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/bincode-org/bincode/pull/692](https://redirect.github.com/bincode-org/bincode/pull/692)
-   Update spec for `Option<T>` encoding by [@&#8203;mkeeter](https://redirect.github.com/mkeeter) in [https://github.com/bincode-org/bincode/pull/702](https://redirect.github.com/bincode-org/bincode/pull/702)
-   Fixed [#&#8203;707](https://redirect.github.com/bincode-org/bincode/issues/707) by [@&#8203;Vrtgs](https://redirect.github.com/Vrtgs) in [https://github.com/bincode-org/bincode/pull/708](https://redirect.github.com/bincode-org/bincode/pull/708)
-   Miri check by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/704](https://redirect.github.com/bincode-org/bincode/pull/704)
-   Fixed broken miri CI script by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/712](https://redirect.github.com/bincode-org/bincode/pull/712)
-   Fixed a warning in a derive test that would cause CI to fail by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/716](https://redirect.github.com/bincode-org/bincode/pull/716)
-   Put clarifying language in migration guide regarding the serde:: and non-serde:: paths by [@&#8203;mcclure](https://redirect.github.com/mcclure) in [https://github.com/bincode-org/bincode/pull/715](https://redirect.github.com/bincode-org/bincode/pull/715)
-   Fixed new clippy lints by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/721](https://redirect.github.com/bincode-org/bincode/pull/721)
-   Update virtue requirement from 0.0.16 to 0.0.17 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/bincode-org/bincode/pull/719](https://redirect.github.com/bincode-org/bincode/pull/719)
-   Add prerelease warning to readme.md by [@&#8203;xaocon](https://redirect.github.com/xaocon) in [https://github.com/bincode-org/bincode/pull/728](https://redirect.github.com/bincode-org/bincode/pull/728)
-   Update virtue requirement from 0.0.17 to 0.0.18 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/bincode-org/bincode/pull/731](https://redirect.github.com/bincode-org/bincode/pull/731)
-   Fix typo in spec.md by [@&#8203;DragonDev1906](https://redirect.github.com/DragonDev1906) in [https://github.com/bincode-org/bincode/pull/730](https://redirect.github.com/bincode-org/bincode/pull/730)
-   Implement basic traits for `Compat` and `BorrowCompat` by [@&#8203;skibon02](https://redirect.github.com/skibon02) in [https://github.com/bincode-org/bincode/pull/734](https://redirect.github.com/bincode-org/bincode/pull/734)
-   chore typo fix README.md by [@&#8203;Hack666r](https://redirect.github.com/Hack666r) in [https://github.com/bincode-org/bincode/pull/737](https://redirect.github.com/bincode-org/bincode/pull/737)
-   Fix CI and clippy by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/747](https://redirect.github.com/bincode-org/bincode/pull/747)
-   Document making serde an optional dependency by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/746](https://redirect.github.com/bincode-org/bincode/pull/746)
-   Finally got around to updating the spec based on feedback by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/741](https://redirect.github.com/bincode-org/bincode/pull/741)
-   Expose types implementing `serde::Serializer` and `Deserializer` by [@&#8203;fjarri](https://redirect.github.com/fjarri) in [https://github.com/bincode-org/bincode/pull/729](https://redirect.github.com/bincode-org/bincode/pull/729)
-   make serde decode api consistent by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/748](https://redirect.github.com/bincode-org/bincode/pull/748)
-   Decode context by [@&#8203;ZoeyR](https://redirect.github.com/ZoeyR) in [https://github.com/bincode-org/bincode/pull/749](https://redirect.github.com/bincode-org/bincode/pull/749)
-   2.0.0 stable by [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) in [https://github.com/bincode-org/bincode/pull/742](https://redirect.github.com/bincode-org/bincode/pull/742)

#### New Contributors

-   [@&#8203;luben](https://redirect.github.com/luben) made their first contribution in [https://github.com/bincode-org/bincode/pull/342](https://redirect.github.com/bincode-org/bincode/pull/342)
-   [@&#8203;apgoetz](https://redirect.github.com/apgoetz) made their first contribution in [https://github.com/bincode-org/bincode/pull/373](https://redirect.github.com/bincode-org/bincode/pull/373)
-   [@&#8203;mbr](https://redirect.github.com/mbr) made their first contribution in [https://github.com/bincode-org/bincode/pull/346](https://redirect.github.com/bincode-org/bincode/pull/346)
-   [@&#8203;manuthambi](https://redirect.github.com/manuthambi) made their first contribution in [https://github.com/bincode-org/bincode/pull/360](https://redirect.github.com/bincode-org/bincode/pull/360)
-   [@&#8203;ehooi](https://redirect.github.com/ehooi) made their first contribution in [https://github.com/bincode-org/bincode/pull/392](https://redirect.github.com/bincode-org/bincode/pull/392)
-   [@&#8203;saethlin](https://redirect.github.com/saethlin) made their first contribution in [https://github.com/bincode-org/bincode/pull/337](https://redirect.github.com/bincode-org/bincode/pull/337)
-   [@&#8203;VictorKoenders](https://redirect.github.com/VictorKoenders) made their first contribution in [https://github.com/bincode-org/bincode/pull/409](https://redirect.github.com/bincode-org/bincode/pull/409)
-   [@&#8203;Seppel3210](https://redirect.github.com/Seppel3210) made their first contribution in [https://github.com/bincode-org/bincode/pull/423](https://redirect.github.com/bincode-org/bincode/pull/423)
-   [@&#8203;andrenth](https://redirect.github.com/andrenth) made their first contribution in [https://github.com/bincode-org/bincode/pull/430](https://redirect.github.com/bincode-org/bincode/pull/430)
-   [@&#8203;songzhi](https://redirect.github.com/songzhi) made their first contribution in [https://github.com/bincode-org/bincode/pull/446](https://redirect.github.com/bincode-org/bincode/pull/446)
-   [@&#8203;Popog](https://redirect.github.com/Popog) made their first contribution in [https://github.com/bincode-org/bincode/pull/456](https://redirect.github.com/bincode-org/bincode/pull/456)
-   [@&#8203;ygf11](https://redirect.github.com/ygf11) made their first contribution in [https://github.com/bincode-org/bincode/pull/457](https://redirect.github.com/bincode-org/bincode/pull/457)
-   [@&#8203;5225225](https://redirect.github.com/5225225) made their first contribution in [https://github.com/bincode-org/bincode/pull/465](https://redirect.github.com/bincode-org/bincode/pull/465)
-   [@&#8203;poljar](https://redirect.github.com/poljar) made their first contribution in [https://github.com/bincode-org/bincode/pull/492](https://redirect.github.com/bincode-org/bincode/pull/492)
-   [@&#8203;ppamorim](https://redirect.github.com/ppamorim) made their first contribution in [https://github.com/bincode-org/bincode/pull/500](https://redirect.github.com/bincode-org/bincode/pull/500)
-   [@&#8203;BRA1L0R](https://redirect.github.com/BRA1L0R) made their first contribution in [https://github.com/bincode-org/bincode/pull/507](https://redirect.github.com/bincode-org/bincode/pull/507)
-   [@&#8203;ajeetdsouza](https://redirect.github.com/ajeetdsouza) made their first contribution in [https://github.com/bincode-org/bincode/pull/525](https://redirect.github.com/bincode-org/bincode/pull/525)
-   [@&#8203;bronsonp](https://redirect.github.com/bronsonp) made their first contribution in [https://github.com/bincode-org/bincode/pull/529](https://redirect.github.com/bincode-org/bincode/pull/529)
-   [@&#8203;xobs](https://redirect.github.com/xobs) made their first contribution in [https://github.com/bincode-org/bincode/pull/533](https://redirect.github.com/bincode-org/bincode/pull/533)
-   [@&#8203;cronokirby](https://redirect.github.com/cronokirby) made their first contribution in [https://github.com/bincode-org/bincode/pull/542](https://redirect.github.com/bincode-org/bincode/pull/542)
-   [@&#8203;maciejhirsz](https://redirect.github.com/maciejhirsz) made their first contribution in [https://github.com/bincode-org/bincode/pull/552](https://redirect.github.com/bincode-org/bincode/pull/552)
-   [@&#8203;SabrinaJewson](https://redirect.github.com/SabrinaJewson) made their first contribution in [https://github.com/bincode-org/bincode/pull/562](https://redirect.github.com/bincode-org/bincode/pull/562)
-   [@&#8203;trevyn](https://redirect.github.com/trevyn) made their first contribution in [https://github.com/bincode-org/bincode/pull/580](https://redirect.github.com/bincode-org/bincode/pull/580)
-   [@&#8203;gz](https://redirect.github.com/gz) made their first contribution in [https://github.com/bincode-org/bincode/pull/583](https://redirect.github.com/bincode-org/bincode/pull/583)
-   [@&#8203;Speedy37](https://redirect.github.com/Speedy37) made their first contribution in [https://github.com/bincode-org/bincode/pull/585](https://redirect.github.com/bincode-org/bincode/pull/585)
-   [@&#8203;MrGVSV](https://redirect.github.com/MrGVSV) made their first contribution in [https://github.com/bincode-org/bincode/pull/586](https://redirect.github.com/bincode-org/bincode/pull/586)
-   [@&#8203;odysa](https://redirect.github.com/odysa) made their first contribution in [https://github.com/bincode-org/bincode/pull/602](https://redirect.github.com/bincode-org/bincode/pull/602)
-   [@&#8203;DavidKorczynski](https://redirect.github.com/DavidKorczynski) made their first contribution in [https://github.com/bincode-org/bincode/pull/604](https://redirect.github.com/bincode-org/bincode/pull/604)
-   [@&#8203;bigbass1997](https://redirect.github.com/bigbass1997) made their first contribution in [https://github.com/bincode-org/bincode/pull/630](https://redirect.github.com/bincode-org/bincode/pull/630)
-   [@&#8203;elpiel](https://redirect.github.com/elpiel) made their first contribution in [https://github.com/bincode-org/bincode/pull/634](https://redirect.github.com/bincode-org/bincode/pull/634)
-   [@&#8203;dependabot](https://redirect.github.com/dependabot) made their first contribution in [https://github.com/bincode-org/bincode/pull/638](https://redirect.github.com/bincode-org/bincode/pull/638)
-   [@&#8203;dullbananas](https://redirect.github.com/dullbananas) made their first contribution in [https://github.com/bincode-org/bincode/pull/635](https://redirect.github.com/bincode-org/bincode/pull/635)
-   [@&#8203;aegroto](https://redirect.github.com/aegroto) made their first contribution in [https://github.com/bincode-org/bincode/pull/670](https://redirect.github.com/bincode-org/bincode/pull/670)
-   [@&#8203;CXWorks](https://redirect.github.com/CXWorks) made their first contribution in [https://github.com/bincode-org/bincode/pull/683](https://redirect.github.com/bincode-org/bincode/pull/683)
-   [@&#8203;shahn](https://redirect.github.com/shahn) made their first contribution in [https://github.com/bincode-org/bincode/pull/681](https://redirect.github.com/bincode-org/bincode/pull/681)
-   [@&#8203;richardpringle](https://redirect.github.com/richardpringle) made their first contribution in [https://github.com/bincode-org/bincode/pull/684](https://redirect.github.com/bincode-org/bincode/pull/684)
-   [@&#8203;mzachar](https://redirect.github.com/mzachar) made their first contribution in [https://github.com/bincode-org/bincode/pull/686](https://redirect.github.com/bincode-org/bincode/pull/686)
-   [@&#8203;jfsulliv](https://redirect.github.com/jfsulliv) made their first contribution in [https://github.com/bincode-org/bincode/pull/698](https://redirect.github.com/bincode-org/bincode/pull/698)
-   [@&#8203;mkeeter](https://redirect.github.com/mkeeter) made their first contribution in [https://github.com/bincode-org/bincode/pull/702](https://redirect.github.com/bincode-org/bincode/pull/702)
-   [@&#8203;Vrtgs](https://redirect.github.com/Vrtgs) made their first contribution in [https://github.com/bincode-org/bincode/pull/708](https://redirect.github.com/bincode-org/bincode/pull/708)
-   [@&#8203;mcclure](https://redirect.github.com/mcclure) made their first contribution in [https://github.com/bincode-org/bincode/pull/715](https://redirect.github.com/bincode-org/bincode/pull/715)
-   [@&#8203;xaocon](https://redirect.github.com/xaocon) made their first contribution in [https://github.com/bincode-org/bincode/pull/728](https://redirect.github.com/bincode-org/bincode/pull/728)
-   [@&#8203;DragonDev1906](https://redirect.github.com/DragonDev1906) made their first contribution in [https://github.com/bincode-org/bincode/pull/730](https://redirect.github.com/bincode-org/bincode/pull/730)
-   [@&#8203;skibon02](https://redirect.github.com/skibon02) made their first contribution in [https://github.com/bincode-org/bincode/pull/734](https://redirect.github.com/bincode-org/bincode/pull/734)
-   [@&#8203;Hack666r](https://redirect.github.com/Hack666r) made their first contribution in [https://github.com/bincode-org/bincode/pull/737](https://redirect.github.com/bincode-org/bincode/pull/737)
-   [@&#8203;fjarri](https://redirect.github.com/fjarri) made their first contribution in [https://github.com/bincode-org/bincode/pull/729](https://redirect.github.com/bincode-org/bincode/pull/729)

**Full Changelog**: https://github.com/bincode-org/bincode/compare/v1.3.1...v2.0.0

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MC4xMS4xOCIsInVwZGF0ZWRJblZlciI6IjQwLjExLjE4IiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-05-19 06:17_

---

_Comment by @MichaReiser on 2025-05-19 06:56_

I ran the cpython benchmark and the wall time remains unchanged.

---

_Merged by @MichaReiser on 2025-05-19 06:57_

---

_Closed by @MichaReiser on 2025-05-19 06:57_

---

_Branch deleted on 2025-05-19 06:57_

---

_Comment by @github-actions[bot] on 2025-05-19 06:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
