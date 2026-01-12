```yaml
number: 13164
title: Update EmbarkStudios/cargo-deny-action action to v2
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/embarkstudios-cargo-deny-action-2.x
created_at: 2025-04-28T02:46:31Z
updated_at: 2025-04-28T08:57:01Z
url: https://github.com/astral-sh/uv/pull/13164
synced_at: 2026-01-12T16:10:34Z
```

# Update EmbarkStudios/cargo-deny-action action to v2

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [EmbarkStudios/cargo-deny-action](https://redirect.github.com/EmbarkStudios/cargo-deny-action) | action | major | `v1` -> `v2.0.11` |

---

### Release Notes

<details>
<summary>EmbarkStudios/cargo-deny-action (EmbarkStudios/cargo-deny-action)</summary>

### [`v2.0.11`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v2.0.11)

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v2.0.10...v2.0.11)

#### \[0.18.2] - 2025-03-10

##### Added

-   [PR#753](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/753) resolved [#&#8203;752](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/752) by adding back the `advisories.unmaintained` config option. See the [docs](https://embarkstudios.github.io/cargo-deny/checks/advisories/cfg.html#the-unmaintained-field-optional) for how it can be used. The default matches the current behavior, which is to error on any `unmaintained` advisory, but adding `unmaintained = "workspace"` to the `[advisories]` table will mean unmaintained advisories will only error if the crate is a direct dependency of your workspace.

#### \[0.18.1] - 2025-02-27

##### Fixed

-   [PR#749](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/749) updated `krates` to pull in the fix for [EmbarkStudios/krates#100](https://redirect.github.com/EmbarkStudios/krates/issues/100).

### [`v2.0.10`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v2.0.10)

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v2.0.9...v2.0.10)

-   [PR#96](https://redirect.github.com/EmbarkStudios/cargo-deny-action/pull/96) resolved [#&#8203;94](https://redirect.github.com/EmbarkStudios/cargo-deny-action/issues/94) by switching to the directory the manifest path is located in and doing `rustup toolchain install` if `rustup show` failed due to any reason

### [`v2.0.9`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v2.0.9): Release 2.0.9 - cargo-deny 0.18.0

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v2.0.8...v2.0.9)

-   [`d8395c1`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/commit/d8395c1) removed the rustup update.

### [`v2.0.8`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v2.0.8)

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v2.0.7...v2.0.8)

-   [PR#93](https://redirect.github.com/EmbarkStudios/cargo-deny-action/pull/93) pins to a hash instead of tag, avoiding future breakage from eg. rustup changes.

### [`v2.0.7`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v2.0.7): Release 2.0.7 - cargo-deny 0.18.0

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v2.0.6...v2.0.7)

-   [PR#92](https://redirect.github.com/EmbarkStudios/cargo-deny-action/pull/92) fixed an issue introduced by the latest rustup release.

### [`v2.0.6`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v2.0.6): Release 2.0.6 - cargo-deny 0.18.0

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v2.0.5...v2.0.6)

##### Changed

-   [PR#746](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/746) changed the directory naming of advisory databases, [again](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/745), so the name uses the last path component and a different, but also stable, hashing algorithm. Eg. the default `https://github.com/rustsec/advisory-db` will now be placed in `$CARGO_HOME/advisory-dbs/advisory-db-3157b0e258782691`.
-   [PR#746](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/746) changed the MSRV to 1.85.0 and uses edition 2024.

##### Fixed

-   [PR#746](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/746) fixes an issue when using cargo 1.85.0 where source urls were not being properly assigned to crates.io due to the constant being used no longer matching the new path used in cargo 1.85.0 causing eg. workspace dependency checks to fail.

### [`v2.0.5`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v2.0.5): Release 2.0.5 - cargo-deny 0.17.0

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v2.0.4...v2.0.5)

##### Changed

-   [PR#745](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/745) updated `tame-index` to [0.18.0](https://redirect.github.com/EmbarkStudios/tame-index/releases/tag/0.18.0) so that cargo 1.85.0 is transparently supported along with older cargo versions.
-   [PR#745](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/745) now uses the same stable hashing as cargo 1.85.0 for the advisory databases, which changes their path, but will notably now be the same across all host platforms.

### [`v2.0.4`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v2.0.4): Release 2.0.4 - cargo-deny 0.16.3

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v2.0.3...v2.0.4)

-   Update base image to rust 1.83.0 so that version 4 lockfiles are supported with no config changes

### [`v2.0.3`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v2.0.3): Release 2.0.3 - cargo-deny 0.16.3

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v2.0.2...v2.0.3)

##### Changed

-   [PR#721](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/721) updated `rust-version` to 1.81.0 to accurately reflect the minimum rust version required to compile, resolving [#&#8203;720](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/720).
-   [PR#722](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/722) updated the SPDX license list to 3.25.0.

##### Fixed

-   [PR#726](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/726) resolved [#&#8203;725](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/725) by adding the `unnecessary-skip` diagnostic, emitted when there is a `skip` configured for a crate that only has one version in the graph.

### [`v2.0.2`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v2.0.2): Release 2.0.2 - cargo-deny 0.16.2

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v2.0.1...v2.0.2)

##### Fixed

-   [PR#703](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/703) resolved [#&#8203;696](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/696) by no longer emitting errors when failing to deserialize deprecated fields, and removed some lingering documentation that wasn't removed in [PR#611](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/611).
-   [PR#719](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/719) updated to `krates` -> 0.17.5, fixing an issue where `cargo-deny` could [panic](https://redirect.github.com/EmbarkStudios/krates/issues/97) due to [incorrectly resolving](https://redirect.github.com/EmbarkStudios/krates/issues/84) features for different versions of the same crate referenced by a single crate.
-   [PR#719](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/719) resolved [#&#8203;706](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/706) by removing a warning issued when users use ignored scheme modifiers for source urls.
-   [PR#719](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/719) resolved [#&#8203;718](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/718) by updating the book with missing arguments.

##### Added

-   [PR#715](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/715) resolved [#&#8203;714](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/714) by adding support for Edition 2024. Thanks [@&#8203;kpcyrd](https://redirect.github.com/kpcyrd)!
-   [PR#710](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/710) resolved [#&#8203;708](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/708) by allowing for unpublished workspace crates to be excluded from the dependency graph that checks are run against, either via the `--exclude-unpublished` CLI argument or the `graph.exclude-unpublished` config field. Thanks [@&#8203;Tastaturtaste](https://redirect.github.com/Tastaturtaste)!

##### Changed

-   [PR#711](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/711) updated `goblin` -> 0.9.2
-   [PR#713](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/713) updated various crates, notably `rustsec` -> 0.30.

### [`v2.0.1`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v2.0.1): Release 2.0.1 - cargo-deny 0.16.1

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v2...v2.0.1)

##### Fixed

-   [PR#691](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/691) fixed an issue where workspace dependencies that used the current dir '.' path component would incorrectly trigger the `unused-workspace-dependency` lint.

### [`v2.0.0`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v2.0.0): Release 2.0.0 - cargo-deny 0.16.0

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.6.3...v2)

#### `Action`

##### Added

-   [PR#78](https://redirect.github.com/EmbarkStudios/cargo-deny-action/pull/78) added SSH support, thanks [@&#8203;nagua](https://redirect.github.com/nagua)!

##### Changed

-   This release includes breaking changes in cargo-deny, so this release begins the `v2` tag, using `v1` will be stable but not follow future `cargo-deny` releases.

#### `cargo-deny`

##### Removed

-   [PR#681](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/681) finished the deprecation introduced in [PR#611](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/611), making the usage of the deprecated fields into errors.

##### `[advisories]`

The following fields have all been removed in favor of denying all advisories by default. To ignore an advisory the [`ignore`](https://embarkstudios.github.io/cargo-deny/checks/advisories/cfg.html#the-ignore-field-optional) field can be used as before.

-   `vulnerability` - Vulnerability advisories are now `deny` by default
-   `unmaintained` - Unmaintained advisories are now `deny` by default
-   `unsound` - Unsound advisories are now `deny` by default
-   `notice` - Notice advisories are now `deny` by default
-   `severity-threshold` - The severity of vulnerabilities is now irrelevant

##### `[licenses]`

The following fields have all been removed in favor of denying all licenses that are not explicitly allowed via either [`allow`](https://embarkstudios.github.io/cargo-deny/checks/licenses/cfg.html#the-allow-field-optional) or [`exceptions`](https://embarkstudios.github.io/cargo-deny/checks/licenses/cfg.html#the-exceptions-field-optional).

-   `unlicensed` - Crates whose license(s) cannot be confidently determined are now always errors. The [`clarify`](https://embarkstudios.github.io/cargo-deny/checks/licenses/cfg.html#the-clarify-field-optional) field can be used to help cargo-deny determine the license.
-   `allow-osi-fsf-free` - The OSI/FSF Free attributes are now irrelevant, only whether it is explicitly allowed.
-   `copyleft` - The copyleft attribute is now irrelevant, only whether it is explicitly allowed.
-   `default` - The default is now `deny`.
-   `deny` - All licenses are now denied by default, this field added nothing.

##### Changed

-   [PR#685](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/685) follows up on [PR#673](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/673), moving the fields that were added to their own separate [`bans.workspace-dependencies`](https://embarkstudios.github.io/cargo-deny/checks/bans/cfg.html#the-workspace-dependencies-field-optional) section. This is an unannounced breaking change but is fairly minor and 0.15.0 was never released on github actions so the amount of people affected by this will be (hopefully) small. This also makes the workspace duplicate detection off by default since the field is optional, *but* makes it so that if not specified workspace duplicates are now `deny` instead of `warn`.

##### Fixed

-   [PR#685](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/685) resolved [#&#8203;682](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/682) by adding the `include-path-dependencies` field, allowing path dependencies to be ignored if it is `false`.

### [`v1.6.3`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.6.3): Release 1.6.3 - cargo-deny 0.14.21

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.6.2...v1.6.3)

##### Fixed

-   [PR#643](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/643) resolved [#&#8203;629](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/629) by making the hosted git (github, gitlab, bitbucket) org/user name comparison case-insensitive. Thanks [@&#8203;pmnlla](https://redirect.github.com/pmnlla)!
-   [PR#649](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/649) fixed an issue where depending on the same crate multiple times by using different `cfg()/triple` targets could cause features to be resolved incorrectly and thus crates to be not pulled into the graph used for checking.

#### \[0.14.20] - 2024-03-23

##### Fixed

-   [PR#642](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/642) resolved [#&#8203;641](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/641) by pinning `gix-transport` (and its unique dependencies) to 0.41.2 as a workaround for `cargo install` not using the lockfile. See [this issue](https://redirect.github.com/Byron/gitoxide/issues/1328) for more information.

### [`v1.6.2`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.6.2): Release 1.6.2 - cargo-deny 0.14.19

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.6.1...v1.6.2)

##### Changed

-   [PR#639](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/639) updated tame-index to avoid an error if you don't used `--locked`.

#### \[0.14.18] - 2024-03-21

##### Fixed

-   [PR#638](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/638) resolved [#&#8203;636](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/636) by updating `krates`.

#### \[0.14.17] - 2024-03-17

##### Changed

-   [PR#631](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/631) improved the diagnostic for when the yank check fails due to some issue with retrieving or reading the index information.
-   [PR#633](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/633) updated `gix` -> 0.60.

### [`v1.6.1`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.6.1)

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.6.0...v1.6.1)

##### Fixed

-   [PR#626](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/626) resolved [#&#8203;625](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/625) by explicitly checking that a license identified as Pixar was actually (probably) the Pixar license, instead of a normal Apache-2.0 license.

### [`v1.6.0`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.6.0)

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.5.15...v1.6.0)

#### action changes

-   Color output is now always enabled so that colors show up in the action output.

#### 0.14.15

##### Added

-   [PR#618](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/618) added metadata notes to diagnostics when a license is rejected, as well as removing span information for accepted licenses unless the log level is `info` or higher to make the diagnostic clearer by default.

#### 0.14.14

##### Fixed

-   [PR#617](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/617) resolved [#&#8203;576](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/576) by updating the SPDX license list to 3.23.

#### 0.14.13

##### Fixed

-   [PR#615](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/615) fixed an issue introduced in [PR#605](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/605) where the various `bans` diagnostic codes could not have their lint level changed via the CLI. It also introduced the `deprecated` diagnostic code.

#### 0.14.12

##### Changed

-   [PR#605](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/605) did a major refactor of configuration, both how it is deserialized and changing (hopefully improving) many options.
-   [PR#605](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/605) moved `targets`, `exclude`, `all-features`, `features`, `no-default-features`, and `exclude` into the `[graph]` table.
-   [PR#605](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/605) moved `feature-depth` into the `[output]` table.

##### Added

-   [PR#613](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/613) added support for [basic shell expansion](https://embarkstudios.github.io/cargo-deny/checks/advisories/cfg.html#the-db-path-field-optional) to `advisories.db-path`, which expands support beyond just `~` to include environment variable expansion.

##### Fixed

-   [PR#601](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/601) resolved [#&#8203;600](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/600) by outputting the correct spans when a license was both allowed and denied.
-   [PR#605](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/605) resolved [#&#8203;264](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/264) be replacing `toml` and `serde` with `toml-span`.
-   [PR#605](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/605) resolved [#&#8203;539](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/539) by simplifying the very common `name = "<crate_name>", version = "<requirements>"` used to target specific crates into either a plain [package spec string](https://embarkstudios.github.io/cargo-deny/checks/cfg.html#string-format) or the simpler `crate = "<package spec>"`.
-   [PR#605](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/605) resolved [#&#8203;578](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/578) by adding a `reason = "<reason>"` field to *many* fields within the configuration that are provided in diagnostics. `[bans.deny]` also has an additional `use-instead = "<url/crate_name>"`. [PR#610](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/610) did this for the `advisories.ignore` field.
-   [PR#605](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/605) resolved [#&#8203;579](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/579) by allowing yanked crates to be ignored by specifying a [PackageSpec](https://embarkstudios.github.io/cargo-deny/checks/cfg.html#package-specs) in the `[advisories.ignore]` array.

##### Deprecated

-   [PR#606](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/606) and [PR#611](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/611) together deprecated several fields listed below. See [PR#611](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/611) for how to change your config to opt-in to the new behavior that will become the default when the deprecated fields are removed in a future minor version.
    -   `[advisories]`
        -   `vulnerability`
        -   `unmaintained`
        -   `unsound`
        -   `notice`
        -   `severity-threshold`
    -   `[licenses]`
        -   `unlicensed`
        -   `allow-osi-fsf-free`
        -   `copyleft`
        -   `default`
        -   `deny`

### [`v1.5.15`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.5.15): Release 1.5.15 - cargo-deny 0.14.11

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.5.14...v1.5.15)

##### Fixed

-   Resolved [https://github.com/EmbarkStudios/cargo-deny-action/issues/71](https://redirect.github.com/EmbarkStudios/cargo-deny-action/issues/71) that was introduced in the previous release.

### [`v1.5.14`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.5.14): Release 1.5.14 - cargo-deny 0.14.11

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.5.13...v1.5.14)

##### Added

-   Added the `manifest-path` key as a shorthand for doing `arguments: --manifest-path <path>`

### [`v1.5.13`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.5.13): Release 1.5.13 - cargo-deny 0.14.11

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.5.12...v1.5.13)

##### Fixed

-   [PR#599](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/599) resolved [#&#8203;488](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/488) by treating git and path sources differently. Thanks [@&#8203;kpreid](https://redirect.github.com/kpreid)!

### [`v1.5.12`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.5.12): Release 1.5.12 - cargo-deny 0.14.10

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.5.11...v1.5.12)

##### Fixed

-   [PR#596](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/596) updated `krates` *again* to pull in [krates#77](https://redirect.github.com/EmbarkStudios/krates/pull/77).

### [`v1.5.11`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.5.11): Release 1.5.11 - cargo-deny 0.14.9

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.5.10...v1.5.11)

##### Fixed

-   [PR#594](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/594) updated `krates` *again* to pull in [krates#75](https://redirect.github.com/EmbarkStudios/krates/pull/75).

### [`v1.5.10`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.5.10): Release 1.5.10 - cargo-deny 0.14.8

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.5.9...v1.5.10)

##### Fixed

-   [PR#592](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/592) updated `krates` *again* to pull in [krates#73](https://redirect.github.com/EmbarkStudios/krates/pull/73).

### [`v1.5.9`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.5.9): Release 1.5.9 - cargo-deny 0.14.7

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.5.8...v1.5.9)

##### Fixed

-   [PR#591](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/591) updated `krates` *again* to pull in [krates#71](https://redirect.github.com/EmbarkStudios/krates/pull/71).

### [`v1.5.8`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.5.8): Release 1.5.8 - cargo-deny 0.14.6

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.5.7...v1.5.8)

##### Fixed

-   [PR#590](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/590) updated `krates` to fix an issue with crates that directly have a dependency on 2 or more versions of the same crate.

##### Added

-   [PR#590](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/590) resolved [#&#8203;405](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/405) by emitting warnings when a `wrapper` crate for a banned crate does not have a dependency on that crate.

##### Changed

-   [PR#591](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/591) updated `gix` and `tame-index`.

### [`v1.5.7`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.5.7): Release 1.5.7 - cargo-deny 0.14.5

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.5.6...v1.5.7)

##### Fixed

-   [PR#588](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/588) resolved an issue introduced in \[0.14.4] where features that reference dev-only dependencies in non-workspace crates would cause a [panic](https://redirect.github.com/EmbarkStudios/krates/issues/66).

### [`v1.5.6`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.5.6): Release 1.5.6 - cargo-deny 0.14.4

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.5.5...v1.5.6)

##### Fixed

-   [PR#586](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/586) resolved 2 issues with crate graph creation, see [krates#60](https://redirect.github.com/EmbarkStudios/krates/issues/60) and [krates#64](https://redirect.github.com/EmbarkStudios/krates/issues/64) for more details.

### [`v1.5.5`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.5.5): Release 1.5.5 - cargo-deny 0.14.2

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.5.4...v1.5.5)

##### Added

-   [PR#545](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/545) added the ability to specify additional license exceptions via [additional configuration files](https://embarkstudios.github.io/cargo-deny/checks/licenses/cfg.html#additional-exceptions-configuration-file).
-   [PR#549](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/549) added the [`bans.build`](https://embarkstudios.github.io/cargo-deny/checks/bans/cfg.html#the-build-field-optional) configuration option, opting in to checking for [file extensions](https://embarkstudios.github.io/cargo-deny/checks/bans/cfg.html#the-script-extensions-field-optional), [native executables](https://embarkstudios.github.io/cargo-deny/checks/bans/cfg.html#the-executables-field-optional), and [interpreted scripts](https://embarkstudios.github.io/cargo-deny/checks/bans/cfg.html#the-interpreted-field-optional). This resolved [#&#8203;43](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/43).

##### Changed

-   [PR#557](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/557) introduced changes to how [`dev-dependencies`](https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html#development-dependencies) are handled. By default, crates that are only used as dev-dependencies (ie, there are no normal nor build dependency edges linking them to other crates) will no longer be considered when checking for [`multiple-versions`](https://embarkstudios.github.io/cargo-deny/checks/bans/cfg.html#the-multiple-versions-field-optional) violations. This can be re-enabled via the [`bans.multiple-versions-include-dev`](https://embarkstudios.github.io/cargo-deny/checks/bans/cfg.html#the-multiple-versions-include-dev-field-optional) config field. Additionally, licenses are no longer checked for `dev-dependencies`, but can be re-enabled via [`licenses.include-dev`](https://embarkstudios.github.io/cargo-deny/checks/licenses/cfg.html#the-include-dev-field-optional) the config field. `dev-dependencies` can also be completely disabled altogether, but this applies to all checks, including `advisories` and `sources`, so is not enabled by default. This behavior can be enabled by using the [`exclude-dev`](https://embarkstudios.github.io/cargo-deny/checks/cfg.html#the-exclude-dev-field-optional) field, or the `--exclude-dev` command line flag. This change resolved [#&#8203;322](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/322), [#&#8203;329](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/329), [#&#8203;413](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/413) and [#&#8203;497](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/497).

##### Fixed

-   [PR#549](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/549) fixed [#&#8203;548](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/548) by correctly locating cargo registry indices from an git ssh url.
-   [PR#549](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/549) fixed [#&#8203;552](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/552) by correctly handling signal interrupts and removing the advisory-dbs lock file.
-   [PR#549](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/549) fixed [#&#8203;553](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/553) by adding the `native-certs` feature flag that can enable the OS native certificate store.

##### Deprecated

-   [PR#549](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/549) moved `bans.allow-build-scripts` to [`bans.build.allow-build-scripts`](https://embarkstudios.github.io/cargo-deny/checks/bans/cfg.html#the-allow-build-scripts-field-optional). `bans.allow-build-scripts` is still supported, but emits a warning.

### [`v1.5.4`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.5.4): Release 1.5.4 - cargo-deny 0.14.0

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.5.3...v1.5.4)

Updated the cargo version to 1.71.0 which should give significant improvements to run times due to using the crates.io sparse index instead of the old git index.

### [`v1.5.3`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.5.3): Release 1.5.3 - cargo-deny 0.14.0

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.5.2...v1.5.3)

##### Changed

-   [PR#520] resolved [#&#8203;522](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/522) by completely removing all dependencies upon `git2` and `openssl`. This was done by transitioning from `git2` -> `gix` for all git operations, both directly in this crate, as well as replacing [`crates-index`](https://redirect.github.com/frewsxcv/rust-crates-index) with [`tame-index`](https://redirect.github.com/EmbarkStudios/tame-index).
-   [PR#520] bumped the MSRV from `1.65.0` -> `1.70.0`
-   [PR#523](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/523) added "(try `cargo update -p <crate_name>`)" when an advisory is detected for a crate. Thanks [@&#8203;Victor-N-Suadicani](https://redirect.github.com/Victor-N-Suadicani)!

##### Fixed

-   [PR#520] resolved [#&#8203;361](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/361) by printing output when a fetch is being performed to clarify what is taking time.
-   [PR#520] (possibly) resolved [#&#8203;435](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/435) by switching all git operations from `git2` to `gix`.
-   [PR#520] resolved [#&#8203;439](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/439) by using minimal refspecs for cloning and fetching all remote git repositories (indices or advisory databases) where only the remote HEAD is needed to update the local repository, regardless of the default remote branch pointed to by HEAD.
-   [PR#520] resolved [#&#8203;446](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/446) by ensuring (and testing) that crates from non-registry sources are not checked for advisories, eg. in the case that a local crate is named and versioned the same as a crate from crates.io that has an advisory that affects it.
-   [PR#520] resolved [#&#8203;515](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/515) by always opening the correct registry index based upon the environment.
-   [PR#531](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/531) resolved [#&#8203;210](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/210) by adding `osi` and `fsf` options to `licenses.allow-osi-fsf-free`. Thanks [@&#8203;zkxs](https://redirect.github.com/zkxs)!
-   [PR#533](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/533) resolved [#&#8203;521](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/521) and [#&#8203;524](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/524) by allowing clarifications to add files that are used to verify the license information is up to date, rather than needing to match one of the license files that was discovered.
-   [PR#534](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/534) resolved [#&#8203;479](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/479) by improving how advisory databases are cloned and/or fetched, notably each database now uses `gix`'s [file-based locking](https://docs.rs/gix-lock/7.0.2/gix_lock/struct.Marker.html#method.acquire_to_hold_resource) to ensure that only one process has mutable access to an advisory database repo at a time.

##### Removed

-   [PR#520] removed all features, notably `standalone`. This is due to cargo still being in transition from `git2` -> `gix` and having no way to compiled *without* OpenSSL. Once cargo is a better state with regards to this we can add back that feature.

[PR#520]: https://redirect.github.com/EmbarkStudios/cargo-deny/pull/520

### [`v1.5.2`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.5.2): Release 1.5.2 - cargo-deny 0.13.9

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.5.1...v1.5.2)

##### Fixed

-   [PR#506](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/506) replaced `atty` (unmaintained) with `is-terminal`. Thanks [@&#8203;tottoto](https://redirect.github.com/tottoto)!
-   [PR#511](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/511) resolved [#&#8203;494](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/494), [#&#8203;507](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/507), and [#&#8203;510](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/510) by fixing up how and when urls are normalized.
-   [PR#512](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/512) resolved [#&#8203;509](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/509) by fixing casing of the root configuration keys.
-   [PR#513](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/513) resolved [#&#8203;508](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/508) by correctly using the crates.io sparse index when checking for yanked crates if specified by the user, as well as falling back to the regular git index if the sparse index is not present.

### [`v1.5.1`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.5.1): Release 1.5.1 - cargo-deny 0.13.8

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.5.0...v1.5.1)

##### Added

-   [PR#504](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/504) (though really [PR#365](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/365)) resolved [#&#8203;350](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/350) by adding the `deny-multiple-versions` field to `bans.deny` entries, allowing specific crates to deny multiple versions while allowing/warning on them more generally. Thanks [@&#8203;leops](https://redirect.github.com/leops)!
-   [PR#493](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/493) resolved [#&#8203;437](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/437) by also looking for deny configuration files in `.cargo`. Thanks [@&#8203;DJMcNab](https://redirect.github.com/DJMcNab)!
-   [PR#502](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/502) resolved [#&#8203;500](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/500) by adding initial support for [sparse indices](https://blog.rust-lang.org/inside-rust/2023/01/30/cargo-sparse-protocol.html).

##### Fixed

-   [PR#503](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/503) resolved [#&#8203;498](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/498) by falling back to more lax parsing of the SPDX expression of crate if fails to parse according to the stricter but more correct rules.

### [`v1.5.0`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.5.0): Release 1.5.0 - cargo-deny 0.13.7

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.4.0...v1.5.0)

Update from cargo-deny 0.13.5 to 0.13.7, apparently I missed two releases, that's embarrassing.

#### 0.13.7

##### Fixed

-   [PR#491](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/491) resolved [#&#8203;490](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/490) by building libgit2 from vendored sources instead of relying on potentially outdated packages.

#### 0.13.6

##### Changed

-   [PR#489](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/489) updated dependencies, notably `clap`, `cargo`, and `git2`

##### Added

-   [PR#485](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/485) added this project and repository to our Security Bug Bounty Program and has Private vulnerability reporting enabled. See [`SECURITY.md`](./SECURITY.md) for more details.
-   [PR#487](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/487) added `allow-wildcard-paths`, fixing [#&#8203;488](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/448) by allowing wildcards to be denied, but allowing them for internal, private crates. Thanks [@&#8203;sribich](https://giqthub.com/sribich)!

##### Fixed

-   [PR#489](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/489) fixed an issue where git sources where `branch=master` would be incorrectly categorized as not specifying the branch (ie use HEAD of default branch).

### [`v1.4.0`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.4.0): Release 1.4.0 - cargo-deny 0.13.5

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.3.2...v1.4.0)

##### Changed

-   Updated to cargo-deny 0.13.5

### [`v1.3.2`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.3.2): - cargo-deny 0.12.1

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.3.1...v1.3.2)

##### Added

-   [PR#54](https://redirect.github.com/PR/cargo-deny-action/issues/54) resolved [#&#8203;53](https://redirect.github.com/EmbarkStudios/cargo-deny-action/issues/53) by adding the `credentials` parameter for passing in a private access token to allow cargo to fetch private github repositories. Thanks [@&#8203;danielhaap83](https://redirect.github.com/danielhaap83)!

### [`v1.3.1`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.3.1): - cargo-deny 0.12.1

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.3.0...v1.3.1)

##### Fixed

-   [PR#426](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/426) fixed an oversight in [PR#422](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/422), fully resolving [#&#8203;412](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/412) by allowing both `https` and `ssh` URLs for advisory databases. Thanks [@&#8203;jbg](https://redirect.github.com/jbg)!

##### Changed

-   [PR#427](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/427) updated dependencies.

### [`v1.3.0`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.3.0): - cargo-deny 0.12.0

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.2.17...v1.3.0)

##### Removed

-   [PR#423](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/423) removed the `fix` subcommand. This functionality was far too complicated for far too little benefit.

##### Fixed

-   [PR#420](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/420) resolved [#&#8203;388](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/388) by adding the ability to fetch advisory databases via the `git` CLI. Thanks [@&#8203;danielhaap83](https://redirect.github.com/danielhaap83)!
-   [PR#422](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/422) fixed [#&#8203;380](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/380) and [#&#8203;410](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/410) by updating a few transitive dependencies that use `git2`, as well as removing the usage of `rustsec`'s `git` feature so that we now use `git2 v0.14`, resolving a crash issue in new `libgit2` versions available in eg. rolling release distros such as Arch. This should also make it easier to update and improve git related functionality since more of it is inside cargo-deny itself now.
-   [PR#424](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/424) *really* fixed (there's even a test now!) [#&#8203;384](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/384) by adding each version's reverse dependency graph in the ascending order.

### [`v1.2.17`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.2.17): - cargo-deny 0.11.4

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.2.16...v1.2.17)

#### Changed

-   [PR#51](https://redirect.github.com/EmbarkStudios/cargo-deny-action/pull/51) updated the image to use Rust 1.60.0 by default. Thanks [@&#8203;MarcoIeni](https://redirect.github.com/MarcoIeni)!

### [`v1.2.16`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.2.16): - cargo-deny 0.11.4

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.2.15...v1.2.16)

#### Added

-   [PR#49](https://redirect.github.com/EmbarkStudios/cargo-deny-action/pull/49) added the `command-arguments` option to the action. Thanks [@&#8203;ryo33](https://redirect.github.com/ryo33)!

### [`v1.2.15`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.2.15): - cargo-deny 0.11.3

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.2.14...v1.2.15)

##### Fixed

-   Accidentally change how arguments were forwarded to cargo-deny which broken more complicated invocations

### [`v1.2.14`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.2.14): - cargo-deny 0.11.3

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.2.13...v1.2.14)

##### Added

-   Added `git` to the image, resolving [#&#8203;40](https://redirect.github.com/EmbarkStudios/cargo-deny-action/issues/40)

### [`v1.2.13`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.2.13): - cargo-deny 0.11.3

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.2.12...v1.2.13)

##### Changed

-   Added the `rust-version` github actions variable, allowing you to specify a specific cargo version to use when running cargo-deny, including nightly, or other unstable versions.

### [`v1.2.12`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.2.12): - cargo-deny 0.11.3

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.2.11...v1.2.12)

##### Fixed

-   [PR#407](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/407) resolved [#&#8203;406](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/406) by always checking license exceptions first.

### [`v1.2.11`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.2.10...v1.2.11)

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.2.10...v1.2.11)

### [`v1.2.10`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.2.10): - cargo-deny 0.11.1

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.2.9...v1.2.10)

##### Added

-   [PR#391](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/391) resolved [#&#8203;344](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/344) by adding `[licenses.ignore-sources]` to ignore license checking for crates sourced from 1 or more specified registries. Thanks [@&#8203;ShellWowza](https://redirect.github.com/ShellWowza)!
-   [PR#396](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/396) resolved [#&#8203;366](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/366) by also looking for `.deny.toml` in addition to `deny.toml` if a config file is not specified.

##### Changed

-   [PR#392](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/392) updated all dependencies.

##### Fixed

-   [PR#393](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/393) resolved [#&#8203;371](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/371) by changing the default for version requirements specified in config files to accept all versions, rather than using the almost-but-not-quite default of `*`.
-   [PR#394](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/394) resolved [#&#8203;147](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/147) by ignore *all* private crates, not only the ones in the workspace.
-   [PR#395](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/395) resolved [#&#8203;375](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/375) by fixing a potential infinite loop when using `[bans.skip-tree]`.

### [`v1.2.9`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.2.9): - cargo-deny 0.11.0

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.2.8...v1.2.9)

Fixed image to use proper tag.

### [`v1.2.8`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.2.8): - cargo-deny 0.11.0

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.2.7...v1.2.8)

Updated the cargo version in the image to 1.57.0 to allow for the use of [custom profiles](https://doc.rust-lang.org/cargo/reference/profiles.html#custom-profiles).

### [`v1.2.7`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.2.7): v1.2.6 - cargo-deny 0.11.0

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.2.6...v1.2.7)

#### \[0.11.0] - 2021-12-06

##### Changed

-   [PR#382](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/382) updated dependencies and bumped the Minimum Stable Rust Version to **1.56.1**.

#### \[0.10.3] - 2021-11-22

##### Changed

-   [PR#379](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/379) updated `askalono` which got rid of the `failure` dependency, which was pulling in a lot of additional crates that are now gone.

##### Fixed

-   [PR#379](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/379) fixed [#&#8203;378](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/378) which was an edge case where the `sources` check was executed against a crate that didn't use any crates from crates.io, and the config file was shorter than the crates.io URL.

#### \[0.10.2] - 2021-11-21

##### Fixed

-   [PR#376](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/376) fixed the JSON formatting when using `--format json` output option. Thanks [@&#8203;dnaka91](https://redirect.github.com/dnaka91)!

##### Changed

-   [PR#377](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/377) updated dependencies.

#### \[0.10.1] - 2021-11-10

##### Fixed

-   [PR#347](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/374) resolved [#&#8203;372](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/372) by correcting a slight mistake that resulted in an incorrect hash making cargo-deny unable to lookup index or crate information from the local file system.

#### \[0.10.0] - 2021-10-29

##### Added

-   [PR#353](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/353) resolved [#&#8203;351](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/351) by adding the `sources.private` field to blanket allow git repositories sourced from a particular url.
-   [PR#359](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/359) resolved [#&#8203;341](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/341) and [#&#8203;357](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/357) by adding support for the [`--frozen`, `--locked`, and `--offline`](https://doc.rust-lang.org/cargo/commands/cargo-metadata.html#manifest-options) flags to determine whether network access is allowed, and whether the `Cargo.lock` file can be created and/or modified.
-   [PR#368](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/368) added the `licenses.unused-allowed-license` field to control whether the [L006 - license was not encountered](https://embarkstudios.github.io/cargo-deny/checks/licenses/diags.html#l006---license-was-not-encountered) diagnostic. Thanks [@&#8203;thomcc](https://redirect.github.com/thomcc)!

##### Changed

-   [PR#358](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/358) bumped the Minimum Stable Rust Version to **1.53.0**.
-   [PR#358](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/358) bumped various dependencies, notably `semver` to `1.0.3`.

#### \[0.9.1] - 2021-03-26

##### Changed

-   Updated dependencies

### [`v1.2.6`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.2.6): Release 1.2.6 - cargo-deny 0.9.1

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.2.5...v1.2.6)

##### Changed

-   Updated dependencies

### [`v1.2.5`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.2.5): - cargo-deny 0.9.0

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.2.4...v1.2.5)

##### Changed

-   Updated `krates`, which in turn uses an updated `cargo_metadata` which uses [`camino`](https://docs.rs/camino) for utf-8 paths. Rather than support both vanilla Path/Buf and Utf8Path/Buf, cargo-deny now just uses Utf8Path/Buf, which means that non-utf-8 paths for things like your Cargo.toml manifest or license paths will no longer function. This is a breaking change, that can be reverted if it disruptive for users, but the assumption is that cargo-deny is operating on normal checkouts of rust repositories that are overwhelmingly going to be utf-8 compatible paths.

### [`v1.2.4`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.2.4): Update image

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.2.3...v1.2.4)

Updates the base image to rust 1.50.0 to fix issue if you pin to it via eg rust-toolchain.

### [`v1.2.3`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.2.3): - cargo-deny 0.8.5

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.2.2...v1.2.3)

##### Added

-   [PR#315](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/315) resolved [#&#8203;312](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/312) by adding support for excluding packages in the deny configuration file, in addition to the existing support for the `--exclude` CLI option. Thanks [@&#8203;luser](https://redirect.github.com/luser)!

##### Fixed

-   [PR#318](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/318) fixed [#&#8203;316](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/316) by adding a workaround for crate versions with pre-release identifiers in them that could be erroneously marked as matching advisories in an advisory database. Thanks for reporting this [@&#8203;djc](https://redirect.github.com/djc)!

### [`v1.2.2`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.2.2): - cargo-deny 0.8.4

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.2.1...v1.2.2)

##### Changed

-   Updated dependencies, notably `rustsec`, `crossbeam`\*, and `cargo`.
-   Bumped the Minimum Stable Rust Version to **1.44.1**.

### [`v1.2.1`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/v1.2.1): - cargo-deny 0.8.1

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/1.2.0...v1.2.1)

Updates cargo-deny from 0.7.3 -> 0.8.1

##### Added

-   [PR#238](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/238) resolved [#&#8203;225](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/225) by adding a `wrappers` field to `[bans.deny]` entries, which allows the banned crate to be used only if it is a direct dependency of one of the wrapper crates. Thanks [@&#8203;Stupremee](https://redirect.github.com/Stupremee)!
-   [PR#244](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/244) resolved [#&#8203;69](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/69) by adding support for multiple advisory databases, which will all be checked during the `advisory` check. Thanks [@&#8203;Stupremee](https://redirect.github.com/Stupremee)!
-   [PR#243](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/243) resolved [#&#8203;54](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/54) by adding support for compiling and using `cargo` crate directly via the `standalone` feature. This allows `cargo-deny` to be used without cargo being installed, but it still requires [**rustc**](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/295) to be available. Thanks [@&#8203;Stupremee](https://redirect.github.com/Stupremee)!
-   [PR#275](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/275) resolved [#&#8203;64](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/64) by adding a diagnostic when a user tries to ignore an advisory identifier that doesn't exist in any database.
-   [PR#262](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/262) added the `fix` subcommand, which was added to bring `cargo-deny` to feature parity with `cargo-audit` so that it can take over for `cargo-audit` as the [official frontend](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/194) for the the [RustSec Advisory Database](https://redirect.github.com/RustSec/advisory-db).

##### Changed

-   `advisories.db-url` has been deprecated in favor of `advisories.db-urls` since multiple databses are now supported.
-   `advisories.db-path` is now no longer the directory into which the advisory database is cloned into, but rather a root directory where each unique database is placed in a canonicalized directory similar to how `.cargo/registry/index` directories work.
-   [PR#274](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/274) resolved [#&#8203;115](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/115) by normalizing git urls. Thanks [@&#8203;senden9](https://redirect.github.com/senden9)!

##### Fixed

-   [#&#8203;265](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/265) A transitive dependency (`smol_str`) forced the usage of the latest Rust stable version (1.46) which was unintended. We now state the MSRV in the README and check for it in CI so that changing the MSRV is a conscious decision.
-   [PR#287](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/287) fixed [#&#8203;286](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/286), which could happen if using a git source where the representation differed slightly between the user specified id and the id used for dependencies.
-   [PR#249](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/249) fixed [#&#8203;190](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/190) by printing a different diagnostic for when the path specified for a clarification license file could not be found. Thanks [@&#8203;khodzha](https://redirect.github.com/khodzha)!
-   [PR#297](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/297) fixed a couple of diagnostics to have codes.
-   [PR#296](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/296) resolved [#&#8203;288](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/288) by improving the information in diagnostics pertaining to advisories. Thanks [@&#8203;tomasfarias](https://redirect.github.com/tomasfarias)!

### [`v1.2.0`](https://redirect.github.com/EmbarkStudios/cargo-deny-action/releases/tag/1.2.0): - cargo-deny 0.8.1

[Compare Source](https://redirect.github.com/EmbarkStudios/cargo-deny-action/compare/v1.1.0...1.2.0)

Updates cargo-deny from 0.7.3 -> 0.8.1

##### Added

-   [PR#238](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/238) resolved [#&#8203;225](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/225) by adding a `wrappers` field to `[bans.deny]` entries, which allows the banned crate to be used only if it is a direct dependency of one of the wrapper crates. Thanks [@&#8203;Stupremee](https://redirect.github.com/Stupremee)!
-   [PR#244](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/244) resolved [#&#8203;69](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/69) by adding support for multiple advisory databases, which will all be checked during the `advisory` check. Thanks [@&#8203;Stupremee](https://redirect.github.com/Stupremee)!
-   [PR#243](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/243) resolved [#&#8203;54](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/54) by adding support for compiling and using `cargo` crate directly via the `standalone` feature. This allows `cargo-deny` to be used without cargo being installed, but it still requires [**rustc**](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/295) to be available. Thanks [@&#8203;Stupremee](https://redirect.github.com/Stupremee)!
-   [PR#275](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/275) resolved [#&#8203;64](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/64) by adding a diagnostic when a user tries to ignore an advisory identifier that doesn't exist in any database.
-   [PR#262](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/262) added the `fix` subcommand, which was added to bring `cargo-deny` to feature parity with `cargo-audit` so that it can take over for `cargo-audit` as the [official frontend](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/194) for the the [RustSec Advisory Database](https://redirect.github.com/RustSec/advisory-db).

##### Changed

-   `advisories.db-url` has been deprecated in favor of `advisories.db-urls` since multiple databses are now supported.
-   `advisories.db-path` is now no longer the directory into which the advisory database is cloned into, but rather a root directory where each unique database is placed in a canonicalized directory similar to how `.cargo/registry/index` directories work.
-   [PR#274](https://redirect.github.com/EmbarkStudios/cargo-deny/pull/274) resolved [#&#8203;115](https://redirect.github.com/EmbarkStudios/cargo-deny/issues/115) by normalizing git urls. Thanks [@&#8203;senden9](https://redirect.github.com/senden9)!

##### Fixe

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

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4yNTcuMyIsInVwZGF0ZWRJblZlciI6IjM5LjI1Ny4zIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-04-28 02:46_

---

_Merged by @konstin on 2025-04-28 08:56_

---

_Closed by @konstin on 2025-04-28 08:56_

---

_Branch deleted on 2025-04-28 08:57_

---
