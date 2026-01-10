```yaml
number: 10802
title: "[`refurb`] Implement `write-whole-file` (`FURB103`)"
type: pull_request
state: merged
author: augustelalande
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: write-whole-file
created_at: 2024-04-06T16:10:39Z
updated_at: 2024-04-11T18:35:44Z
url: https://github.com/astral-sh/ruff/pull/10802
synced_at: 2026-01-10T22:37:01Z
```

# [`refurb`] Implement `write-whole-file` (`FURB103`)

---

_Pull request opened by @augustelalande on 2024-04-06 16:10_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement `write-whole-file` (`FURB103`), part of #1348. This is largely a copy and paste of `read-whole-file` #7682. 

## Test Plan

Text fixture added.


---

_@augustelalande reviewed on 2024-04-06 16:22_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB103_FURB103.py.snap`:88 on 2024-04-06 16:22_

I understand that `foobar` is being placed after `newline="\r\n"` because the generator uses source order, and `foobar` comes later, but I'm not sure how to remedy it.

---

_Comment by @github-actions[bot] on 2024-04-06 16:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+137 -0 violations, +0 -0 fixes in 5 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+35 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4f169bd2f3e27b8530da4b82d0d3b25b796eff39/airflow/cli/commands/kubernetes_command.py#L70'>airflow/cli/commands/kubernetes_command.py:70:14:</a> FURB103 `open` and `write` should be replaced by `Path(...).write_text(yaml.dump(sanitized_pod))`
+ <a href='https://github.com/apache/airflow/blob/4f169bd2f3e27b8530da4b82d0d3b25b796eff39/airflow/cli/commands/pool_command.py#L147'>airflow/cli/commands/pool_command.py:147:10:</a> FURB103 `open` and `write` should be replaced by `Path(filepath)....`
+ <a href='https://github.com/apache/airflow/blob/4f169bd2f3e27b8530da4b82d0d3b25b796eff39/airflow/example_dags/example_kubernetes_executor.py#L91'>airflow/example_dags/example_kubernetes_executor.py:91:18:</a> FURB103 `open` and `write` should be replaced by `Path("/foo/volume_mount_test.txt").write_text("Hello")`
+ <a href='https://github.com/apache/airflow/blob/4f169bd2f3e27b8530da4b82d0d3b25b796eff39/airflow/providers/databricks/operators/databricks_sql.py#L152'>airflow/providers/databricks/operators/databricks_sql.py:152:18:</a> FURB103 `open` and `write` should be replaced by `Path(self._output_path)....`
+ <a href='https://github.com/apache/airflow/blob/4f169bd2f3e27b8530da4b82d0d3b25b796eff39/airflow/providers/fab/auth_manager/security_manager/override.py#L776'>airflow/providers/fab/auth_manager/security_manager/override.py:776:18:</a> FURB103 `open` and `write` should be replaced by `Path(password_path).write_text(password)`
+ <a href='https://github.com/apache/airflow/blob/4f169bd2f3e27b8530da4b82d0d3b25b796eff39/airflow/providers/google/cloud/hooks/cloud_sql.py#L529'>airflow/providers/google/cloud/hooks/cloud_sql.py:529:14:</a> FURB103 `open` and `write` should be replaced by `Path(proxy_path_tmp).write_bytes(response.content)`
+ <a href='https://github.com/apache/airflow/blob/4f169bd2f3e27b8530da4b82d0d3b25b796eff39/airflow/providers/imap/hooks/imap.py#L291'>airflow/providers/imap/hooks/imap.py:291:14:</a> FURB103 `open` and `write` should be replaced by `Path(file_path).write_bytes(payload)`
+ <a href='https://github.com/apache/airflow/blob/4f169bd2f3e27b8530da4b82d0d3b25b796eff39/airflow/providers/microsoft/azure/hooks/wasb.py#L391'>airflow/providers/microsoft/azure/hooks/wasb.py:391:14:</a> FURB103 `open` and `write` should be replaced by `Path(file_path).write_bytes(stream.readall())`
+ <a href='https://github.com/apache/airflow/blob/4f169bd2f3e27b8530da4b82d0d3b25b796eff39/dev/breeze/src/airflow_breeze/utils/docs_publisher.py#L104'>dev/breeze/src/airflow_breeze/utils/docs_publisher.py:104:18:</a> FURB103 `open` and `write` should be replaced by `Path(os.path.join(output_dir, "..", "stable.txt")).write_text(self._current_version)`
+ <a href='https://github.com/apache/airflow/blob/4f169bd2f3e27b8530da4b82d0d3b25b796eff39/docs/exts/docs_build/dev_index_generator.py#L68'>docs/exts/docs_build/dev_index_generator.py:68:10:</a> FURB103 `open` and `write` should be replaced by `Path(out_file).write_text(content)`
+ <a href='https://github.com/apache/airflow/blob/4f169bd2f3e27b8530da4b82d0d3b25b796eff39/docs/exts/redirects.py#L80'>docs/exts/redirects.py:80:18:</a> FURB103 `open` and `write` should be replaced by `Path(redirected_filename).write_text(TEMPLATE.format(to_path))`
+ <a href='https://github.com/apache/airflow/blob/4f169bd2f3e27b8530da4b82d0d3b25b796eff39/docs/exts/sphinx_script_update.py#L87'>docs/exts/sphinx_script_update.py:87:10:</a> FURB103 `open` and `write` should be replaced by `Path(cache_filepath).write_bytes(res.content)`
... 23 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+41 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/basic_plot.py#L42'>examples/models/basic_plot.py:42:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename)....`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/buttons.py#L75'>examples/models/buttons.py:75:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="Button widgets", doc))`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/calendars.py#L104'>examples/models/calendars.py:104:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="Calendar 2014", doc))`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/colors.py#L69'>examples/models/colors.py:69:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename)....`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/custom.py#L117'>examples/models/custom.py:117:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename)....`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/customjs.py#L38'>examples/models/customjs.py:38:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename)....`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/dateaxis.py#L36'>examples/models/dateaxis.py:36:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename)....`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/daylight.py#L107'>examples/models/daylight.py:107:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="Daylight Plot", doc))`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/gauges.py#L110'>examples/models/gauges.py:110:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="Gauges", doc))`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/glyphs.py#L115'>examples/models/glyphs.py:115:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="Glyphs", doc))`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/maps.py#L59'>examples/models/maps.py:59:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename)....`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/maps_cities.py#L46'>examples/models/maps_cities.py:46:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename)....`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/sliders.py#L74'>examples/models/sliders.py:74:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="sliders", doc))`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/tile_source.py#L38'>examples/models/tile_source.py:38:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename)....`
... 27 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+19 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/c48315b753d59c5d8e418e89edb49e1b816d0cfb/package.py#L734'>package.py:734:14:</a> FURB103 `open` and `write` should be replaced by `Path(p12).write_bytes(certificate_data)`
+ <a href='https://github.com/rotki/rotki/blob/c48315b753d59c5d8e418e89edb49e1b816d0cfb/rotkehlchen/chain/ethereum/airdrops.py#L248'>rotkehlchen/chain/ethereum/airdrops.py:248:18:</a> FURB103 `open` and `write` should be replaced by `Path(filename).write_text(response.text, encoding='utf8')`
+ <a href='https://github.com/rotki/rotki/blob/c48315b753d59c5d8e418e89edb49e1b816d0cfb/rotkehlchen/chain/ethereum/airdrops.py#L284'>rotkehlchen/chain/ethereum/airdrops.py:284:14:</a> FURB103 `open` and `write` should be replaced by `Path(filename)....`
+ <a href='https://github.com/rotki/rotki/blob/c48315b753d59c5d8e418e89edb49e1b816d0cfb/rotkehlchen/chain/ethereum/utils.py#L224'>rotkehlchen/chain/ethereum/utils.py:224:10:</a> FURB103 `open` and `write` should be replaced by `Path(avatars_dir / f'{ens_name}.png').write_bytes(avatar)`
+ <a href='https://github.com/rotki/rotki/blob/c48315b753d59c5d8e418e89edb49e1b816d0cfb/rotkehlchen/db/dbhandler.py#L300'>rotkehlchen/db/dbhandler.py:300:14:</a> FURB103 `open` and `write` should be replaced by `Path(self.user_data_dir / DBINFO_FILENAME).write_text(rlk_jsondumps(dbinfo), encoding='utf8')`
+ <a href='https://github.com/rotki/rotki/blob/c48315b753d59c5d8e418e89edb49e1b816d0cfb/rotkehlchen/db/dbhandler.py#L593'>rotkehlchen/db/dbhandler.py:593:18:</a> FURB103 `open` and `write` should be replaced by `Path(tempdbpath).write_bytes(unencrypted_db_data)`
+ <a href='https://github.com/rotki/rotki/blob/c48315b753d59c5d8e418e89edb49e1b816d0cfb/rotkehlchen/icons.py#L190'>rotkehlchen/icons.py:190:14:</a> FURB103 `open` and `write` should be replaced by `Path(self.iconfile_path(asset)).write_bytes(response.content)`
+ <a href='https://github.com/rotki/rotki/blob/c48315b753d59c5d8e418e89edb49e1b816d0cfb/rotkehlchen/tests/api/test_caching.py#L42'>rotkehlchen/tests/api/test_caching.py:42:10:</a> FURB103 `open` and `write` should be replaced by `Path(f'{icons_dir}/ETH_small.png').write_bytes(b'')`
+ <a href='https://github.com/rotki/rotki/blob/c48315b753d59c5d8e418e89edb49e1b816d0cfb/rotkehlchen/tests/api/test_caching.py#L45'>rotkehlchen/tests/api/test_caching.py:45:10:</a> FURB103 `open` and `write` should be replaced by `Path(f'{icons_dir}/BTC_small.png').write_bytes(b'')`
+ <a href='https://github.com/rotki/rotki/blob/c48315b753d59c5d8e418e89edb49e1b816d0cfb/rotkehlchen/tests/api/test_caching.py#L48'>rotkehlchen/tests/api/test_caching.py:48:10:</a> FURB103 `open` and `write` should be replaced by `Path(f'{icons_dir}/AVAX_small.png').write_bytes(b'')`
... 9 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+35 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/36aa0177bd0312c32503ca3e368f25664e724ef7/analytics/management/commands/check_analytics_state.py#L37'>analytics/management/commands/check_analytics_state.py:37:14:</a> FURB103 `open` and `write` should be replaced by `Path(state_file_tmp)....`
+ <a href='https://github.com/zulip/zulip/blob/36aa0177bd0312c32503ca3e368f25664e724ef7/corporate/tests/test_stripe.py#L169'>corporate/tests/test_stripe.py:169:18:</a> FURB103 `open` and `write` should be replaced by `Path(fixture_path)....`
+ <a href='https://github.com/zulip/zulip/blob/36aa0177bd0312c32503ca3e368f25664e724ef7/corporate/tests/test_stripe.py#L298'>corporate/tests/test_stripe.py:298:14:</a> FURB103 `open` and `write` should be replaced by `Path(fixture_file).write_text(file_content)`
+ <a href='https://github.com/zulip/zulip/blob/36aa0177bd0312c32503ca3e368f25664e724ef7/scripts/lib/setup_venv.py#L272'>scripts/lib/setup_venv.py:272:10:</a> FURB103 `open` and `write` should be replaced by `Path(script_path).write_text("".join(lines))`
+ <a href='https://github.com/zulip/zulip/blob/36aa0177bd0312c32503ca3e368f25664e724ef7/scripts/lib/zulip_tools.py#L535'>scripts/lib/zulip_tools.py:535:10:</a> FURB103 `open` and `write` should be replaced by `Path(hash_path).write_text(new_hash)`
+ <a href='https://github.com/zulip/zulip/blob/36aa0177bd0312c32503ca3e368f25664e724ef7/tools/lib/pretty_print.py#L160'>tools/lib/pretty_print.py:160:18:</a> FURB103 `open` and `write` should be replaced by `Path(fn).write_text(phtml)`
+ <a href='https://github.com/zulip/zulip/blob/36aa0177bd0312c32503ca3e368f25664e724ef7/tools/lib/provision.py#L388'>tools/lib/provision.py:388:14:</a> FURB103 `open` and `write` should be replaced by `Path(apt_hash_file_path).write_text(new_apt_dependencies_hash)`
+ <a href='https://github.com/zulip/zulip/blob/36aa0177bd0312c32503ca3e368f25664e724ef7/tools/lib/provision_inner.py#L366'>tools/lib/provision_inner.py:366:10:</a> FURB103 `open` and `write` should be replaced by `Path(version_file)....`
+ <a href='https://github.com/zulip/zulip/blob/36aa0177bd0312c32503ca3e368f25664e724ef7/tools/setup/generate_integration_bots_avatars.py#L54'>tools/setup/generate_integration_bots_avatars.py:54:10:</a> FURB103 `open` and `write` should be replaced by `Path(bot_avatar_path).write_bytes(avatar)`
+ <a href='https://github.com/zulip/zulip/blob/36aa0177bd0312c32503ca3e368f25664e724ef7/zerver/data_import/gitter.py#L396'>zerver/data_import/gitter.py:396:10:</a> FURB103 `open` and `write` should be replaced by `Path(output_file)....`
+ <a href='https://github.com/zulip/zulip/blob/36aa0177bd0312c32503ca3e368f25664e724ef7/zerver/data_import/import_util.py#L749'>zerver/data_import/import_util.py:749:10:</a> FURB103 `open` and `write` should be replaced by `Path(output_file)....`
+ <a href='https://github.com/zulip/zulip/blob/36aa0177bd0312c32503ca3e368f25664e724ef7/zerver/data_import/rocketchat.py#L301'>zerver/data_import/rocketchat.py:301:14:</a> FURB103 `open` and `write` should be replaced by `Path(target_path).write_bytes(emoji_data)`
... 23 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/bff9d0bf9c15170dd6781b55eada809cee573857/bin/maintenance/generate_moment_locales.py#L100'>bin/maintenance/generate_moment_locales.py:100:10:</a> FURB103 `open` and `write` should be replaced by `Path(output).write_text(header + locales)`
+ <a href='https://github.com/indico/indico/blob/bff9d0bf9c15170dd6781b55eada809cee573857/indico/cli/i18n.py#L296'>indico/cli/i18n.py:296:18:</a> FURB103 `open` and `write` should be replaced by `Path(json_file).write_text(output)`
+ <a href='https://github.com/indico/indico/blob/bff9d0bf9c15170dd6781b55eada809cee573857/indico/cli/i18n.py#L321'>indico/cli/i18n.py:321:10:</a> FURB103 `open` and `write` should be replaced by `Path(_get_messages_react_pot(directory)).write_bytes(output)`
+ <a href='https://github.com/indico/indico/blob/bff9d0bf9c15170dd6781b55eada809cee573857/indico/cli/setup.py#L674'>indico/cli/setup.py:674:14:</a> FURB103 `open` and `write` should be replaced by `Path(self.config_path).write_text(config + '\n')`
+ <a href='https://github.com/indico/indico/blob/bff9d0bf9c15170dd6781b55eada809cee573857/indico/web/assets/blueprint.py#L111'>indico/web/assets/blueprint.py:111:14:</a> FURB103 `open` and `write` should be replaced by `Path(cache_file)....`
+ <a href='https://github.com/indico/indico/blob/bff9d0bf9c15170dd6781b55eada809cee573857/indico/web/assets/blueprint.py#L63'>indico/web/assets/blueprint.py:63:14:</a> FURB103 `open` and `write` should be replaced by `Path(cache_file).write_text(data)`
+ <a href='https://github.com/indico/indico/blob/bff9d0bf9c15170dd6781b55eada809cee573857/setup.py#L42'>setup.py:42:18:</a> FURB103 `open` and `write` should be replaced by `Path(json_file).write_bytes(output)`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB103 | 137 | 137 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@T-256 reviewed on 2024-04-09 12:51_

---

_Review comment by @T-256 on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB103_FURB103.py.snap`:28 on 2024-04-09 12:51_

I think `encoding="utf8"` keyword argument should go after `foobar` positional argument

---

_@augustelalande reviewed on 2024-04-09 20:18_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB103_FURB103.py.snap`:28 on 2024-04-09 20:18_

Ya it's the same thing I commented above, but I'm not sure how to correct it.

---

_@dhruvmanila requested changes on 2024-04-10 07:44_

Thanks you for the PR. 

I think a lot of the definitions and functions are repeated as you've mentioned. It would be useful to move the common structs and functions to `crates/ruff_linter/src/rules/refurb/helpers.rs` module to avoid having duplicate logic. You can find similar examples in other rule modules as well. Can you please make this change?

---

_Label `rule` added by @dhruvmanila on 2024-04-10 07:44_

---

_Comment by @augustelalande on 2024-04-10 21:17_

@dhruvmanila Done. Any suggestion on the comments above?

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-04-11 06:15_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-04-11 07:17_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/refurb/helpers.rs`:70 on 2024-04-11 07:19_

We should just limit this to the `refurb` directory for now. If there's any need in the future to use this in another rule module, we can make it at the crate level.

```suggestion
pub(super) enum OpenMode {
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/refurb/helpers.rs`:69 on 2024-04-11 07:22_

nit: I think we can make this enum use copy semantics:

```suggestion
// Helpers for read-whole-file and write-whole-file
#[derive(Debug, Copy, Clone)]
```

Clippy will then probably start complaining in certain places to use copy semantics instead of reference. Refer https://rust-lang.github.io/rust-clippy/master/index.html#/trivially_copy_pass_by_ref

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/refurb/helpers.rs`:106 on 2024-04-11 07:23_

We should make this limited as well. Use `super` instead of `crate in the `pub` declaration. I would suggest to make this change for all public items in this module.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs`:112 on 2024-04-11 07:49_

I think we can avoid the clone here and the one in `match_write_call` (`.to_vec()` call) by using a `Vec<&'a [Expr]>` instead of `Vec<Vec<Expr>>`. Here's what I'm suggesting:

```diff
diff --git a/crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs b/crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs
index 9830f3364..76ed21566 100644
--- a/crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs
+++ b/crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs
@@ -62,13 +62,14 @@ pub(crate) fn write_whole_file(checker: &mut Checker, with: &ast::StmtWith) {
     }
 
     // Then we need to match each `open` operation with exactly one `write` call.
-    let mut matcher = WriteMatcher::new(candidates);
-    visitor::walk_body(&mut matcher, &with.body);
-    let contents = matcher.contents();
+    let (matches, contents) = {
+        let mut matcher = WriteMatcher::new(candidates);
+        visitor::walk_body(&mut matcher, &with.body);
+        matcher.finish()
+    };
 
     // All the matched operations should be reported.
-    let diagnostics: Vec<Diagnostic> = matcher
-        .into_matches()
+    let diagnostics: Vec<Diagnostic> = matches
         .iter()
         .zip(contents)
         .map(|(open, content)| {
@@ -89,7 +90,7 @@ pub(crate) fn write_whole_file(checker: &mut Checker, with: &ast::StmtWith) {
 struct WriteMatcher<'a> {
     candidates: Vec<helpers::FileOpen<'a>>,
     matches: Vec<helpers::FileOpen<'a>>,
-    contents: Vec<Vec<Expr>>,
+    contents: Vec<&'a [Expr]>,
     loop_counter: u32,
 }
 
@@ -103,12 +104,8 @@ impl<'a> WriteMatcher<'a> {
         }
     }
 
-    fn into_matches(self) -> Vec<helpers::FileOpen<'a>> {
-        self.matches
-    }
-
-    fn contents(&self) -> Vec<Vec<Expr>> {
-        self.contents.clone()
+    fn finish(self) -> (Vec<helpers::FileOpen<'a>>, Vec<&'a [Expr]>) {
+        (self.matches, self.contents)
     }
 }
 
@@ -144,7 +141,7 @@ impl<'a> Visitor<'a> for WriteMatcher<'a> {
 }
 
 /// Match `x.write(foo)` expression and return expression `x` and `foo` on success.
-fn match_write_call(expr: &Expr) -> Option<(&Expr, Vec<Expr>)> {
+fn match_write_call(expr: &Expr) -> Option<(&Expr, &[Expr])> {
     let call = expr.as_call_expr()?;
     let attr = call.func.as_attribute_expr()?;
     let method_name = &attr.attr;
@@ -157,5 +154,5 @@ fn match_write_call(expr: &Expr) -> Option<(&Expr, Vec<Expr>)> {
         return None;
     }
 
-    Some((attr.value.as_ref(), call.arguments.args.as_ref().to_vec()))
+    Some((&*attr.value, &*call.arguments.args))
 }
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB103_FURB103.py.snap`:88 on 2024-04-11 08:12_

Yeah, that's unfortunate. I think I'd suggest to have two different variants of `make_suggestion`. We can keep the one in `read_whole_file` as is.

Here, one thing to note is that the `write` function only takes in one positional argument which we can assume. So, the `make_suggestion` in `write_whole_file` would take in the argument expression as an additional parameter.

We can use [`relocate_expr`](https://github.com/astral-sh/ruff/blob/1b31d4e9f16d56ac71a32b8f610011e92b61a665/crates/ruff_python_ast/src/relocate.rs) to relocate it to the default range before calling the `make_suggestion` function.

```rs
pub(super) fn make_suggestion(
    open: &FileOpen<'_>,
    arg: &Expr,
    generator: Generator,
) -> SourceCodeSnippet {
    let name = ast::ExprName {
        id: open.mode.pathlib_method(),
        ctx: ast::ExprContext::Load,
        range: TextRange::default(),
    };
    let call = ast::ExprCall {
        func: Box::new(name.into()),
        arguments: ast::Arguments {
            args: Box::new([arg.clone()]),
            keywords: open.keywords.iter().copied().cloned().collect(),
            range: TextRange::default(),
        },
        range: TextRange::default(),
    };
    SourceCodeSnippet::from_str(&generator.expr(&call.into()))
}
```

---

_@dhruvmanila requested changes on 2024-04-11 08:14_

Thank you for quickly following up with the requested change.

I've a few suggestion to avoid cloning and have provided a suggestion for the incorrect argument order that you've highlighted.

I haven't looked at the test cases yet which I'll do so now.

---

_Comment by @dhruvmanila on 2024-04-11 08:29_

@augustelalande Sorry, I was playing around with the suggestions I made which fixes the "suggestion" problem, so I've pushed that. I'll list down what all changes I made, feel free to revert any of them if you think otherwise:

1. Use copy semantics on `OpenMode`
2. Make everything in `helpers` scoped to `super`
3. Implement `make_suggestion` twice where the `write_*` one would take in a single argument, relocate it to the default text range and send it to the generator. This fixes the bug you commented about
4. Avoid having multiple clones for the `WriteMatcher` struct

---

_@dhruvmanila approved on 2024-04-11 08:43_

Thank you for working on this!

---

_Label `preview` added by @dhruvmanila on 2024-04-11 08:43_

---

_Comment by @dhruvmanila on 2024-04-11 08:47_

(Looking at the ecosystem checks, making sure they're correct.)

---

_Merged by @dhruvmanila on 2024-04-11 08:51_

---

_Closed by @dhruvmanila on 2024-04-11 08:51_

---

_Comment by @augustelalande on 2024-04-11 18:35_

Thanks for cleaning things up. `relocate_expr` is exactly what I was looking for.

---

_Branch deleted on 2024-04-11 18:35_

---
