```yaml
number: 7735
title: "Issue with Installing Torch-TensorRT via `uv add` Command with External Repository Links"
type: issue
state: closed
author: ChenMoFeiJin
labels:
  - bug
assignees: []
created_at: 2024-09-27T10:32:58Z
updated_at: 2025-01-02T17:43:16Z
url: https://github.com/astral-sh/uv/issues/7735
synced_at: 2026-01-10T04:36:20Z
```

# Issue with Installing Torch-TensorRT via `uv add` Command with External Repository Links

---

_Issue opened by @ChenMoFeiJin on 2024-09-27 10:32_

---
**Platform:**  Ubuntu 20.04
**Version:**  uv 0.4.16
---

### Description:

When attempting to install **Torch-TensorRT** using the `uv add` command, I encountered an error. The command I ran was:

```bash
uv add torch-tensorrt
```

The following error was returned:

```
###########################################################################################
The package you are trying to install is only a placeholder project on PyPI.org repository.
To install Torch-TensorRT please run the following command:

$ pip install torch-tensorrt -f https://github.com/NVIDIA/Torch-TensorRT/releases
###########################################################################################
```

Installing the package manually with `pip` works perfectly fine:

```bash
pip install torch-tensorrt -f https://github.com/NVIDIA/Torch-TensorRT/releases
```

However, when trying to use `uv add` with the same link, the following error occurs:

```bash
$ uv add torch-tensorrt -f https://github.com/pytorch/TensorRT/releases --verbose
DEBUG uv 0.4.16
DEBUG Found project root: `/path/to/project`
DEBUG Found workspace root: `/path/to/project`
DEBUG Adding current workspace member: `/path/to/project`
DEBUG Reading requests from `/path/to/project/.python-version`
DEBUG The virtual environment's Python version satisfies `Python 3.12`
DEBUG Using request timeout of 30s
DEBUG No cache entry for: https://github.com/pytorch/TensorRT/releases
error: Failed to read `--find-links` URL: https://github.com/pytorch/TensorRT/releases
  Caused by: Received some unexpected HTML from https://github.com/pytorch/TensorRT/releases
  Caused by: Unexpected fragment (expected `#sha256=...` or similar) on URL: start-of-content
```

It seems that `uv` is unable to properly handle external `--find-links` URLs when installing from sources like GitHub, as it encounters an unexpected HTML response.

---

_Comment by @konstin on 2024-09-27 14:26_

It seems that torch-tensorrt doesn't publish wheels for Python 3.12 (only up to 3.11), so we fall back to the 0.0.0.post1 dummy package. On the github releases page there's a Python 3.12 wheel then (https://github.com/pytorch/TensorRT/releases/download/v2.4.0/torch_tensorrt-2.4.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.manylinux_2_34_x86_64.whl).

While I'm open to extending uv's find links parser to work more like pip, i'm hesitant to specifically support github releases: The page at https://github.com/pytorch/TensorRT/releases only has a few recent releases, which means that `uv lock` could break in the future when the released you had lock is moved to the second releases page. Ideally, the would be a single flat html page with all torch-tensorrt github releases that we could use.

---

_Comment by @ChenMoFeiJin on 2024-09-27 15:08_

Thank you for your feedback. You raise a good point about the limitations of GitHub releases. Indeed, since `uv.lock` records the exact `.whl` or `.tar.gz` download URL, we can rebuild the environment even if the release has moved beyond GitHub's first page. The file link stays active unless removed by GitHub. (I admit I may not fully grasp the workings of `uv.lock`.)

Hence, it would be wise to consider a more robust approach to managing GitHub releases. This could prove particularly useful for those dependent on specific versions not available on PyPI or other simple HTML pages. Although there's a slight risk of file removal, it's unlikely for widely used libraries such as Torch-TensorRT.

I understand if you find additional processing unnecessary. Thank you for explaining that Torch-TensorRT does not have a build for version 3.12 on PyPI, and it's not just a general placeholder. With this in mind, I'm thinking of downgrading to version 3.11 as a temporary workaround. If you think this issue warrants further discussion, please feel free to leave it open. Otherwise, we can consider it resolved.

---

_Comment by @matteo-bruni on 2024-09-27 16:58_

torch-tensorrt is available on index https://download.pytorch.org/whl/cu124 so you can install it using --extra-index-url
(you can also use https://download.pytorch.org/whl/cu118/ or https://download.pytorch.org/whl/cu121 for other cuda version)

you also need to use --index-strategy unsafe-best-match because it needs packaging>23 and in the pytorch index only packaging==22.0 is available (why they release 3rd party wheels?? same problem with pillow which is not up to date with upstream)



---

_Comment by @goldyfruit on 2024-12-22 23:08_

Kind of having the same issue with https://whl.smartgic.io/.

```
uv pip install -f https://whl.smartgic.io/ ggwave
Using Python 3.11.11 environment at: /home/goldyfruit/Virtualenvs/hivemind
error: Failed to read `--find-links` URL: https://whl.smartgic.io/
  Caused by: Received some unexpected HTML from https://whl.smartgic.io/
  Caused by: Missing href attribute on anchor link: `/`
```

Not sure how to fix this.

---

_Comment by @moreati on 2025-01-02 15:36_

In the source of https://whl.smartgic.io/, it appears the anchor that uv is complaining about _does_ have an href, but the value is empty string.

```html
<body onload="initPage()">
	<header>
		<div class="wrapper">
			<div class="breadcrumbs">Folder Path</div>
				<h1>
					<a href="">/</a>
				</h1>
```

If I'm reading the source of Python pip correctly, then it ignores empty or missing href attributes - skipping over them

- `pip._internal.index.collector.HTMLLinkParser` gathers dict objects of `<a>` attributes in `self.anchors`
  https://github.com/pypa/pip/blob/bc553db53c264abe3bb63c6bcd6fc6f303c6f6e3/src/pip/_internal/index/collector.py#L291-L292 
- `pip._internal.index.collector.parse_links()` yields objects returned by `Link.from_element()` if not None
  https://github.com/pypa/pip/blob/bc553db53c264abe3bb63c6bcd6fc6f303c6f6e3/src/pip/_internal/index/collector.py#L245-L249
- `pip._internal.model.link.Link.from_element()` returns None if the href attribute is absent, or the value is falsey
  https://github.com/pypa/pip/blob/bc553db53c264abe3bb63c6bcd6fc6f303c6f6e3/src/pip/_internal/models/link.py#L321-L323

So if it was desired `uv pip` could be made more `pip` like by ignoring missing or empty `<a href>`, rather than treating it as an error.

---

_Comment by @charliermarsh on 2025-01-02 16:01_

Ok thanks. I can fix it.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-02 16:32_

---

_Label `bug` added by @charliermarsh on 2025-01-02 16:32_

---

_Closed by @charliermarsh on 2025-01-02 17:43_

---
