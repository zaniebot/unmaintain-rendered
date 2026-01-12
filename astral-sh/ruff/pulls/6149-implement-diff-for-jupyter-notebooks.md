```yaml
number: 6149
title: "Implement `--diff` for Jupyter Notebooks"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: dhruv/jupyter-diff
created_at: 2023-07-28T12:49:02Z
updated_at: 2023-07-29T04:43:58Z
url: https://github.com/astral-sh/ruff/pull/6149
synced_at: 2026-01-12T15:55:20Z
```

# Implement `--diff` for Jupyter Notebooks

---

_@dhruvmanila_

## Summary

Implement `--diff` for Jupyter Notebooks

## Test Plan

1. Use `crates/ruff/resources/test/fixtures/jupyter/isort.ipynb` as a test case
   and add a markdown cell in between the code cells to check that the diff
   outputs the correct cell index.
2. Run the command:
    `cargo run --bin ruff --package ruff_cli -- check --no-cache --isolated --select=ALL crates/ruff/resources/test/fixtures/jupyter/isort.ipynb --fix --diff`

<details><summary>Example output:</summary>
<p>

```diff
--- /Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 0
+++ /Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 0
@@ -1,3 +0,0 @@
-from pathlib import Path
-import random
-import math
--- /Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 4
+++ /Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 4
@@ -1,5 +1,3 @@
-from typing import Any
-import collections
 # Newline should be added here
 def foo():
     pass

--- /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 8
+++ /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 8
@@ -1,8 +1,7 @@
 import pprint
 import tempfile
 
-from IPython import display
 import matplotlib.pyplot as plt
-
 import tensorflow as tf
-import tensorflow_datasets as tfds
+import tensorflow_datasets as tfds
+from IPython import display
--- /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 10
+++ /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 10
@@ -1,5 +1,4 @@
 import tensorflow_models as tfm
 
 # These are not in the tfm public API for v2.9. They will be available in v2.10
-from official.vision.serving import export_saved_model_lib
-import official.core.train_lib
+from official.vision.serving import export_saved_model_lib
--- /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 13
+++ /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 13
@@ -1,5 +1,5 @@
-exp_config = tfm.core.exp_factory.get_exp_config('resnet_imagenet')
-tfds_name = 'cifar10'
+exp_config = tfm.core.exp_factory.get_exp_config("resnet_imagenet")
+tfds_name = "cifar10"
 ds,ds_info = tfds.load(
 tfds_name,
 with_info=True)
--- /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 15
+++ /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 15
@@ -6,12 +6,12 @@
 # Configure training and testing data
 batch_size = 128
 
-exp_config.task.train_data.input_path = ''
+exp_config.task.train_data.input_path = ""
 exp_config.task.train_data.tfds_name = tfds_name
-exp_config.task.train_data.tfds_split = 'train'
+exp_config.task.train_data.tfds_split = "train"
 exp_config.task.train_data.global_batch_size = batch_size
 
-exp_config.task.validation_data.input_path = ''
+exp_config.task.validation_data.input_path = ""
 exp_config.task.validation_data.tfds_name = tfds_name
-exp_config.task.validation_data.tfds_split = 'test'
+exp_config.task.validation_data.tfds_split = "test"
 exp_config.task.validation_data.global_batch_size = batch_size
--- /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 17
+++ /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 17
@@ -1,16 +1,16 @@
 logical_device_names = [logical_device.name for logical_device in tf.config.list_logical_devices()]
 
-if 'GPU' in ''.join(logical_device_names):
-  print('This may be broken in Colab.')
-  device = 'GPU'
-elif 'TPU' in ''.join(logical_device_names):
-  print('This may be broken in Colab.')
-  device = 'TPU'
+if "GPU" in "".join(logical_device_names):
+  print("This may be broken in Colab.")
+  device = "GPU"
+elif "TPU" in "".join(logical_device_names):
+  print("This may be broken in Colab.")
+  device = "TPU"
 else:
-  print('Running on CPU is slow, so only train for a few steps.')
-  device = 'CPU'
+  print("Running on CPU is slow, so only train for a few steps.")
+  device = "CPU"
 
-if device=='CPU':
+if device=="CPU":
   train_steps = 20
   exp_config.trainer.steps_per_loop = 5
 else:
@@ -20,9 +20,9 @@
 exp_config.trainer.summary_interval = 100
 exp_config.trainer.checkpoint_interval = train_steps
 exp_config.trainer.validation_interval = 1000
-exp_config.trainer.validation_steps =  ds_info.splits['test'].num_examples // batch_size
+exp_config.trainer.validation_steps =  ds_info.splits["test"].num_examples // batch_size
 exp_config.trainer.train_steps = train_steps
-exp_config.trainer.optimizer_config.learning_rate.type = 'cosine'
+exp_config.trainer.optimizer_config.learning_rate.type = "cosine"
 exp_config.trainer.optimizer_config.learning_rate.cosine.decay_steps = train_steps
 exp_config.trainer.optimizer_config.learning_rate.cosine.initial_learning_rate = 0.1
 exp_config.trainer.optimizer_config.warmup.linear.warmup_steps = 100
--- /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 21
+++ /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 21
@@ -1,14 +1,14 @@
 logical_device_names = [logical_device.name for logical_device in tf.config.list_logical_devices()]
 
 if exp_config.runtime.mixed_precision_dtype == tf.float16:
-    tf.keras.mixed_precision.set_global_policy('mixed_float16')
+    tf.keras.mixed_precision.set_global_policy("mixed_float16")
 
-if 'GPU' in ''.join(logical_device_names):
+if "GPU" in "".join(logical_device_names):
   distribution_strategy = tf.distribute.MirroredStrategy()
-elif 'TPU' in ''.join(logical_device_names):
+elif "TPU" in "".join(logical_device_names):
   tf.tpu.experimental.initialize_tpu_system()
-  tpu = tf.distribute.cluster_resolver.TPUClusterResolver(tpu='/device:TPU_SYSTEM:0')
+  tpu = tf.distribute.cluster_resolver.TPUClusterResolver(tpu="/device:TPU_SYSTEM:0")
   distribution_strategy = tf.distribute.experimental.TPUStrategy(tpu)
 else:
-  print('Warning: this will be really slow.')
+  print("Warning: this will be really slow.")
   distribution_strategy = tf.distribute.OneDeviceStrategy(logical_device_names[0])
--- /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 23
+++ /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 23
@@ -1,5 +1,3 @@
 with distribution_strategy.scope():
   model_dir = tempfile.mkdtemp()
   task = tfm.core.task_factory.get_task(exp_config.task, logging_dir=model_dir)
-
-#  tf.keras.utils.plot_model(task.build_model(), show_shapes=True)
--- /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 24
+++ /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 24
@@ -1,4 +1,4 @@
 for images, labels in task.build_inputs(exp_config.task.train_data).take(1):
   print()
-  print(f'images.shape: {str(images.shape):16}  images.dtype: {images.dtype!r}')
-  print(f'labels.shape: {str(labels.shape):16}  labels.dtype: {labels.dtype!r}')
+  print(f"images.shape: {images.shape!s:16}  images.dtype: {images.dtype!r}")
+  print(f"labels.shape: {labels.shape!s:16}  labels.dtype: {labels.dtype!r}")
--- /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 27
+++ /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 27
@@ -1 +1 @@
-plt.hist(images.numpy().flatten());
+plt.hist(images.numpy().flatten())
--- /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 29
+++ /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 29
@@ -1,2 +1,2 @@
-label_info = ds_info.features['label']
+label_info = ds_info.features["label"]
 label_info.int2str(1)
--- /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 31
+++ /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 31
@@ -10,9 +10,6 @@
     if predictions is None:
       plt.title(label_info.int2str(labels[i]))
     else:
-      if labels[i] == predictions[i]:
-        color = 'g'
-      else:
-        color = 'r'
+      color = "g" if labels[i] == predictions[i] else "r"
       plt.title(label_info.int2str(predictions[i]), color=color)
     plt.axis("off")
--- /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 35
+++ /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 35
@@ -1,3 +1,3 @@
-plt.figure(figsize=(10, 10));
+plt.figure(figsize=(10, 10))
 for images, labels in task.build_inputs(exp_config.task.validation_data).take(1):
   show_batch(images, labels)
--- /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 37
+++ /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 37
@@ -1,7 +1,7 @@
 model, eval_logs = tfm.core.train_lib.run_experiment(
     distribution_strategy=distribution_strategy,
     task=task,
-    mode='train_and_eval',
+    mode="train_and_eval",
     params=exp_config,
     model_dir=model_dir,
     run_post_eval=True)
--- /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 38
+++ /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 38
@@ -1 +0,0 @@
-#  tf.keras.utils.plot_model(model, show_shapes=True)
--- /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 40
+++ /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 40
@@ -1,4 +1,4 @@
 for key, value in eval_logs.items():
     if isinstance(value, tf.Tensor):
       value = value.numpy()
-    print(f'{key:20}: {value:.3f}')
+    print(f"{key:20}: {value:.3f}")
--- /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 42
+++ /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 42
@@ -4,5 +4,5 @@
 
 show_batch(images, labels, tf.cast(predictions, tf.int32))
 
-if device=='CPU':
-  plt.suptitle('The model was only trained for a few steps, it is not expected to do well.')
+if device=="CPU":
+  plt.suptitle("The model was only trained for a few steps, it is not expected to do well.")
--- /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 45
+++ /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 45
@@ -1,8 +1,8 @@
 # Saving and exporting the trained model
 export_saved_model_lib.export_inference_graph(
-    input_type='image_tensor',
+    input_type="image_tensor",
     batch_size=1,
     input_image_size=[32, 32],
     params=exp_config,
     checkpoint_path=tf.train.latest_checkpoint(model_dir),
-    export_dir='./export/')
+    export_dir="./export/")
--- /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 47
+++ /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 47
@@ -1,3 +1,3 @@
 # Importing SavedModel
-imported = tf.saved_model.load('./export/')
-model_fn = imported.signatures['serving_default']
+imported = tf.saved_model.load("./export/")
+model_fn = imported.signatures["serving_default"]
--- /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 49
+++ /Users/dhruv/playground/ruff/notebooks/image_classification.ipynb:cell 49
@@ -1,10 +1,10 @@
 plt.figure(figsize=(10, 10))
-for data in tfds.load('cifar10', split='test').batch(12).take(1):
+for data in tfds.load("cifar10", split="test").batch(12).take(1):
   predictions = []
-  for image in data['image']:
-    index = tf.argmax(model_fn(image[tf.newaxis, ...])['logits'], axis=1)[0]
+  for image in data["image"]:
+    index = tf.argmax(model_fn(image[tf.newaxis, ...])["logits"], axis=1)[0]
     predictions.append(index)
-  show_batch(data['image'], data['label'], predictions)
+  show_batch(data["image"], data["label"], predictions)
 
-  if device=='CPU':
-    plt.suptitle('The model was only trained for a few steps, it is not expected to do better than random.')
+  if device=="CPU":
+    plt.suptitle("The model was only trained for a few steps, it is not expected to do better than random.")

Would fix 61 errors.
```

</p>
</details> 

resolves: #4727


---

_Comment by @dhruvmanila on 2023-07-28 12:49_

Current dependencies on/for this PR:
* main
  * **PR #6149** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6149" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/6149?utm_source=stack-comment).

---

_Comment by @github-actions[bot] on 2023-07-28 13:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.2±0.13ms     4.0 MB/sec    1.00     10.2±0.11ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1986.5±32.60µs     8.4 MB/sec    1.00  1982.2±19.77µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    215.8±3.38µs    13.7 MB/sec    1.01    217.0±0.85µs    13.6 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.06ms     6.0 MB/sec    1.00      4.3±0.09ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.01     13.7±0.08ms     3.0 MB/sec    1.00     13.6±0.09ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.02    464.7±5.66µs     6.3 MB/sec    1.00    455.0±0.66µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.07ms     4.2 MB/sec    1.00      6.1±0.05ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.3±0.05ms     5.6 MB/sec    1.00      7.2±0.03ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1499.2±10.58µs    11.1 MB/sec    1.00  1494.2±11.11µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    165.3±0.89µs    17.9 MB/sec    1.00    163.6±2.37µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.03ms     8.0 MB/sec    1.00      3.1±0.01ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.1±0.12ms     4.5 MB/sec    1.01      9.1±0.12ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1752.4±38.09µs     9.5 MB/sec    1.01  1772.4±48.73µs     9.4 MB/sec
formatter/numpy/globals.py                 1.00    192.7±5.49µs    15.3 MB/sec    1.02    196.4±6.09µs    15.0 MB/sec
formatter/pydantic/types.py                1.00      3.8±0.06ms     6.7 MB/sec    1.03      3.9±0.09ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     12.1±0.14ms     3.4 MB/sec    1.02     12.3±0.30ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.07ms     5.2 MB/sec    1.00      3.2±0.04ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   393.1±24.75µs     7.5 MB/sec    1.00    388.5±9.20µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.08ms     4.7 MB/sec    1.01      5.5±0.10ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.07ms     6.0 MB/sec    1.01      6.8±0.13ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1351.1±43.37µs    12.3 MB/sec    1.00  1355.2±18.11µs    12.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    149.5±3.95µs    19.7 MB/sec    1.00    149.2±3.31µs    19.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.04ms     8.8 MB/sec    1.01      2.9±0.05ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh approved on 2023-07-29 00:32_

---

_@charliermarsh reviewed on 2023-07-29 00:32_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:296 on 2023-07-29 00:32_

Why is this necessary here?

---

_@dhruvmanila reviewed on 2023-07-29 03:10_

---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/diagnostics.rs`:296 on 2023-07-29 03:10_

Jupyter notebook cells don't necessarily have a newline at the end. For example,
                                                  
```python
print("hello")
```
                                                  
For above code in a cell, there'll only be one line, and it won't have a newline at the end. If it did, there'd be two lines, and the second line would be empty:

```python
print("hello")
 
```

---

I'll add this as a comment.


---

_@dhruvmanila reviewed on 2023-07-29 03:13_

---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/diagnostics.rs`:296 on 2023-07-29 03:13_

<img width="229" alt="Screenshot 2023-07-29 at 08 41 56" src="https://github.com/astral-sh/ruff/assets/67177269/fe65dcb0-96dd-48ab-94d1-6cd0adf7ae6e">

Visually speaking, the first cell doesn't have a newline at the end while the second one does. The diff would output a lot of "No newline at the end of file" which is incorrect.

---

_Comment by @dhruvmanila on 2023-07-29 03:25_

~Oh, I need to avoid adding newlines if the cells are not changed otherwise there'll be blank lines for each unchanged cells.~

Edit: Actually, the newlines should be printed between files and not between cells.

---

_Merged by @dhruvmanila on 2023-07-29 04:22_

---

_Closed by @dhruvmanila on 2023-07-29 04:22_

---

_Branch deleted on 2023-07-29 04:22_

---
