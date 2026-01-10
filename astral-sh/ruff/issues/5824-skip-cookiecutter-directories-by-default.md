```yaml
number: 5824
title: Skip cookiecutter directories by default
type: issue
state: open
author: konstin
labels:
  - needs-decision
assignees: []
created_at: 2023-07-17T07:58:29Z
updated_at: 2023-07-19T01:18:22Z
url: https://github.com/astral-sh/ruff/issues/5824
synced_at: 2026-01-10T11:09:48Z
```

# Skip cookiecutter directories by default

---

_Issue opened by @konstin on 2023-07-17 07:58_

[cookiecutter](https://github.com/cookiecutter/cookiecutter) is popular way to create projects or modules from templates. Some repositories include cookiecutter templates in subdirectories. The files in those template are often not valid python since they contain jinja2 template tags, e.g. `from transformers import {{cookiecutter.camelcase_modelname}}Tokenizer` or `{% if cookiecutter.has_slow_class == "True" and  cookiecutter.has_fast_class == "True" -%}`.

One examples is [transformers](https://github.com/huggingface/transformers). If we run `ruff .`¹ on the repo, we get a bunch of errors related to cookiecutter:

```
templates/adding_a_missing_tokenization_test/cookiecutter-template-{{cookiecutter.modelname}}/test_tokenization_{{cookiecutter.lowercase_modelname}}.py:20:2: E999 SyntaxError: Unexpected token '%'
templates/adding_a_missing_tokenization_test/cookiecutter-template-{{cookiecutter.modelname}}/test_tokenization_{{cookiecutter.lowercase_modelname}}.py:78:52: W291 [*] Trailing whitespace
templates/adding_a_missing_tokenization_test/cookiecutter-template-{{cookiecutter.modelname}}/test_tokenization_{{cookiecutter.lowercase_modelname}}.py:78:53: W292 [*] No newline at end of file
templates/adding_a_new_example_script/{{cookiecutter.directory_name}}/run_{{cookiecutter.example_shortcut}}.py:21:2: E999 SyntaxError: Unexpected token '%'
templates/adding_a_new_example_script/{{cookiecutter.directory_name}}/run_{{cookiecutter.example_shortcut}}.py:487:1: W293 [*] Blank line contains whitespace
templates/adding_a_new_example_script/{{cookiecutter.directory_name}}/run_{{cookiecutter.example_shortcut}}.py:885:34: W291 [*] Trailing whitespace
templates/adding_a_new_model/cookiecutter-template-{{cookiecutter.modelname}}/__init__.py:19:2: E999 SyntaxError: Unexpected token '%'
templates/adding_a_new_model/cookiecutter-template-{{cookiecutter.modelname}}/configuration_{{cookiecutter.lowercase_modelname}}.py:23:37: E999 SyntaxError: Unexpected token '_PRETRAINED_CONFIG_ARCHIVE_MAP'
templates/adding_a_new_model/cookiecutter-template-{{cookiecutter.modelname}}/configuration_{{cookiecutter.lowercase_modelname}}.py:140:1: W293 [*] Blank line contains whitespace
templates/adding_a_new_model/cookiecutter-template-{{cookiecutter.modelname}}/configuration_{{cookiecutter.lowercase_modelname}}.py:241:1: W293 [*] Blank line contains whitespace
templates/adding_a_new_model/cookiecutter-template-{{cookiecutter.modelname}}/configuration_{{cookiecutter.lowercase_modelname}}.py:241:5: W292 [*] No newline at end of file
templates/adding_a_new_model/cookiecutter-template-{{cookiecutter.modelname}}/modeling_flax_{{cookiecutter.lowercase_modelname}}.py:17:2: E999 SyntaxError: Unexpected token '%'
templates/adding_a_new_model/cookiecutter-template-{{cookiecutter.modelname}}/modeling_tf_{{cookiecutter.lowercase_modelname}}.py:17:2: E999 SyntaxError: Unexpected token '%'
templates/adding_a_new_model/cookiecutter-template-{{cookiecutter.modelname}}/modeling_{{cookiecutter.lowercase_modelname}}.py:17:2: E999 SyntaxError: Unexpected token '%'
templates/adding_a_new_model/cookiecutter-template-{{cookiecutter.modelname}}/test_modeling_flax_{{cookiecutter.lowercase_modelname}}.py:16:2: E999 SyntaxError: Unexpected token '%'
templates/adding_a_new_model/cookiecutter-template-{{cookiecutter.modelname}}/test_modeling_flax_{{cookiecutter.lowercase_modelname}}.py:544:78: W291 [*] Trailing whitespace
templates/adding_a_new_model/cookiecutter-template-{{cookiecutter.modelname}}/test_modeling_tf_{{cookiecutter.lowercase_modelname}}.py:16:2: E999 SyntaxError: Unexpected token '%'
templates/adding_a_new_model/cookiecutter-template-{{cookiecutter.modelname}}/test_modeling_{{cookiecutter.lowercase_modelname}}.py:18:2: E999 SyntaxError: Unexpected token '%'
templates/adding_a_new_model/cookiecutter-template-{{cookiecutter.modelname}}/to_replace_{{cookiecutter.lowercase_modelname}}.py:30:2: E999 SyntaxError: Unexpected token '%'
templates/adding_a_new_model/cookiecutter-template-{{cookiecutter.modelname}}/tokenization_fast_{{cookiecutter.lowercase_modelname}}.py:17:2: E999 SyntaxError: Unexpected token '%'
templates/adding_a_new_model/cookiecutter-template-{{cookiecutter.modelname}}/tokenization_{{cookiecutter.lowercase_modelname}}.py:17:2: E999 SyntaxError: Unexpected token '%'
```

It would be better to instead skip all paths that contain `{{cookiecutter`. Paths that contain `{{` are not valid python modules and i don't know of any other usage of `{{` in paths.

¹ transformers itself uses `ruff examples tests src utils setup.py conftest.py` to ignore e.g. templates

---

_Comment by @sbrugman on 2023-07-17 09:53_

For reference [Copier](https://copier.readthedocs.io/en/stable/) is another popular alternative to cookiecutter. It uses `.jinja` extensions so this will not be an issue. Cookiecutter does not seem to use this unfortunately.  

---

_Label `needs-decision` added by @charliermarsh on 2023-07-19 01:18_

---
