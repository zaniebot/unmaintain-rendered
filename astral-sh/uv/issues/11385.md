```yaml
number: 11385
title: "Write `--no-cache`'s temporary cache into venv not /tmp to allow use of hardlinks in more cases"
type: issue
state: open
author: edmorley
labels:
  - enhancement
assignees: []
created_at: 2025-02-10T14:13:48Z
updated_at: 2025-07-02T15:17:05Z
url: https://github.com/astral-sh/uv/issues/11385
synced_at: 2026-01-10T03:32:45Z
```

# Write `--no-cache`'s temporary cache into venv not /tmp to allow use of hardlinks in more cases

---

_Issue opened by @edmorley on 2025-02-10 14:13_

### Summary

For our use case (https://github.com/heroku/heroku-buildpack-python/issues/1616 / https://github.com/heroku/buildpacks-python/issues/248) it's preferable for us to cache `site-packages` between builds, instead of uv's cache. 

As such, we don't need/want uv's cache to be persisted.

As mentioned in the docs uv always needs a cache, so when using its `--no-cache` option a cache is still written, but into a temporary directory:
https://docs.astral.sh/uv/concepts/cache/#cache-directory

Currently (uv 0.5.29) this temporary cache is written into the system temp dir:
https://github.com/astral-sh/uv/blob/cbb94e40b391dc6376498710fb025ddc7a1b4385/crates/uv-cache/src/cli.rs#L44-L45
https://github.com/astral-sh/uv/blob/cbb94e40b391dc6376498710fb025ddc7a1b4385/crates/uv-cache/src/lib.rs#L152-L160

However, in some environments (including several of ours) `/tmp` is on a different device/mount than the environment into which packages are being installed. (In our case a Kubernetes pod, with sub-path mounts mounted at `/app`, `/tmp` and `/home` etc over the read-only container image).

Since hardlinks can't be made cross-device/mount, this then unsurprisingly results in:

```
warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
       If the cache and target directories are on different filesystems, hardlinking may not be supported.
       If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
```

We could work around this by using `--cache-dir <some path on the same mount as site-packages>`, however:
- `--cache-dir` is (understandably) ignored when using `--no-cache` (side note: perhaps those options should be marked as conflicting, at least when both are specified as CLI args, rather than env vars?). And without `--no-cache` we then have to perform manual clean-up ourselves.
- It requires us to calculate a custom `--cache-dir` based on the location of `site-packages` (which can vary between our environments and end user configuration)

...both of which seems like extra work that uv should be handling for us when we use `--no-cache`?

It seems that if `--no-cache` instead defaulted to co-locating the temporary cache inside the venv (and/or the environment specified by `UV_PROJECT_ENVIRONMENT`), then it would be much more likely that hardlinks would work, and uv would give the best performance out of the box in more environments?

### Example

_No response_

---

_Label `enhancement` added by @edmorley on 2025-02-10 14:13_

---

_Comment by @zanieb on 2025-02-10 15:41_

Seems reasonable to co-locate these, cc @charliermarsh 

---

_Comment by @charliermarsh on 2025-02-11 00:14_

That seems reasonable. (It's a little bit of an annoying change since we initialize the cache "far away" from where we discover the virtual environment.)

---

_Comment by @zanieb on 2025-02-11 00:29_

I guess there's like..

```diff
diff --git a/crates/uv-cache/src/lib.rs b/crates/uv-cache/src/lib.rs
index 4049d3f4b..706f0f3c3 100644
--- a/crates/uv-cache/src/lib.rs
+++ b/crates/uv-cache/src/lib.rs
@@ -159,6 +159,27 @@ impl Cache {
         })
     }
 
+    /// If the cache is temporary, relocate it to a temporary directory within the given base.
+    pub fn relocate_temp(self, base: &PathBuf) -> Result<Self, io::Error> {
+        let Some(temp) = self.temp_dir else {
+            return Ok(self);
+        };
+        let new_temp = tempfile::tempdir_in(base)?;
+        for child in fs_err::read_dir(temp.path())? {
+            let child = child?;
+            let path = child.path();
+            if let Some(name) = path.file_name() {
+                let new_path = new_temp.path().join(name);
+                fs_err::rename(path, &new_path)?;
+            }
+        }
+        Ok(Self {
+            root: new_temp.path().to_path_buf(),
+            refresh: self.refresh,
+            temp_dir: Some(Arc::new(new_temp)),
+        })
+    }
+
     /// Set the [`Refresh`] policy for the cache.
     #[must_use]
     pub fn with_refresh(self, refresh: Refresh) -> Self {
```

---

_Comment by @zanieb on 2025-02-11 00:29_

(I wish you could construct a `TempDir` from an existing path)

---

_Comment by @rahulnht on 2025-02-13 09:08_

Another usecase, where having cache defaultdir in venv is useful, is while running uv inside docker (with non-root privileges). /var (or other temp) directories are not r/w by default with standard user, and to avoid privilege escalation just for creating a cache, we're currently setting  `UV_CACHE_DIR=./.cache` as a workaround.

Would be nice to have this in the same space as venv to avoid such workarounds

---

_Comment by @shaneikennedy on 2025-02-13 12:46_

I drafted something here, it's a naive approach but I think it solves everyone's problems here and doesn't couple the virtualenv implementation with the cache implementation. Let me know what you think 👍 [Make the cache dir wherever uv is called from when --no-cache specified #11477](https://github.com/astral-sh/uv/pull/11477)

---

_Comment by @daniel-albuschat on 2025-06-24 12:26_

Please also consider read-only filesystems when implementing this. It is a security best practice to run your containers with read-only root filesystems. We already made an exclusion to this for `uv` to the `/tmp/` directory, which we mount into a per-instance empty directory, and set `UV_CACHE_DIR` to something in `/tmp/`. We didn't need this for `poetry` or `pipenv`.

It seems that the latest version of `uv` already tries to write something in `./.venv/` as of very recently, which, btw., made our production services break, because `pwd` is read-only. 😢

Thanks!
Daniel

---

_Comment by @charliermarsh on 2025-06-24 13:27_

@daniel-albuschat -- In that case, what if we wrote the file locks to a configurable directory, like `UV_LOCK_DIR`? Would that solve your problem?

---

_Comment by @daniel-albuschat on 2025-06-24 14:09_

@charliermarsh Yes, that would definitely help. Even more helpful would be one variable that could be used as a root for everything that uv writes to, so that if `UV_CACHE_DIR` and the theoretical `UV_LOCK_DIR` (and potentially other directories/places) were not set, they would default to e.g. `${UV_TEMP_DIR}/uv-lock` AND `${UV_TEMP_DIR}/uv-cache`, respectively. Or something similar. Having one variable would be future-proof.

---

_Comment by @coretl on 2025-06-30 15:58_

I found this ticket while trying to make a `devcontainer.json` that uses uv, and I wanted to comment with my attempt (and failure) to keep hard link mode when `.venv` is a volume mount. 

I followed the docs:
https://docs.astral.sh/uv/guides/integration/docker/#developing-in-a-container

Which led me to:
```json
    // Mount the venv as a volume so it doesn't exist outside the container
    "mounts": [
        {
            "target": "/workspaces/${localWorkspaceFolderBasename}/.venv",
            "type": "volume"
        }
    ],
```
but then had to change `UV_LINK_MODE=copy` to silence the warnings as per the docs.

I wondered if I could keep the hard link mode if I set `UV_CACHE_DIR="/workspaces/${localWorkspaceFolderBasename}/.venv/uv_cache"`, but that gives an error:
```
error: Project virtual environment directory `/workspaces/project/.venv` cannot be used because it is not a valid Python environment (no Python executable was found)
```
and inspecting `.venv` I only see a `uv_cache` directory within it.

I would like to add my vote for being to colocate the venv and the cache in the same volume for devcontainer purposes.

---

_Comment by @zanieb on 2025-06-30 16:02_

I think in that case you'd want

```
    "mounts": [
        {
            "target": "/workspaces/${localWorkspaceFolderBasename}/.uv",
            "type": "volume"
        }
    ],
```
```
UV_CACHE_DIR=/workspaces/${localWorkspaceFolderBasename}/.uv/cache
UV_PROJECT_ENVIRONMENT=/workspaces/${localWorkspaceFolderBasename}/.uv/env
```

---

_Comment by @coretl on 2025-07-01 16:09_

Thank you, that works.

However I am starting to think that doing a volume mount of the project env won't work for me as I use the same devcontainer to mount an entire directory of python projects in, and would like to be able to `uv sync` any one of these. I'm debating using a `UV_CACHE_DIR` that is a volume mount, then `.venv` for each project would be on the host, but using `UV_LINK_MODE=symlink` so the venv would only work within the container. Initial attempts seem like this would work, although I don't know if there are any performance concerns with using `UV_LINK_MODE=symlink`. I'll do some more testing.


---
