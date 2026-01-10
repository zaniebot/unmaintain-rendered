```yaml
number: 15395
title: "Treat `--upgrade-package` on the command-line as overriding `upgrade = false` in configuration"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/up
created_at: 2025-08-20T16:58:15Z
updated_at: 2025-08-21T15:20:57Z
url: https://github.com/astral-sh/uv/pull/15395
synced_at: 2026-01-10T06:44:33Z
```

# Treat `--upgrade-package` on the command-line as overriding `upgrade = false` in configuration

---

_Pull request opened by @charliermarsh on 2025-08-20 16:58_

## Summary

Right now, if you put `upgrade = false` in a `uv.toml`, then pass `--upgrade-package numpy` on the CLI, we won't upgrade NumPy. This PR fixes that interaction by ensuring that when we "combine", we look at those arguments holistically (i.e., we bundle `upgrade` and `upgrade-package` into a single struct, which then goes through the `.combine` logic), rather than combining `upgrade` and `upgrade-package` independently.

If approved, I then need to add the same thing for `no-build-isolation`, `reinstall`, `no-build`, and `no-binary`.

---

_Label `bug` added by @charliermarsh on 2025-08-20 16:58_

---

_Marked ready for review by @charliermarsh on 2025-08-20 16:58_

---

_Review requested from @konstin by @charliermarsh on 2025-08-20 18:06_

---

_Review requested from @zanieb by @charliermarsh on 2025-08-20 18:06_

---

_Comment by @konstin on 2025-08-21 08:48_

The new behavior makes sense to me! 

---

One thing that I found unexpected is that `--no-upgrade` causes `--upgrade-package` to be silently ignored:

```
$ uv venv -c -p 3.12 && uv-debug pip install numpy==2 && uv-debug pip install --no-upgrade --upgrade-package numpy pandas
Using CPython 3.12.11
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
Resolved 1 package in 8ms
Installed 1 package in 18ms
 + numpy==2.0.0
Resolved 6 packages in 25ms
Installed 5 packages in 17ms
 + pandas==2.3.1
 + python-dateutil==2.9.0.post0
 + pytz==2025.2
 + six==1.17.0
 + tzdata==2025.2
```

This is different from `--no-build` and `--no-binary`:

```
$ uv venv -c -p 3.12 && uv-debug pip install numpy==2 && uv-debug pip install --no-build --no-binary numpy pandas
Using CPython 3.12.11
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
Resolved 1 package in 8ms
Installed 1 package in 15ms
 + numpy==2.0.0
error: the argument '--no-build' cannot be used with '--no-binary <NO_BINARY>'

Usage: uv-debug pip install --no-build <PACKAGE|--requirements <REQUIREMENTS>|--editable <EDITABLE>|--group <GROUP>>

For more information, try '--help'.
```

When reading the code, my main confusion was that the struct isn't fully usable, the upgrade package list is only consider if neither `--upgrade` nor `--no-upgrade` are passed and the upgrade value is `None`. I was struggling to verify that the merge logic is correct, if we had two `Option<UpgradeSelection>`, only `Some` if any upgrade option was passed, the merging logic could be written as:

```rust
#[derive(Debug, Clone)]
pub enum UpgradeSelection {
    All,
    None,
    // Packages are relevant, otherwise `Option<UpgradeSelection>` would be `None`.
    Packages(Vec<Requirement>),
}

impl UpgradeSelection {
    #[must_use]
    pub fn combine(self, other: Self) -> Self {
        match self {
            // Setting `--upgrade` or `--no-upgrade` clear previous `--upgrade-package` selections.
            Self::All | Self::None => self,
            Self::Packages(self_packages) => match other {
                // With `--upgrade` previously, `--upgrade-package` is subsumed by upgrading all packages.
                Self::All => other,
                // With `--no-upgrade` previously, and now `--upgrade-package`, upgrade the explicit package list.
                Self::None => Self::Packages(self_packages),
                // If both had `--upgrade-package`, upgrade the combined package list.
                Self::Packages(other_packages) => {
                    let mut combined = self_packages;
                    combined.extend(other_packages);
                    Self::Packages(combined)
                }
            },
        }
    }
}
```

---

_Comment by @zanieb on 2025-08-21 11:39_

I agree with

> One thing that I found unexpected is that --no-upgrade causes --upgrade-package to be silently ignored

I'd expect `--upgrade-package` to take precedence.

Otherwise, I'm a +1 on the behavior. It sounds actually doesn't require encoding a relationship between the persistent configuration and the CLI like we were discussing? Or does `upgrade-package = ["foo"]` with `--no-upgrade` perform no upgrades?

---

_Comment by @charliermarsh on 2025-08-21 11:44_

I thought we had discussed the the no-upgrade / upgrade-package behavior and decided we wanted to change it but that it was breaking? That’s why I left a TODO. It’s easy to change now either way.

> Or does upgrade-package = ["foo"] with --no-upgrade perform no upgrades?

Yeah, that doesn’t perform any upgrades (which I think is correct).

---

_Comment by @konstin on 2025-08-21 11:48_

I didn't see a TODO item for that, but delaying the breaking change to 0.9 makes sense, should we add a warning now that `--no-upgrade` with `--upgrade-package` doesn't work as expected that becomes an error in 0.9?

---

_Comment by @zanieb on 2025-08-21 11:48_

> I thought we had discussed the the no-upgrade / upgrade-package behavior and decided we wanted to change it but that it was breaking?

Oh I see, I didn't realize you were separating it out into a bug fix and a breaking change. Sorry. I don't mind doing it that way as long as we're aligned on the end behavior.

---

_Comment by @zanieb on 2025-08-21 11:49_

> > Or does upgrade-package = ["foo"] with --no-upgrade perform no upgrades?
>
> Yeah, that doesn’t perform any upgrades (which I think is correct).

Yeah that sounds correct to me. It does introduce that complexity of different semantics depending on the origin layer, but it sounds worth it.

---

_Comment by @charliermarsh on 2025-08-21 12:48_

Ah sorry, I guess the TODO is in the test suite (specifically, above the test with the undesirable result) and was introduced in the PR that added the tests, so doesn't appear here.

---

_Comment by @charliermarsh on 2025-08-21 12:49_

> I didn't see a TODO item for that, but delaying the breaking change to 0.9 makes sense, should we add a warning now that --no-upgrade with --upgrade-package doesn't work as expected that becomes an error in 0.9?

I think I'd rather _not_ do this because I don't want them to be an error in 0.9, I want `--upgrade-package` to override `--no-upgrade`.


---

_Comment by @zanieb on 2025-08-21 12:49_

> I think I'd rather not do this because I don't want them to be an error in 0.9, I want --upgrade-package to override --no-upgrade.

I agree.

---

_Comment by @konstin on 2025-08-21 12:50_

That's even better!

---

_Comment by @charliermarsh on 2025-08-21 12:52_

(I'll take a look at @konstin's suggested refactor, might make things simpler.)

---

_Comment by @charliermarsh on 2025-08-21 13:36_

Okay, we now use an `Option` around the outer struct, etc.

---

_Review comment by @konstin on `crates/uv-configuration/src/package_options.rs`:138 on 2025-08-21 14:05_

Should this type be (de)serialize?

---

_@konstin approved on 2025-08-21 14:05_

---

_@charliermarsh reviewed on 2025-08-21 15:11_

---

_Review comment by @charliermarsh on `crates/uv-configuration/src/package_options.rs`:138 on 2025-08-21 15:11_

No, thanks :)

---

_Merged by @charliermarsh on 2025-08-21 15:20_

---

_Closed by @charliermarsh on 2025-08-21 15:20_

---

_Branch deleted on 2025-08-21 15:20_

---
