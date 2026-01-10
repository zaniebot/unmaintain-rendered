---
number: 9840
title: "`uv add tensorflow` Fails in Python 3.9"
type: issue
state: closed
author: caipeter888
labels: []
assignees: []
created_at: 2024-12-12T14:09:33Z
updated_at: 2024-12-12T14:21:17Z
url: https://github.com/astral-sh/uv/issues/9840
synced_at: 2026-01-10T01:24:46Z
---

# `uv add tensorflow` Fails in Python 3.9

---

_Issue opened by @caipeter888 on 2024-12-12 14:09_

## Description

In `uv 0.5.8`, the command `uv add tensorflow` fails in a Python 3.9 environment due to an issue with the package `tensorflow-io-gcs-filesystem`. However, the command  `uv pip install tensorflow` works successfully.

## Environment

- uv version: `uv 0.5.8 (80d41671b 2024-12-11)`
- System: `Windows 11 24H2 26100.2454`
- Python version: `cpython-3.9.20-windows-x86_64-none    D:\Users\user\AppData\Roaming\uv\python\cpython-3.9.20-windows-x86_64-none\python.exe`

## Steps to Reproduce

1. Cache Cleaning:

   ```
   Z:\>uv cache clean
   No cache found at: D:\Users\user\AppData\Local\uv\cache
   ```

2. Create Python 3.9 Project:

   ```
   Z:\>uv init --python 3.9 tensorflow-project
   Initialized project `tensorflow-project` at `Z:\tensorflow-project`

   Z:\>cd tensorflow-project
   ```

3. Attempt to Add TensorFlow Using `uv add tensorflow`:

   ```
   Z:\tensorflow-project>uv add tensorflow
   Using CPython 3.9.20
   Creating virtual environment at: .venv
   Resolved 41 packages in 2.29s
   error: Distribution `tensorflow-io-gcs-filesystem==0.37.1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform
   ```

4. Verbose Output:

   ```
   Z:\tensorflow-project>uv add tensorflow --verbose >output.txt 2>&1
   ```

   See `output.txt` for the verbose output.

   [output.txt](https://github.com/user-attachments/files/18112648/output.txt)

5. Successfully Install TensorFlow Using `uv pip install tensorflow`:

   ```
   Z:\tensorflow-project>uv pip install tensorflow
   Resolved 41 packages in 1.26s
   Prepared 41 packages in 48.81s
   warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
            If the cache and target directories are on different filesystems, hardlinking may not be supported.
            If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
   Installed 41 packages in 20.56s

   + absl-py==2.1.0
   + astunparse==1.6.3
   + certifi==2024.8.30
   + charset-normalizer==3.4.0
   + flatbuffers==24.3.25
   + gast==0.6.0
   + google-pasta==0.2.0
   + grpcio==1.68.1
   + h5py==3.12.1
   + idna==3.10
   + importlib-metadata==8.5.0
   + keras==3.7.0
   + libclang==18.1.1
   + markdown==3.7
   + markdown-it-py==3.0.0
   + markupsafe==3.0.2
   + mdurl==0.1.2
   + ml-dtypes==0.4.1
   + namex==0.0.8
   + numpy==2.0.2
   + opt-einsum==3.4.0
   + optree==0.13.1
   + packaging==24.2
   + protobuf==5.29.1
   + pygments==2.18.0
   + requests==2.32.3
   + rich==13.9.4
   + setuptools==75.6.0
   + six==1.17.0
   + tensorboard==2.18.0
   + tensorboard-data-server==0.7.2
   + tensorflow==2.18.0
   + tensorflow-intel==2.18.0
   + tensorflow-io-gcs-filesystem==0.31.0
   + termcolor==2.5.0
   + typing-extensions==4.12.2
   + urllib3==2.2.3
   + werkzeug==3.1.3
   + wheel==0.45.1
   + wrapt==1.17.0
   + zipp==3.21.0

   Z:\tensorflow-project>uv run python -c "import tensorflow as tf;print(tf.reduce_sum(tf.random.normal([1000, 1000])))"
   2024-12-12 22:03:45.312946: I tensorflow/core/util/port.cc:153] oneDNN custom operations are on. You may see slightly different numerical results due to floating-point round-off errors from different computation orders. To turn them off, set the environment variable `TF_ENABLE_ONEDNN_OPTS=0`.
   2024-12-12 22:03:46.816076: I tensorflow/core/util/port.cc:153] oneDNN custom operations are on. You may see slightly different numerical results due to floating-point round-off errors from different computation orders. To turn them off, set the environment variable `TF_ENABLE_ONEDNN_OPTS=0`.
   2024-12-12 22:03:48.965224: I tensorflow/core/platform/cpu_feature_guard.cc:210] This TensorFlow binary is optimized to use available CPU instructions in performance-critical operations.
   To enable the following instructions: AVX2 AVX512F AVX512_VNNI AVX512_BF16 FMA, in other operations, rebuild TensorFlow with the appropriate compiler flags.
   tf.Tensor(-79.78926, shape=(), dtype=float32)
   ```


---

_Comment by @my1e5 on 2024-12-12 14:20_

* See https://github.com/astral-sh/uv/issues/9711

---

_Comment by @charliermarsh on 2024-12-12 14:21_

👍 Looks like a case of that tracking issue.

---

_Closed by @charliermarsh on 2024-12-12 14:21_

---
