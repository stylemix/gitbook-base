# Customization

### Field layout

Change fields layout by passing `layout` prop:

```markup
<fields layout="horizontal" />
```

Currently form fields support the following built-in layouts:

* `horizontal`
* `vertical`
* `inline`
* `empty`

### Custom form structure

You can display each form fields individually by attribute name:

```markup
<div class="col">
  <field name="first_name" />
</div>
<div class="col">
  <field name="last_name" />
</div>
<field name="you_bio" />
```

{% hint style="warning" %}
Unfortunately in that case you can not render rest fields with `<fields />`. Otherwise you'll face to rendering duplicates of fields.
{% endhint %}

### Fields grouping

You can structure form layout by field groups. First, assign group ID for each field, then selectively render fields referencing by group ID.

#### Assign group ID in back-end

```php
public function fields()
{
    return [
        TextField::make('name')
            ->group('primary')
            ->required(),
        EmailField::make('email')
            ->group('primary')
            ->required(),
        TextareaField::make('message')
            ->required(),
        CheckboxField::make('newsletter')
            ->required(),
    ];
}
```

#### Assign group ID in front-end

```javascript
this.setFields(data.fields)
this.setFieldGroup('primary', ['name', 'email'])
```

#### Render fields by groups

```markup
<div class="col">
  <fields group="primary" />
</div>
<div class="col">
  <fields ungrouped />
</div>
```

Note that fields that were not assigned to any group are rendered by `ungrouped` property.

