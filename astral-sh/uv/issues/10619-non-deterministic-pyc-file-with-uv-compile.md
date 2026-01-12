```yaml
number: 10619
title: "Non-deterministic .pyc file with `UV_COMPILE_BYTECODE`"
type: issue
state: open
author: jackvreeken
labels:
  - bug
  - external
assignees: []
created_at: 2025-01-14T23:50:15Z
updated_at: 2025-02-06T22:04:29Z
url: https://github.com/astral-sh/uv/issues/10619
synced_at: 2026-01-12T16:00:17Z
```

# Non-deterministic .pyc file with `UV_COMPILE_BYTECODE`

---

_@jackvreeken_

While playing around with getting deterministic builds to work (see #10523 ), I noticed something odd. I was getting two possible image digests, with the difference between the two solely being because of a differently sized .pyc file (one is a few bytes larger than the other).

The offending file seems to be `scipy/stats/tests/__pycache__/test_sampling.cpython-313.pyc` , and only that one for some reason. Because the source file is 54 KB, and because I also wanted to upload the two resulting bytecode files, I made a repository instead of a gist: https://github.com/jackvreeken/uv-scipy-bytecode-issue/tree/87a8691b75f60007c277866da76bf70b7f799845 . The folder names refer to the first four characters of the digest I was getting at the time.

I tried to figure out if there's a difference between the bytecodes with `dis`, but that says they are identical (if I filter out the "at 0x...., file " differences).

I hope it's something stupid on my end, I just found it rather suspicious (and messing with deterministic builds).

<details>
  <summary>uv.lock, Dockerfile, and Docker command</summary>

uv.lock section of SciPy
```
[[package]]
name = "scipy"
version = "1.15.1"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "numpy" },
]
sdist = { url = "https://files.pythonhosted.org/packages/76/c6/8eb0654ba0c7d0bb1bf67bf8fbace101a8e4f250f7722371105e8b6f68fc/scipy-1.15.1.tar.gz", hash = "sha256:033a75ddad1463970c96a88063a1df87ccfddd526437136b6ee81ff0312ebdf6", size = 59407493 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/2b/bf/dd68965a4c5138a630eeed0baec9ae96e5d598887835bdde96cdd2fe4780/scipy-1.15.1-cp313-cp313-macosx_10_13_x86_64.whl", hash = "sha256:100193bb72fbff37dbd0bf14322314fc7cbe08b7ff3137f11a34d06dc0ee6b85", size = 41441136 },
    { url = "https://files.pythonhosted.org/packages/ef/5e/4928581312922d7e4d416d74c416a660addec4dd5ea185401df2269ba5a0/scipy-1.15.1-cp313-cp313-macosx_12_0_arm64.whl", hash = "sha256:2114a08daec64980e4b4cbdf5bee90935af66d750146b1d2feb0d3ac30613692", size = 32533699 },
    { url = "https://files.pythonhosted.org/packages/32/90/03f99c43041852837686898c66767787cd41c5843d7a1509c39ffef683e9/scipy-1.15.1-cp313-cp313-macosx_14_0_arm64.whl", hash = "sha256:6b3e71893c6687fc5e29208d518900c24ea372a862854c9888368c0b267387ab", size = 24807289 },
    { url = "https://files.pythonhosted.org/packages/9d/52/bfe82b42ae112eaba1af2f3e556275b8727d55ac6e4932e7aef337a9d9d4/scipy-1.15.1-cp313-cp313-macosx_14_0_x86_64.whl", hash = "sha256:837299eec3d19b7e042923448d17d95a86e43941104d33f00da7e31a0f715d3c", size = 27929844 },
    { url = "https://files.pythonhosted.org/packages/f6/77/54ff610bad600462c313326acdb035783accc6a3d5f566d22757ad297564/scipy-1.15.1-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:82add84e8a9fb12af5c2c1a3a3f1cb51849d27a580cb9e6bd66226195142be6e", size = 38031272 },
    { url = "https://files.pythonhosted.org/packages/f1/26/98585cbf04c7cf503d7eb0a1966df8a268154b5d923c5fe0c1ed13154c49/scipy-1.15.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:070d10654f0cb6abd295bc96c12656f948e623ec5f9a4eab0ddb1466c000716e", size = 40210217 },
    { url = "https://files.pythonhosted.org/packages/fd/3f/3d2285eb6fece8bc5dbb2f9f94d61157d61d155e854fd5fea825b8218f12/scipy-1.15.1-cp313-cp313-musllinux_1_2_x86_64.whl", hash = "sha256:55cc79ce4085c702ac31e49b1e69b27ef41111f22beafb9b49fea67142b696c4", size = 42587785 },
    { url = "https://files.pythonhosted.org/packages/48/7d/5b5251984bf0160d6533695a74a5fddb1fa36edd6f26ffa8c871fbd4782a/scipy-1.15.1-cp313-cp313-win_amd64.whl", hash = "sha256:c352c1b6d7cac452534517e022f8f7b8d139cd9f27e6fbd9f3cbd0bfd39f5bef", size = 43640439 },
    { url = "https://files.pythonhosted.org/packages/e7/b8/0e092f592d280496de52e152582030f8a270b194f87f890e1a97c5599b81/scipy-1.15.1-cp313-cp313t-macosx_10_13_x86_64.whl", hash = "sha256:0458839c9f873062db69a03de9a9765ae2e694352c76a16be44f93ea45c28d2b", size = 41619862 },
    { url = "https://files.pythonhosted.org/packages/f6/19/0b6e1173aba4db9e0b7aa27fe45019857fb90d6904038b83927cbe0a6c1d/scipy-1.15.1-cp313-cp313t-macosx_12_0_arm64.whl", hash = "sha256:af0b61c1de46d0565b4b39c6417373304c1d4f5220004058bdad3061c9fa8a95", size = 32610387 },
    { url = "https://files.pythonhosted.org/packages/e7/02/754aae3bd1fa0f2479ade3cfdf1732ecd6b05853f63eee6066a32684563a/scipy-1.15.1-cp313-cp313t-macosx_14_0_arm64.whl", hash = "sha256:71ba9a76c2390eca6e359be81a3e879614af3a71dfdabb96d1d7ab33da6f2364", size = 24883814 },
    { url = "https://files.pythonhosted.org/packages/1f/ac/d7906201604a2ea3b143bb0de51b3966f66441ba50b7dc182c4505b3edf9/scipy-1.15.1-cp313-cp313t-macosx_14_0_x86_64.whl", hash = "sha256:14eaa373c89eaf553be73c3affb11ec6c37493b7eaaf31cf9ac5dffae700c2e0", size = 27944865 },
    { url = "https://files.pythonhosted.org/packages/84/9d/8f539002b5e203723af6a6f513a45e0a7671e9dabeedb08f417ac17e4edc/scipy-1.15.1-cp313-cp313t-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:f735bc41bd1c792c96bc426dece66c8723283695f02df61dcc4d0a707a42fc54", size = 39883261 },
    { url = "https://files.pythonhosted.org/packages/97/c0/62fd3bab828bcccc9b864c5997645a3b86372a35941cdaf677565c25c98d/scipy-1.15.1-cp313-cp313t-musllinux_1_2_x86_64.whl", hash = "sha256:2722a021a7929d21168830790202a75dbb20b468a8133c74a2c0230c72626b6c", size = 42093299 },
    { url = "https://files.pythonhosted.org/packages/e4/1f/5d46a8d94e9f6d2c913cbb109e57e7eed914de38ea99e2c4d69a9fc93140/scipy-1.15.1-cp313-cp313t-win_amd64.whl", hash = "sha256:bc7136626261ac1ed988dca56cfc4ab5180f75e0ee52e58f1e6aa74b5f3eacd5", size = 43181730 },
]
```

Docker file
```Dockerfile
FROM ghcr.io/astral-sh/uv:0.5.18 AS uv

# First, bundle the dependencies into the task root.
FROM public.ecr.aws/lambda/python:3.13 AS builder

# Enable bytecode compilation, to improve cold-start performance.
ENV UV_COMPILE_BYTECODE=1

# Disable installer metadata, to create a deterministic layer.
ENV UV_NO_INSTALLER_METADATA=1

# Setting source date epoch to any value enables hash-based Python bytecode invalidation
# instead of timestamp-based for reproducibility.
# Here, we also use the value to set the timestamps of dependencies and application code.
ENV SOURCE_DATE_EPOCH=0

# Enable copy mode to support bind mount caching.
ENV UV_LINK_MODE=copy

# Bundle the dependencies into the Lambda task root via `uv pip install --target`.
#
# Omit any local packages (`--no-emit-workspace`) and development dependencies (`--no-dev`).
# This ensures that the Docker layer cache is only invalidated when the `pyproject.toml` or `uv.lock`
# files change, but remains robust to changes in the application code.
RUN --mount=from=uv,source=/uv,target=/bin/uv \
    --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    --mount=type=bind,source=some_package/pyproject.toml,target=some_package/pyproject.toml \
    uv export --project some_package --frozen --no-emit-workspace --no-dev --no-editable -o requirements.txt && \
    uv pip install -r requirements.txt --target "${LAMBDA_TASK_ROOT}"

# Copy the application code into the builder image and set the timestamps.
# This layer is invalidated when the application code changes.
COPY some_package "${LAMBDA_TASK_ROOT}"/app

FROM public.ecr.aws/lambda/python:3.13

# Copy the runtime dependencies and application code into the final image.
COPY --from=builder "${LAMBDA_TASK_ROOT}" "${LAMBDA_TASK_ROOT}"

CMD ["app/handler.lambda_handler"]
```

Build command:
```
docker buildx build --no-cache --build-arg SOURCE_DATE_EPOCH=0 --build-arg BUILDKIT_CREATED_AT=1970-01-01T00:00:00Z --platform linux/amd64 --provenance=false -t test-image-1 --output type=docker,buildinfo=false,rewrite-timestamp=true -f path/to/Dockerfile .
```
</details>

---

_Comment by @zanieb on 2025-01-15 00:31_

I think this is discussed at https://github.com/astral-sh/uv/issues/10523 too.

Specifically

> all bytecode files, a timestamp is written into the header of each one to decide when to invalidate it

Or is this referring to something else?

---

_Comment by @jackvreeken on 2025-01-15 00:44_

This issue is related to https://github.com/astral-sh/uv/issues/10523 in the sense that we're both trying to accomplish deterministic docker builds. Wheres #10523 focuses on time stamps on files, this issue focuses on an inconsistent file size. Sometimes `uv` builds a slightly bigger .pyc than other times (there are two options), and only for the file mentioned. 

I changed the wording in the opening to reflect that it's a about a **size** difference. 


> all bytecode files, a timestamp is written into the header of each one to decide when to invalidate it

I don't run into any issues on that front (so maybe the allegedly included time stamp is the same? or omitted?), but I don't know `pyc` file format that well.

```
$ cmp d0ab/test_sampling.cpython-313.pyc 9797/test_sampling.cpython-313.pyc
d0ab/test_sampling.cpython-313.pyc 9797/test_sampling.cpython-313.pyc differ: char 32818, line 138
```
Does not seem like it's the header at least.

---

_Comment by @zanieb on 2025-01-15 00:58_

Interesting, thanks for clarifying!

I guess the next step is to look at how we compile the files and see if it's our problem or an upstream issue :) 

The script is at https://github.com/astral-sh/uv/blob/a6602ad416a922060efb3c5f19daf4977c50ace3/crates/uv-installer/src/pip_compileall.py

Can you reproduce with `pip`? Can you reproduce with `python -m compileall`?

---

_Comment by @jackvreeken on 2025-01-15 14:20_

I have a hard time getting pip to do anything close to deterministic, so just tested `python -m compileall` and `uv pip install` (see https://github.com/jackvreeken/uv-scipy-bytecode-issue/tree/a69c4dd696d61c41f2b40c322ecd5971cf113970 for Dockerfiles, build command, etc).

I performed a 100 builds of each, to see how often it would return which digest:

```
ENV UV_NO_INSTALLER_METADATA=1
ENV SOURCE_DATE_EPOCH=0
ENV UV_LINK_MODE=copy

RUN --mount=from=uv,source=/uv,target=/bin/uv \
    --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv export --frozen --no-emit-workspace --no-dev --no-editable -o requirements.txt && \
    uv pip install -r requirements.txt --target "${LAMBDA_TASK_ROOT}" && \
    python -m compileall -f "${LAMBDA_TASK_ROOT}"
```

Gives 100% the same digest all the time. ( `sha256:2c6980d7e6ddba3f34bef8c1fae622e326b249ad4572c80e32255b9fac7fa39c` )

```
ENV UV_COMPILE_BYTECODE=1
ENV UV_NO_INSTALLER_METADATA=1
ENV SOURCE_DATE_EPOCH=0
ENV UV_LINK_MODE=copy

RUN --mount=from=uv,source=/uv,target=/bin/uv \
    --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv export --frozen --no-emit-workspace --no-dev --no-editable -o requirements.txt && \
    uv pip install -r requirements.txt --target "${LAMBDA_TASK_ROOT}"
```

Gives:
   35 times `sha256:6e7cf168bf6a068e1e56a96333558d512521030734b1406ee1afb14ba8215fdf`
   65 times `sha256:ff6cef2b78720fb4a4e141d2141a60e3883cd48b4d8cda50a1770cc3bb296f9d`


---

_Comment by @zanieb on 2025-01-15 14:54_

Thanks! cc @konstin 

---

_Comment by @charliermarsh on 2025-01-15 16:34_

That's fairly surprising... We be good to understand what's going on there.

---

_Assigned to @konstin by @konstin on 2025-01-15 19:07_

---

_Comment by @jackvreeken on 2025-01-19 23:08_

I was a bit afraid it was just happening on my system, so tried a few others:

(Running with [c3daf706](https://github.com/jackvreeken/uv-scipy-bytecode-issue/tree/c3daf706aa34cce3c27cf76f7dd1b9127cf51532) of my little test repo)

| Environment | ff6cef2b | 6e7cf168 |
|-------------|-----------|-----------|
| Windows Desktop, Windows 11, Rancher Desktop 1.9 | 71 | 29 |
| ... as above, but Rancher Desktop 1.17 (Fresh Install) | 80 | 20 |
| Windows Laptop, Windows 10, Rancher Desktop 1.16 | 93 | 7 |
| Linux Server, TrueNAS Scale | 77 | 23 |
| Linux NUC 1, Ubuntu 24.04 | 99 | 1 |
| Linux NUC 2, Ubuntu 24.04 | 98 | 2 |
| Linux EC2, Amazon Linux 2023 | 98 | 2 |

So it _does_ happen on Linux, just not very often. Hopefully that means you guys are able to reproduce and debug 🤞

---

_Unassigned @konstin by @konstin on 2025-01-20 15:25_

---

_Label `bug` added by @konstin on 2025-01-20 15:25_

---

_Comment by @konstin on 2025-02-03 14:16_

It sounds like CPython does not guarantee deterministic `.pyc` compilation, as the marshal module may serialize refcounting information that depends on the interpreter state:

* https://github.com/python/cpython/issues/78274
* https://bugs.python.org/issue34033

I'm not sure yet if we can mitigate this downstream.

---

_Label `upstream` added by @konstin on 2025-02-03 14:16_

---

_Comment by @konstin on 2025-02-06 12:42_

Reported upstream with a minimal reproducer: https://github.com/python/cpython/issues/129724

---

_Comment by @encukou on 2025-02-06 15:06_

Correct, CPython doesn't guarantee reproducibility here.
I agree it's surprising -- we tend to merge PRs that make them "more reproducible", so by now the *easy* cases of non-reproducible pycs are solved :)


---

_Comment by @konstin on 2025-02-06 15:09_

Do you by change have insights into why the numbers are different for our case, i.e. where does marshal serialize those counts from and what's different if we processed an `import foo` earlier, or more generally why `sys.getrefcount` gets marshaled? I tried reading marshal.c but i didn't get the big picture of why the format does that.

---

_Comment by @encukou on 2025-02-06 15:29_

`FLAG_REF` gets added if an object might appear later in the file, in which case it can later be referenced by `TYPE_REF` (using an index to the array of all objects with `FLAG_REF`).
To determine if the object might appear later, marshal uses a heuristic -- it skips objects with a refcount of 1: https://github.com/python/cpython/blob/55f17b77c305be877ac856d6426b13591cbc7fc8/Python/marshal.c#L385-L392

If an object makes its way into some container or instance attribute, its refcount will go up and it'll start getting a `FLAG_REF` when it's marshalled. (And the indices of all subsequent flagged objects will change.)

In the [repo](https://github.com/jackvreeken/uv-scipy-bytecode-issue/tree/87a8691b75f60007c277866da76bf70b7f799845), this affects a `frozenset({'randint', 'nchypergeom_fisher', 'nchypergeom_wallenius'})`.

You can analyze this like so (with [marshlaparser](https://pypi.org/project/marshalparser/) installed):
```
diff --color=always -U3 <(python -m marshalparser 6e7c/test_sampling.cpython-313.pyc -p) <(python -m marshalparser ff6c/test_sampling.cpython-313.pyc -p) | less -R
```

```diff
--- /dev/fd/63  2025-02-06 16:22:08.091555673 +0100
+++ /dev/fd/62  2025-02-06 16:22:08.092555697 +0100
@@ -3645,19 +3645,19 @@
         result=b'TestDiscreteAliasUrn', type=<class 'bytes'>
         n=32812/0x802c byte=(b'69', b'i', 0b1101001) TYPE_INT 
         result=633, type=<class 'int'>
-        n=32817/0x8031 byte=(b'3e', b'>', 0b111110) TYPE_FROZENSET 
+        n=32817/0x8031 byte=(b'3e', b'>', 0b111110) TYPE_FROZENSET REF[462]
           tuple/list/set size: 3
-          n=32822/0x8036 byte=(b'5a', b'Z', 0b1011010) TYPE_SHORT_ASCII_INTERNED REF[462]
+          n=32822/0x8036 byte=(b'5a', b'Z', 0b1011010) TYPE_SHORT_ASCII_INTERNED REF[463]
           result=b'randint', type=<class 'bytes'>
-          n=32831/0x803f byte=(b'5a', b'Z', 0b1011010) TYPE_SHORT_ASCII_INTERNED REF[463]
+          n=32831/0x803f byte=(b'5a', b'Z', 0b1011010) TYPE_SHORT_ASCII_INTERNED REF[464]
           result=b'nchypergeom_fisher', type=<class 'bytes'>
-          n=32851/0x8053 byte=(b'5a', b'Z', 0b1011010) TYPE_SHORT_ASCII_INTERNED REF[464]
+          n=32851/0x8053 byte=(b'5a', b'Z', 0b1011010) TYPE_SHORT_ASCII_INTERNED REF[465]
           result=b'nchypergeom_wallenius', type=<class 'bytes'>
-        result=frozenset({b'nchypergeom_wallenius', b'randint', b'nchypergeom_fisher'}), type=<class 'frozenset'>
-        n=32874/0x806a byte=(b'7a', b'z', 0b1111010) TYPE_SHORT_ASCII REF[465]
+        result=frozenset({b'randint', b'nchypergeom_fisher', b'nchypergeom_wallenius'}), type=<class 'frozenset'>
+        n=32874/0x806a byte=(b'7a', b'z', 0b1111010) TYPE_SHORT_ASCII REF[466]
         result=b'distname, params', type=<class 'bytes'>
         n=32892/0x807c byte=(b'63', b'c', 0b1100011) TYPE_CODE 
-          n=32913/0x8091 byte=(b'73', b's', 0b1110011) TYPE_STRING REF[466]
+          n=32913/0x8091 byte=(b'73', b's', 0b1110011) TYPE_STRING REF[467]
...
(and lots more cases of "ref" numbers being offset by 1)
```


---

_Comment by @notatallshaw on 2025-02-06 15:43_

I assume from this discussion you are also going to see this non-deterministic behavior with Python's standard library [compile directory call ](https://docs.python.org/3/library/compileall.html#compileall.compile_dir) with [more than 1 worker](https://docs.python.org/3/library/compileall.html#cmdoption-compileall-j), e.g.

```bash
python -m compileall -j4 /path/to/directory
```

---

_Comment by @konstin on 2025-02-06 22:04_

@encukou Thank you for the explanation!

---
