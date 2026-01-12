```yaml
number: 2216
title: "Data descriptors aren't supported in zip files"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - registry
assignees: []
created_at: 2024-03-05T19:24:31Z
updated_at: 2024-04-05T20:23:49Z
url: https://github.com/astral-sh/uv/issues/2216
synced_at: 2026-01-12T15:58:36Z
```

# Data descriptors aren't supported in zip files

---

_@charliermarsh_

If you run:

```shell
cargo run pip install --extra-index-url https://buf.build/gen/python hashb_foxglove_protocolbuffers_python==25.3.0.1.20240226043130+465630478360 --force-reinstall -n
```

You'll hit an error like:

```
error: Failed to download: hashb-foxglove-protocolbuffers-python==25.3.0.1.20240226043130+465630478360
  Caused by: The wheel hashb_foxglove_protocolbuffers_python-25.3.0.1.20240226043130+465630478360-py3-none-any.whl is not a valid zip file
  Caused by: feature not supported: 'stream reading entries with data descriptors (planned to be reintroduced)'
```


---

_Label `bug` added by @charliermarsh on 2024-03-05 19:24_

---

_Label `registry` added by @charliermarsh on 2024-03-05 19:24_

---

_Comment by @akshayjshah on 2024-03-05 22:05_

👋🏽 Hi from Buf, the company running the `buf.build` registry! Happy to chat about why and how we're using this feature if it's helpful.

Regardless, we (and our customers) really appreciate the attention you're giving to #2202 😻 

---

_Comment by @charliermarsh on 2024-03-05 22:54_

Hey thanks for chiming in! I'd be happy to hear about why / how you're using data descriptors. I'd like to support them but it'll require either making some upgrades to the `async_zip` crate or adding fallback on our end to do a synchronous download-then-unzip. (Right now, we unzip directly to disk as we stream the download in-memory.)

---

_Comment by @stefanvanburen on 2024-03-06 13:52_

Hi Charlie - I don't think our use of data descriptors is intentional, just what our [downstream library (klauspost/compress) does when creating zip files](https://github.com/klauspost/compress/blob/master/zip/writer.go#L357). FWIW, it seems like similar behavior [exists in the Go standard library](https://cs.opensource.google/go/go/+/refs/tags/go1.22.1:src/archive/zip/writer.go;l=358). The calling code is [roughly here](https://github.com/bufbuild/buf/blob/main/private/pkg/storage/storagearchive/storagearchive.go#L162-L175), but it's nothing special - I believe any call to `zip.Writer.Create` with a regular file will write a data descriptor.

Hope that helps - not sure there's much we can do on our end, but I'll be watching over at https://github.com/Majored/rs-async-zip/issues/122 to see if things can be resolved over there. Thanks again for all the work!

---

_Comment by @charliermarsh on 2024-03-06 14:22_

Thanks @stefanvanburen -- I can also try to take a look today. We use an async-zip fork anyway (https://github.com/charliermarsh/rs-async-zip) so we can easily experiment with different behaviors.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-06 14:22_

---

_Comment by @charliermarsh on 2024-03-06 19:46_

Data descriptors add a lot of complexity for streaming. In ZIP, each entry looks like [header][file contents][data descriptor]. The [data descriptor] is optional, and the [header] tells you if it's present. The [header] tells you the length of [file contents], so that you can read the body. But if [data descriptor] is present, the the length is in [data descriptor], not [header]... So you can't stream it (trivially), because you don't know the length of the file until you're past it.

In theory it should be possible to make it work for Deflate, since the compressed body will be self-terminating. Alternatively, we could fall back to downloading the entire file, then unzipping it from disk.


---

_Comment by @charliermarsh on 2024-03-06 22:34_

I think the best path is to catch this error and fall back to downloading + unzipping from disk. It's not _that_ much of a slowdown.

---

_Closed by @charliermarsh on 2024-03-10 15:39_

---

_Comment by @charliermarsh on 2024-04-05 20:23_

We now support data descriptors natively.

---
