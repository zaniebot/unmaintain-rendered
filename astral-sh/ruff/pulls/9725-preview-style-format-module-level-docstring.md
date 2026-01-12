```yaml
number: 9725
title: "Preview Style: Format module level docstring"
type: pull_request
state: merged
author: Glyphack
labels:
  - bug
  - formatter
  - preview
assignees: []
merged: true
base: main
head: formatter-module-docstring
created_at: 2024-01-30T22:55:03Z
updated_at: 2024-02-05T16:22:41Z
url: https://github.com/astral-sh/ruff/pull/9725
synced_at: 2026-01-12T15:55:30Z
```

# Preview Style: Format module level docstring

---

_@Glyphack_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR adds support for formatting module level docstrings as described in https://peps.python.org/pep-0257/.

Since [black does not support](https://github.com/psf/black/issues/3493) formatting docstrings and this is not intentional we need to keep this rule in preview.

On the formatting style, we do one thing different than function and class docstrings which is keeping the trailing new line. This pattern is already used by python projects([example](https://github.com/PostHog/HouseWatch/blob/c8962f5464839fbbc0a60f398aa765d4bc8437b6/housewatch/settings/__init__.py#L7)) and also [docformat](https://github.com/PyCQA/docformatter#example) does it.

Fixes: #9701 

## Test Plan

<!-- How was it tested? -->


---

_Label `bug` added by @MichaReiser on 2024-01-31 07:28_

---

_Label `formatter` added by @MichaReiser on 2024-01-31 07:28_

---

_Comment by @github-actions[bot] on 2024-01-31 16:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 2 project errors)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff format --no-preview --exclude tests/roots/test-pycode/cp_1251_coded.py</pre>
</p>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+607 -952 lines in 592 files in 11 projects; 1 project error; 31 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+1 -3 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/scripts/evaluate_release_tag.py#L1'>scripts/evaluate_release_tag.py~L1</a>
```diff
-"""Evaluate release tag for whether docs should be built or not.
-
-"""
+"""Evaluate release tag for whether docs should be built or not."""
 
 import argparse
 from subprocess import check_output
```

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+16 -18 lines across 4 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/apache/airflow/blob/8fb4c71857324fa2366179819830d53f32ef5a3f/airflow/providers/google/cloud/utils/field_sanitizer.py#L67'>airflow/providers/google/cloud/utils/field_sanitizer.py~L67</a>
```diff
 >>>         },
 >>>     }
 >>> }
->>> sanitizer=GcpBodyFieldSanitizer(FIELDS_TO_SANITIZE)
+>>> sanitizer = GcpBodyFieldSanitizer(FIELDS_TO_SANITIZE)
 >>> sanitizer.sanitize(body)
 >>> json.dumps(body, indent=2)
 {
```
<a href='https://github.com/apache/airflow/blob/8fb4c71857324fa2366179819830d53f32ef5a3f/airflow/providers/google/cloud/utils/mlengine_prediction_summary.py#L88'>airflow/providers/google/cloud/utils/mlengine_prediction_summary.py~L88</a>
```diff
 
 .. code-block:: python
 
-    subprocess.check_call(
-        [
-            "python",
-            "-m",
-            "airflow.providers.google.cloud.utils.mlengine_prediction_summary",
-            "--prediction_path=gs://...",
-            "--metric_fn_encoded=" + metric_fn_encoded,
-            "--metric_keys=log_loss,mse",
-            "--runner=DataflowRunner",
-            "--staging_location=gs://...",
-            "--temp_location=gs://...",
-        ]
-    )
+    subprocess.check_call([
+        "python",
+        "-m",
+        "airflow.providers.google.cloud.utils.mlengine_prediction_summary",
+        "--prediction_path=gs://...",
+        "--metric_fn_encoded=" + metric_fn_encoded,
+        "--metric_keys=log_loss,mse",
+        "--runner=DataflowRunner",
+        "--staging_location=gs://...",
+        "--temp_location=gs://...",
+    ])
 
 .. spelling:word-list::
 
```
<a href='https://github.com/apache/airflow/blob/8fb4c71857324fa2366179819830d53f32ef5a3f/tests/providers/google/cloud/operators/test_life_sciences.py#L15'>tests/providers/google/cloud/operators/test_life_sciences.py~L15</a>
```diff
 # KIND, either express or implied.  See the License for the
 # specific language governing permissions and limitations
 # under the License.
-"""Tests for Google Life Sciences Run Pipeline operator """
+"""Tests for Google Life Sciences Run Pipeline operator"""
 
 from __future__ import annotations
 
```
<a href='https://github.com/apache/airflow/blob/8fb4c71857324fa2366179819830d53f32ef5a3f/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L15'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py~L15</a>
```diff
 # specific language governing permissions and limitations
 # under the License.
 """
-   This DAG will not work unless you create an Amazon EMR cluster running
-   Apache Hive and copy data into it following steps 1-4 (inclusive) here:
-   https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/EMRforDynamoDB.Tutorial.html
+This DAG will not work unless you create an Amazon EMR cluster running
+Apache Hive and copy data into it following steps 1-4 (inclusive) here:
+https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/EMRforDynamoDB.Tutorial.html
 """
 
 from __future__ import annotations
```

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+11 -11 lines across 11 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/aws/aws-sam-cli/blob/36e9495a0f1113aa394509cf37fceee43e93384c/samcli/hook_packages/terraform/hooks/prepare/types.py#L1'>samcli/hook_packages/terraform/hooks/prepare/types.py~L1</a>
```diff
-""" Contains the data types used in the TF prepare hook"""
+"""Contains the data types used in the TF prepare hook"""
 
 from abc import ABC
 from copy import deepcopy
```
<a href='https://github.com/aws/aws-sam-cli/blob/36e9495a0f1113aa394509cf37fceee43e93384c/samcli/hook_packages/terraform/hooks/prepare/utilities.py#L1'>samcli/hook_packages/terraform/hooks/prepare/utilities.py~L1</a>
```diff
-""" Maintain the utilities functions used in prepare hook """
+"""Maintain the utilities functions used in prepare hook"""
 
 from samcli.hook_packages.terraform.hooks.prepare.constants import COMPILED_REGULAR_EXPRESSION
 
```
<a href='https://github.com/aws/aws-sam-cli/blob/36e9495a0f1113aa394509cf37fceee43e93384c/samcli/lib/bootstrap/companion_stack/companion_stack_builder.py#L1'>samcli/lib/bootstrap/companion_stack/companion_stack_builder.py~L1</a>
```diff
 """
-    Companion stack template builder
+Companion stack template builder
 """
 
 from typing import Dict, cast
```
<a href='https://github.com/aws/aws-sam-cli/blob/36e9495a0f1113aa394509cf37fceee43e93384c/samcli/lib/bootstrap/companion_stack/companion_stack_manager.py#L1'>samcli/lib/bootstrap/companion_stack/companion_stack_manager.py~L1</a>
```diff
 """
-    Companion stack manager
+Companion stack manager
 """
 
 import logging
```
<a href='https://github.com/aws/aws-sam-cli/blob/36e9495a0f1113aa394509cf37fceee43e93384c/samcli/lib/bootstrap/companion_stack/data_types.py#L1'>samcli/lib/bootstrap/companion_stack/data_types.py~L1</a>
```diff
 """
-    Date type classes for companion stacks
+Date type classes for companion stacks
 """
 
 import posixpath
```
<a href='https://github.com/aws/aws-sam-cli/blob/36e9495a0f1113aa394509cf37fceee43e93384c/samcli/lib/cookiecutter/interactive_flow_creator.py#L1'>samcli/lib/cookiecutter/interactive_flow_creator.py~L1</a>
```diff
-""" This module parses a json/yaml file that defines a flow of questions to fulfill the cookiecutter context"""
+"""This module parses a json/yaml file that defines a flow of questions to fulfill the cookiecutter context"""
 
 from typing import Dict, Optional, Tuple
 
```
<a href='https://github.com/aws/aws-sam-cli/blob/36e9495a0f1113aa394509cf37fceee43e93384c/samcli/lib/cookiecutter/processor.py#L1'>samcli/lib/cookiecutter/processor.py~L1</a>
```diff
-""" Define a processor to process the cookiecutter context before/after generating a cookiecutter project"""
+"""Define a processor to process the cookiecutter context before/after generating a cookiecutter project"""
 
 from abc import ABC, abstractmethod
 from typing import Dict
```
<a href='https://github.com/aws/aws-sam-cli/blob/36e9495a0f1113aa394509cf37fceee43e93384c/samcli/lib/cookiecutter/question.py#L1'>samcli/lib/cookiecutter/question.py~L1</a>
```diff
-""" This module represents the questions to ask to the user to fulfill the cookiecutter context. """
+"""This module represents the questions to ask to the user to fulfill the cookiecutter context."""
 
 from abc import ABC, abstractmethod
 from enum import Enum
```
<a href='https://github.com/aws/aws-sam-cli/blob/36e9495a0f1113aa394509cf37fceee43e93384c/samcli/lib/pipeline/bootstrap/resource.py#L1'>samcli/lib/pipeline/bootstrap/resource.py~L1</a>
```diff
-""" Represents AWS resource"""
+"""Represents AWS resource"""
 
 from typing import Optional
 
```
<a href='https://github.com/aws/aws-sam-cli/blob/36e9495a0f1113aa394509cf37fceee43e93384c/samcli/lib/pipeline/bootstrap/stage.py#L1'>samcli/lib/pipeline/bootstrap/stage.py~L1</a>
```diff
-""" Application Environment """
+"""Application Environment"""
 
 import hashlib
 import json
```
<a href='https://github.com/aws/aws-sam-cli/blob/36e9495a0f1113aa394509cf37fceee43e93384c/samcli/lib/sync/sync_flow.py#L1'>samcli/lib/sync/sync_flow.py~L1</a>
```diff
-"""SyncFlow base class """
+"""SyncFlow base class"""
 
 import logging
 from abc import ABC, abstractmethod
```

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+491 -809 lines across 490 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/arrow.py#L1'>examples/basic/annotations/arrow.py~L1</a>
```diff
-""" A demonstration of configuring different arrow types.
+"""A demonstration of configuring different arrow types.
 
 .. bokeh-example-metadata::
     :apis: bokeh.plotting.figure.circle, bokeh.plotting.figure.add_layout
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/arrowheads.py#L1'>examples/basic/annotations/arrowheads.py~L1</a>
```diff
-""" A display of available arrow head styles.
+"""A display of available arrow head styles.
 
 .. bokeh-example-metadata::
     :apis: bokeh.models.Plot, bokeh.models.Arrow, bokeh.models.Label
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/band.py#L1'>examples/basic/annotations/band.py~L1</a>
```diff
-""" An interactive numerical band plot based on simple Python array of data.
+"""An interactive numerical band plot based on simple Python array of data.
     It is a combination of scatter plots and line plots added with a band of covered area.
     The line passes through the mean of the area covered by the band.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/box_annotation.py#L1'>examples/basic/annotations/box_annotation.py~L1</a>
```diff
-""" A timeseries plot of glucose data readings. This example demonstrates
+"""A timeseries plot of glucose data readings. This example demonstrates
 adding box annotations as well as a multi-line title.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/colorbar_log.py#L1'>examples/basic/annotations/colorbar_log.py~L1</a>
```diff
-""" A demonstration of a ColorBar with a log color scale.
+"""A demonstration of a ColorBar with a log color scale.
 
 .. bokeh-example-metadata::
     :apis: bokeh.models.ColorBar, bokeh.models.LogColorMapper
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/label.py#L1'>examples/basic/annotations/label.py~L1</a>
```diff
-""" A scatter plot that demonstrates different ways of adding labels.
+"""A scatter plot that demonstrates different ways of adding labels.
 
 .. bokeh-example-metadata::
     :apis: bokeh.models.ColumnDataSource, bokeh.models.Label, bokeh.models.LabelSet bokeh.plotting.figure.scatter
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legend.py#L1'>examples/basic/annotations/legend.py~L1</a>
```diff
-""" Line and marker plots that demonstrate automatic legends.
+"""Line and marker plots that demonstrate automatic legends.
 
 .. bokeh-example-metadata::
     :apis: bokeh.layouts.gridplot, bokeh.plotting.figure.circle, bokeh.plotting.figure.line, bokeh.plotting.figure.square
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_item_visibility.py#L1'>examples/basic/annotations/legends_item_visibility.py~L1</a>
```diff
-""" Marker and line plots that demonstrate manual control of legend visibility of individual items
+"""Marker and line plots that demonstrate manual control of legend visibility of individual items
 
 .. bokeh-example-metadata::
     :apis: bokeh.models.LegendItem, bokeh.plotting.figure.circle, bokeh.plotting.figure.line
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/slope.py#L1'>examples/basic/annotations/slope.py~L1</a>
```diff
-""" A marker plot that demonstrates a slope.
+"""A marker plot that demonstrates a slope.
 
 .. bokeh-example-metadata::
     :apis: bokeh.models.slope, bokeh.plotting.figure.circle
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/whisker.py#L1'>examples/basic/annotations/whisker.py~L1</a>
```diff
-""" A marker plot that shows the relationship between car type and highway MPG from the autompg
+"""A marker plot that shows the relationship between car type and highway MPG from the autompg
 sample data. This example demonstrates the use of whiskers to display quantile ranges in the plot.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/areas/stacked_area.py#L1'>examples/basic/areas/stacked_area.py~L1</a>
```diff
-""" A stacked area plot using data from a pandas DataFrame.
+"""A stacked area plot using data from a pandas DataFrame.
 
 .. bokeh-example-metadata::
     :apis: bokeh.plotting.figure.varea_stack
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/axes/logplot.py#L1'>examples/basic/axes/logplot.py~L1</a>
```diff
-""" A log plot using functions with different growth rates. This example
+"""A log plot using functions with different growth rates. This example
 demonstrates using a log axis on a Bokeh plot. Various line styles and glyph
 combinations are automatically added to a legend.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/bars/basic.py#L1'>examples/basic/bars/basic.py~L1</a>
```diff
-""" A simple bar chart using plain Python lists.
+"""A simple bar chart using plain Python lists.
 
 .. bokeh-example-metadata::
     :apis: bokeh.plotting.figure.vbar
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/bars/colormapped.py#L1'>examples/basic/bars/colormapped.py~L1</a>
```diff
-""" A bar chart based on simple Python lists of data. This example demonstrates
+"""A bar chart based on simple Python lists of data. This example demonstrates
 automatic colormapping.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/bars/colors.py#L1'>examples/basic/bars/colors.py~L1</a>
```diff
-""" A simple bar chart using plain Python lists. This example demonstrates
+"""A simple bar chart using plain Python lists. This example demonstrates
 setting bar colors from a ``ColumnDataSource``.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/bars/dodged.py#L1'>examples/basic/bars/dodged.py~L1</a>
```diff
-""" A bar chart based on plain Python lists. This example demonstrates creating
+"""A bar chart based on plain Python lists. This example demonstrates creating
 a using a ``dodge`` transform to "group" the bars.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/bars/intervals.py#L1'>examples/basic/bars/intervals.py~L1</a>
```diff
-""" An interval chart showing Olympic sprint time data as intervals.
+"""An interval chart showing Olympic sprint time data as intervals.
 
 .. bokeh-example-metadata::
     :sampledata: sprint
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/bars/mixed.py#L1'>examples/basic/bars/mixed.py~L1</a>
```diff
-""" A combined bar and line chart using simple Python lists. This example
+"""A combined bar and line chart using simple Python lists. This example
 demonstrates mixing nested categorical factors with top-level categorical
 factors.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/bars/nested.py#L1'>examples/basic/bars/nested.py~L1</a>
```diff
-""" A grouped bar chart using plain Python lists. This example demonstrates
+"""A grouped bar chart using plain Python lists. This example demonstrates
 creating a ``FactorRange`` with nested categories.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/bars/nested_colormapped.py#L1'>examples/basic/bars/nested_colormapped.py~L1</a>
```diff
-""" A bar chart based on simple Python lists of data. This example demonstrates
+"""A bar chart based on simple Python lists of data. This example demonstrates
 automatic colormapping of nested categorical factors.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/bars/pandas_groupby_colormapped.py#L1'>examples/basic/bars/pandas_groupby_colormapped.py~L1</a>
```diff
-""" A bar chart using the `Auto MPG dataset`_. This example demonstrates
+"""A bar chart using the `Auto MPG dataset`_. This example demonstrates
 automatic handing of Pandas GroupBy objects and colormapping with
 ``factor_cmap``.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/bars/pandas_groupby_nested.py#L1'>examples/basic/bars/pandas_groupby_nested.py~L1</a>
```diff
-""" A grouped bar chart using a cleaned up version of the `Auto MPG dataset`_.
+"""A grouped bar chart using a cleaned up version of the `Auto MPG dataset`_.
 This examples demonstrates automatic handing of Pandas GroupBy objects and
 colormapping nested factors with ``factor_cmap``. A hover tooltip displays
 information for each bar.
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/bars/sorted.py#L1'>examples/basic/bars/sorted.py~L1</a>
```diff
-""" A bar chart based on simple Python lists of data. The example below
+"""A bar chart based on simple Python lists of data. The example below
 sorts the fruit categories in ascending order based on counts and
 rearranges the bars accordingly.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/bars/stacked.py#L1'>examples/basic/bars/stacked.py~L1</a>
```diff
-""" A stacked bar chart using plain Python lists.
+"""A stacked bar chart using plain Python lists.
 
 .. bokeh-example-metadata::
     :apis: bokeh.plotting.figure.vbar_stack
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/bars/stacked_grouped.py#L1'>examples/basic/bars/stacked_grouped.py~L1</a>
```diff
-""" A stacked bar chart grouped into four groups using data in plain Python lists.
+"""A stacked bar chart grouped into four groups using data in plain Python lists.
 
 .. bokeh-example-metadata::
     :apis: bokeh.models.ColumnDataSource, bokeh.models.FactorRange, bokeh.plotting.figure.vbar_stack
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/bars/stacked_split.py#L1'>examples/basic/bars/stacked_split.py~L1</a>
```diff
-""" A split, horizontal stacked bar chart using plain Python lists.
+"""A split, horizontal stacked bar chart using plain Python lists.
 
 .. bokeh-example-metadata::
     :apis: bokeh.plotting.figure.hbar_stack
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/color_mappers.py#L1'>examples/basic/data/color_mappers.py~L1</a>
```diff
-""" A color mapping plot with color spectrum scale. The example plots demonstrates
+"""A color mapping plot with color spectrum scale. The example plots demonstrates
 log mapping and linear mapping with different color palette.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/transform_jitter.py#L1'>examples/basic/data/transform_jitter.py~L1</a>
```diff
-""" Two scatter plots in a grid. This example demonstrate the application of a
+"""Two scatter plots in a grid. This example demonstrate the application of a
 ``jitter`` transform to data.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/transform_markers.py#L1'>examples/basic/data/transform_markers.py~L1</a>
```diff
-""" A scatter plot using the `Palmer penguin dataset`_. This example
+"""A scatter plot using the `Palmer penguin dataset`_. This example
 demonstrates color and marker mapping with basic plot elements. The chart
 shows correlation between body mass and flipper length for three different
 penguin species.
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/layouts/anscombe.py#L1'>examples/basic/layouts/anscombe.py~L1</a>
```diff
-""" A reproduction of `Anscombe's Quartet`_ using the low-level |bokeh.models|
+"""A reproduction of `Anscombe's Quartet`_ using the low-level |bokeh.models|
 API that also includes HTML content in a ``Div``.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/layouts/grid.py#L1'>examples/basic/layouts/grid.py~L1</a>
```diff
-"""  A grid plot that shows four figures that use different glyphs.
+"""A grid plot that shows four figures that use different glyphs.
 
 .. bokeh-example-metadata::
     :apis: bokeh.layouts.gridplot
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/lines/arcs.py#L1'>examples/basic/lines/arcs.py~L1</a>
```diff
-""" An arc graph using pre-defined data points. This example
+"""An arc graph using pre-defined data points. This example
 demonstrates the use of the ``arc`` method to make a graph
 by drawing three arcs of defined radius, start and end angles
 at a specified point.
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/lines/line_single.py#L1'>examples/basic/lines/line_single.py~L1</a>
```diff
-""" A line graph using user-defined data points. This example
+"""A line graph using user-defined data points. This example
 demonstrates the use of the ``line`` method to make a graph
 by drawing straight lines between defined points.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/lines/lorenz.py#L1'>examples/basic/lines/lorenz.py~L1</a>
```diff
-""" A plot of the `Lorenz attractor`_. This example demonstrates using
+"""A plot of the `Lorenz attractor`_. This example demonstrates using
 ``multi_line`` to display many lines with a single vectorized glyph.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/color_scatter.py#L1'>examples/basic/scatters/color_scatter.py~L1</a>
```diff
-""" A basic scatter. This example plot demonstrates manual colormapping and
+"""A basic scatter. This example plot demonstrates manual colormapping and
 many different plot tools.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/elements.py#L1'>examples/basic/scatters/elements.py~L1</a>
```diff
-""" A scatter plot using data from the `Periodic Table`_. This example
+"""A scatter plot using data from the `Periodic Table`_. This example
 demonstrates using basic hover tooltips and adding labels for individual
 points.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/image_url.py#L1'>examples/basic/scatters/image_url.py~L1</a>
```diff
-""" An scatter plot showing `Bokeh image logo`_ as marker.
+"""An scatter plot showing `Bokeh image logo`_ as marker.
 
 .. bokeh-example-metadata::
     :apis: bokeh.plotting.figure.image_url
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/markers.py#L1'>examples/basic/scatters/markers.py~L1</a>
```diff
-""" A scatter plot showing every marker type.
+"""A scatter plot showing every marker type.
 
 .. bokeh-example-metadata::
     :apis: bokeh.plotting.figure.scatter, bokeh.plotting.figure.text
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/color_sliders.py#L1'>examples/interaction/js_callbacks/color_sliders.py~L1</a>
```diff
-""" An interactive plot of colors. This example demonstrates adding widgets and
+"""An interactive plot of colors. This example demonstrates adding widgets and
 ``CustomJS`` callbacks that can update a plot.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/customjs_for_hover.py#L1'>examples/interaction/js_callbacks/customjs_for_hover.py~L1</a>
```diff
-""" An interactive plot showcasing Bokeh's ability to add interactions
+"""An interactive plot showcasing Bokeh's ability to add interactions
 using Custom Javascript.
 
 This example demonstrates adding links between points on a graph.
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/customjs_for_selection.py#L1'>examples/interaction/js_callbacks/customjs_for_selection.py~L1</a>
```diff
-""" An interactive plot showcasing Bokeh's ability to add interactions
+"""An interactive plot showcasing Bokeh's ability to add interactions
 using Custom Javascript.
 
 This example demonstrates selecting highlighted points on a graph.
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/doc_js_events.py#L1'>examples/interaction/js_callbacks/doc_js_events.py~L1</a>
```diff
-""" Demonstration of how to register document JS event callbacks. """
+"""Demonstration of how to register document JS event callbacks."""
 
 from bokeh.events import Event
 from bokeh.io import curdoc
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/js_on_event.py#L1'>examples/interaction/js_callbacks/js_on_event.py~L1</a>
```diff
-""" Demonstration of how to register event callbacks using an adaptation
+"""Demonstration of how to register event callbacks using an adaptation
 of the color_scatter example from the bokeh gallery
 """
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/setvalue.py#L1'>examples/interaction/js_callbacks/setvalue.py~L1</a>
```diff
-""" A visualization of buttons in bokeh.models. This example demonstrates
+"""A visualization of buttons in bokeh.models. This example demonstrates
 changing the value of an object when a certain event (like clicking of a button)
 happens.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/slider.py#L1'>examples/interaction/js_callbacks/slider.py~L1</a>
```diff
-""" An interactive plot of the ``sin`` function. This example demonstrates
+"""An interactive plot of the ``sin`` function. This example demonstrates
 adding widgets and ``CustomJS`` callbacks that can update a plot.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/slider_callback_policy.py#L1'>examples/interaction/js_callbacks/slider_callback_policy.py~L1</a>
```diff
-""" An plot of two interactive sliders. The values are updated
+"""An plot of two interactive sliders. The values are updated
 simultaneously as the slider bars are dragged to different values.
 This example demonstrates how ``CustomJS`` callbacks react to user
 interaction events.
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/legends/legend_hide.py#L1'>examples/interaction/legends/legend_hide.py~L1</a>
```diff
-""" The legend_hide feature is used to hide corresponding lines on a plot.
+"""The legend_hide feature is used to hide corresponding lines on a plot.
 This examples demonstrates an interactive line chart for stock prices over
 time which allows the user to click on the legend entries to hide or show
 the corresponding lines.
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tools/range_tool.py#L1'>examples/interaction/tools/range_tool.py~L1</a>
```diff
-""" A timeseries plot using stock price data. This example demonstrates a range
+"""A timeseries plot using stock price data. This example demonstrates a range
 tool controlling the range of another plot. The highlighted range area on the
 lower plot may be dragged to update the range on the top plot.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tooltips/tooltip_content.py#L1'>examples/interaction/tooltips/tooltip_content.py~L1</a>
```diff
-""" A visualization of adding tooltips to widgets in bokeh.models.
+"""A visualization of adding tooltips to widgets in bokeh.models.
 This example demonstrates defining two distinct tooltip widgets,
 i.e. plaintext and html tooltip, and accepting text input through those widgets.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tooltips/tooltip_description.py#L1'>examples/interaction/tooltips/tooltip_description.py~L1</a>
```diff
-""" A visualization of Multichoice tooltips in bokeh.models.
+"""A visualization of Multichoice tooltips in bokeh.models.
 
 This example demonstrates user input text box where the user can input from a
 list of various options of elements to choose one or more of them, and then
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/basic_plot.py#L1'>examples/models/basic_plot.py~L1</a>
```diff
-""" A scatter plot of a smooth periodic oscillation. This example demonstrates red
+"""A scatter plot of a smooth periodic oscillation. This example demonstrates red
 circle scatter markers with black outlines, using the low-level ``bokeh.models``
 API.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/buttons.py#L1'>examples/models/buttons.py~L1</a>
```diff
-""" A rendering of all button widgets.
+"""A rendering of all button widgets.
 
 This example demonstrates the types of button widgets available
 along with radio buttons and checkboxes.
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/calendars.py#L1'>examples/models/calendars.py~L1</a>
```diff
-""" A rendering of the 2014 monthly calendar.
+"""A rendering of the 2014 monthly calendar.
 
 This example demonstrates the usage of plotting several
 plots together using ``gridplot``.
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/daylight.py#L1'>examples/models/daylight.py~L1</a>
```diff
-""" A plot showing the daylight hours in a city's local time (which are
+"""A plot showing the daylight hours in a city's local time (which are
 discontinuous due to summer time) using the ``Patch`` and ``Line`` plot
 elements to draw custom outlines and fills.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/latex_labels.py#L1'>examples/models/latex_labels.py~L1</a>
```diff
-""" A plot showing LaTeX ``Label`` objects in many different locations,
+"""A plot showing LaTeX ``Label`` objects in many different locations,
 inside and outside the figure.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/maps_cities.py#L1'>examples/models/maps_cities.py~L1</a>
```diff
-""" A plot showing a map of the world, highlighting cities where the population
+"""A plot showing a map of the world, highlighting cities where the population
 is over 5,000 people, made using the ``GMapPlot`` class.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/tile_source.py#L1'>examples/models/tile_source.py~L1</a>
```diff
-""" Example to demonstrate creating map-based visualizations and
+"""Example to demonstrate creating map-based visualizations and
 working with geographical data using WMTSTileSource in Bokeh.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/toolbars.py#L1'>examples/models/toolbars.py~L1</a>
```diff
-""" This example shows multiple ways to place the toolbar and the x and y axes
+"""This example shows multiple ways to place the toolbar and the x and y axes
 with respect to the plot
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/toolbars2.py#L1'>examples/models/toolbars2.py~L1</a>
```diff
-""" This example shows multiple ways to place the toolbar with respect to a grid of plots.
+"""This example shows multiple ways to place the toolbar with respect to a grid of plots.
 
 .. bokeh-example-metadata::
     :apis: bokeh.layouts.column, bokeh.layouts.gridplot, bokeh.layouts.row, bokeh.plotting.figure, bokeh.plotting.show
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/transform_jitter.py#L1'>examples/models/transform_jitter.py~L1</a>
```diff
-""" This example demonstrates how to us a jitter transform on coordinate data.
+"""This example demonstrates how to us a jitter transform on coordinate data.
 
 .. bokeh-example-metadata::
     :apis: bokeh.plotting.figure.circle, bokeh.models.sources.ColumnDataSource, bokeh.models.Circle, bokeh.models.Jitter
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/twin_axis.py#L1'>examples/models/twin_axis.py~L1</a>
```diff
-""" This example demonstrates how to create a plot with two y-axes.
+"""This example demonstrates how to create a plot with two y-axes.
 
 .. bokeh-example-metadata::
     :apis: bokeh.plotting.figure.circle, bokeh.models.sources.ColumnDataSource, bokeh.models.markers.Circle
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/widgets.py#L1'>examples/models/widgets.py~L1</a>
```diff
-""" An Example demonstrating the use of multiple widgets. This example uses
+"""An Example demonstrating the use of multiple widgets. This example uses
 buttons, groups, inputs, panels, sliders, and tables, using the low-level
 ``bokeh.models`` API.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/webgl/clustering.py#L1'>examples/output/webgl/clustering.py~L1</a>
```diff
-""" Example inspired by an example from the scikit-learn project:
+"""Example inspired by an example from the scikit-learn project:
 
 http://scikit-learn.org/stable/auto_examples/cluster/plot_cluster_comparison.html
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/webgl/line_compare.py#L1'>examples/output/webgl/line_compare.py~L1</a>
```diff
-""" Compare WebGL, SVG with canvas line.
-
-"""
+"""Compare WebGL, SVG with canvas line."""
 
 import numpy as np
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/webgl/marker_compare.py#L1'>examples/output/webgl/marker_compare.py~L1</a>
```diff
-""" Compare WebGL and SVG markers with canvas markers.
+"""Compare WebGL and SVG markers with canvas markers.
 
 This covers all markers supported by scatter. The plots are put in tabs,
 so that you can easily switch to compare positioning and appearance.
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/webgl/scatter_blend.py#L1'>examples/output/webgl/scatter_blend.py~L1</a>
```diff
-""" The penguins dataset, drawn twice with semi-transparent markers. This is
+"""The penguins dataset, drawn twice with semi-transparent markers. This is
 an interesting use-case to test blending, because several samples itself
 overlap, and by drawing the set twice with different colors, we realize
 even more interesting blending. Also note how this makes use of
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/aspect.py#L1'>examples/plotting/aspect.py~L1</a>
```diff
-""" This example demonstrates how a circle with a data-space radius appears
+"""This example demonstrates how a circle with a data-space radius appears
 when plotted with different aspect scales specified.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/brewer.py#L1'>examples/plotting/brewer.py~L1</a>
```diff
-""" A plot of randomly stacked area styled using the Brewer palette
+"""A plot of randomly stacked area styled using the Brewer palette
 from the `brewer` dictionary.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/custom_tooltip.py#L1'>examples/plotting/custom_tooltip.py~L1</a>
```diff
-""" A plot of periodic table elements using a Periodic table dataset.
+"""A plot of periodic table elements using a Periodic table dataset.
 This example demonstrates the use of custom css tooltips when creating plots.
 The chart shows correlation between atomic mass and density of periodic table
 elements.
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/customjs_expr.py#L1'>examples/plotting/customjs_expr.py~L1</a>
```diff
-""" This example demonstrates using CustomJS Expression to create a sinusoidal line chart.
+"""This example demonstrates using CustomJS Expression to create a sinusoidal line chart.
 
 .. bokeh-example-metadata::
     :apis: bokeh.models.CustomJSExpr, bokeh.model.DataModel
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/customjs_hover.py#L1'>examples/plotting/customjs_hover.py~L1</a>
```diff
-""" A map of North Africa and South Europe with three interactive location
+"""A map of North Africa and South Europe with three interactive location
 points. When hovering over the points, its lat-lon is shown. This example
 demonstrates using CustomJSHover model and HoverTool to customize the
 formatting of values in tooltip fields.
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/dotplot.py#L1'>examples/plotting/dotplot.py~L1</a>
```diff
-""" A categorical dot plot based on simple Python lists of data.
+"""A categorical dot plot based on simple Python lists of data.
 
 .. bokeh-example-metadata::
     :apis: bokeh.plotting.figure.circle, bokeh.plotting.figure.segment
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/filtering.py#L1'>examples/plotting/filtering.py~L1</a>
```diff
-""" A map representation of unemployment rate in US using the `US_States Dataset`_.
+"""A map representation of unemployment rate in US using the `US_States Dataset`_.
 This example demonstrates using IndexFilter, ColorMapper and HoverTool with
 basic plot elements such as patches. When hovering over the points,
 the state and its umemployment rate is shown.
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/histogram.py#L1'>examples/plotting/histogram.py~L1</a>
```diff
-"""  A grid plot shows histograms for four different probability distributions.
+"""A grid plot shows histograms for four different probability distributions.
 
 .. bokeh-example-metadata::
     :apis: bokeh.plotting.figure.line, bokeh.plotting.figure.quad
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/hover.py#L1'>examples/plotting/hover.py~L1</a>
```diff
-""" This example displays a hoverful scatter plot of random data points
+"""This example displays a hoverful scatter plot of random data points
 showing how the hover widget works.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/image_alpha.py#L1'>examples/plotting/image_alpha.py~L1</a>
```diff
-""" An example demonstrating how to add alpha value (transparency) to images in different ways.
+"""An example demonstrating how to add alpha value (transparency) to images in different ways.
 
 .. bokeh-example-metadata::
     :apis: bokeh.plotting.figure, bokeh.plotting.figure.image, bokeh.layouts.Column
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/interactive_legend.py#L1'>examples/plotting/interactive_legend.py~L1</a>
```diff
-""" A line plot using stock price data. Sometimes it is desirable to be able to
+"""A line plot using stock price data. Sometimes it is desirable to be able to
 hide glyphs by clicking on an entry in a ``Legend``. This can be accomplished
 by setting the legend ``click_policy`` property to ``"hide"``.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/iris.py#L1'>examples/plotting/iris.py~L1</a>
```diff
-""" A scatter plot using `Fisher's Iris dataset`_. This example demonstrates
+"""A scatter plot using `Fisher's Iris dataset`_. This example demonstrates
 manual color mapping with basic plot elements. The chart shows correlation
 between petal width and length for three different iris species.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/line_on_off.py#L1'>examples/plotting/line_on_off.py~L1</a>
```diff
-""" Example demonstrating turning lines on and off - with JS only.
+"""Example demonstrating turning lines on and off - with JS only.
 It involves checking and unchecking the checkboxes representing
 the plotted lines to turn the lines on/off.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/line_select.py#L1'>examples/plotting/line_select.py~L1</a>
```diff
-""" Example demonstrating line selection together with customJS.
+"""Example demonstrating line selection together with customJS.
 It involves clicking on any of the plotted lines to select/ deselect the line.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/ridgeplot_subcoordinates.py#L1'>examples/plotting/ridgeplot_subcoordinates.py~L1</a>
```diff
-""" A `ridgeline plot`_ using the `Perceptions of Probability`_ dataset.
+"""A `ridgeline plot`_ using the `Perceptions of Probability`_ dataset.
 
 This example demonstrates the uses of sub-coordinates to position ridge lines at
 different categories. This effectively allows the user to create sub-plots, while
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/sprint.py#L1'>examples/plotting/sprint.py~L1</a>
```diff
-""" A scatter plot comparing historical Olympic sprint times, based on a
+"""A scatter plot comparing historical Olympic sprint times, based on a
 `New York Times interactive`_. Tapping any of the scatter points will open a
 new browser tab for the Wikipedia entry of the sprinter.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/stocks.py#L1'>examples/plotting/stocks.py~L1</a>
```diff
-""" A timeseries plot using stock price data. This example demonstrates adding
+"""A timeseries plot using stock price data. This example demonstrates adding
 multiple plots to a gridplot, and configuring grid bands on an axis.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/tap.py#L1'>examples/plotting/tap.py~L1</a>
```diff
-""" An interactive numerical tap plot based on a simple Python array of data.
+"""An interactive numerical tap plot based on a simple Python array of data.
     You can select any datapoint by tapping on it.
     This highlights that datapoint and displays all other datapoints in a faded color.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/trefoil.py#L1'>examples/plotting/trefoil.py~L1</a>
```diff
-""" This example shows a Radiation Warning Symbol (Trefoil). It demonstrates
+"""This example shows a Radiation Warning Symbol (Trefoil). It demonstrates
 rendering annular wegdes, different arrow heads and adding arc and segment
 glyphs.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/apply_theme.py#L1'>examples/server/app/apply_theme.py~L1</a>
```diff
-""" Example demonstrating how to apply a theme
-
-"""
+"""Example demonstrating how to apply a theme"""
 
 # External imports
 import numpy as np
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/clustering/main.py#L1'>examples/server/app/clustering/main.py~L1</a>
```diff
-""" A `k-nearest neighbors`_ (KNN) chart using datasets from scikit-learn. This
+"""A `k-nearest neighbors`_ (KNN) chart using datasets from scikit-learn. This
 example demonstrates solving both classification and regression problems.
 
 .. note::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/contour_animated.py#L1'>examples/server/app/contour_animated.py~L1</a>
```diff
-""" Animated contour plot.
+"""Animated contour plot.
 
 Use the ``bokeh serve`` command to run the example by executing:
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/crossfilter/main.py#L1'>examples/server/app/crossfilter/main.py~L1</a>
```diff
-""" A crossfilter plot map that uses the `Auto MPG dataset`_. This example
+"""A crossfilter plot map that uses the `Auto MPG dataset`_. This example
 demonstrates the relationship of datasets together. A hover tooltip displays
 information on each dot.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/dash/main.py#L1'>examples/server/app/dash/main.py~L1</a>
```diff
-""" A dashboard that utilizes `Auto MPG`_ and stocks dataset.
+"""A dashboard that utilizes `Auto MPG`_ and stocks dataset.
 This example demonstrates the use of Bootstrap templates to compile several types
 of charts in one file.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/duffing_oscillator.py#L1'>examples/server/app/duffing_oscillator.py~L1</a>
```diff
-""" Simulation of Duffing oscillator which is harmonic motion with a
+"""Simulation of Duffing oscillator which is harmonic motion with a
 sinusoidal driving force and damping. It exhibits chaotic behaviour for some
 combinations of driving and damping parameters. This example demonstrates the
 use of mathtext on ``Div``, ``Paragraph`` and ``Slider`` objects, as well as
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/events_app.py#L1'>examples/server/app/events_app.py~L1</a>
```diff
-""" Demonstration Bokeh app of how to register event callbacks in both
+"""Demonstration Bokeh app of how to register event callbacks in both
 Javascript and Python using an adaptation of the color_scatter example
 from the bokeh gallery. This example extends the js_events.py example
 with corresponding Python event callbacks.
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/export_csv/main.py#L1'>examples/server/app/export_csv/main.py~L1</a>
```diff
-""" A column salary chart with minimum and maximum values.
+"""A column salary chart with minimum and maximum values.
 This example shows the capability of exporting a csv file from ColumnDataSource.
 
 """
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/faces/main.py#L1'>examples/server/app/faces/main.py~L1</a>
```diff
-""" A face detection example that uses the `Haar Cascade`_ algorithm.
+"""A face detection example that uses the `Haar Cascade`_ algorithm.
 This example shows the capability of Bokeh streaming with an integrated
 OpenCV package.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/fourier_animated.py#L1'>examples/server/app/fourier_animated.py~L1</a>
```diff
-""" Show a streaming, animated representation of Fourier Series.
+"""Show a streaming, animated representation of Fourier Series.
 
 The example was inspired by `this video`_.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/gapminder/main.py#L1'>examples/server/app/gapminder/main.py~L1</a>
```diff
-""" A gapminder chart using a population, life expectancy, and fertility dataset.
+"""A gapminder chart using a population, life expectancy, and fertility dataset.
 This example shows the data visualization capability of Bokeh to recreate charts.
 
 """
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/image_blur.py#L1'>examples/server/app/image_blur.py~L1</a>
```diff
-""" An image blurring example. This sample shows the capability
+"""An image blurring example. This sample shows the capability
 of Bokeh to transform images to have certain effects.
 
 .. note::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/line_on_off.py#L1'>examples/server/app/line_on_off.py~L1</a>
```diff
-""" Example demonstrating turning lines on and off - with bokeh server
-
-"""
+"""Example demonstrating turning lines on and off - with bokeh server"""
 
 import numpy as np
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/movies/main.py#L1'>examples/server/app/movies/main.py~L1</a>
```diff
-""" An interactivate categorized chart based on a movie dataset.
+"""An interactivate categorized chart based on a movie dataset.
 This example shows the ability of Bokeh to create a dashboard with different
 sorting options based on a given dataset.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/ohlc/main.py#L1'>examples/server/app/ohlc/main.py~L1</a>
```diff
-""" A stock OHLC chart that monitors the MACD indicator.
+"""A stock OHLC chart that monitors the MACD indicator.
 This example shows the streaming and updating feature of Bokeh charts.
 
 """
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/population.py#L1'>examples/server/app/population.py~L1</a>
```diff
-""" A chart that uses the population dataset of the globe.
+"""A chart that uses the population dataset of the globe.
 This example shows the ability of Bokeh to use two different kinds of graph into
 one file and sorting them by having different colors and lines.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/selection_histogram.py#L1'>examples/server/app/selection_histogram.py~L1</a>
```diff
-""" Present a scatter plot with linked histograms on both axes.
+"""Present a scatter plot with linked histograms on both axes.
 
 Use the ``bokeh serve`` command to run the example by executing:
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/app.py#L1'>examples/server/app/server_auth/app.py~L1</a>
```diff
-""" Present an interactive function explorer with slider widgets.
+"""Present an interactive function explorer with slider widgets.
 
 Scrub the sliders to change the properties of the ``sin`` curve, or
 type into the title text box to update the title of the plot.
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/simple_hdf5/main.py#L1'>examples/server/app/simple_hdf5/main.py~L1</a>
```diff
-""" A simple chart visualizing hdf5 files.
+"""A simple chart visualizing hdf5 files.
 
 .. note::
     This example needs the hdf5 library to run.
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/sliders.py#L1'>examples/server/app/sliders.py~L1</a>
```diff
-""" Present an interactive function explorer with slider widgets.
+"""Present an interactive function explorer with slider widgets.
 
 Scrub the sliders to change the properties of the ``sin`` curve, or
 type into the title text box to update the title of the plot.
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/spectrogram/main.py#L1'>examples/server/app/spectrogram/main.py~L1</a>
```diff
-""" A spectogram chart that uses a waterfall dataset.
+"""A spectogram chart that uses a waterfall dataset.
 This example shows the streaming efficiency of Bokeh with live audio.
 
 .. note::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/stocks/main.py#L1'>examples/server/app/stocks/main.py~L1</a>
```diff
-""" Create a simple stocks correlation dashboard.
+"""Create a simple stocks correlation dashboard.
 
 Choose stocks to compare in the drop down widgets, and make selections
 on the plots to update the summary and histograms accordingly.
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/surface3d/main.py#L1'>examples/server/app/surface3d/main.py~L1</a>
```diff
-""" A 3D graph using the ColumnDataSource of Bokeh with the Graph3d library.
+"""A 3D graph using the ColumnDataSource of Bokeh with the Graph3d library.
 This example shows the custom extension feature of Bokeh.
 
 """
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/taylor.py#L1'>examples/server/app/taylor.py~L1</a>
```diff
-""" A taylor series visualization graph. This example demonstrates
+"""A taylor series visualization graph. This example demonstrates
 the ability of Bokeh for inputted expressions to reflect on a chart.
 
 """
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/weather/main.py#L1'>examples/server/app/weather/main.py~L1</a>
```diff
-""" A weather chart for three cities using a csv file.
+"""A weather chart for three cities using a csv file.
 This illustration demonstrates different interpretation of the same data
 with the distribution option.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_axis_labels_titles_labels.py#L1'>examples/styling/mathtext/latex_axis_labels_titles_labels.py~L1</a>
```diff
-""" This example demonstrates the use of inline mathtext on titles and ``Label`` annotations.
+"""This example demonstrates the use of inline mathtext on titles and ``Label`` annotations.
 This example makes use of all the three LaTeX delimiters pairs ``$$...$$``, ``\\[...\\]`` and
 ``\\(...\\)``.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_bessel.py#L1'>examples/styling/mathtext/latex_bessel.py~L1</a>
```diff
-""" Bessel functions of the first kind. This example demonstrates the use of
+"""Bessel functions of the first kind. This example demonstrates the use of
 mathtext on titles, ``Label`` annotations and axis labels.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_blackbody_radiation.py#L1'>examples/styling/mathtext/latex_blackbody_radiation.py~L1</a>
```diff
-""" A plot of spectral radiance curves for an ideal radiating blackbody at
+"""A plot of spectral radiance curves for an ideal radiating blackbody at
 various temperatures. This example demonstrates the use of mathtext on axes
 and in ``Div`` objects.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_div_widget.py#L1'>examples/styling/mathtext/latex_div_widget.py~L1</a>
```diff
-""" This example demonstrates the use of inline mathtext on a DIV element.
+"""This example demonstrates the use of inline mathtext on a DIV element.
 
 .. bokeh-example-metadata::
     :apis: bokeh.models.widgets.Div
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_normal_distribution.py#L1'>examples/styling/mathtext/latex_normal_distribution.py~L1</a>
```diff
-""" A plot of the Normal (Gaussian) distribution. This example demonstrates the
+"""A plot of the Normal (Gaussian) distribution. This example demonstrates the
 use of mathtext on axes and in ``Div`` objects.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_schrodinger.py#L1'>examples/styling/mathtext/latex_schrodinger.py~L1</a>
```diff
-""" Solution of Schrödinger's equation for the motion of a particle in one
+"""Solution of Schrödinger's equation for the motion of a particle in one
 dimension in a parabolic potential well. This example demonstrates the use of
 mathtext on ``Label`` and ``Title`` annotations.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_slider_widget_title.py#L1'>examples/styling/mathtext/latex_slider_widget_title.py~L1</a>
```diff
-""" This example demonstrates the use of mathtext on a ``Slider`` widget.
+"""This example demonstrates the use of mathtext on a ``Slider`` widget.
 
 .. bokeh-example-metadata::
     :apis: bokeh.models.widgets.Slider
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_tick_labels.py#L1'>examples/styling/mathtext/latex_tick_labels.py~L1</a>
```diff
-""" This example demonstrates the use of mathtext on tick labels through overwriting the labels
+"""This example demonstrates the use of mathtext on tick labels through overwriting the labels
 by adding a dictionary with pairs of position and mathtext.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/mathml_axis_labels.py#L1'>examples/styling/mathtext/mathml_axis_labels.py~L1</a>
```diff
-""" This example demonstrates the use of mathtext as an axis label using a MathML object.
+"""This example demonstrates the use of mathtext as an axis label using a MathML object.
 
 .. bokeh-example-metadata::
     :apis: bokeh.plotting.figure.line, bokeh.models.MathML
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/plots/hatch_grid_band.py#L1'>examples/styling/plots/hatch_grid_band.py~L1</a>
```diff
-""" A simple line plot demonstrating hatched patterns for grid bands.
+"""A simple line plot demonstrating hatched patterns for grid bands.
 
 .. bokeh-example-metadata::
     :apis: bokeh.plotting.figure.line, bokeh.models.Grid
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/categorical/correlogram.py#L1'>examples/topics/categorical/correlogram.py~L1</a>
```diff
-""" A categorical plot showing the correlations in mineral content for 214 samples of glass fragments
+"""A categorical plot showing the correlations in mineral content for 214 samples of glass fragments
 obtained during forensic work.
 
 The dataset contains seven variables measuring the amounts of magnesium (Mg),
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/categorical/heatmap_unemployment.py#L1'>examples/topics/categorical/heatmap_unemployment.py~L1</a>
```diff
-"""  A categorical heatmap using unemployment data. This example demonstrates
+"""A categorical heatmap using unemployment data. This example demonstrates
 adding a ``ColorBar`` to a plot.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/categorical/les_mis.py#L1'>examples/topics/categorical/les_mis.py~L1</a>
```diff
-""" A reproduction of Mike Bostock's `Les Misérables Co-occurrence`_ chart.
+"""A reproduction of Mike Bostock's `Les Misérables Co-occurrence`_ chart.
 This example demonstrates a basic hover tooltip.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/categorical/periodic.py#L1'>examples/topics/categorical/periodic.py~L1</a>
```diff
-""" A rendering of the `Periodic table`_. This example demonstrates combining
+"""A rendering of the `Periodic table`_. This example demonstrates combining
 several glyphs in a single plot. A hover tooltip displays detailed information
 for each element.
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/categorical/ridgeplot.py#L1'>examples/topics/categorical/ridgeplot.py~L1</a>
```diff
-""" A `ridgeline plot`_ using the `Perceptions of Probability`_ dataset.
+"""A `ridgeline plot`_ using the `Perceptions of Probability`_ dataset.
 
 This example demonstrates the uses of categorical offsets to position categorical
 values explicitly, which in this case allows for makeshift sub-plots. This is
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/categorical/scatter_jitter.py#L1'>examples/topics/categorical/scatter_jitter.py~L1</a>
```diff
-""" A categorical scatter plot based on GitHub commit history. This example
+"""A categorical scatter plot based on GitHub commit history. This example
 demonstrates using a ``jitter`` transform.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/categorical/slope_graph.py#L1'>examples/topics/categorical/slope_graph.py~L1</a>
```diff
-""" A categorical scatter plot showing the CO2 emissions of selected countries in the years 2000 and 2010. This example
+"""A categorical scatter plot showing the CO2 emissions of selected countries in the years 2000 and 2010. This example
 demonstrates using the `segment` glyph.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/contour/contour.py#L1'>examples/topics/contour/contour.py~L1</a>
```diff
-""" A contour plot containing lines at each contour level and filled polygons
+"""A contour plot containing lines at each contour level and filled polygons
 between each pair of contour levels.
 
 .. bokeh-example-metadata::
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/contour/contour_polar.py#L1'>examples/topics/contour/contour_polar.py~L1</a>
```diff
-""" Contour plot with polar grid and many visual properties.
+"""Contour plot with polar grid and many visual properties.
 
 .. bokeh-example-metadata::
     :apis: bokeh.plotting.figure.contour, bokeh.models.ContourRenderer.contruct_color_bar
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/contour/contour_simple.py#L1'>examples/topics/contour/contour_simple.py~L1</a>
```diff
-""" Simple contour plot showing both contour lines and filled polygons.
+"""Simple contour plot showing both contour lines and filled polygons.
 
 .. bokeh-example-metadata::
     :apis: bokeh.plotting.figure.contour, bokeh.models.Contour...*[Comment body truncated]*

---

_Marked ready for review by @Glyphack on 2024-01-31 16:30_

---

_Comment by @Glyphack on 2024-01-31 16:34_

@MichaReiser I think this is ready for review. The only testing failure is happening because we are formatting module docstrings now and we remove the last new line character from docstrings after format([example](https://github.com/astral-sh/ruff/pull/9725/files#diff-4907300a6642e5c2d6fc508e6a89e18066b40c57d0afc2cf7d118e00f60f0ed8L75)). I'm not sure if we want to keep this or should I opt out of this for module docstrings to keep compatibility. Because this is not listed under [intentional deviations](https://github.com/astral-sh/ruff/tree/main/crates/ruff_python_formatter#intentional-deviations).

---

_@MichaReiser reviewed on 2024-01-31 16:56_

The changes here look good but I think we have to look into why we trim the last line or this will be a very disruptive change.

---

_Comment by @MichaReiser on 2024-01-31 18:25_

@Glyphack I can take a look tomorrow morning except if you want to do it. I'm really curious what's causing this difference. That's not at all what I expected.

---

_Comment by @MichaReiser on 2024-01-31 18:28_

Oh I think I have a suspision... The difference is that module level docstring have no indent... Some code in the docstring formatting must be relying on the indent to be present.

---

_Comment by @Glyphack on 2024-01-31 18:39_

@MichaReiser I am interested and looking into it. Will post my findings.

Another thing I checked is that looks like black is not formatting module docstrings.

source:
```
"""
Docstring with trailing newline. That is preserved.
"""

def function():
    """Docstring with trailing newline. That is not preserved.
    """
```

black output:
```
"""
Docstring with trailing newline. That is preserved.
"""


def function():
    """Docstring with trailing newline. That is not preserved."""
```

ruff output if we format module docstrings:

```
"""
Docstring with trailing newline. That is preserved."""


def function():
    """Docstring with trailing newline. That is not preserved."""
```

---

_Renamed from "Format module level docstring" to "Preview Style: Format module level docstring" by @Glyphack on 2024-02-01 07:30_

---

_Converted to draft by @Glyphack on 2024-02-01 08:03_

---

_Marked ready for review by @Glyphack on 2024-02-01 18:40_

---

_Review requested from @MichaReiser by @Glyphack on 2024-02-01 18:46_

---

_Label `preview` added by @MichaReiser on 2024-02-02 06:15_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/docstring.rs`:215 on 2024-02-02 06:30_

I think this is a bug in our codebock formatting or docstring formatting over all and is not specific to module formatting (CC: @BurntSushi). 

For example:

```python
class Test:
    
    """
    Black's `Preview.module_docstring_newlines`


    A code black to format
"""
```

Black formats this to 

```python
class Test:
    """
    Black's `Preview.module_docstring_newlines`


    A code black to format
    """
```

and Ruff formats it to 

```python
class Test:
    """
    Black's `Preview.module_docstring_newlines`


    A code black to format"""
```

Note how the last line gets collapsed. 

The relevant logic is in https://github.com/astral-sh/ruff/blob/935b59adaeb7da4d0b21f524d5da46583e779e04/crates/ruff_python_formatter/src/string/docstring.rs#L388-L398

My understanding is that the last line should only be collapsed for docstrings with a single content line:

Collapse 

```python
class Test:
    """Black A code black to format
    
    """
```

Don't collapse for 

```python
class Test:
    """Black A code black to format
    d
    """
```

Keep it collapsed when it was collapsed in the source code

```python
class Test:
    """Black A code black to format
    a"""
```

I suggest that we revert this change and address this issue in its own PR


---

_@MichaReiser approved on 2024-02-02 06:30_

Nice! I suggest that we revert the changes in `docstring.rs` and address the "collapse" issue separately because it isn't specific to module docstrings.

---

_@Glyphack reviewed on 2024-02-02 22:31_

---

_Review comment by @Glyphack on `crates/ruff_python_formatter/src/string/docstring.rs`:215 on 2024-02-02 22:31_

I reverted the change just note that the current implementation  would have a different formatting if applied twice in the following example. Just flagging it so if you want we can keep this PR open until that one gets fixed.

```
"""
A docstring

.. code-block:: python

    button.on_event(ButtonClick, callback)

"""

Format once:
"""
A docstring

.. code-block:: python

    button.on_event(ButtonClick, callback)
"""

Format twice:
"""
A docstring

.. code-block:: python

    button.on_event(ButtonClick, callback)"""
```

---

_Merged by @MichaReiser on 2024-02-05 15:03_

---

_Closed by @MichaReiser on 2024-02-05 15:03_

---

_Branch deleted on 2024-02-05 16:22_

---
