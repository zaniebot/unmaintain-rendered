```yaml
number: 14687
title: "Support http/https URLs in `uv python --python-downloads-json-url`"
type: pull_request
state: open
author: geofft
labels:
  - enhancement
assignees: []
base: main
head: geofft/python-download-old-versions
created_at: 2025-07-17T18:54:32Z
updated_at: 2025-10-14T11:59:24Z
url: https://github.com/astral-sh/uv/pull/14687
synced_at: 2026-01-10T06:36:15Z
```

# Support http/https URLs in `uv python --python-downloads-json-url`

---

_Pull request opened by @geofft on 2025-07-17 18:54_

_No description provided._

---

_Review requested from @zanieb by @geofft on 2025-07-17 18:54_

---

_Review requested from @jtfmumm by @geofft on 2025-07-17 18:54_

---

_Review requested from @konstin by @geofft on 2025-07-17 18:54_

---

_Comment by @geofft on 2025-07-17 19:11_

I'll fix up docs and I can add some test coverage.

This lets you do things like
```
target/debug/uv python list --python-downloads-json-url https://github.com/astral-sh/uv/raw/0.7.12/crates/uv-python/download-metadata.json
```

The high-level goal here is making it a little bit easier for users to roll back if we unexpectedly break something in python-build-standalone. I also intend to add, likely in separate PRs,
* a `--uv-tag 0.7.12` option that is syntax sugar for the above
* maybe some form of explicit versioning to the JSON, retroactively calling the current format version 1 but printing a nice message if we see `{"version": "2",  ...}` (we do not necessarily have to ever move to version 2, but having the error is nice)
* something to astral-sh/setup-uv to let you use this option

---

_@zanieb reviewed on 2025-07-17 22:30_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:373 on 2025-07-17 22:30_

Why do we need to clone here? That seems weird?

---

_Comment by @zanieb on 2025-07-17 22:32_

I might need a couple sentence summary in the PR description about the high-level approach here.

---

_Comment by @geofft on 2025-07-17 23:18_

> I might need a couple sentence summary in the PR description about the high-level approach here.

In terms of implementation, or something more like design/intended semantics? In terms of implementation, this is broken down into two commits which you might prefer for reviewing, but the change is basically:
- Instead of having static methods on `ManagedPythonDownload` that populate a `once_cell`, make an explicit `ManagedPythonDownloader` object to hold that state, and adjust a handful of `'static` lifetimes as needed
- Make the `ManagedPythonDownloader` constructor async, so we can do HTTP requests from it
- Also have its callers pass in a client
- Actually do the HTTP request

Oh, one note which I forgot to mention is that per [`serde::from_reader`'s docs](https://docs.rs/serde_json/latest/serde_json/fn.from_reader.html), you definitely shouldn't call it on an unbuffered reader (I ran `strace` and confirmed it's `read`(2)ing one byte at a time) and you might still get better results calling it on a big string than on a buffered reader. So I'm reading the whole thing into memory regardless of source, which should be fine because it's under two megabytes at the moment.

In terms of semantics, this breaks the exciting corner case of parsing `irc://localhost/path/to/file.json` as the local file `/path/to/file.json` and instead treats that as a filename. An `http`/`https` URL is handled (hopefully with the normal set of network configuration for uv?), a valid `file` URL is handled, an invalid `file` URL (with a hostname, except on Windows where you get a UNC path back, or with `%00` or something similarly exciting) is an error, and everything else is treated as a path. There should be no other changes to semantics.

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:240 on 2025-07-18 10:12_

Not your fault but I'm only now seeing it.

```suggestion
            .map(|request| InstallRequest::new(request, &downloader))
```

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:247 on 2025-07-18 10:12_

```suggestion
            .map(|request| InstallRequest::new(request, &downloader))
```

---

_Review comment by @konstin on `crates/uv-python/src/downloads.rs`:376 on 2025-07-18 10:16_

```suggestion
        downloader
            .iter_all()
            .filter(move |download| self.satisfied_by_download(download))
```

---

_Review comment by @konstin on `crates/uv/src/commands/python/list.rs`:118 on 2025-07-18 10:18_

```suggestion
            .map(|request| request.iter_downloads(&downloader))
```

---

_Review comment by @konstin on `crates/uv-python/src/downloads.rs`:638 on 2025-07-18 10:19_

This call looks backwards, flipping the method so that the call is `self.iter_downloads(request)` would look more natural

---

_Review comment by @konstin on `crates/uv-python/src/downloads.rs`:668 on 2025-07-18 11:06_

I tried improving the URL parsing and reusability situation in https://github.com/astral-sh/uv/pull/14712, but I didn't get anything that's noticeable better than the current logic

---

_@konstin reviewed on 2025-07-18 11:07_

Needs a test as mentioned, the code is looking good already.

---

_@geofft reviewed on 2025-07-18 12:23_

---

_Review comment by @geofft on `crates/uv-python/src/downloads.rs`:373 on 2025-07-18 12:23_

Huh, I needed this to get past some sort of lifetiime issue because the iterator captures `self`, but it seems fine now. I must have fixed something else better. I'll get rid of it, thanks!

---

_@zanieb reviewed on 2025-07-18 12:24_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:633 on 2025-07-18 12:24_

With the rename, this is a little weird? Like `ManagedPythonDownloader::from_request` -> `ManagedPythonDownload` when I'd expect `from_..` to return `Self`.

---

_@zanieb reviewed on 2025-07-18 12:26_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:629 on 2025-07-18 12:26_

I might rename this to `ManagedPythonDownloads` then have `download_for_request`and `iter_downloads` methods  (or just `find` and `iter`)? This doesn't actually download the versions, which is a little misleading.

---

_@geofft reviewed on 2025-07-18 12:27_

---

_Review comment by @geofft on `crates/uv-python/src/downloads.rs`:633 on 2025-07-18 12:27_

Yeah, for this and Konsti's comment on "this call looks backwards" I was trying to minimize the amount of textual churn, but probably better to not optimize for that (especially now that reviewers have seen the commits so far :) ), I can rework things a bit.

---

_@zanieb reviewed on 2025-07-18 12:27_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:664 on 2025-07-18 12:27_

This should have a docstring. What's the tldr on why this can't just take a `BaseClientBuilder`? I'm not worried about the performance, but it'd be a little clearer that it doesn't necessarily need a materialized client.

---

_@zanieb reviewed on 2025-07-18 12:28_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:679 on 2025-07-18 12:28_

nit: I'd probably write this as a `let Ok(...) else return` instead of `if let Ok(...) else return` so you can early return

---

_@zanieb reviewed on 2025-07-18 12:30_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:629 on 2025-07-18 12:30_

I think the rename is also a little confusing because the client you pass to this struct won't be used by `ManagedPythonDownload` for the actual download

---

_@geofft reviewed on 2025-07-18 12:30_

---

_Review comment by @geofft on `crates/uv-python/src/downloads.rs`:629 on 2025-07-18 12:30_

Yes, that occurred to me too. Maybe `ManagedPythonDownloadList` for a little more visual distinction than the plural?

---

_Label `enhancement` added by @konstin on 2025-07-18 12:31_

---

_@zanieb reviewed on 2025-07-18 12:31_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:664 on 2025-07-18 12:31_

I looked at the callsites and it seems like you have a `BaseClientBuilder` you could provide instead?

---

_@zanieb reviewed on 2025-07-18 12:34_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:629 on 2025-07-18 12:34_

Yeah I initially suggested that too and edited it out. I don't mind either way — `ManagedPythonDownloads` is a little closer to our typical naming in the project.

---

_@zanieb reviewed on 2025-07-18 12:34_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:633 on 2025-07-18 12:34_

Would be resolved by https://github.com/astral-sh/uv/pull/14687#discussion_r2215923723

Makes sense. In general, don't be afraid to refactor in uv.

---

_@geofft reviewed on 2025-07-18 12:35_

---

_Review comment by @geofft on `crates/uv-python/src/downloads.rs`:664 on 2025-07-18 12:35_

It could, the only reason is that two of its callers construct a client shortly after calling this, and so if we passed a builder, we'd be calling `.build()` twice.. If that's not a problem I can definitely change that (and lazy-construct the client if we're in the HTTP(S) case only). But I was worried I'd have to do something like pass a `&mut Option<BaseClient>` to pass the constructed client back to the caller to reuse it, or something.

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:664 on 2025-07-28 22:07_

I think it's probably fine if we build twice.

---

_@zanieb reviewed on 2025-07-28 22:07_

---

_Renamed from "Support http/https URLs in uv python --python-downloads-json-url" to "Support http/https URLs in `uv python --python-downloads-json-url`" by @konstin on 2025-08-04 17:56_

---

_Review comment by @konstin on `crates/uv-python/src/downloads.rs`:722 on 2025-08-06 10:21_

let-chains are not supported by our minimum rust version yet :/ (if CI still passes with that after rebasing we need to fix CI)

---

_Review comment by @konstin on `crates/uv-settings/src/settings.rs`:833 on 2025-08-06 10:23_

This reads like there is a particle missing

---

_@konstin reviewed on 2025-08-06 10:24_

we need a (wiremock) test, otherwise r+ me on the code.

---

_Comment by @MeitarR on 2025-10-14 11:56_

Hi @geofft , this feature is something I'm very interested in as well.

I noticed there hasn't been any recent activity - are you still planning to work on it? If not, I'd be happy to help move it forward or open a continuation PR based on your work.

---

_Review request for @jtfmumm removed by @konstin on 2025-10-14 11:59_

---
