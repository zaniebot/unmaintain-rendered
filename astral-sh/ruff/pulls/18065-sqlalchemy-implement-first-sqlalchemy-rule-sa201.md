```yaml
number: 18065
title: "[`sqlalchemy`] Implement first SQLAlchemy rule (`SA201`)"
type: pull_request
state: open
author: kreathon
labels:
  - rule
  - needs-decision
assignees: []
base: main
head: add-missing-mapped-type-annotation-rule
created_at: 2025-05-13T09:01:18Z
updated_at: 2025-05-15T16:34:28Z
url: https://github.com/astral-sh/ruff/pull/18065
synced_at: 2026-01-12T15:56:11Z
```

# [`sqlalchemy`] Implement first SQLAlchemy rule (`SA201`)

---

_@kreathon_

## Summary

Due to the interest in https://github.com/astral-sh/ruff/issues/16864 in the rules I proposed in https://github.com/miketheman/flake8-sqlalchemy/issues/54 (but were never implemented), I created this draft to add the first `SQLAlchemy` rule.

See changes in `crates/ruff_linter/src/rules/sqlalchemy/rules/missing_mapped_type_annotation.rs` for the motivation of this rule.

## Implementation and Limitations

Currently, it supports most commonly used column mappings (`mapped_column`, `relationship`, ...), but still lacking support for e.g. [`composite`](https://github.com/miketheman/flake8-sqlalchemy/issues/54).

Auto fixes might be possible in some cases, but can be tricky because it would require an understanding of  [`type_annotation_map`](https://docs.sqlalchemy.org/en/20/orm/declarative_tables.html#customizing-the-type-map).


## Test Plan

`SA001.py` snapshot test was added.






---

_Label `needs-decision` added by @MichaReiser on 2025-05-13 09:16_

---

_Label `rule` added by @MichaReiser on 2025-05-13 09:16_

---

_Comment by @kreathon on 2025-05-13 10:00_

Here is a little bit more background:

### Docs

> The [mapped_column()](https://docs.sqlalchemy.org/en/20/orm/mapping_api.html#sqlalchemy.orm.mapped_column) construct in modern Python is normally augmented by the use of [PEP 484](https://peps.python.org/pep-0484/) Python type annotations, where it is capable of deriving its column-configuration information from type annotations associated with the attribute as declared in the Declarative mapped class. These type annotations, if used, must be present within a special SQLAlchemy type called [Mapped](https://docs.sqlalchemy.org/en/20/orm/internals.html#sqlalchemy.orm.Mapped), which is a generic type that indicates a specific Python type within it.
>
>  -- <cite>https://docs.sqlalchemy.org/en/20/orm/declarative_tables.html#orm-annotated-declarative-automated-mapping-with-type-annotations</cite>

Both forms are used in the examples ( tagged as `Annotated Example` and `Non-Annotated Example`).

The `Non-Annotated` form is also a natural migration step to SQLAlchemy 2.0:

> Users of 1.x SQLAlchemy will note the use of the [mapped_column()](https://docs.sqlalchemy.org/en/20/orm/mapping_api.html#sqlalchemy.orm.mapped_column) construct, which is new as of the SQLAlchemy 2.0 series. This ORM-specific construct is intended first and foremost to be a drop-in replacement for the use of [Column](https://docs.sqlalchemy.org/en/20/core/metadata.html#sqlalchemy.schema.Column) within Declarative mappings only, adding new ORM-specific convenience features such as the ability to establish [mapped_column.deferred](https://docs.sqlalchemy.org/en/20/orm/mapping_api.html#sqlalchemy.orm.mapped_column.params.deferred) within the construct, and most importantly to indicate to typing tools such as [Mypy](https://mypy.readthedocs.io/en/stable/) and [Pylance](https://github.com/microsoft/pylance-release) an accurate representation of how the attribute will behave at runtime at both the class level as well as the instance level. As will be seen in the following sections, it’s also at the forefront of a new annotation-driven configuration style introduced in SQLAlchemy 2.0.

>  -- <cite>https://docs.sqlalchemy.org/en/20/orm/declarative_tables.html#orm-annotated-declarative-automated-mapping-with-type-annotations</cite>


### Personal Background

In a codebase I am working with, in some places the annotations were missing (for whatever reason). We were not aware that these columns were treated as `Any` causing some bugs in the past.

---

_Comment by @Daverball on 2025-05-14 06:07_

I'd suggest collecting all the SQAlchemy 2.0 specific rules into the `SA2` prefix, so making this `SA201` rather than `SA001`. That way we leave the door open to put generally useful rules into `SA0` and legacy rules into `SA1`, making it easier to read through the rules once there are more of them.

---

_Renamed from "[`sqlalchemy`] Implement first SQLAlchemy rule (`SA001`)" to "[`sqlalchemy`] Implement first SQLAlchemy rule (`SA201`)" by @kreathon on 2025-05-15 16:34_

---
