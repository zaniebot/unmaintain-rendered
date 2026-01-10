---
number: 2172
title: Support for Odoo ORM
type: issue
state: closed
author: merlinz01
labels:
  - question
assignees: []
created_at: 2025-12-22T21:30:52Z
updated_at: 2025-12-23T00:43:57Z
url: https://github.com/astral-sh/ty/issues/2172
synced_at: 2026-01-10T01:52:52Z
---

# Support for Odoo ORM

---

_Issue opened by @merlinz01 on 2025-12-22 21:30_

### Question

Would you be interested in supporting the Odoo ORM in ty?

Quirks of the system that make type checking mess up:

- Ty already knows that `self.env["some.model.name"]` is a reference to a subclass of `odoo.models.BaseModel`. But it doesn't know specifically what model it is because it is not accessed by importing it.
- Odoo has its own system of model inheritance where fields and methods defined on a model are available to other models that have `_inherit` set to the `_name` of the first model.
- The model definitions are scattered through multiple directories each containing an array of Python modules. Model inheritance order is defined by dependencies in a `__manifest__.py` file in each module.

Example single module structure:

```
product_barcode_generator/
├── __init__.py
├── __manifest__.py
├── models
    ├── __init__.py
    ├── product_product.py
    └── product_template.py
```

Example model definition:

```python
from odoo import api, models


class ProductProduct(models.Model):
    _inherit = "product.product"

    @api.model_create_multi
    def create(self, vals_list):
        for vals in vals_list:
            if not vals.get("barcode"):
                vals["barcode"] = self.env["ir.sequence"].next_by_code("product.barcode")
        return super().create(vals_list)

    def action_generate_barcode(self):
        """Generate barcodes for products that don't have one."""
        for product in self:
            if not product.barcode:
                product.barcode = self.env["ir.sequence"].next_by_code("product.barcode")
```

This is what ty currently says about the above code:

```bash
$ ty check custom/product_barcode_generator/models/product_product.py
error[unresolved-attribute]: Object of type `BaseModel` has no attribute `next_by_code`
  --> custom/product_barcode_generator/models/product_product.py:11:35
   |
 9 |         for vals in vals_list:
10 |             if not vals.get("barcode"):
11 |                 vals["barcode"] = self.env["ir.sequence"].next_by_code("product.barcode")
   |                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
12 |         return super().create(vals_list)
   |
info: rule `unresolved-attribute` is enabled by default

error[unresolved-attribute]: Object of type `Self@action_generate_barcode` has no attribute `barcode`
  --> custom/product_barcode_generator/models/product_product.py:17:20
   |
15 |         """Generate barcodes for products that don't have one."""
16 |         for product in self:
17 |             if not product.barcode:
   |                    ^^^^^^^^^^^^^^^
18 |                 product.barcode = self.env["ir.sequence"].next_by_code("product.barcode")
   |
info: rule `unresolved-attribute` is enabled by default

error[unresolved-attribute]: Unresolved attribute `barcode` on type `Self@action_generate_barcode`.
  --> custom/product_barcode_generator/models/product_product.py:18:17
   |
16 |         for product in self:
17 |             if not product.barcode:
18 |                 product.barcode = self.env["ir.sequence"].next_by_code("product.barcode")
   |                 ^^^^^^^^^^^^^^^
   |
info: rule `unresolved-attribute` is enabled by default

error[unresolved-attribute]: Object of type `BaseModel` has no attribute `next_by_code`
  --> custom/product_barcode_generator/models/product_product.py:18:35
   |
16 |         for product in self:
17 |             if not product.barcode:
18 |                 product.barcode = self.env["ir.sequence"].next_by_code("product.barcode")
   |                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
info: rule `unresolved-attribute` is enabled by default

Found 4 diagnostics
```

### Version

ty 0.0.4

---

_Label `question` added by @merlinz01 on 2025-12-22 21:30_

---

_Comment by @carljm on 2025-12-22 22:37_

I think we will have to restrict special-cased support for libraries (if/when we add any -- we haven't yet) to the minimum number of extremely widely-used libraries. The maintenance burden of such special-cased support is significant, and sufficient to cast doubt on the wisdom of doing it at all, even for extremely popular libraries. So I doubt that we would add special-cased support for Odoo. I would strongly recommend to the developers of Odoo and other libraries to consider Python static type checkers in the development of these libraries.

---

_Closed by @carljm on 2025-12-22 22:37_

---
