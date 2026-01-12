```yaml
number: 11019
title: "[UP031] When encountering `\"%s\" % var` offer unsafe fix"
type: pull_request
state: merged
author: plredmond
labels:
  - rule
assignees: []
merged: true
base: main
head: ruff.UP031
created_at: 2024-04-19T00:23:09Z
updated_at: 2024-05-09T21:32:03Z
url: https://github.com/astral-sh/ruff/pull/11019
synced_at: 2026-01-12T15:55:34Z
```

# [UP031] When encountering `"%s" % var` offer unsafe fix

---

_@plredmond_

Resolves #10187

<details>
<summary>Old PR description; accurate through commit e86dd7d; probably best to leave this fold closed</summary>

## Description of change

In the case of a printf-style format string with only one %-placeholder and a variable at right (e.g.  `"%s" % var`):

* The new behavior attempts to dereference the variable and then match on the bound expression to distinguish between a 1-tuple (fix), n-tuple (bug :bug:), or a non-tuple (fix). Dereferencing is via `analyze::typing::find_binding_value`.
* If the variable cannot be dereferenced, then the type-analysis routine is called to distinguish only tuple (no-fix) or non-tuple (fix). Type analysis is via `analyze::typing::is_tuple`.
* If any of the above fails, the rule still fires, but no fix is offered.

## Alternatives

* If the reviewers think that singling out the 1-tuple case is too complicated, I will remove that.
* The ecosystem results show that no new fixes are detected. So I could probably delete all the variable dereferencing code and code that tries to generate fixes, tbh.

## Changes to existing behavior

**All the previous rule-firings and fixes are unchanged except for** the "false negatives" in `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP031_1.py`. Those previous "false negatives" are now true positives and so I moved them to `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP031_0.py`.

<details>
<summary>Existing false negatives that are now true positives</summary>

```
crates/ruff_linter/resources/test/fixtures/pyupgrade/UP031_0.py:134:1: UP031 Use format specifiers instead of percent format
    |
133 | # UP031 (no longer false negatives)
134 | 'Hello %s' % bar
    | ^^^^^^^^^^^^^^^^ UP031
135 |
136 | 'Hello %s' % bar.baz
    |
    = help: Replace with format specifiers

crates/ruff_linter/resources/test/fixtures/pyupgrade/UP031_0.py:136:1: UP031 Use format specifiers instead of percent format
    |
134 | 'Hello %s' % bar
135 |
136 | 'Hello %s' % bar.baz
    | ^^^^^^^^^^^^^^^^^^^^ UP031
137 |
138 | 'Hello %s' % bar['bop']
    |
    = help: Replace with format specifiers

crates/ruff_linter/resources/test/fixtures/pyupgrade/UP031_0.py:138:1: UP031 Use format specifiers instead of percent format
    |
136 | 'Hello %s' % bar.baz
137 |
138 | 'Hello %s' % bar['bop']
    | ^^^^^^^^^^^^^^^^^^^^^^^ UP031
    |
    = help: Replace with format specifiers
```
One of them newly offers a fix.
```
 # UP031 (no longer false negatives)
-'Hello %s' % bar
+'Hello {}'.format(bar)
```
This fix occurs because the new code dereferences `bar` to where it was defined earlier in the file as a non-tuple:
```python
bar = {"bar": y}
```

---

</details>

## Behavior requiring new tests

Additionally, we now handle a few cases that we didn't previously test. These cases are when a string has a single %-placeholder and the righthand operand to the modulo operator is a variable **which can be dereferenced.** One of those was shown in the previous section (the "dereference non-tuple" case).

<details>
<summary>New cases handled</summary>

```
crates/ruff_linter/resources/test/fixtures/pyupgrade/UP031_0.py:126:1: UP031 [*] Use format specifiers instead of percent format
    |
125 | t1 = (x,)
126 | "%s" % t1
    | ^^^^^^^^^ UP031
127 | # UP031: deref t1 to 1-tuple, offer fix
    |
    = help: Replace with format specifiers

crates/ruff_linter/resources/test/fixtures/pyupgrade/UP031_0.py:130:1: UP031 Use format specifiers instead of percent format
    |
129 | t2 = (x,y)
130 | "%s" % t2
    | ^^^^^^^^^ UP031
131 | # UP031: deref t2 to n-tuple, this is a bug
    |
    = help: Replace with format specifiers
```
One of these offers a fix.
```
 t1 = (x,)
-"%s" % t1
+"{}".format(t1[0])
 # UP031: deref t1 to 1-tuple, offer fix
```
The other doesn't offer a fix because it's a bug.

---

</details>

---

</details>


## Changes to existing behavior

In the case of a string with a single %-placeholder and a single ambiguous righthand argument to the modulo operator, (e.g. `"%s" % var`) the rule now fires and offers a fix. We explain about this in the "fix safety" section of the updated documentation.


## Documentation changes

I swapped the order of the "known problems" and the "examples" sections so that the examples which describe the rule are first, before the exceptions to the rule are described. I also tweaked the language to be more explicit, as I had trouble understanding the documentation at first. The "known problems" section is now "fix safety" but the content is largely similar.

The diff of the documentation changes looks a little difficult unless you look at the individual commits. 

---

_Comment by @github-actions[bot] on 2024-04-19 00:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+658 -0 violations, +0 -0 fixes in 9 projects; 35 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+63 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/customjs_for_hover.py#L37'>examples/interaction/js_callbacks/customjs_for_hover.py:37:8:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/trail.py#L73'>examples/models/trail.py:73:23:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/trail.py#L92'>examples/models/trail.py:92:23:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/app.py#L56'>examples/server/app/server_auth/app.py:56:50:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L168'>src/bokeh/application/application.py:168:32:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L355'>src/bokeh/client/connection.py:355:31:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/bootstrap.py#L92'>src/bokeh/command/bootstrap.py:92:13:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L88'>src/bokeh/command/subcommands/file_output.py:88:19:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L487'>src/bokeh/command/subcommands/serve.py:487:19:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L494'>src/bokeh/command/subcommands/serve.py:494:19:</a> UP031 Use format specifiers instead of percent format
... 53 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+326 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Accessdata/Integrations/Accessdata/Accessdata.py#L380'>Packs/Accessdata/Integrations/Accessdata/Accessdata.py:380:9:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/ActiveMQ/Integrations/ActiveMQ/ActiveMQ.py#L25'>Packs/ActiveMQ/Integrations/ActiveMQ/ActiveMQ.py:25:25:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/ACME/ACME.py#L176'>Packs/Ansible_Powered_Integrations/Integrations/ACME/ACME.py:176:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/AlibabaCloud/AlibabaCloud.py#L136'>Packs/Ansible_Powered_Integrations/Integrations/AlibabaCloud/AlibabaCloud.py:136:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/AzureComputeV3/AzureComputeV3.py#L136'>Packs/Ansible_Powered_Integrations/Integrations/AzureComputeV3/AzureComputeV3.py:136:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/AzureNetworking/AzureNetworking.py#L136'>Packs/Ansible_Powered_Integrations/Integrations/AzureNetworking/AzureNetworking.py:136:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/CiscoIOS/CiscoIOS.py#L181'>Packs/Ansible_Powered_Integrations/Integrations/CiscoIOS/CiscoIOS.py:181:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/CiscoNXOS/CiscoNXOS.py#L181'>Packs/Ansible_Powered_Integrations/Integrations/CiscoNXOS/CiscoNXOS.py:181:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/DNS/DNS.py#L136'>Packs/Ansible_Powered_Integrations/Integrations/DNS/DNS.py:136:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/HCloud/HCloud.py#L136'>Packs/Ansible_Powered_Integrations/Integrations/HCloud/HCloud.py:136:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/Kubernetes/Kubernetes.py#L136'>Packs/Ansible_Powered_Integrations/Integrations/Kubernetes/Kubernetes.py:136:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/Linux/Linux.py#L176'>Packs/Ansible_Powered_Integrations/Integrations/Linux/Linux.py:176:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/MicrosoftWindows/MicrosoftWindows.py#L173'>Packs/Ansible_Powered_Integrations/Integrations/MicrosoftWindows/MicrosoftWindows.py:173:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/OpenSSL/OpenSSL.py#L176'>Packs/Ansible_Powered_Integrations/Integrations/OpenSSL/OpenSSL.py:176:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/VMwareV2/VMwareV2.py#L136'>Packs/Ansible_Powered_Integrations/Integrations/VMwareV2/VMwareV2.py:136:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/AwakeSecurity/Integrations/AwakeSecurity/AwakeSecurity.py#L447'>Packs/AwakeSecurity/Integrations/AwakeSecurity/AwakeSecurity.py:447:5:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery.py#L336'>Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery.py:336:64:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery.py#L56'>Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery.py:56:21:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery.py#L58'>Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery.py:58:21:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py#L386'>Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py:386:27:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py#L481'>Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py:481:41:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py#L490'>Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py:490:17:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py#L539'>Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py:539:26:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py#L588'>Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py:588:20:</a> UP031 Use format specifiers instead of percent format
... 302 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/docker/docker-py/blob/b6464dbed92b14b2c61d5ee49805fce041a3e083/docker/utils/ports.py#L41'>docker/utils/ports.py:41:22:</a> UP031 Use format specifiers instead of percent format
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+120 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/models.py#L201'>securedrop/models.py:201:16:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/models.py#L299'>securedrop/models.py:299:16:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/models.py#L94'>securedrop/models.py:94:16:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_logger.py#L66'>securedrop/pretty_bad_protocol/_logger.py:66:14:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_meta.py#L1000'>securedrop/pretty_bad_protocol/_meta.py:1000:29:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_meta.py#L1026'>securedrop/pretty_bad_protocol/_meta.py:1026:27:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_meta.py#L1034'>securedrop/pretty_bad_protocol/_meta.py:1034:23:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_meta.py#L1037'>securedrop/pretty_bad_protocol/_meta.py:1037:22:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_meta.py#L1048'>securedrop/pretty_bad_protocol/_meta.py:1048:29:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_meta.py#L1050'>securedrop/pretty_bad_protocol/_meta.py:1050:29:</a> UP031 Use format specifiers instead of percent format
... 110 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/88de65d986b21de97782486e2cd5285b0609c057/ibis/backends/postgres/tests/test_functions.py#L746'>ibis/backends/postgres/tests/test_functions.py:746:55:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/ibis-project/ibis/blob/88de65d986b21de97782486e2cd5285b0609c057/ibis/backends/postgres/tests/test_functions.py#L759'>ibis/backends/postgres/tests/test_functions.py:759:34:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/ibis-project/ibis/blob/88de65d986b21de97782486e2cd5285b0609c057/ibis/backends/postgres/tests/test_functions.py#L772'>ibis/backends/postgres/tests/test_functions.py:772:55:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/ibis-project/ibis/blob/88de65d986b21de97782486e2cd5285b0609c057/ibis/backends/risingwave/tests/test_functions.py#L546'>ibis/backends/risingwave/tests/test_functions.py:546:55:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/ibis-project/ibis/blob/88de65d986b21de97782486e2cd5285b0609c057/ibis/backends/risingwave/tests/test_functions.py#L561'>ibis/backends/risingwave/tests/test_functions.py:561:55:</a> UP031 Use format specifiers instead of percent format
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+33 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/b455522cb99600c3e271972d4ca87bdb3f3c9a75/examples/pytorch/CaptumExample/Titanic_Captum_Interpret.py#L133'>examples/pytorch/CaptumExample/Titanic_Captum_Interpret.py:133:39:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/mlflow/mlflow/blob/b455522cb99600c3e271972d4ca87bdb3f3c9a75/examples/pytorch/mnist_tensorboard_artifact.py#L218'>examples/pytorch/mnist_tensorboard_artifact.py:218:9:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/mlflow/mlflow/blob/b455522cb99600c3e271972d4ca87bdb3f3c9a75/examples/pytorch/mnist_tensorboard_artifact.py#L226'>examples/pytorch/mnist_tensorboard_artifact.py:226:9:</a> UP031 Use format specifiers instead of percent format
+ examples/rapids/mlflow_project/notebooks/rapids_mlflow.ipynb:cell 13:44:18: UP031 Use format specifiers instead of percent format
+ examples/sklearn_elasticnet_wine/train.ipynb:cell 2:66:15: UP031 Use format specifiers instead of percent format
+ examples/sklearn_elasticnet_wine/train.ipynb:cell 2:67:15: UP031 Use format specifiers instead of percent format
+ examples/sklearn_elasticnet_wine/train.ipynb:cell 2:68:15: UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/mlflow/mlflow/blob/b455522cb99600c3e271972d4ca87bdb3f3c9a75/mlflow/legacy_databricks_cli/configure/provider.py#L131'>mlflow/legacy_databricks_cli/configure/provider.py:131:13:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/mlflow/mlflow/blob/b455522cb99600c3e271972d4ca87bdb3f3c9a75/mlflow/legacy_databricks_cli/configure/provider.py#L175'>mlflow/legacy_databricks_cli/configure/provider.py:175:25:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/mlflow/mlflow/blob/b455522cb99600c3e271972d4ca87bdb3f3c9a75/mlflow/projects/_project_spec.py#L111'>mlflow/projects/_project_spec.py:111:17:</a> UP031 Use format specifiers instead of percent format
... 23 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (+82 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/misc/dump-ast.py#L42'>misc/dump-ast.py:42:34:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/misc/gen_blog_post_html.py#L31'>misc/gen_blog_post_html.py:31:26:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/mypy/config_parser.py#L346'>mypy/config_parser.py:346:27:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/mypy/config_parser.py#L666'>mypy/config_parser.py:666:12:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/mypy/dmypy/client.py#L599'>mypy/dmypy/client.py:599:37:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/mypy/main.py#L1320'>mypy/main.py:1320:13:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/mypy/main.py#L1487'>mypy/main.py:1487:17:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/mypy/main.py#L1491'>mypy/main.py:1491:17:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/mypy/semanal.py#L2015'>mypy/semanal.py:2015:31:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/mypy/semanal.py#L2332'>mypy/semanal.py:2332:17:</a> UP031 Use format specifiers instead of percent format
... 72 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build/blob/c97af94e3a5d4ba2228cd5425e1290e5bb4ee30a/tests/test_hello_fortran.py#L61'>tests/test_hello_fortran.py:61:9:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/scikit-build/scikit-build/blob/c97af94e3a5d4ba2228cd5425e1290e5bb4ee30a/tests/test_hello_fortran.py#L62'>tests/test_hello_fortran.py:62:9:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/scikit-build/scikit-build/blob/c97af94e3a5d4ba2228cd5425e1290e5bb4ee30a/tests/test_issue342_cmake_osx_args_in_setup.py#L156'>tests/test_issue342_cmake_osx_args_in_setup.py:156:41:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/scikit-build/scikit-build/blob/c97af94e3a5d4ba2228cd5425e1290e5bb4ee30a/tests/test_setup.py#L349'>tests/test_setup.py:349:45:</a> UP031 Use format specifiers instead of percent format
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+24 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/cli/shell.py#L39'>indico/cli/shell.py:39:29:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/cli/shell.py#L41'>indico/cli/shell.py:41:29:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/cli/shell.py#L52'>indico/cli/shell.py:52:29:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/cli/shell.py#L54'>indico/cli/shell.py:54:29:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/core/db/sqlalchemy/custom/ip_network.py#L44'>indico/core/db/sqlalchemy/custom/ip_network.py:44:22:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/migrations/versions/20200904_1543_f37d509e221c_add_user_profile_picture_source_column.py#L38'>indico/migrations/versions/20200904_1543_f37d509e221c_add_user_profile_picture_source_column.py:38:16:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/modules/events/static/offline.py#L196'>indico/modules/events/static/offline.py:196:41:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/modules/events/static/offline.py#L198'>indico/modules/events/static/offline.py:198:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/modules/events/static/offline.py#L202'>indico/modules/events/static/offline.py:202:42:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/modules/rb/api.py#L329'>indico/modules/rb/api.py:329:22:</a> UP031 Use format specifiers instead of percent format
... 14 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP031 | 658 | 658 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+658 -0 violations, +0 -0 fixes in 9 projects; 35 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+63 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/customjs_for_hover.py#L37'>examples/interaction/js_callbacks/customjs_for_hover.py:37:8:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/trail.py#L73'>examples/models/trail.py:73:23:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/trail.py#L92'>examples/models/trail.py:92:23:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/app.py#L56'>examples/server/app/server_auth/app.py:56:50:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L168'>src/bokeh/application/application.py:168:32:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L355'>src/bokeh/client/connection.py:355:31:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/bootstrap.py#L92'>src/bokeh/command/bootstrap.py:92:13:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L88'>src/bokeh/command/subcommands/file_output.py:88:19:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L487'>src/bokeh/command/subcommands/serve.py:487:19:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L494'>src/bokeh/command/subcommands/serve.py:494:19:</a> UP031 Use format specifiers instead of percent format
... 53 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+326 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Accessdata/Integrations/Accessdata/Accessdata.py#L380'>Packs/Accessdata/Integrations/Accessdata/Accessdata.py:380:9:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/ActiveMQ/Integrations/ActiveMQ/ActiveMQ.py#L25'>Packs/ActiveMQ/Integrations/ActiveMQ/ActiveMQ.py:25:25:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/ACME/ACME.py#L176'>Packs/Ansible_Powered_Integrations/Integrations/ACME/ACME.py:176:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/AlibabaCloud/AlibabaCloud.py#L136'>Packs/Ansible_Powered_Integrations/Integrations/AlibabaCloud/AlibabaCloud.py:136:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/AzureComputeV3/AzureComputeV3.py#L136'>Packs/Ansible_Powered_Integrations/Integrations/AzureComputeV3/AzureComputeV3.py:136:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/AzureNetworking/AzureNetworking.py#L136'>Packs/Ansible_Powered_Integrations/Integrations/AzureNetworking/AzureNetworking.py:136:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/CiscoIOS/CiscoIOS.py#L181'>Packs/Ansible_Powered_Integrations/Integrations/CiscoIOS/CiscoIOS.py:181:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/CiscoNXOS/CiscoNXOS.py#L181'>Packs/Ansible_Powered_Integrations/Integrations/CiscoNXOS/CiscoNXOS.py:181:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/DNS/DNS.py#L136'>Packs/Ansible_Powered_Integrations/Integrations/DNS/DNS.py:136:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/HCloud/HCloud.py#L136'>Packs/Ansible_Powered_Integrations/Integrations/HCloud/HCloud.py:136:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/Kubernetes/Kubernetes.py#L136'>Packs/Ansible_Powered_Integrations/Integrations/Kubernetes/Kubernetes.py:136:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/Linux/Linux.py#L176'>Packs/Ansible_Powered_Integrations/Integrations/Linux/Linux.py:176:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/MicrosoftWindows/MicrosoftWindows.py#L173'>Packs/Ansible_Powered_Integrations/Integrations/MicrosoftWindows/MicrosoftWindows.py:173:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/OpenSSL/OpenSSL.py#L176'>Packs/Ansible_Powered_Integrations/Integrations/OpenSSL/OpenSSL.py:176:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Ansible_Powered_Integrations/Integrations/VMwareV2/VMwareV2.py#L136'>Packs/Ansible_Powered_Integrations/Integrations/VMwareV2/VMwareV2.py:136:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/AwakeSecurity/Integrations/AwakeSecurity/AwakeSecurity.py#L447'>Packs/AwakeSecurity/Integrations/AwakeSecurity/AwakeSecurity.py:447:5:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery.py#L336'>Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery.py:336:64:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery.py#L56'>Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery.py:56:21:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery.py#L58'>Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery.py:58:21:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py#L386'>Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py:386:27:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py#L481'>Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py:481:41:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py#L490'>Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py:490:17:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py#L539'>Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py:539:26:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/demisto/content/blob/6fbe2174ff85a349efc19147e44a5e7f95909482/Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py#L588'>Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py:588:20:</a> UP031 Use format specifiers instead of percent format
... 302 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/docker/docker-py/blob/b6464dbed92b14b2c61d5ee49805fce041a3e083/docker/utils/ports.py#L41'>docker/utils/ports.py:41:22:</a> UP031 Use format specifiers instead of percent format
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+120 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/models.py#L201'>securedrop/models.py:201:16:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/models.py#L299'>securedrop/models.py:299:16:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/models.py#L94'>securedrop/models.py:94:16:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_logger.py#L66'>securedrop/pretty_bad_protocol/_logger.py:66:14:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_meta.py#L1000'>securedrop/pretty_bad_protocol/_meta.py:1000:29:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_meta.py#L1026'>securedrop/pretty_bad_protocol/_meta.py:1026:27:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_meta.py#L1034'>securedrop/pretty_bad_protocol/_meta.py:1034:23:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_meta.py#L1037'>securedrop/pretty_bad_protocol/_meta.py:1037:22:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_meta.py#L1048'>securedrop/pretty_bad_protocol/_meta.py:1048:29:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_meta.py#L1050'>securedrop/pretty_bad_protocol/_meta.py:1050:29:</a> UP031 Use format specifiers instead of percent format
... 110 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/88de65d986b21de97782486e2cd5285b0609c057/ibis/backends/postgres/tests/test_functions.py#L746'>ibis/backends/postgres/tests/test_functions.py:746:55:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/ibis-project/ibis/blob/88de65d986b21de97782486e2cd5285b0609c057/ibis/backends/postgres/tests/test_functions.py#L759'>ibis/backends/postgres/tests/test_functions.py:759:34:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/ibis-project/ibis/blob/88de65d986b21de97782486e2cd5285b0609c057/ibis/backends/postgres/tests/test_functions.py#L772'>ibis/backends/postgres/tests/test_functions.py:772:55:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/ibis-project/ibis/blob/88de65d986b21de97782486e2cd5285b0609c057/ibis/backends/risingwave/tests/test_functions.py#L546'>ibis/backends/risingwave/tests/test_functions.py:546:55:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/ibis-project/ibis/blob/88de65d986b21de97782486e2cd5285b0609c057/ibis/backends/risingwave/tests/test_functions.py#L561'>ibis/backends/risingwave/tests/test_functions.py:561:55:</a> UP031 Use format specifiers instead of percent format
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+33 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/b455522cb99600c3e271972d4ca87bdb3f3c9a75/examples/pytorch/CaptumExample/Titanic_Captum_Interpret.py#L133'>examples/pytorch/CaptumExample/Titanic_Captum_Interpret.py:133:39:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/mlflow/mlflow/blob/b455522cb99600c3e271972d4ca87bdb3f3c9a75/examples/pytorch/mnist_tensorboard_artifact.py#L218'>examples/pytorch/mnist_tensorboard_artifact.py:218:9:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/mlflow/mlflow/blob/b455522cb99600c3e271972d4ca87bdb3f3c9a75/examples/pytorch/mnist_tensorboard_artifact.py#L226'>examples/pytorch/mnist_tensorboard_artifact.py:226:9:</a> UP031 Use format specifiers instead of percent format
+ examples/rapids/mlflow_project/notebooks/rapids_mlflow.ipynb:cell 13:44:18: UP031 Use format specifiers instead of percent format
+ examples/sklearn_elasticnet_wine/train.ipynb:cell 2:66:15: UP031 Use format specifiers instead of percent format
+ examples/sklearn_elasticnet_wine/train.ipynb:cell 2:67:15: UP031 Use format specifiers instead of percent format
+ examples/sklearn_elasticnet_wine/train.ipynb:cell 2:68:15: UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/mlflow/mlflow/blob/b455522cb99600c3e271972d4ca87bdb3f3c9a75/mlflow/legacy_databricks_cli/configure/provider.py#L131'>mlflow/legacy_databricks_cli/configure/provider.py:131:13:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/mlflow/mlflow/blob/b455522cb99600c3e271972d4ca87bdb3f3c9a75/mlflow/legacy_databricks_cli/configure/provider.py#L175'>mlflow/legacy_databricks_cli/configure/provider.py:175:25:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/mlflow/mlflow/blob/b455522cb99600c3e271972d4ca87bdb3f3c9a75/mlflow/projects/_project_spec.py#L111'>mlflow/projects/_project_spec.py:111:17:</a> UP031 Use format specifiers instead of percent format
... 23 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (+82 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/misc/dump-ast.py#L42'>misc/dump-ast.py:42:34:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/misc/gen_blog_post_html.py#L31'>misc/gen_blog_post_html.py:31:26:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/mypy/config_parser.py#L346'>mypy/config_parser.py:346:27:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/mypy/config_parser.py#L666'>mypy/config_parser.py:666:12:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/mypy/dmypy/client.py#L599'>mypy/dmypy/client.py:599:37:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/mypy/main.py#L1320'>mypy/main.py:1320:13:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/mypy/main.py#L1487'>mypy/main.py:1487:17:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/mypy/main.py#L1491'>mypy/main.py:1491:17:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/mypy/semanal.py#L2015'>mypy/semanal.py:2015:31:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/df35dcf020b3b03a8e3280edf8ada8c6ad8e0da5/mypy/semanal.py#L2332'>mypy/semanal.py:2332:17:</a> UP031 Use format specifiers instead of percent format
... 72 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build/blob/c97af94e3a5d4ba2228cd5425e1290e5bb4ee30a/tests/test_hello_fortran.py#L61'>tests/test_hello_fortran.py:61:9:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/scikit-build/scikit-build/blob/c97af94e3a5d4ba2228cd5425e1290e5bb4ee30a/tests/test_hello_fortran.py#L62'>tests/test_hello_fortran.py:62:9:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/scikit-build/scikit-build/blob/c97af94e3a5d4ba2228cd5425e1290e5bb4ee30a/tests/test_issue342_cmake_osx_args_in_setup.py#L156'>tests/test_issue342_cmake_osx_args_in_setup.py:156:41:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/scikit-build/scikit-build/blob/c97af94e3a5d4ba2228cd5425e1290e5bb4ee30a/tests/test_setup.py#L349'>tests/test_setup.py:349:45:</a> UP031 Use format specifiers instead of percent format
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+24 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/cli/shell.py#L39'>indico/cli/shell.py:39:29:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/cli/shell.py#L41'>indico/cli/shell.py:41:29:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/cli/shell.py#L52'>indico/cli/shell.py:52:29:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/cli/shell.py#L54'>indico/cli/shell.py:54:29:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/core/db/sqlalchemy/custom/ip_network.py#L44'>indico/core/db/sqlalchemy/custom/ip_network.py:44:22:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/migrations/versions/20200904_1543_f37d509e221c_add_user_profile_picture_source_column.py#L38'>indico/migrations/versions/20200904_1543_f37d509e221c_add_user_profile_picture_source_column.py:38:16:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/modules/events/static/offline.py#L196'>indico/modules/events/static/offline.py:196:41:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/modules/events/static/offline.py#L198'>indico/modules/events/static/offline.py:198:40:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/modules/events/static/offline.py#L202'>indico/modules/events/static/offline.py:202:42:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/indico/indico/blob/fc9d975d16b02bd70a072e0396522b39999081e7/indico/modules/rb/api.py#L329'>indico/modules/rb/api.py:329:22:</a> UP031 Use format specifiers instead of percent format
... 14 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP031 | 658 | 658 | 0 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @plredmond on 2024-04-19 00:51_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:337 on 2024-04-19 01:03_

I think this will only return `true` if the value is definitively a tuple, but if the value can't be determined, it _could_ still be a tuple. Imagine, e .g., a function argument with no annotation.

---

_@charliermarsh reviewed on 2024-04-19 01:03_

---

_@charliermarsh reviewed on 2024-04-19 01:03_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:323 on 2024-04-19 01:03_

Nit: By convention I'd prefer the constant on the right-hand side of the operator. At first I misread this.

---

_@charliermarsh reviewed on 2024-04-19 01:04_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:337 on 2024-04-19 01:04_

In other words, couldn't the value still be a tuple, even if `is_tuple` returns `false`? I'm actually not sure why we aren't seeing more fixes in the ecosystem checks given this.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:312 on 2024-04-19 05:56_

I probably misread this, but isn't the method always returning `Cow::Owned`? 

---

_@MichaReiser reviewed on 2024-04-19 05:57_

---

_Label `rule` added by @dhruvmanila on 2024-04-19 08:15_

---

_@plredmond reviewed on 2024-04-19 14:13_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:337 on 2024-04-19 14:13_

Yeah, it seems odd. The code using `find_binding_value` should not be suceeding in all cases, so we should be seeing the behavior fall back to `is_tuple` sometimes, and therefore offer that fix (wrongly) in some of those cases. :thinking: 

---

_@plredmond reviewed on 2024-04-19 14:14_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:312 on 2024-04-19 14:14_

The method uses `?` syntax to unwrap optionals in at least one place.

---

_@charliermarsh reviewed on 2024-04-19 14:20_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:312 on 2024-04-19 14:20_

I think the suggestion is that this can be `Option<String>`, since you aren't using the `Cow::Borrowed` variant.

---

_@charliermarsh reviewed on 2024-04-19 14:25_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:337 on 2024-04-19 14:25_

Yeah, the behavior is matching my expectation.

```python
zzz = "abc"
'Hello %s' % zzz
```

Here, `find_binding_value` returns a string, so we hit line 332 and still offer a fix.

For cases in which `find_binding_value` returns `None`, like:

```python
zzz = foo()
'Hello %s' % zzz
```

We're also offering a fix, because `analyze::typing::is_tuple` returns `false` there (we don't know if `foo()` is or isn't a tuple).


---

_@charliermarsh reviewed on 2024-04-19 14:28_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:337 on 2024-04-19 14:28_

Even `analyze::typing::find_binding_value` isn't completely correct here, because there could be a conditional binding:

```python
if x > 0:
  zzz = (1,)
else:
  zzz = "foo"
```

So you'd need to make sure that there is exactly one binding to `zzz` in the scope.

---

_@plredmond reviewed on 2024-04-19 16:49_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:312 on 2024-04-19 16:49_

Ok! I was cargo culting the `Cow::Borrowed` constructor. Will fix.

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:337 on 2024-04-19 16:50_

It seems that dereffing the variable and/or trying to analyze its type is not worth the additional code.

---

_@plredmond reviewed on 2024-04-19 16:51_

---

_@plredmond reviewed on 2024-04-19 17:31_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:337 on 2024-04-19 17:31_

Code won't exist in the next version that I push.

---

_@plredmond reviewed on 2024-04-19 17:31_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:312 on 2024-04-19 17:31_

Code won't exist in the next version that I push.

---

_@plredmond reviewed on 2024-04-19 17:31_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:323 on 2024-04-19 17:31_

Code won't exist in the next version that I push.

---

_Renamed from "[UP031] When encountering `"%s" % var` attempt to dereference `var` and offer fixes more consistently." to "[UP031] When encountering `"%s" % var` offer unsafe fix" by @plredmond on 2024-04-19 17:45_

---

_@charliermarsh approved on 2024-04-20 01:48_

---

_Merged by @plredmond on 2024-04-22 15:40_

---

_Closed by @plredmond on 2024-04-22 15:40_

---

_Branch deleted on 2024-04-22 15:40_

---

_Comment by @ThiefMaster on 2024-05-09 21:32_

Am I the only one who does not like the fact that this even converts `%` formatting in cases where it introduces escaping of literal `{` in the format string/

I mean cases like this:

```diff
 def friendly_name(cls):
-    return '{%s}' % cls.name
+    return f'{{{cls.name}}}'

 def get_regex(cls, **kwargs):
-    return re.compile(r'\{%s}' % re.escape(cls.name))
+    return re.compile(rf'\{{{re.escape(cls.name)}}}')

```

IMHO this is clearly less readable than the previous version.

---
