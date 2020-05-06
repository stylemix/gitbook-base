# Configuring attributes

### Common props for all attribute types

* `fillable(boolean $value = true)` - Is attribute fillable form input, should attribute appear in entity form. Default: false;
* `required(boolean $value = true)` - Is attribute should be required when validating request input. Default: false;
* `defaultValue(mixed $value)` - Default value newly created model. Default: null;
* `multiple(boolean $value = true)` - Is attribute multiple. Treats all input as indexed array of values. Default: false;
* `label(string $label)` - Label for attribute \(used in generating form field\). Default: converts attribute name for humanized text my\_number &gt; My number;
* `placeholder(string $label)` - Placeholder for attribute \(used in generating form field\). Default: null;
* `excludeFilter(boolean $value = true)` - Attribute is not available for filtering in [QueryBuilder](../query-builder.md);
* `excludeSearching(boolean $value = true)` - Attribute is not available for full text searching in [QueryBuilder](../query-builder.md);
* `excludeSort(boolean $value = true)` - Attribute is not available for sorting in [QueryBuilder](../query-builder.md);

