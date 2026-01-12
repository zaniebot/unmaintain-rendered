```yaml
number: 10939
title: "Add `UV_PYTHON_DOWNLOADS_JSON_URL` to set custom managed python sources"
type: pull_request
state: merged
author: MeitarR
labels: []
assignees: []
merged: true
base: main
head: uv-python-downloads-json-file
created_at: 2025-01-24T15:40:43Z
updated_at: 2025-07-17T19:22:17Z
url: https://github.com/astral-sh/uv/pull/10939
synced_at: 2026-01-12T16:09:35Z
```

# Add `UV_PYTHON_DOWNLOADS_JSON_URL` to set custom managed python sources

---

_@MeitarR_

## Summary

Add an option to overwrite the list of available Python downloads from a local JSON file by using the environment variable `UV_PYTHON_DOWNLOADS_JSON_URL`

as an experimental support for providing custom sources for Python distribution binaries #8015 

related #10203

I probably should make the JSON to be fetched from a remote URL instead of a local file.
please let me know what you think and I will modify the code accordingly.

## Test Plan

### normal run
```
root@75c66494ba8b:/# /code/target/release/uv python list
cpython-3.14.0a4+freethreaded-linux-x86_64-gnu    <download available>
cpython-3.14.0a4-linux-x86_64-gnu                 <download available>
cpython-3.13.1+freethreaded-linux-x86_64-gnu      <download available>
cpython-3.13.1-linux-x86_64-gnu                   <download available>
cpython-3.12.8-linux-x86_64-gnu                   <download available>
cpython-3.11.11-linux-x86_64-gnu                  <download available>
cpython-3.10.16-linux-x86_64-gnu                  <download available>
cpython-3.9.21-linux-x86_64-gnu                   <download available>
cpython-3.8.20-linux-x86_64-gnu                   <download available>
cpython-3.7.9-linux-x86_64-gnu                    <download available>
pypy-3.10.14-linux-x86_64-gnu                     <download available>
pypy-3.9.19-linux-x86_64-gnu                      <download available>
pypy-3.8.16-linux-x86_64-gnu                      <download available>
pypy-3.7.13-linux-x86_64-gnu                      <download available>
```

### empty JSON file
```sh
root@75c66494ba8b:/# export UV_PYTHON_DOWNLOADS_JSON_URL=/code/crates/uv-python/my-download-metadata.json 
root@75c66494ba8b:/# cat $UV_PYTHON_DOWNLOADS_JSON_URL 
{}
root@75c66494ba8b:/# /code/target/release/uv python list
root@75c66494ba8b:/# 
```

### JSON file with valid version
```sh
root@75c66494ba8b:/# export UV_PYTHON_DOWNLOADS_JSON_URL=/code/crates/uv-python/my-download-metadata.json 
root@75c66494ba8b:/# cat $UV_PYTHON_DOWNLOADS_JSON_URL 
{
  "cpython-3.11.9-linux-x86_64-gnu": {
    "name": "cpython",
    "arch": {
      "family": "x86_64",
      "variant": null
    },
    "os": "linux",
    "libc": "gnu",
    "major": 3,
    "minor": 11,
    "patch": 9,
    "prerelease": "",
    "url": "https://github.com/astral-sh/python-build-standalone/releases/download/20240814/cpython-3.11.9%2B20240814-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz",
    "sha256": "daa487c7e73005c4426ac393273117cf0e2dc4ab9b2eeda366e04cd00eea00c9",
    "variant": null
  }
}
root@75c66494ba8b:/# /code/target/release/uv python list
cpython-3.11.9-linux-x86_64-gnu    <download available>
root@75c66494ba8b:/# 
```

### Remote Path

```sh
root@75c66494ba8b:/# export UV_PYTHON_DOWNLOADS_JSON_URL=http://a.com/file.json 
root@75c66494ba8b:/# /code/target/release/uv python list
error: Remote python downloads JSON is not yet supported, please use a local path (without `file://` prefix)
```


---

_Assigned to @zanieb by @zanieb on 2025-01-24 15:45_

---

_@zanieb reviewed on 2025-01-24 22:32_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:476 on 2025-01-24 22:32_

Note this variable will need to be defined in `crates/uv-static/src/env_vars.rs`

I wonder if we should call it `_URL` or `_URI`? We can assume a local path and error with an informative message about the lack of remote file support for now, but we should have a forward-looking name.

We also need to be considerate about how the fetch is implemented, e.g., if we're making a network request it should probably be async which would mean this this function needs to be async (which I would be disappointed by). What kind of change would we need to make to avoid that?

---

_@zanieb reviewed on 2025-01-24 22:34_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:487 on 2025-01-24 22:34_

Maybe a bit of a silly question, but is there a reason we shouldn't deserialize directly into `ManagedPythonDownload`? Why the intermediate type?

---

_@zanieb reviewed on 2025-01-24 22:35_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:476 on 2025-01-24 22:35_

(I don't think we need remote file support to add this feature — but I would like to know how invasive it would be)

---

_@zanieb reviewed on 2025-01-24 22:36_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:486 on 2025-01-24 22:36_

A panic is probably not acceptable here. We'd need to handle this gracefully, i.e., by surfacing the error to the user.

---

_@zanieb reviewed on 2025-01-24 22:38_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:476 on 2025-01-24 22:38_

 A remote file also introduces the complexity of caching 🤔 though perhaps we could avoid that since installs don't happen _that_ often.

---

_@MeitarR reviewed on 2025-01-25 21:32_

---

_Review comment by @MeitarR on `crates/uv-python/src/downloads.rs`:487 on 2025-01-25 21:32_

TBH, that's what the AI suggested, but I had the same thought as you, so I tried to derive the `Deserialize` on `ManagedPythonDownload` and then on `PythonInstallationKey`. I understood that `target_lexicon`'s enums do not implement the `Deserialize` trait, and even if I overcome this, the conversion from JSON will not be as is. So the only way to improve it (the way I see it) is to implement `Deserialize` on our own and define the JSON structs inside the implementation.

If you see a better way, please let me know

---

_@MeitarR reviewed on 2025-01-25 21:45_

---

_Review comment by @MeitarR on `crates/uv-python/src/downloads.rs`:476 on 2025-01-25 21:45_

looks like it won't be that bad to make it async.
we would also need to make the following async:
- `iter_downloads`
- `from_request`
- `PytestRequest::new`

![image](https://github.com/user-attachments/assets/1bb78b40-cea4-49ba-8f42-839386757513)

But maybe we could use `tokio::runtime::Handle::current().block_on` or construct a new runtime for the request only.

---

_Review requested from @zanieb by @MeitarR on 2025-01-25 21:46_

---

_@zanieb reviewed on 2025-01-27 16:55_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:487 on 2025-01-27 16:55_

It looks like there's a `serde` feature for `target_lexicon`, did you try that?

---

_@zanieb reviewed on 2025-01-27 16:55_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:487 on 2025-01-27 16:55_

What part is not "as-is"? Like some custom mapping of architectures or something?

---

_@MeitarR reviewed on 2025-01-27 20:48_

---

_Review comment by @MeitarR on `crates/uv-python/src/downloads.rs`:487 on 2025-01-27 20:48_

> It looks like there's a serde feature for target_lexicon, did you try that?

from the [code](https://github.com/bytecodealliance/target-lexicon/blob/c3f72c4ced783523aba166d1c52f1b355de8c81a/src/lib.rs#L67), I understood that they only implemented it for the `Triple` struct, so it not useful here :(

> What part is not "as-is"? Like some custom mapping of architectures or something?

yes, it's small details, like the `implementation` that will need special implementation, or the `arch`. the main problem here is the structs of `target_lexicon`

---

_Renamed from "custom sources for Python distribution binaries" to "Add `UV_PYTHON_DOWNLOADS_JSON_URL` to set custom managed python sources" by @MeitarR on 2025-01-27 20:52_

---

_Comment by @MeitarR on 2025-01-30 18:44_

@zanieb is there something left for me to do to get this merged? 

---

_Comment by @zanieb on 2025-02-03 21:55_

Just need to find the time to review it carefully and make sure we're happy maintaining / supporting this in perpetuity.

---

_Comment by @Gankra on 2025-02-21 16:19_

Apologies for the delay! I pushed up a tweak to the design to make it remain totally static and cheap when the environment variable isn't set. My main hesitation with this solution now is just that we are maintaining two copies of this "raw json" => "digested rust types" logic: One copy in python, and one copy in Rust.

This would be really easy to accept if this digestion code was shared, but it's a lot of work to do that.

The *easiest* path forward would be drop the python translation script, directly embed the json statically, and just parse it at runtime (still caching it in a Once so it's a one-time-cost). The only barrier to accepting that solution is someone doing profiling to verify the performance impact is negligible.

---

_Comment by @MeitarR on 2025-02-22 21:00_

> Apologies for the delay! I pushed up a tweak to the design to make it remain totally static and cheap when the environment variable isn't set. My main hesitation with this solution now is just that we are maintaining two copies of this "raw json" => "digested rust types" logic: One copy in python, and one copy in Rust.
> 
> This would be really easy to accept if this digestion code was shared, but it's a lot of work to do that.
> 
> The _easiest_ path forward would be drop the python translation script, directly embed the json statically, and just parse it at runtime (still caching it in a Once so it's a one-time-cost). The only barrier to accepting that solution is someone doing profiling to verify the performance impact is negligible.

@Gankra, I agree with you.

I implemented the "_easiest_ path" and pushed it to this branch.
I'm concerned about the size that is added to the binary (~~810K~~ 501K~).
and about the implementation, it will probably be better to minify the file on compile time and not save a duplicate. another option is to only save the minified version in the repo, I leave it for you to decide.

About the profiling part, I do not know how to do it (would be happy to learn). is it something that you will take care of?

---

_Converted to draft by @MeitarR on 2025-02-28 21:55_

---

_Marked ready for review by @MeitarR on 2025-02-28 23:22_

---

_Comment by @Gankra on 2025-03-18 13:20_

Performance-wise this seems to have negligible impact -- hyperfine shows no real difference before and after on trivial workfloads. This is good.

The main concern then is whether we're ok with bloat on the uv binary. On my macos release builds the difference is actually only 200KB? (31.9MB vs 31.7MB). That difference seems pretty acceptable, especially since we could optimize it later if we really wanted. It's less than 1% size bloat.

---

_Comment by @Gankra on 2025-03-18 13:22_

I'll rereview/rebase on the assumption that we're happy with the perf and want to land this approach.

---

_Review comment by @Gankra on `crates/uv-python/minify-download-metadata.py`:11 on 2025-03-18 18:09_

ooh i like this approach as a compromise!

---

_Review comment by @Gankra on `crates/uv-python/src/downloads.rs`:472 on 2025-03-18 18:12_

Surely these should be more like u64?

---

_Review comment by @Gankra on `crates/uv-python/src/downloads.rs`:776 on 2025-03-18 18:30_

Interesting, in the old implementation an unknown name was treated as a hard error.

---

_Review comment by @Gankra on `crates/uv-python/src/downloads.rs`:819 on 2025-03-18 18:36_

Another instance of us becoming more lenient -- and also probably a mistake? This debug suggests we're skipping the entry but actually you just continue on with PythonVariant::Default.

---

_@zanieb had review dismissed on 2025-03-18 18:55_

Alright, I'm supportive of this change. I think we'll want to be _really_ sure that the ordering has not regressed here. We don't have exhaustive test coverage for the ordering of managed Python versions.

---

_Comment by @zanieb on 2025-03-18 18:56_

@MeitarR could you write a script that demonstrates that the sorted parsed Python installations list is equivalent to the one from `downloads.inc`? 

---

_Review comment by @Gankra on `crates/uv-python/template-download-metadata.py`:80 on 2025-03-18 18:59_

This logic appears to be getting dropped in this PR. target-lexicon bottoms out in `Riscv64(Riscv64)` for this input instead of `Riscv64(Riscv64gc)`

---

_Review comment by @Gankra on `crates/uv-python/template-download-metadata.py`:73 on 2025-03-18 19:02_

This logic appears to be getting dropped in this PR. target_lexicon only recognizes

```rust
            "armv5t" => Armv5t,
            "armv5te" => Armv5te,
            "armv5tej" => Armv5tej,
 ```

---

_Review comment by @Gankra on `crates/uv-python/src/installation.rs`:485 on 2025-03-18 19:07_

Why did this change?

---

_Review comment by @Gankra on `crates/uv-python/src/downloads.rs`:518 on 2025-03-18 19:08_

We try to add additional context to these kinds of errors.

---

_Review comment by @Gankra on `crates/uv-python/src/download-metadata-minified.json`:1 on 2025-03-18 19:09_

We'll have to make sure we regenerate this before merging, in case any releases have happened in the interim.

---

_@Gankra requested changes on 2025-03-18 19:11_

The general approach seems good now, but I found a lot of changes in behaviour in my code review.

We definitely should have some kind of test for this?

---

_@zanieb reviewed on 2025-03-18 19:16_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:472 on 2025-03-18 19:16_

I'm not sure we need a u64 for Python version, do we? I feel like we're many years from 3.256.0?

---

_@Gankra reviewed on 2025-03-18 19:20_

---

_Review comment by @Gankra on `crates/uv-python/src/downloads.rs`:472 on 2025-03-18 19:20_

Hm fair. Still u8 feels needlessly spicey, especially for a temp deserialization format.

---

_@zanieb reviewed on 2025-03-18 19:21_

---

_Review comment by @zanieb on `crates/uv-python/src/download-metadata-minified.json`:1 on 2025-03-18 19:21_

(changes have already occurred yeah)

---

_Comment by @zanieb on 2025-03-18 19:21_

Thanks @Gankra

We don't have a ton of test coverage because it requires installing managed Python versions which is fairly expensive. We added a feature flag for that though, so we can add tests to the `python_find` and `python_install` suites.

---

_Comment by @Gankra on 2025-03-18 19:42_

We could probably add some unit tests for some of the corner cases I noticed. Or one that checks that the static json has no substantial losses. (Hmm could we debug_assert that none of these misparse warnings are ever hit?)

---

_Comment by @MeitarR on 2025-03-18 20:53_

@zanieb @Gankra Thank you for the review! I will try to get the fixes by Sunday

---

_@MeitarR reviewed on 2025-03-18 20:54_

---

_Review comment by @MeitarR on `crates/uv-python/src/downloads.rs`:776 on 2025-03-18 20:54_

Do you want to preserve the "hard error", or leave it as it is now?

---

_@MeitarR reviewed on 2025-03-18 20:58_

---

_Review comment by @MeitarR on `crates/uv-python/src/installation.rs`:485 on 2025-03-18 20:58_

without this change, some tests fail as we choose the wrong version, I tried to investigate why and because what I saw was that we will get rc versions instead of what we wanted, this was the right change

---

_@zanieb reviewed on 2025-03-18 21:44_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:472 on 2025-03-18 21:44_

The `PythonInstallationKey` uses too u8s though

---

_@zanieb reviewed on 2025-03-18 21:44_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:472 on 2025-03-18 21:44_

What failure mode are you worried about? I'm open to changing it

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:472 on 2025-03-18 21:45_

We also use u8s for arbitrary Python versions, e.g.,

https://github.com/astral-sh/uv/blob/d712ff243e9f360d6409d0e6855d6f8f93d29d5b/crates/uv-python/src/python_version.rs#L158-L175

---

_@zanieb reviewed on 2025-03-18 21:45_

---

_Comment by @MeitarR on 2025-03-22 20:37_

> @MeitarR could you write a script that demonstrates that the sorted parsed Python installations list is equivalent to the one from `downloads.inc`?

@zanieb 

well, it don't exactly match but only because different ordering by the `os` / `arch` / `libc`, which for my understanding is neglectable (please correct me here if I'm wrong).

So I wrote 2 scripts
`split_files.sh`
```sh
#!/bin/sh

os_list=`jq -r 'map(.os) | unique | .[]' downloads.json`
os_array=`echo $os_list`

arch_list=`jq -r 'map(.arch) | unique | .[]' downloads.json`
arch_array=`echo $arch_list`

libc_list=`jq -r 'map(.libc) | unique | .[]' downloads.json`
libc_array=`echo $libc_list`

# Iterate over the bash array
for os in $os_array; do
  for arch in $arch_array; do
    for libc in $libc_array; do
      jq "[.[] | select(.os == \"$os\" and .arch == \"$arch\" and .libc == \"$libc\")]" downloads.json > downloads_${os}_${arch}_${libc}.json
    done
  done
done

# save all os, arch, libc the unique values as lists in a single json
jq '{  os: map(.os) | unique,  arch: map(.arch) | unique,  libc: map(.libc) | unique}' downloads.json > downloads_unique.json
```

and `diff_all_files.sh`
```sh
#!/bin/bash
verbose=false

usage() {
    echo "Usage: $0 [-v] TARGET_DIR"
    echo "  -v: verbose mode"
    echo "  TARGET_DIR: directory to compare files against"
    exit 1
}

while getopts "v" opt; do
    case $opt in
        v) verbose=true ;;
        ?) usage ;;
    esac
done
shift $((OPTIND-1))

if [ $# -ne 1 ]; then
    usage
fi

target_dir="$1"

if [ ! -d "$target_dir" ]; then
    echo "Error: Target directory '$target_dir' does not exist"
    exit 1
fi

for file in downloads_*.json; do
    if [ -f "$file" ]; then
        diff_output=$(diff "$file" "$target_dir/$file")
        if [ -n "$diff_output" ]; then
            echo "Differences found in $file:"
            if [ "$verbose" = true ]; then
                code --diff "$file" "$target_dir/$file"
            fi
        else
            echo "No differences found in $file"
        fi
    fi
done
```

and apply this patch to your code in both versions you want to compare (do it in 2 different worktrees)

```diff
diff --git a/crates/uv-python/src/downloads.rs b/crates/uv-python/src/downloads.rs
index aa5ca64fd..c6d4f0508 100644
--- a/crates/uv-python/src/downloads.rs
+++ b/crates/uv-python/src/downloads.rs
@@ -466,7 +466,29 @@ impl ManagedPythonDownload {
 
     /// Iterate over all [`ManagedPythonDownload`]s.
     pub fn iter_all() -> impl Iterator<Item = &'static ManagedPythonDownload> {
-        PYTHON_DOWNLOADS.iter()
+        let result = PYTHON_DOWNLOADS;
+
+        use std::fs::File;
+        use std::io::Write;
+        let json_downloads: Vec<_> = result
+            .iter()
+            .map(|download| {
+                serde_json::json!({
+                    "url": download.url,
+                    "os": download.key.os.to_string(),
+                    "arch": download.key.arch.to_string(),
+                    "libc": download.key.libc.to_string(),
+                    "version": download.key.version().to_string(),
+                })
+            })
+            .collect();
+        let json_downloads =
+            serde_json::to_string(&json_downloads).expect("Failed to serialize downloads");
+        let mut file = File::create("downloads.json").expect("Failed to create file");
+        file.write_all(json_downloads.as_bytes())
+            .expect("Failed to write to file");
+
+        result.iter()
     }
 
     pub fn url(&self) -> &'static str {

```

then it expects you to run this on both of the worktrees

```sh
cargo run -- -v python list; ../uv/split_files.sh
```

and then on one of them

```sh
./diff_all_files.sh -v <path to the other worktree>
```

if there was changes it will also opens them in vscode diff

##### example good output
```
No differences found in downloads_linux_aarch64_gnu.json
No differences found in downloads_linux_aarch64_gnueabi.json
No differences found in downloads_linux_aarch64_gnueabihf.json
No differences found in downloads_linux_aarch64_musl.json
No differences found in downloads_linux_aarch64_none.json
No differences found in downloads_linux_armv7_gnu.json
No differences found in downloads_linux_armv7_gnueabi.json
No differences found in downloads_linux_armv7_gnueabihf.json
No differences found in downloads_linux_armv7_musl.json
No differences found in downloads_linux_armv7_none.json
No differences found in downloads_linux_powerpc64le_gnu.json
No differences found in downloads_linux_powerpc64le_gnueabi.json
No differences found in downloads_linux_powerpc64le_gnueabihf.json
No differences found in downloads_linux_powerpc64le_musl.json
No differences found in downloads_linux_powerpc64le_none.json
No differences found in downloads_linux_riscv64gc_gnu.json
No differences found in downloads_linux_riscv64gc_gnueabi.json
No differences found in downloads_linux_riscv64gc_gnueabihf.json
No differences found in downloads_linux_riscv64gc_musl.json
No differences found in downloads_linux_riscv64gc_none.json
No differences found in downloads_linux_s390x_gnu.json
No differences found in downloads_linux_s390x_gnueabi.json
No differences found in downloads_linux_s390x_gnueabihf.json
No differences found in downloads_linux_s390x_musl.json
No differences found in downloads_linux_s390x_none.json
No differences found in downloads_linux_x86_64_gnu.json
No differences found in downloads_linux_x86_64_gnueabi.json
No differences found in downloads_linux_x86_64_gnueabihf.json
No differences found in downloads_linux_x86_64_musl.json
No differences found in downloads_linux_x86_64_none.json
No differences found in downloads_linux_x86_64_v2_gnu.json
No differences found in downloads_linux_x86_64_v2_gnueabi.json
No differences found in downloads_linux_x86_64_v2_gnueabihf.json
No differences found in downloads_linux_x86_64_v2_musl.json
No differences found in downloads_linux_x86_64_v2_none.json
No differences found in downloads_linux_x86_64_v3_gnu.json
No differences found in downloads_linux_x86_64_v3_gnueabi.json
No differences found in downloads_linux_x86_64_v3_gnueabihf.json
No differences found in downloads_linux_x86_64_v3_musl.json
No differences found in downloads_linux_x86_64_v3_none.json
No differences found in downloads_linux_x86_64_v4_gnu.json
No differences found in downloads_linux_x86_64_v4_gnueabi.json
No differences found in downloads_linux_x86_64_v4_gnueabihf.json
No differences found in downloads_linux_x86_64_v4_musl.json
No differences found in downloads_linux_x86_64_v4_none.json
No differences found in downloads_linux_x86_gnu.json
No differences found in downloads_linux_x86_gnueabi.json
No differences found in downloads_linux_x86_gnueabihf.json
No differences found in downloads_linux_x86_musl.json
No differences found in downloads_linux_x86_none.json
No differences found in downloads_macos_aarch64_gnu.json
No differences found in downloads_macos_aarch64_gnueabi.json
No differences found in downloads_macos_aarch64_gnueabihf.json
No differences found in downloads_macos_aarch64_musl.json
No differences found in downloads_macos_aarch64_none.json
No differences found in downloads_macos_armv7_gnu.json
No differences found in downloads_macos_armv7_gnueabi.json
No differences found in downloads_macos_armv7_gnueabihf.json
No differences found in downloads_macos_armv7_musl.json
No differences found in downloads_macos_armv7_none.json
No differences found in downloads_macos_powerpc64le_gnu.json
No differences found in downloads_macos_powerpc64le_gnueabi.json
No differences found in downloads_macos_powerpc64le_gnueabihf.json
No differences found in downloads_macos_powerpc64le_musl.json
No differences found in downloads_macos_powerpc64le_none.json
No differences found in downloads_macos_riscv64gc_gnu.json
No differences found in downloads_macos_riscv64gc_gnueabi.json
No differences found in downloads_macos_riscv64gc_gnueabihf.json
No differences found in downloads_macos_riscv64gc_musl.json
No differences found in downloads_macos_riscv64gc_none.json
No differences found in downloads_macos_s390x_gnu.json
No differences found in downloads_macos_s390x_gnueabi.json
No differences found in downloads_macos_s390x_gnueabihf.json
No differences found in downloads_macos_s390x_musl.json
No differences found in downloads_macos_s390x_none.json
No differences found in downloads_macos_x86_64_gnu.json
No differences found in downloads_macos_x86_64_gnueabi.json
No differences found in downloads_macos_x86_64_gnueabihf.json
No differences found in downloads_macos_x86_64_musl.json
No differences found in downloads_macos_x86_64_none.json
No differences found in downloads_macos_x86_64_v2_gnu.json
No differences found in downloads_macos_x86_64_v2_gnueabi.json
No differences found in downloads_macos_x86_64_v2_gnueabihf.json
No differences found in downloads_macos_x86_64_v2_musl.json
No differences found in downloads_macos_x86_64_v2_none.json
No differences found in downloads_macos_x86_64_v3_gnu.json
No differences found in downloads_macos_x86_64_v3_gnueabi.json
No differences found in downloads_macos_x86_64_v3_gnueabihf.json
No differences found in downloads_macos_x86_64_v3_musl.json
No differences found in downloads_macos_x86_64_v3_none.json
No differences found in downloads_macos_x86_64_v4_gnu.json
No differences found in downloads_macos_x86_64_v4_gnueabi.json
No differences found in downloads_macos_x86_64_v4_gnueabihf.json
No differences found in downloads_macos_x86_64_v4_musl.json
No differences found in downloads_macos_x86_64_v4_none.json
No differences found in downloads_macos_x86_gnu.json
No differences found in downloads_macos_x86_gnueabi.json
No differences found in downloads_macos_x86_gnueabihf.json
No differences found in downloads_macos_x86_musl.json
No differences found in downloads_macos_x86_none.json
No differences found in downloads_unique.json
No differences found in downloads_windows_aarch64_gnu.json
No differences found in downloads_windows_aarch64_gnueabi.json
No differences found in downloads_windows_aarch64_gnueabihf.json
No differences found in downloads_windows_aarch64_musl.json
No differences found in downloads_windows_aarch64_none.json
No differences found in downloads_windows_armv7_gnu.json
No differences found in downloads_windows_armv7_gnueabi.json
No differences found in downloads_windows_armv7_gnueabihf.json
No differences found in downloads_windows_armv7_musl.json
No differences found in downloads_windows_armv7_none.json
No differences found in downloads_windows_powerpc64le_gnu.json
No differences found in downloads_windows_powerpc64le_gnueabi.json
No differences found in downloads_windows_powerpc64le_gnueabihf.json
No differences found in downloads_windows_powerpc64le_musl.json
No differences found in downloads_windows_powerpc64le_none.json
No differences found in downloads_windows_riscv64gc_gnu.json
No differences found in downloads_windows_riscv64gc_gnueabi.json
No differences found in downloads_windows_riscv64gc_gnueabihf.json
No differences found in downloads_windows_riscv64gc_musl.json
No differences found in downloads_windows_riscv64gc_none.json
No differences found in downloads_windows_s390x_gnu.json
No differences found in downloads_windows_s390x_gnueabi.json
No differences found in downloads_windows_s390x_gnueabihf.json
No differences found in downloads_windows_s390x_musl.json
No differences found in downloads_windows_s390x_none.json
No differences found in downloads_windows_x86_64_gnu.json
No differences found in downloads_windows_x86_64_gnueabi.json
No differences found in downloads_windows_x86_64_gnueabihf.json
No differences found in downloads_windows_x86_64_musl.json
No differences found in downloads_windows_x86_64_none.json
No differences found in downloads_windows_x86_64_v2_gnu.json
No differences found in downloads_windows_x86_64_v2_gnueabi.json
No differences found in downloads_windows_x86_64_v2_gnueabihf.json
No differences found in downloads_windows_x86_64_v2_musl.json
No differences found in downloads_windows_x86_64_v2_none.json
No differences found in downloads_windows_x86_64_v3_gnu.json
No differences found in downloads_windows_x86_64_v3_gnueabi.json
No differences found in downloads_windows_x86_64_v3_gnueabihf.json
No differences found in downloads_windows_x86_64_v3_musl.json
No differences found in downloads_windows_x86_64_v3_none.json
No differences found in downloads_windows_x86_64_v4_gnu.json
No differences found in downloads_windows_x86_64_v4_gnueabi.json
No differences found in downloads_windows_x86_64_v4_gnueabihf.json
No differences found in downloads_windows_x86_64_v4_musl.json
No differences found in downloads_windows_x86_64_v4_none.json
No differences found in downloads_windows_x86_gnu.json
No differences found in downloads_windows_x86_gnueabi.json
No differences found in downloads_windows_x86_gnueabihf.json
No differences found in downloads_windows_x86_musl.json
No differences found in downloads_windows_x86_none.json
```

##### example output before the riscv fix
```
No differences found in downloads_linux_aarch64_gnu.json
No differences found in downloads_linux_aarch64_gnueabi.json
No differences found in downloads_linux_aarch64_gnueabihf.json
No differences found in downloads_linux_aarch64_musl.json
No differences found in downloads_linux_aarch64_none.json
No differences found in downloads_linux_armv7_gnu.json
No differences found in downloads_linux_armv7_gnueabi.json
No differences found in downloads_linux_armv7_gnueabihf.json
No differences found in downloads_linux_armv7_musl.json
No differences found in downloads_linux_armv7_none.json
No differences found in downloads_linux_powerpc64le_gnu.json
No differences found in downloads_linux_powerpc64le_gnueabi.json
No differences found in downloads_linux_powerpc64le_gnueabihf.json
No differences found in downloads_linux_powerpc64le_musl.json
No differences found in downloads_linux_powerpc64le_none.json
diff: ../uv2/downloads_linux_riscv64_gnu.json: No such file or directory
No differences found in downloads_linux_riscv64_gnu.json
diff: ../uv2/downloads_linux_riscv64_gnueabi.json: No such file or directory
No differences found in downloads_linux_riscv64_gnueabi.json
diff: ../uv2/downloads_linux_riscv64_gnueabihf.json: No such file or directory
No differences found in downloads_linux_riscv64_gnueabihf.json
diff: ../uv2/downloads_linux_riscv64_musl.json: No such file or directory
No differences found in downloads_linux_riscv64_musl.json
diff: ../uv2/downloads_linux_riscv64_none.json: No such file or directory
No differences found in downloads_linux_riscv64_none.json
No differences found in downloads_linux_s390x_gnu.json
No differences found in downloads_linux_s390x_gnueabi.json
No differences found in downloads_linux_s390x_gnueabihf.json
No differences found in downloads_linux_s390x_musl.json
No differences found in downloads_linux_s390x_none.json
No differences found in downloads_linux_x86_64_gnu.json
No differences found in downloads_linux_x86_64_gnueabi.json
No differences found in downloads_linux_x86_64_gnueabihf.json
No differences found in downloads_linux_x86_64_musl.json
No differences found in downloads_linux_x86_64_none.json
No differences found in downloads_linux_x86_64_v2_gnu.json
No differences found in downloads_linux_x86_64_v2_gnueabi.json
No differences found in downloads_linux_x86_64_v2_gnueabihf.json
No differences found in downloads_linux_x86_64_v2_musl.json
No differences found in downloads_linux_x86_64_v2_none.json
No differences found in downloads_linux_x86_64_v3_gnu.json
No differences found in downloads_linux_x86_64_v3_gnueabi.json
No differences found in downloads_linux_x86_64_v3_gnueabihf.json
No differences found in downloads_linux_x86_64_v3_musl.json
No differences found in downloads_linux_x86_64_v3_none.json
No differences found in downloads_linux_x86_64_v4_gnu.json
No differences found in downloads_linux_x86_64_v4_gnueabi.json
No differences found in downloads_linux_x86_64_v4_gnueabihf.json
No differences found in downloads_linux_x86_64_v4_musl.json
No differences found in downloads_linux_x86_64_v4_none.json
No differences found in downloads_linux_x86_gnu.json
No differences found in downloads_linux_x86_gnueabi.json
No differences found in downloads_linux_x86_gnueabihf.json
No differences found in downloads_linux_x86_musl.json
No differences found in downloads_linux_x86_none.json
No differences found in downloads_macos_aarch64_gnu.json
No differences found in downloads_macos_aarch64_gnueabi.json
No differences found in downloads_macos_aarch64_gnueabihf.json
No differences found in downloads_macos_aarch64_musl.json
No differences found in downloads_macos_aarch64_none.json
No differences found in downloads_macos_armv7_gnu.json
No differences found in downloads_macos_armv7_gnueabi.json
No differences found in downloads_macos_armv7_gnueabihf.json
No differences found in downloads_macos_armv7_musl.json
No differences found in downloads_macos_armv7_none.json
No differences found in downloads_macos_powerpc64le_gnu.json
No differences found in downloads_macos_powerpc64le_gnueabi.json
No differences found in downloads_macos_powerpc64le_gnueabihf.json
No differences found in downloads_macos_powerpc64le_musl.json
No differences found in downloads_macos_powerpc64le_none.json
diff: ../uv2/downloads_macos_riscv64_gnu.json: No such file or directory
No differences found in downloads_macos_riscv64_gnu.json
diff: ../uv2/downloads_macos_riscv64_gnueabi.json: No such file or directory
No differences found in downloads_macos_riscv64_gnueabi.json
diff: ../uv2/downloads_macos_riscv64_gnueabihf.json: No such file or directory
No differences found in downloads_macos_riscv64_gnueabihf.json
diff: ../uv2/downloads_macos_riscv64_musl.json: No such file or directory
No differences found in downloads_macos_riscv64_musl.json
diff: ../uv2/downloads_macos_riscv64_none.json: No such file or directory
No differences found in downloads_macos_riscv64_none.json
No differences found in downloads_macos_s390x_gnu.json
No differences found in downloads_macos_s390x_gnueabi.json
No differences found in downloads_macos_s390x_gnueabihf.json
No differences found in downloads_macos_s390x_musl.json
No differences found in downloads_macos_s390x_none.json
No differences found in downloads_macos_x86_64_gnu.json
No differences found in downloads_macos_x86_64_gnueabi.json
No differences found in downloads_macos_x86_64_gnueabihf.json
No differences found in downloads_macos_x86_64_musl.json
No differences found in downloads_macos_x86_64_none.json
No differences found in downloads_macos_x86_64_v2_gnu.json
No differences found in downloads_macos_x86_64_v2_gnueabi.json
No differences found in downloads_macos_x86_64_v2_gnueabihf.json
No differences found in downloads_macos_x86_64_v2_musl.json
No differences found in downloads_macos_x86_64_v2_none.json
No differences found in downloads_macos_x86_64_v3_gnu.json
No differences found in downloads_macos_x86_64_v3_gnueabi.json
No differences found in downloads_macos_x86_64_v3_gnueabihf.json
No differences found in downloads_macos_x86_64_v3_musl.json
No differences found in downloads_macos_x86_64_v3_none.json
No differences found in downloads_macos_x86_64_v4_gnu.json
No differences found in downloads_macos_x86_64_v4_gnueabi.json
No differences found in downloads_macos_x86_64_v4_gnueabihf.json
No differences found in downloads_macos_x86_64_v4_musl.json
No differences found in downloads_macos_x86_64_v4_none.json
No differences found in downloads_macos_x86_gnu.json
No differences found in downloads_macos_x86_gnueabi.json
No differences found in downloads_macos_x86_gnueabihf.json
No differences found in downloads_macos_x86_musl.json
No differences found in downloads_macos_x86_none.json
Differences found in downloads_unique.json:
No differences found in downloads_windows_aarch64_gnu.json
No differences found in downloads_windows_aarch64_gnueabi.json
No differences found in downloads_windows_aarch64_gnueabihf.json
No differences found in downloads_windows_aarch64_musl.json
No differences found in downloads_windows_aarch64_none.json
No differences found in downloads_windows_armv7_gnu.json
No differences found in downloads_windows_armv7_gnueabi.json
No differences found in downloads_windows_armv7_gnueabihf.json
No differences found in downloads_windows_armv7_musl.json
No differences found in downloads_windows_armv7_none.json
No differences found in downloads_windows_powerpc64le_gnu.json
No differences found in downloads_windows_powerpc64le_gnueabi.json
No differences found in downloads_windows_powerpc64le_gnueabihf.json
No differences found in downloads_windows_powerpc64le_musl.json
No differences found in downloads_windows_powerpc64le_none.json
diff: ../uv2/downloads_windows_riscv64_gnu.json: No such file or directory
No differences found in downloads_windows_riscv64_gnu.json
diff: ../uv2/downloads_windows_riscv64_gnueabi.json: No such file or directory
No differences found in downloads_windows_riscv64_gnueabi.json
diff: ../uv2/downloads_windows_riscv64_gnueabihf.json: No such file or directory
No differences found in downloads_windows_riscv64_gnueabihf.json
diff: ../uv2/downloads_windows_riscv64_musl.json: No such file or directory
No differences found in downloads_windows_riscv64_musl.json
diff: ../uv2/downloads_windows_riscv64_none.json: No such file or directory
No differences found in downloads_windows_riscv64_none.json
No differences found in downloads_windows_s390x_gnu.json
No differences found in downloads_windows_s390x_gnueabi.json
No differences found in downloads_windows_s390x_gnueabihf.json
No differences found in downloads_windows_s390x_musl.json
No differences found in downloads_windows_s390x_none.json
No differences found in downloads_windows_x86_64_gnu.json
No differences found in downloads_windows_x86_64_gnueabi.json
No differences found in downloads_windows_x86_64_gnueabihf.json
No differences found in downloads_windows_x86_64_musl.json
No differences found in downloads_windows_x86_64_none.json
No differences found in downloads_windows_x86_64_v2_gnu.json
No differences found in downloads_windows_x86_64_v2_gnueabi.json
No differences found in downloads_windows_x86_64_v2_gnueabihf.json
No differences found in downloads_windows_x86_64_v2_musl.json
No differences found in downloads_windows_x86_64_v2_none.json
No differences found in downloads_windows_x86_64_v3_gnu.json
No differences found in downloads_windows_x86_64_v3_gnueabi.json
No differences found in downloads_windows_x86_64_v3_gnueabihf.json
No differences found in downloads_windows_x86_64_v3_musl.json
No differences found in downloads_windows_x86_64_v3_none.json
No differences found in downloads_windows_x86_64_v4_gnu.json
No differences found in downloads_windows_x86_64_v4_gnueabi.json
No differences found in downloads_windows_x86_64_v4_gnueabihf.json
No differences found in downloads_windows_x86_64_v4_musl.json
No differences found in downloads_windows_x86_64_v4_none.json
No differences found in downloads_windows_x86_gnu.json
No differences found in downloads_windows_x86_gnueabi.json
No differences found in downloads_windows_x86_gnueabihf.json
No differences found in downloads_windows_x86_musl.json
No differences found in downloads_windows_x86_none.json
```

![image](https://github.com/user-attachments/assets/39c65698-d41e-47a6-8497-ffc5497bfa2a)



---

_@MeitarR reviewed on 2025-03-22 21:10_

---

_Review comment by @MeitarR on `crates/uv-python/minify-download-metadata.py`:11 on 2025-03-22 21:10_

BTW we can later improve it by moving this logic to the compile time and then select only the versions that are relevant to the compiled platform

---

_@MeitarR reviewed on 2025-03-22 21:12_

---

_Review comment by @MeitarR on `crates/uv-python/src/downloads.rs`:819 on 2025-03-22 21:12_

fixed d06cbc0

---

_@MeitarR reviewed on 2025-03-22 21:13_

---

_Review comment by @MeitarR on `crates/uv-python/template-download-metadata.py`:80 on 2025-03-22 21:13_

fixed 2e44711

---

_@MeitarR reviewed on 2025-03-22 21:13_

---

_Review comment by @MeitarR on `crates/uv-python/template-download-metadata.py`:73 on 2025-03-22 21:13_

fixed 2e44711

---

_@MeitarR reviewed on 2025-03-22 21:15_

---

_Review comment by @MeitarR on `crates/uv-python/src/downloads.rs`:518 on 2025-03-22 21:15_

nice catch! added for the `Serde` error a5f1bae. the `Io` one felt to me enough as the path is already included in the print from `Io`

---

_Review requested from @Gankra by @MeitarR on 2025-03-22 21:16_

---

_Review requested from @zanieb by @MeitarR on 2025-03-28 15:29_

---

_Assigned to @Gankra by @zanieb on 2025-04-01 18:33_

---

_Review comment by @Gankra on `crates/uv-python/src/downloads.rs`:776 on 2025-04-04 14:08_

If this was just processing our input I'd be strict to make sure that upstream PBS features get proper support in uv. But when taking user input being lenient is probably fair... maybe this function can take a boolean flag so the two differ?

---

_@Gankra approved on 2025-04-04 14:11_

I think this looks good now! Just one minor improvement but I could take it or leave it.

---

_@j178 reviewed on 2025-04-04 14:42_

---

_Review comment by @j178 on `crates/uv-python/src/downloads.rs`:13 on 2025-04-04 14:42_

Should we use `std::cell::OnceCell` instead?

---

_@MeitarR reviewed on 2025-04-04 15:00_

---

_Review comment by @MeitarR on `crates/uv-python/src/downloads.rs`:776 on 2025-04-04 15:00_

I think adding a Boolean parameter to this function is not so clean (what would it be called?), but I don't have a strong feeling about it, so feel free to modify it as you wish

Also, I'm not sure if it's the right place to enforce it; maybe a unit test will be a better fit (that that check we support all the wanted implementations) 

---

_@MeitarR reviewed on 2025-04-04 15:01_

---

_Review comment by @MeitarR on `crates/uv-python/src/downloads.rs`:13 on 2025-04-04 15:01_

I wanted to, but [get_or_try_init](https://doc.rust-lang.org/std/cell/struct.OnceCell.html#method.get_or_try_init) is nightly-only

---

_Merged by @Gankra on 2025-04-07 17:55_

---

_Closed by @Gankra on 2025-04-07 17:55_

---

_Comment by @Gankra on 2025-04-07 17:56_

Huge kudos for the extreme diligence on landing this!

(security note: i deleted and regenerated the artifacts locally, produced the exact same result)

---

_Branch deleted on 2025-04-11 13:45_

---

_Comment by @geofft on 2025-07-17 19:22_

I'm adding remote URL support in #14687, for the use case of making rollbacks easier if we break something in python-build-standalone, but likely also useful for maintaining your own mirror of Python builds.

---
