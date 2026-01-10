```yaml
number: 693
title: "Show enum defaults in `--help` output"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/show-enum-default-in-help-output
created_at: 2023-12-18T20:14:15Z
updated_at: 2023-12-18T21:50:49Z
url: https://github.com/astral-sh/uv/pull/693
synced_at: 2026-01-10T15:44:44Z
```

# Show enum defaults in `--help` output

---

_Pull request opened by @konstin on 2023-12-18 20:14_

With `Option<T>` and `.unwrap_or_default()` later, the default of `T` isn't shown in the help output.

Old:

```
      --link-mode <LINK_MODE>
          The method to use when installing packages from the global cache

          Possible values:
          - clone:    Clone (i.e., copy-on-write) packages from the wheel into the site packages
          - copy:     Copy packages from the wheel into the site packages
          - hardlink: Hard link packages from the wheel into the site packages

      -q, --quiet
      Do not print any output

      --resolution <RESOLUTION>
          Possible values:
          - highest:       Resolve the highest compatible version of each package
          - lowest:        Resolve the lowest compatible version of each package
          - lowest-direct: Resolve the lowest compatible version of any direct dependencies, and the highest compatible version of any transitive dependencies

      --prerelease <PRERELEASE>
          Possible values:
          - disallow:                 Disallow all pre-release versions
          - allow:                    Allow all pre-release versions
          - if-necessary:             Allow pre-release versions if all versions of a package are pre-release
          - explicit:                 Allow pre-release versions for first-party packages with explicit pre-release markers in their version requirements
          - if-necessary-or-explicit: Allow pre-release versions if all versions of a package are pre-release, or if the package has an explicit pre-release marker in its version requirements
```

![Screenshot from 2023-12-18 21-04-16](https://github.com/astral-sh/puffin/assets/6826232/6b3cb47a-f224-408a-8d7a-186ebeb88ecd)

New:

```
      --link-mode <LINK_MODE>
          The method to use when installing packages from the global cache

          [default: hardlink]

          Possible values:
          - clone:    Clone (i.e., copy-on-write) packages from the wheel into the site packages
          - copy:     Copy packages from the wheel into the site packages
          - hardlink: Hard link packages from the wheel into the site packages

  -q, --quiet
          Do not print any output

      --resolution <RESOLUTION>
          [default: highest]

          Possible values:
          - highest:       Resolve the highest compatible version of each package
          - lowest:        Resolve the lowest compatible version of each package
          - lowest-direct: Resolve the lowest compatible version of any direct dependencies, and the highest compatible version of any transitive dependencies

      --prerelease <PRERELEASE>
          [default: if-necessary-or-explicit]

          Possible values:
          - disallow:                 Disallow all pre-release versions
          - allow:                    Allow all pre-release versions
          - if-necessary:             Allow pre-release versions if all versions of a package are pre-release
          - explicit:                 Allow pre-release versions for first-party packages with explicit pre-release markers in their version requirements
          - if-necessary-or-explicit: Allow pre-release versions if all versions of a package are pre-release, or if the package has an explicit pre-release marker in its version requirements
```

![image](https://github.com/astral-sh/puffin/assets/6826232/26c2c391-d959-4769-999d-481b3f179502)


---

_@charliermarsh reviewed on 2023-12-18 20:25_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/main.rs`:140 on 2023-12-18 20:25_

Nit: Clippy should be suggesting `ResolutionMode::default`.

---

_@charliermarsh approved on 2023-12-18 20:25_

---

_Merged by @charliermarsh on 2023-12-18 21:50_

---

_Closed by @charliermarsh on 2023-12-18 21:50_

---

_Branch deleted on 2023-12-18 21:50_

---
