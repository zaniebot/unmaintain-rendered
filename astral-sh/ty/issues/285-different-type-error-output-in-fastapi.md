```yaml
number: 285
title: Different type error output in fastapi
type: issue
state: closed
author: alphavector
labels: []
assignees: []
created_at: 2025-05-08T23:11:25Z
updated_at: 2025-05-08T23:28:17Z
url: https://github.com/astral-sh/ty/issues/285
synced_at: 2026-01-12T15:54:22Z
```

# Different type error output in fastapi

---

_@alphavector_

### Summary

```
> git clone git@github.com:fastapi/fastapi.git
> cd fastapi
> git rev-parse --short HEAD
9a33ba46
```


mypy
```
> uvx --with-requirements requirements-tests.txt --with 'pydantic>=2.0.2,<3.0.0' mypy fastapi
Success: no issues found in 44 source files
```


ty
```
> uvx --with-requirements requirements-tests.txt --with 'pydantic>=2.0.2,<3.0.0' ty check --ignore 'unresolved-import' --ignore 'unused-ignore-comment' --color=never fastapi
```

<details>
  <summary>output</summary>

```shell
warning: lint:possibly-unresolved-reference: Name `TypeAdapter` used when possibly not defined
   --> fastapi/_compat.py:111:33
    |
110 |         def __post_init__(self) -> None:
111 |             self._type_adapter: TypeAdapter[Any] = TypeAdapter(
    |                                 ^^^^^^^^^^^
112 |                 Annotated[self.field_info.annotation, self.field_info]
113 |             )
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `TypeAdapter` used when possibly not defined
   --> fastapi/_compat.py:111:52
    |
110 |         def __post_init__(self) -> None:
111 |             self._type_adapter: TypeAdapter[Any] = TypeAdapter(
    |                                                    ^^^^^^^^^^^
112 |                 Annotated[self.field_info.annotation, self.field_info]
113 |             )
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `model_process_schema` used when possibly not defined
   --> fastapi/_compat.py:386:56
    |
384 |         definitions: Dict[str, Dict[str, Any]] = {}
385 |         for model in flat_models:
386 |             m_schema, m_definitions, m_nested_models = model_process_schema(
    |                                                        ^^^^^^^^^^^^^^^^^^^^
387 |                 model, model_name_map=model_name_map, ref_prefix=REF_PREFIX
388 |             )
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `model_process_schema` used when possibly not defined
   --> fastapi/_compat.py:386:56
    |
384 |         definitions: Dict[str, Dict[str, Any]] = {}
385 |         for model in flat_models:
386 |             m_schema, m_definitions, m_nested_models = model_process_schema(
    |                                                        ^^^^^^^^^^^^^^^^^^^^
387 |                 model, model_name_map=model_name_map, ref_prefix=REF_PREFIX
388 |             )
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `model_process_schema` used when possibly not defined
   --> fastapi/_compat.py:386:56
    |
384 |         definitions: Dict[str, Dict[str, Any]] = {}
385 |         for model in flat_models:
386 |             m_schema, m_definitions, m_nested_models = model_process_schema(
    |                                                        ^^^^^^^^^^^^^^^^^^^^
387 |                 model, model_name_map=model_name_map, ref_prefix=REF_PREFIX
388 |             )
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `model_process_schema` used when possibly not defined
   --> fastapi/_compat.py:386:56
    |
384 |         definitions: Dict[str, Dict[str, Any]] = {}
385 |         for model in flat_models:
386 |             m_schema, m_definitions, m_nested_models = model_process_schema(
    |                                                        ^^^^^^^^^^^^^^^^^^^^
387 |                 model, model_name_map=model_name_map, ref_prefix=REF_PREFIX
388 |             )
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `REF_PREFIX` used when possibly not defined
   --> fastapi/_compat.py:387:66
    |
385 |         for model in flat_models:
386 |             m_schema, m_definitions, m_nested_models = model_process_schema(
387 |                 model, model_name_map=model_name_map, ref_prefix=REF_PREFIX
    |                                                                  ^^^^^^^^^^
388 |             )
389 |             definitions.update(m_definitions)
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `REF_PREFIX` used when possibly not defined
   --> fastapi/_compat.py:387:66
    |
385 |         for model in flat_models:
386 |             m_schema, m_definitions, m_nested_models = model_process_schema(
387 |                 model, model_name_map=model_name_map, ref_prefix=REF_PREFIX
    |                                                                  ^^^^^^^^^^
388 |             )
389 |             definitions.update(m_definitions)
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `REF_PREFIX` used when possibly not defined
   --> fastapi/_compat.py:387:66
    |
385 |         for model in flat_models:
386 |             m_schema, m_definitions, m_nested_models = model_process_schema(
387 |                 model, model_name_map=model_name_map, ref_prefix=REF_PREFIX
    |                                                                  ^^^^^^^^^^
388 |             )
389 |             definitions.update(m_definitions)
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `REF_PREFIX` used when possibly not defined
   --> fastapi/_compat.py:387:66
    |
385 |         for model in flat_models:
386 |             m_schema, m_definitions, m_nested_models = model_process_schema(
387 |                 model, model_name_map=model_name_map, ref_prefix=REF_PREFIX
    |                                                                  ^^^^^^^^^^
388 |             )
389 |             definitions.update(m_definitions)
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `is_pv1_scalar_field` used when possibly not defined
   --> fastapi/_compat.py:411:17
    |
409 |         if field.sub_fields:  # type: ignore[attr-defined]
410 |             if not all(
411 |                 is_pv1_scalar_field(f)
    |                 ^^^^^^^^^^^^^^^^^^^
412 |                 for f in field.sub_fields  # type: ignore[attr-defined]
413 |             ):
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `is_pv1_scalar_field` used when possibly not defined
   --> fastapi/_compat.py:423:28
    |
421 |             if field.sub_fields is not None:  # type: ignore[attr-defined]
422 |                 for sub_field in field.sub_fields:  # type: ignore[attr-defined]
423 |                     if not is_pv1_scalar_field(sub_field):
    |                            ^^^^^^^^^^^^^^^^^^^
424 |                         return False
425 |             return True
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `REF_PREFIX` used when possibly not defined
   --> fastapi/_compat.py:467:62
    |
465 |         # This expects that GenerateJsonSchema was already used to generate the definitions
466 |         return field_schema(  # type: ignore[no-any-return]
467 |             field, model_name_map=model_name_map, ref_prefix=REF_PREFIX
    |                                                              ^^^^^^^^^^
468 |         )[0]
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `get_flat_models_from_fields` used when possibly not defined
   --> fastapi/_compat.py:471:18
    |
470 |     def get_compat_model_name_map(fields: List[ModelField]) -> ModelNameMap:
471 |         models = get_flat_models_from_fields(fields, known_models=set())
    |                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^
472 |         return get_model_name_map(models)  # type: ignore[no-any-return]
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `get_flat_models_from_fields` used when possibly not defined
   --> fastapi/_compat.py:486:18
    |
484 |         Dict[str, Dict[str, Any]],
485 |     ]:
486 |         models = get_flat_models_from_fields(fields, known_models=set())
    |                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^
487 |         return {}, get_model_definitions(
488 |             flat_models=models, model_name_map=model_name_map
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `get_model_definitions` used when possibly not defined
   --> fastapi/_compat.py:487:20
    |
485 |     ]:
486 |         models = get_flat_models_from_fields(fields, known_models=set())
487 |         return {}, get_model_definitions(
    |                    ^^^^^^^^^^^^^^^^^^^^^
488 |             flat_models=models, model_name_map=model_name_map
489 |         )
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `is_pv1_scalar_field` used when possibly not defined
   --> fastapi/_compat.py:492:16
    |
491 |     def is_scalar_field(field: ModelField) -> bool:
492 |         return is_pv1_scalar_field(field)
    |                ^^^^^^^^^^^^^^^^^^^
493 |
494 |     def is_sequence_field(field: ModelField) -> bool:
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `is_pv1_scalar_sequence_field` used when possibly not defined
   --> fastapi/_compat.py:498:16
    |
497 |     def is_scalar_sequence_field(field: ModelField) -> bool:
498 |         return is_pv1_scalar_sequence_field(field)
    |                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
499 |
500 |     def is_bytes_field(field: ModelField) -> bool:
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `server_urls` used when possibly not defined
    --> fastapi/applications.py:1005:37
     |
1003 |             async def openapi(req: Request) -> JSONResponse:
1004 |                 root_path = req.scope.get("root_path", "").rstrip("/")
1005 |                 if root_path not in server_urls:
     |                                     ^^^^^^^^^^^
1006 |                     if root_path and self.root_path_in_servers:
1007 |                         self.servers.insert(0, {"url": root_path})
     |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `server_urls` used when possibly not defined
    --> fastapi/applications.py:1008:25
     |
1006 |                     if root_path and self.root_path_in_servers:
1007 |                         self.servers.insert(0, {"url": root_path})
1008 |                         server_urls.add(root_path)
     |                         ^^^^^^^^^^^
1009 |                 return JSONResponse(self.openapi())
     |
info: `lint:possibly-unresolved-reference` is enabled by default

error: lint:invalid-type-form: The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
  --> fastapi/background.py:41:22
   |
39 |         self,
40 |         func: Annotated[
41 |             Callable[P, Any],
   |                      ^
42 |             Doc(
43 |                 """
   |
info: `lint:invalid-type-form` is enabled by default

error: lint:invalid-argument-type: Argument to this function is incorrect
   --> fastapi/dependencies/utils.py:128:9
    |
126 |     return get_sub_dependant(
127 |         depends=depends,
128 |         dependency=depends.dependency,
    |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Expected `(...) -> Any`, found `Unknown | ((...) -> Any) | None`
129 |         path=path,
130 |         name=param_name,
    |
info: Function defined here
   --> fastapi/dependencies/utils.py:142:5
    |
142 | def get_sub_dependant(
    |     ^^^^^^^^^^^^^^^^^
143 |     *,
144 |     depends: params.Depends,
145 |     dependency: Callable[..., Any],
    |     ------------------------------ Parameter declared here
146 |     path: str,
147 |     name: Optional[str] = None,
    |
info: `lint:invalid-argument-type` is enabled by default

error: lint:invalid-argument-type: Argument to this function is incorrect
   --> fastapi/dependencies/utils.py:139:47
    |
137 |         "A parameter-less dependency must have a callable dependency"
138 |     )
139 |     return get_sub_dependant(depends=depends, dependency=depends.dependency, path=path)
    |                                               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Expected `(...) -> Any`, found `Unknown | ((...) -> Any) | None`
    |
info: Function defined here
   --> fastapi/dependencies/utils.py:142:5
    |
142 | def get_sub_dependant(
    |     ^^^^^^^^^^^^^^^^^
143 |     *,
144 |     depends: params.Depends,
145 |     dependency: Callable[..., Any],
    |     ------------------------------ Parameter declared here
146 |     path: str,
147 |     name: Optional[str] = None,
    |
info: `lint:invalid-argument-type` is enabled by default

error: lint:invalid-argument-type: Argument to this function is incorrect
   --> fastapi/dependencies/utils.py:294:17
    |
292 |             sub_dependant = get_param_sub_dependant(
293 |                 param_name=param_name,
294 |                 depends=param_details.depends,
    |                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Expected `Depends`, found `Depends | None`
295 |                 path=path,
296 |                 security_scopes=security_scopes,
    |
info: Function defined here
   --> fastapi/dependencies/utils.py:118:5
    |
118 | def get_param_sub_dependant(
    |     ^^^^^^^^^^^^^^^^^^^^^^^
119 |     *,
120 |     param_name: str,
121 |     depends: params.Depends,
    |     ----------------------- Parameter declared here
122 |     path: str,
123 |     security_scopes: Optional[List[str]] = None,
    |
info: `lint:invalid-argument-type` is enabled by default

warning: lint:possibly-unbound-attribute: Attribute `field_info` on type `ModelField | Unknown | None` is possibly unbound
   --> fastapi/dependencies/utils.py:310:23
    |
308 |             continue
309 |         assert param_details.field is not None
310 |         if isinstance(param_details.field.field_info, params.Body):
    |                       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
311 |             dependant.body_params.append(param_details.field)
312 |         else:
    |
info: `lint:possibly-unbound-attribute` is enabled by default

warning: lint:possibly-unresolved-reference: Name `cm` used when possibly not defined
   --> fastapi/dependencies/utils.py:560:44
    |
558 |     elif is_async_gen_callable(call):
559 |         cm = asynccontextmanager(call)(**sub_values)
560 |     return await stack.enter_async_context(cm)
    |                                            ^^
    |
info: `lint:possibly-unresolved-reference` is enabled by default

error: lint:unresolved-attribute: Type `Mapping[str, Any]` has no attribute `getlist`
   --> fastapi/dependencies/utils.py:721:17
    |
719 |     alias = alias or field.alias
720 |     if is_sequence_field(field) and isinstance(values, (ImmutableMultiDict, Headers)):
721 |         value = values.getlist(alias)
    |                 ^^^^^^^^^^^^^^
722 |     else:
723 |         value = values.get(alias, None)
    |
info: `lint:unresolved-attribute` is enabled by default

warning: lint:possibly-unresolved-reference: Name `results` used when possibly not defined
   --> fastapi/dependencies/utils.py:870:17
    |
868 |             ) -> None:
869 |                 result = await fn()
870 |                 results.append(result)  # noqa: B023
    |                 ^^^^^^^
871 |
872 |             async with anyio.create_task_group() as tg:
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `status_code` used when possibly not defined
   --> fastapi/openapi/utils.py:351:62
    |
349 |                     if isinstance(status_code_param.default, int):
350 |                         status_code = str(status_code_param.default)
351 |             operation.setdefault("responses", {}).setdefault(status_code, {})[
    |                                                              ^^^^^^^^^^^
352 |                 "description"
353 |             ] = route.response_description
    |
info: `lint:possibly-unresolved-reference` is enabled by default

warning: lint:possibly-unresolved-reference: Name `status_code` used when possibly not defined
   --> fastapi/openapi/utils.py:370:21
    |
368 |                         response_schema = {}
369 |                 operation.setdefault("responses", {}).setdefault(
370 |                     status_code, {}
    |                     ^^^^^^^^^^^
371 |                 ).setdefault("content", {}).setdefault(route_response_media_type, {})[
372 |                     "schema"
    |
info: `lint:possibly-unresolved-reference` is enabled by default

error: lint:no-matching-overload: No overload of bound method `__init__` matches arguments
   --> fastapi/params.py:84:18
    |
 82 |           self.include_in_schema = include_in_schema
 83 |           self.openapi_examples = openapi_examples
 84 |           kwargs = dict(
    |  __________________^
 85 | |             default=default,
 86 | |             default_factory=default_factory,
 87 | |             alias=alias,
 88 | |             title=title,
 89 | |             description=description,
 90 | |             gt=gt,
 91 | |             ge=ge,
 92 | |             lt=lt,
 93 | |             le=le,
 94 | |             min_length=min_length,
 95 | |             max_length=max_length,
 96 | |             discriminator=discriminator,
 97 | |             multiple_of=multiple_of,
 98 | |             allow_inf_nan=allow_inf_nan,
 99 | |             max_digits=max_digits,
100 | |             decimal_places=decimal_places,
101 | |             **extra,
102 | |         )
    | |_________^
103 |           if examples is not None:
104 |               kwargs["examples"] = examples
    |
info: `lint:no-matching-overload` is enabled by default

error: lint:no-matching-overload: No overload of bound method `__init__` matches arguments
   --> fastapi/params.py:540:18
    |
538 |           self.include_in_schema = include_in_schema
539 |           self.openapi_examples = openapi_examples
540 |           kwargs = dict(
    |  __________________^
541 | |             default=default,
542 | |             default_factory=default_factory,
543 | |             alias=alias,
544 | |             title=title,
545 | |             description=description,
546 | |             gt=gt,
547 | |             ge=ge,
548 | |             lt=lt,
549 | |             le=le,
550 | |             min_length=min_length,
551 | |             max_length=max_length,
552 | |             discriminator=discriminator,
553 | |             multiple_of=multiple_of,
554 | |             allow_inf_nan=allow_inf_nan,
555 | |             max_digits=max_digits,
556 | |             decimal_places=decimal_places,
557 | |             **extra,
558 | |         )
    | |_________^
559 |           if examples is not None:
560 |               kwargs["examples"] = examples
    |
info: `lint:no-matching-overload` is enabled by default

error: lint:call-non-callable: Object of type `None` is not callable
   --> fastapi/routing.py:212:22
    |
211 |     if is_coroutine:
212 |         return await dependant.call(**values)
    |                      ^^^^^^^^^^^^^^^^^^^^^^^^
213 |     else:
214 |         return await run_in_threadpool(dependant.call, **values)
    |
info: `lint:call-non-callable` is enabled by default

error: lint:call-non-callable: Object of type `None` is not callable
   --> fastapi/routing.py:383:19
    |
381 |                 )
382 |             assert dependant.call is not None, "dependant.call must be a function"
383 |             await dependant.call(**solved_result.values)
    |                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
384 |
385 |     return app
    |
info: `lint:call-non-callable` is enabled by default

error: lint:unresolved-attribute: Type `<module 'fastapi'>` has no attribute `exceptions`
   --> fastapi/utils.py:98:15
    |
 96 |         return ModelField(**kwargs)  # type: ignore[arg-type]
 97 |     except (RuntimeError, PydanticSchemaGenerationError):
 98 |         raise fastapi.exceptions.FastAPIError(
    |               ^^^^^^^^^^^^^^^^^^
 99 |             "Invalid args for response field! Hint: "
100 |             f"check that {type_} is a valid Pydantic field type. "
    |
info: `lint:unresolved-attribute` is enabled by default

Found 35 diagnostics

``` 
  
</details>


### Version

ty 0.0.0-alpha.7 (905a3e1e5 2025-05-07)

---

_Comment by @carljm on 2025-05-08 23:21_

Thanks for the report!

We don't consider differing results from mypy to be a bug in and of itself. Many of these specific diagnostics do look like false positives, which we would consider a bug (and some of them will already be gone in the next ty release); I haven't checked all of them.

We do look at false positives broadly across the ecosystem and prioritize the ones we see most frequently. I suspect many of these false positives are already represented there (and many we already have open issues for). If you'd like `fastapi` to also be considered in that analysis, the best thing would be to submit a PR to include it in https://github.com/hauntsaninja/mypy_primer/

I'm going to close this issue, not because we don't want to fix the false positives here, but just because a blanket issue for "everything affecting fastapi" duplicates other existing issues and is too broad to be useful.

---

_Closed by @carljm on 2025-05-08 23:21_

---
