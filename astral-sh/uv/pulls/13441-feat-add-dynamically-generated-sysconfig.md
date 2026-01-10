```yaml
number: 13441
title: "feat: add dynamically generated sysconfig replacement mappings"
type: pull_request
state: merged
author: samypr100
labels:
  - no-build
assignees: []
merged: true
base: main
head: dynamic-sysconfig
created_at: 2025-05-14T02:29:09Z
updated_at: 2025-06-02T18:15:57Z
url: https://github.com/astral-sh/uv/pull/13441
synced_at: 2026-01-10T11:10:41Z
```

# feat: add dynamically generated sysconfig replacement mappings

---

_Pull request opened by @samypr100 on 2025-05-14 02:29_

## Summary

Implementation referenced in https://github.com/astral-sh/uv/pull/12239#issuecomment-2744880003

Closes #12919 #12901

This makes the sysconfig replacements mappings dynamically generated from https://github.com/astral-sh/python-build-standalone/blob/main/cpython-unix/targets.yml

## Test Plan

cargo dev tests, and tested scenario from https://github.com/astral-sh/uv/issues/12901#issuecomment-2822107454


---

_Comment by @samypr100 on 2025-05-14 02:49_

@zanieb Are you ok with this general approach? There is an big assumption here that targets.yml will be the canonical way to obtain these mappings going forward.

It has the (good? or bad?) side-effect that as soon as targets.yml adds a new non-mapped entry, it will fail the generate tests. We can lock it down to only reference build-standalone releases though, but that may require manual updating/upkeep.

---

_Marked ready for review by @samypr100 on 2025-05-20 01:31_

---

_@zanieb reviewed on 2025-05-20 01:46_

---

_Review comment by @zanieb on `crates/uv-dev/src/generate_sysconfig_mappings.rs`:14 on 2025-05-20 01:46_

re

> It has the (good? or bad?) side-effect that as soon as targets.yml adds a new non-mapped entry, it will fail the generate tests. 

Can we just use a tag instead of `main` here and update it in the "Sync Python Releases" job?

---

_Comment by @zanieb on 2025-05-20 01:48_

> Are you ok with this general approach? 

I think so? The targets file may change, but I do think static declaration of the targets is helpful.

I can take a closer look, but I'm at the PyCon Sprints this week and am pretty distracted.

@Gankra might have some thoughts.

---

_Label `no-build` added by @zanieb on 2025-05-20 01:48_

---

_@samypr100 reviewed on 2025-05-20 02:12_

---

_Review comment by @samypr100 on `crates/uv-dev/src/generate_sysconfig_mappings.rs`:14 on 2025-05-20 02:12_

We can use a tagged link, e.g. https://raw.githubusercontent.com/astral-sh/python-build-standalone/refs/tags/20250517/cpython-unix/targets.yml 

Note, if we're syncing you'd effectively get the latest release targets.yml at the cron interval, are you thinking `Sync Python Releases` will also run `cargo dev generate-sysconfig-metadata` and commit the changes?

---

_@samypr100 reviewed on 2025-05-20 02:33_

---

_Review comment by @samypr100 on `crates/uv-dev/src/generate_sysconfig_mappings.rs`:14 on 2025-05-20 02:33_

It would be something roughly like this

```yaml
...
      - name: Sync Sysconfig Targets
        run: |
          latest_tag=$(curl -fsSL -H "Accept: application/json" https://github.com/astral-sh/python-build-standalone/releases/latest | jq -r .tag_name)
          sed -i "s|refs/tags/[^/]\+/cpython-unix|refs/tags/${latest_tag}/cpython-unix|g" src/generate_sysconfig_mappings.rs
          cargo dev generate-sysconfig-metadata 
        working-directory: ./crates/uv-dev
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
...
      - name: "Create Pull Request"
        uses: peter-evans/create-pull-request@271a8d0340265f705b14b6d32b9829c1cb33d45e # v7.0.8
        with:
          commit-message: "Sync latest Python releases"
          add-paths: |
            crates/uv-python/download-metadata.json
            crates/uv-dev/src/generate_sysconfig_mappings.rs
            crates/uv-python/src/sysconfig/generated_mappings.rs
...
```

---

_@zanieb reviewed on 2025-05-20 13:12_

---

_Review comment by @zanieb on `crates/uv-dev/src/generate_sysconfig_mappings.rs`:14 on 2025-05-20 13:12_

Yeah that's what I was thinking, though that implementation should be in a local script so we can run it without CI too.

---

_@samypr100 reviewed on 2025-05-21 02:42_

---

_Review comment by @samypr100 on `crates/uv-dev/src/generate_sysconfig_mappings.rs`:14 on 2025-05-21 02:42_

Changes added in ef0f160b5d15b78bb40d8848f58aa999afd9af27

---

_Review comment by @Gankra on `crates/uv-dev/src/generate_sysconfig_mappings.rs`:138 on 2025-05-27 02:46_

is this dark sorcery or cruft from refactoring?

---

_Review comment by @Gankra on `crates/uv-dev/src/generate_sysconfig_mappings.rs`:134 on 2025-05-27 02:47_

like, ok this works, but, why not just have a multiline string literal?

---

_Review comment by @Gankra on `crates/uv-dev/src/generate_sysconfig_mappings.rs`:134 on 2025-05-27 02:49_

(I'm guessing this is just "because the other generate commands are written like this" which, fair enough)

---

_@Gankra approved on 2025-05-27 02:58_

This all seems vaguely coherent and yeah using a source of truth seems good. Could you give some insight on why the shell script if the rust code already needs to know how to do http requests to github?

Alternatively, could the work the script does be done by fetch-download-metadata.py, so local invokers can just run One Thing?

---

_Assigned to @Gankra by @zanieb on 2025-05-28 18:33_

---

_Comment by @samypr100 on 2025-05-29 02:18_

> This all seems vaguely coherent and yeah using a source of truth seems good. Could you give some insight on why the shell script if the rust code already needs to know how to do http requests to github?

Good question. The shell script exists to update the python-build-standalone release tag used by the .rs file for to generate the sysconfig metadata. This was a change done after https://github.com/astral-sh/uv/pull/13441#discussion_r2096703347.

> Alternatively, could the work the script does be done by fetch-download-metadata.py, so local invokers can just run One Thing?

My thought was to integrate it with the existing cargo dev workflow and tests. I think an alternate script would suffer from the same situation? We'd have to be specific about the release tag we would be pulling in the changes from and it would have to be routinely updated as per https://github.com/astral-sh/uv/pull/13441#discussion_r2096703347 it was not desirable to point it main.  Some other options I can think about are
1. Make a change to the existing code to point to the latest release tag instead every time (rather than main) which will make it "break" less often when running the cargo dev tests (or equivalent alternate python script run).
2. We use an environment variable on the existing .rs file instead to control the tag it pulls from and leave it unset locally (making the tests pass when its unset), but CI sets it.

---

_Review comment by @samypr100 on `crates/uv-dev/src/generate_sysconfig_mappings.rs`:134 on 2025-05-29 02:19_

Indeed, I kept it this way for consistency 😅 

---

_@samypr100 reviewed on 2025-05-29 02:19_

---

_@samypr100 reviewed on 2025-05-29 02:23_

---

_Review comment by @samypr100 on `crates/uv-dev/src/generate_sysconfig_mappings.rs`:138 on 2025-05-29 02:23_

The clippy skip? I took it from the readme https://github.com/rust-lang/rust-clippy#allowingdenying-lints

The formatting skip? I found it in https://github.com/rust-lang/rust/issues/124735#issuecomment-2094818939 (dark sorcery)

---

_@zanieb reviewed on 2025-05-29 12:35_

---

_Review comment by @zanieb on `crates/uv-python/src/sysconfig/generated_mappings.rs`:4 on 2025-05-29 12:35_

Can this include the tag?

---

_@zanieb reviewed on 2025-05-29 12:36_

---

_Review comment by @zanieb on `crates/uv-dev/src/generate_sysconfig_mappings.rs`:51 on 2025-05-29 12:36_

Should these all be referring to the cli reference?

---

_@samypr100 reviewed on 2025-05-29 12:56_

---

_Review comment by @samypr100 on `crates/uv-dev/src/generate_sysconfig_mappings.rs`:51 on 2025-05-29 12:56_

Good catch, fixed in b641ff6a9f6589847f999c47e45eb857b0c421fd 

---

_@samypr100 reviewed on 2025-05-29 12:56_

---

_Review comment by @samypr100 on `crates/uv-python/src/sysconfig/generated_mappings.rs`:4 on 2025-05-29 12:56_

Indeed, addressed in a11a3044020d489ce6cc4a92228ac5fc9e01ab70

---

_@zanieb approved on 2025-05-29 13:20_

---

_Review requested from @geofft by @geofft on 2025-05-30 15:44_

---

_Comment by @codspeed-hq[bot] on 2025-05-31 03:18_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/samypr100%3Adynamic-sysconfig)

### Merging #13441 will **improve performances by 15.79%**

<sub>Comparing <code>samypr100:dynamic-sysconfig</code> (c92019a) with <code>main</code> (13a86a2)</sub>



### Summary

`⚡ 1` improvements  
`✅ 11` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` wheelname_parsing_failure[flyte-long-extension] `` | 110 ns | 95 ns | +15.79% |


---

_Merged by @zanieb on 2025-06-02 15:58_

---

_Closed by @zanieb on 2025-06-02 15:58_

---

_Branch deleted on 2025-06-02 18:15_

---
