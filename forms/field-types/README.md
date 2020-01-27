# Field types

#### Prerequisites

Every field type below named as corresponding class name, i.e. `TextField` is `\Stylemix\Base\Fields\TextField`. 

{% hint style="info" %}
Every field types instance implements fluent method chaining \(like column declarations in [Laravel migrations](https://laravel.com/docs/5.8/migrations#columns)\). For example to fill property `$integer = true` for NumberField, just call `->integer()`. All declared properties below can be filled the same way.
{% endhint %}

#### Common properties:

* `boolean $required = false` If true, adds `required` validation rule
* `boolean $nullable = false` If true, adds `nullable` validation rule
* `boolean $multiple = false` If true, adds `array` validation rule
* `string $label` Label for field. By default filled by humanized version of fields $attribute.
* `string $placeholder` Placeholder for field.
* `string $helpText` Help text usually appears under field. 
* `mixed $initalValue` Value that is filled into model if value is missed.
* `string $component` Vue component name without `form-` prefix. Snake case.

### TextField

* Validation rules: `string`
* Vue component: `text-field`

To give specific input type, call one of methods: `typeColor()`, `typeDate()`, `typeEmail()`, `typeMonth()`, `typeSearch()`, `typeTel()`, `typeText()`, `typeTime()`, `typeUrl()`.

### NumberField

* Validation rules: `numeric` or `integer`
* Vue component: `number-field`
* Properties:
  * `boolean $integer = false`

### EmailField

* Validation rules: `email`
* Vue component: `text-field` with `type: 'email'`

### PasswordField

* Validation rules: `string`
* Vue component: `text-field` with `type: 'password'`

Field is used to fill target model with password hash \(`Hash::make($password)`\). When resolving values from existing model - resolves to null.

### DatetimeField

* Validation rules: `date`
* Vue component: `datetime-field` 

Component properties:

* `options`
  * type: `Object`
  * Options for input widget. Available properties:

    * `outputFormat` - date format for model value. Default to globally configuration `dateTimeModelFormat`.
    * `formatted` - date format for display context. Defaults to global configuration `dateTimeDisplayFormat`.
    * `onlyTime` - make time picker only

    Date formats should be compatible for [momentjs format](https://momentjs.com/docs/#/parsing/string-format/). Example:

    ```text
    ->options([
       'outputFormat' => 'YYYY-MM-DD HH:mm:ss',
    ]),
    ```

### SelectField

* Validation rules:  -
* Vue component: `select-field`

Options declaration:

```php
->options([
    ['value' => 'php', 'label' => 'PHP Language'],
    ['value' => 'js', 'label' => 'JS Language'],
])
```

### CheckboxField

* Validation rules: `boolean`
* Vue component: `checkbox-field`

Value from request is casted to `boolean` when filling.

### CheckboxesField

* Validation rules: -
* Vue component: `checkboxes-field` \(Date/time picker\)

Options declaration is similar to [SelectField](./#selectfield). Value from request is casted to `array`.

### RadiosField

* Validation rules: -
* Vue component: `radios-field`

Options declaration is similar to [SelectField](./#selectfield). Value from request is casted to `array`.

### FileField

* Validation rules: `file`
* Vue component: `file-field` 

### TextareaField

* Validation rules: `string`
* Vue component: `textarea-field` 

### EditorField

* Validation rules: `string`
* Vue component: `editor-field` \(WYSIWYG text editor\)

### LocationField

* Validation rules: -
* Vue component: `location-field` \(Places autocomplete input\)
* Properties:
  * `array $withMap = false` - Use map picker in front-end \(see details in corresponding component docs\).
  * `string $types = null` - Autocomplete result types of [Google Places API](https://developers.google.com/places/web-service/autocomplete#place_types).

Result value type is object with the following properties:

```javascript
{
    latlng, // Comma-seperated string value
    address,
    city,
    zip,
    region,
    country, // ISO 3166-1 alpha-2  code
    zoom, // Zoom level for map
}
```

### RelationField

* Validation rules: -
* Vue component: `relation-field` 
* Properties:
  * `boolean $ajax = true` - Use AJAX autocompletion when searching.
  * `string $ajaxUrl = null` - Custom AJAX url if can't be resolved automatically
  * `array $ajaxParams = null` Additional AJAX query parameters
  * `array $options = []` - Initial options if not using AJAX
  * `boolean $preventCasting = false` - Prevent the value from request casting to integer
* Methods:
  * `setQuery($builder)` - Set the query that will be used to resolve options for non-AJAX mode and resolve current selected options. `$builder` - should be instance of `\Illuminate\Database\Eloquent\Builder`

### AttachmentField

Used as field for Attachment attribute. Works with attachment IDs assigned to model, and File uploads for new attachments. For File uploads request should be sent as [multipart/form-data](https://en.wikipedia.org/wiki/MIME#Form-Data).

* Validation rules: -
* Vue component: `attachment-field` 
* Properties:
  * `string $mediaTag = <attribute without "_id">` - Tag name of attachments

### RepeaterField

Used to make some field / set of fields have more than one value:

#### Single field

```php
RepeaterField::make('phone_numbers', TextField('number'))
```

That will give the following data structure:

```php
[
    'phone_numbers' => [
        '+123456789',
        '+234567890',
        // ...etc
    ],
    
    // ... other model values
]
```

#### Fields set

```php
RepeaterField::make('children', [
    TextField::make('name'),
    NumberField::make('age'),
])
```

That will give the following data structure:

```php
[
    'children' => [
        [
            'name' => 'John',
            'age' => 14,
        ],
        [
            'name' => 'Abigale',
            'age' => 9,
        ],
        // ...etc
    ],

    // ... other model values
]
```

