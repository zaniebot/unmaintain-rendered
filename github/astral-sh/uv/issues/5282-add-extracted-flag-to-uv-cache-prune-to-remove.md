---
number: 5282
title: "Add `--extracted` flag to `uv cache prune` to remove unzipped wheels"
type: issue
state: closed
author: Jorricks
labels:
  - enhancement
  - good first issue
  - help wanted
assignees: []
created_at: 2024-07-22T12:14:08Z
updated_at: 2024-07-24T19:34:20Z
url: https://github.com/astral-sh/uv/issues/5282
synced_at: 2026-01-07T13:12:17-06:00
---

# Add `--extracted` flag to `uv cache prune` to remove unzipped wheels

---

_Issue opened by @Jorricks on 2024-07-22 12:14_

First of all, thank you for the awesome package. It's working blazingly fast and our developers are loving the new speed they have locally.

In our pipelines, the speed has not really improved. The main reason is that pip tools cache is considerably smaller than _uv_. When we store the _uv_ cache, the Gitlab pipeline is spending much more time on zipping the small files, then it is spending on downloading the files new. The difference is considerable; it's 2 times slower when we use Gitlab caches on the _uv_ cache.

A small app easily generates 16000 small files in _uv_ cache and easily comes to 
```
/builds/my_user/my_repository/.cache/uv: found 16373 matching artifact files and directories 
```

This is not only visible in Gitlab, but also in Github as mentioned in this ticket;
https://github.com/actions/setup-python/issues/822#issuecomment-1963717925

Would it maybe be possible to add a flag for CI/CD to concentrate the cache in larger files?

---

_Comment by @konstin on 2024-07-22 12:20_

The uv cache contains all the wheels we install unpacked (in the `archive-v0` bucket, linked to `wheels-v1`), so we can install them quickly by reflinking or hardlinking. Those should be the many small files you're seeing. Does it work better when excluding those directories?

---

_Comment by @Jorricks on 2024-07-22 13:20_

> The uv cache contains all the wheels we install unpacked (in the `archive-v0` bucket, linked to `wheels-v1`), so we can install them quickly by reflinking or hardlinking. Those should be the many small files you're seeing. Does it work better when excluding those directories?

Same package, wildly different result after running `rm -rf .cache/uv/archive-v0`;
```
/builds/my_user/my_repository/.cache/uv: found 2675 matching artifact files and directories 
```

---

_Comment by @charliermarsh on 2024-07-22 13:22_

I think you instead want `rm -rf .cache/uv/wheels-v1`, and then `uv cache prune`.

---

_Comment by @Jorricks on 2024-07-22 13:32_

> I think you instead want `rm -rf .cache/uv/wheels-v1`, and then `uv cache prune`.

You seem to be right;
```
Caused by: failed to read directory `/builds/my_user/my_repository/.cache/uv/archive-v0/3vlR8U66f3qKdzSrxDtyR`
```

---

_Comment by @charliermarsh on 2024-07-22 13:34_

I think we should consider adding some options to `uv cache prune` to streamline this, since it's not good for users to be manipulating the cache arbitrarily (even though I told you to do it 11 minutes ago here :))

It doesn't make a ton of sense to be storing `.cache/uv/wheels-v1` in the CI cache, since you're just downloading and unzipping it in future jobs anyway. The only difference being that you're getting it from GitLab rather than PyPI or similar.

---

_Comment by @charliermarsh on 2024-07-22 13:37_

If you see improvement with something like `rm -rf .cache/uv/wheels-v1` and `uv cache prune`, that would be good to know...

---

_Comment by @Jorricks on 2024-07-22 13:45_

Interestingly enough, when I execute;
```
- rm -rf ${CI_PROJECT_DIR}/.cache/uv/wheels-v1
- uv cache prune --cache-dir ${CI_PROJECT_DIR}/.cache/uv
[2024-07-22 13:42:08] Pruning cache at: .cache/uv
[2024-07-22 13:42:08] Removed 10794 files (402.5MiB)
```
I get
```
/builds/bigdata/adyen_pyspark/.cache/uv: found 3035 matching artifact files and directories 
```

I kinda expected less matching artifact files then when I just ran `rm -rf .cache/uv/wheels-v1`. Does it make sense it's more?

I'll see if I can do some performance tuning on our Github helpers CPU & Memory. They seem to be capping.
![image](https://github.com/user-attachments/assets/16bec743-b2de-4e55-9fd0-856bb969579a)

I'll report back afterwards :)

---

_Comment by @charliermarsh on 2024-07-22 13:48_

I think "more" is correct. With `rm -rf .cache/uv/archive-v0`, you were likely removing some directories that were symlinked elsewhere in the cache (causing those `failed to read directory` errors). So you were removing too many entries before.

---

_Comment by @Jorricks on 2024-07-22 16:10_

Although we cleaned quite a bit, the size is still very large:
```
1.2G	.cache/uv
...
.cache/uv: found 3103 matching artifact files and directories 
```

Using the [FF_USE_FASTZIP](https://docs.gitlab.com/runner/configuration/feature-flags.html#available-feature-flags) flag in combination with `CACHE_COMPRESSION_LEVEL` I managed to get quite fast zipping/extracting performance.

We have 200MB/s internet speed for our S3 cache back-end, but still it seems faster to just download the artifacts from our Python repository.

---

_Comment by @Jorricks on 2024-07-22 16:14_

So I listed the 100 largest file in the `uv` cache.
It seems most of it is coming from one big package; `pyspark==3.2.3`:
```
[2024-07-22 16:09:28] $ find .cache/uv -type f -exec du -h {} + | sort -rh | head -n 100
[2024-07-22 16:09:28] 269M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3-py2.py3-none-any.whl
[2024-07-22 16:09:28] 35M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/rocksdbjni-6.20.3.jar
[2024-07-22 16:09:28] 35M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/rocksdbjni-6.20.3.jar
[2024-07-22 16:09:28] 35M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/rocksdbjni-6.20.3.jar
[2024-07-22 16:09:28] 31M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/hadoop-client-runtime-3.3.1.jar
[2024-07-22 16:09:28] 31M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/hadoop-client-runtime-3.3.1.jar
[2024-07-22 16:09:28] 31M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/hadoop-client-runtime-3.3.1.jar
[2024-07-22 16:09:28] 19M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/hadoop-client-api-3.3.1.jar
[2024-07-22 16:09:28] 19M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/hadoop-client-api-3.3.1.jar
[2024-07-22 16:09:28] 19M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/hadoop-client-api-3.3.1.jar
[2024-07-22 16:09:28] 14M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/breeze_2.12-1.2.jar
[2024-07-22 16:09:28] 14M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/breeze_2.12-1.2.jar
[2024-07-22 16:09:28] 14M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/breeze_2.12-1.2.jar
[2024-07-22 16:09:28] 12M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/spark-catalyst_2.12-3.2.3.jar
[2024-07-22 16:09:28] 12M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/spark-catalyst_2.12-3.2.3.jar
[2024-07-22 16:09:28] 12M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/spark-catalyst_2.12-3.2.3.jar
[2024-07-22 16:09:28] 11M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/spark-core_2.12-3.2.3.jar
[2024-07-22 16:09:28] 11M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/scala-compiler-2.12.15.jar
[2024-07-22 16:09:28] 11M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/hive-exec-2.3.9-core.jar
[2024-07-22 16:09:28] 11M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/spark-core_2.12-3.2.3.jar
[2024-07-22 16:09:28] 11M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/scala-compiler-2.12.15.jar
[2024-07-22 16:09:28] 11M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/hive-exec-2.3.9-core.jar
[2024-07-22 16:09:28] 11M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/spark-core_2.12-3.2.3.jar
[2024-07-22 16:09:28] 11M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/scala-compiler-2.12.15.jar
[2024-07-22 16:09:28] 11M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/hive-exec-2.3.9-core.jar
[2024-07-22 16:09:28] 8.0M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/spark-sql_2.12-3.2.3.jar
[2024-07-22 16:09:28] 8.0M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/spark-sql_2.12-3.2.3.jar
[2024-07-22 16:09:28] 8.0M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/spark-sql_2.12-3.2.3.jar
[2024-07-22 16:09:28] 7.9M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/hive-metastore-2.3.9.jar
[2024-07-22 16:09:28] 7.9M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/hive-metastore-2.3.9.jar
[2024-07-22 16:09:28] 7.9M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/hive-metastore-2.3.9.jar
[2024-07-22 16:09:28] 7.1M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/mesos-1.4.0-shaded-protobuf.jar
[2024-07-22 16:09:28] 7.1M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/mesos-1.4.0-shaded-protobuf.jar
[2024-07-22 16:09:28] 7.1M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/mesos-1.4.0-shaded-protobuf.jar
[2024-07-22 16:09:28] 7.0M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/spire_2.12-0.17.0.jar
[2024-07-22 16:09:28] 7.0M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/spire_2.12-0.17.0.jar
[2024-07-22 16:09:28] 7.0M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/spire_2.12-0.17.0.jar
[2024-07-22 16:09:28] 6.5M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/zstd-jni-1.5.0-4.jar
[2024-07-22 16:09:28] 6.5M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/zstd-jni-1.5.0-4.jar
[2024-07-22 16:09:28] 6.5M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/zstd-jni-1.5.0-4.jar
[2024-07-22 16:09:28] 5.9M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/spark-mllib_2.12-3.2.3.jar
[2024-07-22 16:09:28] 5.9M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/spark-mllib_2.12-3.2.3.jar
[2024-07-22 16:09:28] 5.9M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/spark-mllib_2.12-3.2.3.jar
[2024-07-22 16:09:28] 5.2M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/scala-library-2.12.15.jar
[2024-07-22 16:09:28] 5.2M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/scala-library-2.12.15.jar
[2024-07-22 16:09:28] 5.2M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/scala-library-2.12.15.jar
[2024-07-22 16:09:28] 4.4M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/netty-all-4.1.68.Final.jar
[2024-07-22 16:09:28] 4.4M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/netty-all-4.1.68.Final.jar
[2024-07-22 16:09:28] 4.4M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/netty-all-4.1.68.Final.jar
[2024-07-22 16:09:28] 3.6M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/scala-reflect-2.12.15.jar
[2024-07-22 16:09:28] 3.6M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/kubernetes-model-core-5.4.1.jar
[2024-07-22 16:09:28] 3.6M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/scala-reflect-2.12.15.jar
[2024-07-22 16:09:28] 3.6M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/kubernetes-model-core-5.4.1.jar
[2024-07-22 16:09:28] 3.6M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/scala-reflect-2.12.15.jar
[2024-07-22 16:09:28] 3.6M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/kubernetes-model-core-5.4.1.jar
[2024-07-22 16:09:28] 3.3M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/hadoop-shaded-guava-1.1.1.jar
[2024-07-22 16:09:28] 3.3M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/hadoop-shaded-guava-1.1.1.jar
[2024-07-22 16:09:28] 3.3M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/hadoop-shaded-guava-1.1.1.jar
[2024-07-22 16:09:28] 3.2M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/cats-kernel_2.12-2.1.1.jar
[2024-07-22 16:09:28] 3.2M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/cats-kernel_2.12-2.1.1.jar
[2024-07-22 16:09:28] 3.2M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/cats-kernel_2.12-2.1.1.jar
[2024-07-22 16:09:28] 3.1M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/shapeless_2.12-2.3.3.jar
[2024-07-22 16:09:28] 3.1M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/derby-10.14.2.0.jar
[2024-07-22 16:09:28] 3.1M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/shapeless_2.12-2.3.3.jar
[2024-07-22 16:09:28] 3.1M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/derby-10.14.2.0.jar
[2024-07-22 16:09:28] 3.1M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/shapeless_2.12-2.3.3.jar
[2024-07-22 16:09:28] 3.1M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/derby-10.14.2.0.jar
[2024-07-22 16:09:28] 2.4M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/spark-network-common_2.12-3.2.3.jar
[2024-07-22 16:09:28] 2.4M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/curator-client-2.13.0.jar
[2024-07-22 16:09:28] 2.4M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/spark-network-common_2.12-3.2.3.jar
[2024-07-22 16:09:28] 2.4M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/curator-client-2.13.0.jar
[2024-07-22 16:09:28] 2.4M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/spark-network-common_2.12-3.2.3.jar
[2024-07-22 16:09:28] 2.4M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/curator-client-2.13.0.jar
[2024-07-22 16:09:28] 2.1M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/guava-14.0.1.jar
[2024-07-22 16:09:28] 2.1M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/guava-14.0.1.jar
[2024-07-22 16:09:28] 2.1M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/guava-14.0.1.jar
[2024-07-22 16:09:28] 2.0M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/parquet-column-1.12.2.jar
[2024-07-22 16:09:28] 2.0M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/datanucleus-core-4.1.17.jar
[2024-07-22 16:09:28] 2.0M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/commons-math3-3.4.1.jar
[2024-07-22 16:09:28] 2.0M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/parquet-column-1.12.2.jar
[2024-07-22 16:09:28] 2.0M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/datanucleus-core-4.1.17.jar
[2024-07-22 16:09:28] 2.0M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/commons-math3-3.4.1.jar
[2024-07-22 16:09:28] 2.0M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/parquet-column-1.12.2.jar
[2024-07-22 16:09:28] 2.0M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/datanucleus-core-4.1.17.jar
[2024-07-22 16:09:28] 2.0M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/commons-math3-3.4.1.jar
[2024-07-22 16:09:28] 1.9M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/snappy-java-1.1.8.4.jar
[2024-07-22 16:09:28] 1.9M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/datanucleus-rdbms-4.1.19.jar
[2024-07-22 16:09:28] 1.9M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/snappy-java-1.1.8.4.jar
[2024-07-22 16:09:28] 1.9M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/datanucleus-rdbms-4.1.19.jar
[2024-07-22 16:09:28] 1.9M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/snappy-java-1.1.8.4.jar
[2024-07-22 16:09:28] 1.9M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/datanucleus-rdbms-4.1.19.jar
[2024-07-22 16:09:28] 1.8M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/parquet-jackson-1.12.2.jar
[2024-07-22 16:09:28] 1.8M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/parquet-jackson-1.12.2.jar
[2024-07-22 16:09:28] 1.8M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/parquet-jackson-1.12.2.jar
[2024-07-22 16:09:28] 1.7M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/hive-service-rpc-3.1.2.jar
[2024-07-22 16:09:28] 1.7M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/deps/jars/arrow-vector-2.0.0.jar
[2024-07-22 16:09:28] 1.7M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/hive-service-rpc-3.1.2.jar
[2024-07-22 16:09:28] 1.7M	.cache/uv/built-wheels-v3/index/fd2c9579c1c6f924/pyspark/3.2.3/unfW3Zpiflv9HaTeaDE6C/pyspark-3.2.3.tar.gz/build/lib/pyspark/jars/arrow-vector-2.0.0.jar
[2024-07-22 16:09:28] 1.7M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/hive-service-rpc-3.1.2.jar
[2024-07-22 16:09:28] 1.7M	.cache/uv/archive-v0/gekN10oQ_Vp-g2TxEYqsD/pyspark/jars/arrow-vector-2.0.0.jar
```

It contains the large Wheel and then it also contains the extracted items. The 90 biggest files are all Jars from this specific wheel. Is there a way to remove these JARs / extracted files?

---

_Comment by @charliermarsh on 2024-07-22 16:54_

You could run `uv cache clean pyspark` to remove the PySpark entries specifically. But you'll then have to rebuild it on every CI job?

---

_Comment by @Jorricks on 2024-07-22 17:54_

> It seems most of it is coming from one big package; `pyspark==3.2.3`:

Wow.. It seems that was 858.0MB of the 1.2G, as we have 311MB remaining -> 73% was taken from one package.
While the wheel itself was 269MB.
```
[2024-07-22 17:49:34] $ uv cache clean pyspark --cache-dir .cache/uv
[2024-07-22 17:49:34] Removed 1474 files for pyspark (858.0MiB)
[2024-07-22 17:49:34] $ du -sh .cache/uv
[2024-07-22 17:49:34] 311M	.cache/uv
```

I think `uv` would really benefit from having an option to cache all the artifacts but not storing the extracted files.

---

_Comment by @charliermarsh on 2024-07-22 17:59_

Like, store the built wheel, but none of the unzipped archives? We could probably support that.

---

_Comment by @Jorricks on 2024-07-22 18:10_

> Like, store the built wheel, but none of the unzipped archives? We could probably support that.

Exactly that. At that point, caching the `uv cache` in CI/CD runners would add extra performance. Now it feels that in most cases, you are better off discarding the cache.

---

_Comment by @charliermarsh on 2024-07-22 18:14_

It would be easy to support `uv cache prune --extracted` or something, to delete all the extracted files prior to uploading the cache. Think that's worth trying...?

We _could_ also support unzipping the wheel directly into the virtual environment (rather than to the cache first). That's probably the "right" solution for this use-case, but it's a lot more work. (For pre-built wheels that we download from a registry, we don't even _save_ the wheel itself -- we unzip the extracted files directly to disk.)


---

_Comment by @Jorricks on 2024-07-22 18:31_

> It would be easy to support `uv cache prune --extracted` or something, to delete all the extracted files prior to uploading the cache. Think that's worth trying...?
> 
> We _could_ also support unzipping the wheel directly into the virtual environment (rather than to the cache first). That's probably the "right" solution for this use-case, but it's a lot more work. (For pre-built wheels that we download from a registry, we don't even _save_ the wheel itself -- we unzip the extracted files directly to disk.)

I think the first part would fit my use-case perfectly. At first I was thinking of an environment variable like `UV_MINIFY_CACHE`, but I think making it a separate action such as `uv cache prune --extracted` is probably more in line with what is really happening. That is because I am assuming that this directly hurts the performance of the next `uv` run as well in case it needs to reinstall one of the (earlier) unpacked packages? So if you run multiple `uv pip sync` operations, where run 1 installs, run 2 uninstalls and run 3 installs it again, you might still want the output of the previous runs around.
Also the cleanup commands are incredibly fast, so there is no problem for me running that 👍 

---

_Comment by @charliermarsh on 2024-07-22 19:17_

That should be straightforward to implement. I'll try to find time, but will also mark this as help wanted.

Basically, we want to remove these symlink directories in the `Cache::prune` method:

![Screenshot 2024-07-22 at 3 16 46 PM](https://github.com/user-attachments/assets/61ca2bce-e0a0-4d25-b18d-fd491624e179)


---

_Renamed from "UV cache creating a lot of small files" to "Add `--extracted` flag to `uv cache prune` to remove unzipped wheels" by @charliermarsh on 2024-07-22 19:17_

---

_Label `enhancement` added by @charliermarsh on 2024-07-22 19:17_

---

_Label `good first issue` added by @charliermarsh on 2024-07-22 19:17_

---

_Label `help wanted` added by @charliermarsh on 2024-07-22 19:17_

---

_Referenced in [astral-sh/uv#5391](../../astral-sh/uv/pulls/5391.md) on 2024-07-24 01:52_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-24 01:53_

---

_Comment by @charliermarsh on 2024-07-24 01:53_

Added here: https://github.com/astral-sh/uv/pull/5391

---

_Closed by @charliermarsh on 2024-07-24 19:34_

---
