```yaml
number: 3471
title: Add some new users to the README
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/users
created_at: 2023-03-13T00:03:14Z
updated_at: 2023-03-13T00:15:02Z
url: https://github.com/astral-sh/ruff/pull/3471
synced_at: 2026-01-12T15:55:12Z
```

# Add some new users to the README

---

_@charliermarsh_

_No description provided._

---

_Merged by @charliermarsh on 2023-03-13 00:08_

---

_Closed by @charliermarsh on 2023-03-13 00:08_

---

_Branch deleted on 2023-03-13 00:08_

---

_Comment by @github-actions[bot] on 2023-03-13 00:15_

ℹ️ ecosystem check **detected changes**. (+0, -345, 0 error(s))

<details><summary>zulip (+0, -2)</summary>
<p>

```diff
- zerver/apps.py:32:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- zerver/views/realm.py:185:5: B018 Found useless expression. Either assign it to a variable or remove it.
```

</p>
</details>
<details><summary>bokeh (+0, -308)</summary>
<p>

```diff
- tests/codebase/test_code_quality.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/codebase/test_eslint.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/codebase/test_importlib_metadata.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/codebase/test_isort.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/codebase/test_js_license_set.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/codebase/test_json.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/codebase/test_license.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/codebase/test_no_client_server_common.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/codebase/test_no_ipython_common.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/codebase/test_no_pandas_common.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/codebase/test_no_request_host.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/codebase/test_no_selenium_common.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/codebase/test_no_tornado_common.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/codebase/test_no_typing_extensions_common.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/codebase/test_python_execution_with_OO.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/codebase/test_ruff.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/codebase/test_windows_reserved_filenames.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/embed/test_json_item.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/models/test_datarange1d.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/models/test_plot.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/models/test_sources.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/test_regressions.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/tools/test_box_edit_tool.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/tools/test_box_zoom_tool.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/tools/test_custom_action.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/tools/test_freehand_draw_tool.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/tools/test_pan_tool.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/tools/test_point_draw_tool.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/tools/test_poly_draw_tool.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/tools/test_poly_edit_tool.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/tools/test_range_tool.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/tools/test_reset_tool.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/tools/test_tap_tool.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/tools/test_wheel_pan_tool.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/tools/test_wheel_zoom_tool.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/tools/test_zoom_in_tool.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/tools/test_zoom_out_tool.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/tables/test_cell_editors.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/tables/test_copy_paste.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/tables/test_data_table.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/tables/test_sortable.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/tables/test_source_updates.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_autocomplete_input.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_button.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_checkbox_button_group.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_checkbox_group.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_color_picker.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_copy_paste.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_datepicker.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_daterange_slider.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_dateslider.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_datetime_range_slider.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_div.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_dropdown.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_multi_choice.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_numeric_input.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_paragraph.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_password_input.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_pretext.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_radio_button_group.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_radio_group.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_range_slider.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_select.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_slider.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_spinner.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_text_input.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_textarea_input.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/integration/widgets/test_toggle.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/test_bokehjs.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/test_defaults.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/test_examples.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/_testing/util/test_env.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/application/handlers/test___init___handlers.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/application/handlers/test_code.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/application/handlers/test_code_runner.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/application/handlers/test_directory.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/application/handlers/test_document_lifecycle.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/application/handlers/test_function.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/application/handlers/test_handler.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/application/handlers/test_notebook__handlers.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/application/handlers/test_script.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/application/handlers/test_server_lifecycle.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/application/handlers/test_server_request_handler.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/application/test___init___application.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/application/test_application.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/client/test___init___client.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/client/test_connection.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/client/test_session__client.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/client/test_states.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/client/test_util__client.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/client/test_websocket.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/colors/test___init___colors.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/colors/test_color__colors.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/colors/test_groups.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/colors/test_named.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/colors/test_util__colors.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/command/subcommands/test___init___subcommands.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/command/subcommands/test_info.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/command/subcommands/test_json__subcommands.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/command/subcommands/test_sampledata__subcommands.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/command/subcommands/test_secret.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/command/subcommands/test_serve.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/command/test___init___command.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/command/test_bootstrap.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/command/test_subcommand.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/command/test_util__command.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/_util_property.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test___init___property.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_alias.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_aliases.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_any.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_auto.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_bases.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_color__property.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_container.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_dataspec.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_datetime.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_descriptor_factory.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_descriptors.py:110:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_descriptors.py:111:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_descriptors.py:112:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_descriptors.py:113:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_descriptors.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_either.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_enum.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_include.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_instance.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_json__property.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_nullable.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_numeric.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_override.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_pd.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_primitive.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_required.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_singletons.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_string_properties.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_validation__property.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_vectorization.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_visual.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/property/test_wrappers__property.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/test_enums.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/test_has_props.py:121:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/test_has_props.py:126:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/test_has_props.py:131:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/test_has_props.py:136:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/test_has_props.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/test_json_encoder.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/test_properties.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/test_properties.py:367:13: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/test_properties.py:391:13: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/test_query.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/test_serialization.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/test_templates.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/core/test_validation.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/document/_util_document.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/document/test_callbacks__document.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/document/test_document.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/document/test_events__document.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/document/test_locking.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/document/test_models.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/document/test_modules.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/embed/test___init___embed.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/embed/test_bundle.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/embed/test_elements.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/embed/test_notebook__embed.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/embed/test_server__embed.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/embed/test_standalone.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/embed/test_util__embed.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/embed/test_wrappers__embed.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/io/test___init___io.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/io/test_doc.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/io/test_export.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/io/test_notebook__io.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/io/test_output.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/io/test_saving.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/io/test_showing.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/io/test_state.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/io/test_util__io.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/io/test_webdriver.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/model/test___init___model.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/model/test_data_model.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/model/test_docs.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/model/test_model.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/model/test_util_model.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/_util_models.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_annotations.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_axes.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_callbacks__models.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_defaults.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_dom.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_filters.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_formatters.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_glyph_renderer.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_glyphs.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_glyphs.py:88:1: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_graph_renderer.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_graphs.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_grids.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_layouts__models.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_mappers.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_plots.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_plots.py:491:13: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_ranges.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_sources.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_tools.py:14:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/test_transforms.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/util/test_structure.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/widgets/test_slider.py:139:13: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/widgets/test_slider.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/widgets/test_slider.py:141:13: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/widgets/test_slider.py:47:13: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/widgets/test_slider.py:49:13: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/widgets/test_slider.py:59:13: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/widgets/test_slider.py:61:13: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/widgets/test_slider.py:93:13: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/unit/bokeh/models/widgets/test_slider.py:95:13: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/unit/bokeh/plotting/test___init___plotting.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/plotting/test__decorators.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/plotting/test__graph.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/plotting/test__legends.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/plotting/test__plot.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/plotting/test__renderer.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/plotting/test__stack.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/plotting/test__tools.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/plotting/test_contour.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/plotting/test_figure.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/plotting/test_graph.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/protocol/messages/test_patch_doc.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/protocol/messages/test_pull_doc.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/protocol/messages/test_push_doc.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/protocol/test_message.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/protocol/test_receiver.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test___init___sampledata.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_airport_routes.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_airports.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_anscombe.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_antibiotics.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_autompg.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_autompg2.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_browsers.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_commits.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_daylight.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_degrees.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_gapminder.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_glucose.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_haar_cascade.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_iris.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_les_mis.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_movies_data.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_mtb.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_olympics2014.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_perceptions.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_periodic_table.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_population.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_sample_geojson.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_sample_superstore.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_sea_surface_temperature.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_sprint.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_stocks.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_unemployment.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_unemployment1948.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_us_cities.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_us_counties.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_us_holidays.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_us_marriages_divorces.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_us_states.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/sampledata/test_world_cities.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/server/test_auth_provider.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/server/test_callbacks__server.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/server/test_contexts.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/server/test_server__server.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/server/test_session__server.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/server/test_tornado__server.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/server/test_util.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/server/views/test_metadata_handler.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/server/views/test_root_handler.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/server/views/test_session_handler.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/server/views/test_ws.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/test___init__.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/test___main__.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/test_client_server.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/test_driving.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/test_events.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/test_ext.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/test_layouts.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/test_objects.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/test_palettes.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/test_server.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/test_settings.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/test_themes.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/test_tile_providers.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/test_transform.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/util/test_browser.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/util/test_callback_manager.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/util/test_compiler.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/util/test_dataclasses.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/util/test_dependencies.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/util/test_deprecation.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/util/test_hex.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/util/test_options.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/util/test_package.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/util/test_sampledata__util.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/util/test_strings.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/util/test_token.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/util/test_tornado__util.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/util/test_util__serialization.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/util/test_version.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/unit/bokeh/util/test_warnings.py:13:17: B018 Found useless expression. Either assign it to a variable or remove it.
```

</p>
</details>
<details><summary>airflow (+0, -35)</summary>
<p>

```diff
- airflow/providers/cncf/kubernetes/hooks/kubernetes.py:245:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- airflow/providers/ftp/hooks/ftp.py:274:13: B018 Found useless attribute access. Either assign it to a variable or remove it.
- airflow/providers/github/hooks/github.py:85:13: B018 Found useless attribute access. Either assign it to a variable or remove it.
- airflow/providers/google/cloud/example_dags/example_looker.py:63:5: B018 Found useless expression. Either assign it to a variable or remove it.
- airflow/providers/google/cloud/example_dags/example_vertex_ai.py:423:5: B018 Found useless expression. Either assign it to a variable or remove it.
- airflow/providers/google/cloud/example_dags/example_vertex_ai.py:428:5: B018 Found useless expression. Either assign it to a variable or remove it.
- airflow/providers/google/cloud/example_dags/example_vertex_ai.py:583:5: B018 Found useless expression. Either assign it to a variable or remove it.
- airflow/providers/google/cloud/example_dags/example_vertex_ai.py:638:5: B018 Found useless expression. Either assign it to a variable or remove it.
- airflow/providers/google/cloud/example_dags/example_vertex_ai.py:710:5: B018 Found useless expression. Either assign it to a variable or remove it.
- airflow/providers/google/cloud/example_dags/example_vertex_ai.py:754:5: B018 Found useless expression. Either assign it to a variable or remove it.
- airflow/www/gunicorn_config.py:40:5: B018 Found useless attribute access. Either assign it to a variable or remove it.
- docs/exts/docs_build/spelling_checks.py:169:5: B018 Found useless attribute access. Either assign it to a variable or remove it.
- docs/exts/docs_build/spelling_checks.py:171:5: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/always/test_connection.py:687:13: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/models/test_dagbag.py:319:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/models/test_dagrun.py:1111:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/models/test_dagrun.py:1157:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/models/test_dagrun.py:1213:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/models/test_dagrun.py:1278:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/models/test_dagrun.py:1375:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/models/test_dagrun.py:1432:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/models/test_dagrun.py:1520:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/models/test_dagrun.py:1600:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/models/test_dagrun.py:1683:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/models/test_taskinstance.py:2061:13: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/providers/alibaba/cloud/log/test_oss_task_handler.py:68:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/providers/amazon/aws/log/test_cloudwatch_task_handler.py:63:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/providers/amazon/aws/operators/test_eks.py:466:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/providers/amazon/aws/secrets/test_secrets_manager.py:379:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/providers/amazon/aws/secrets/test_systems_manager.py:195:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/providers/common/sql/operators/test_sql.py:581:13: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/providers/microsoft/azure/hooks/test_azure_data_lake.py:60:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/providers/microsoft/azure/log/test_wasb_task_handler.py:81:17: B018 Found useless attribute access. Either assign it to a variable or remove it.
- tests/system/providers/docker/example_docker_swarm.py:48:9: B018 Found useless expression. Either assign it to a variable or remove it.
- tests/system/providers/elasticsearch/example_elasticsearch_query.py:78:9: B018 Found useless expression. Either assign it to a variable or remove it.
```

</p>
</details>

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---
