# Attribute types

### Text

Used for single line text values.  
Cast: `string`.

### Number

Used for integer and float numbers.  
Cast: `integer` or `float`

Props:

* `integer()` - for integer number only

### Boolean

Used for boolean \(true/false\).  
Cast: `boolean`

### Id

Used for defining primary key. Prevents from filling from input. $name can be omitted when defining.  
Cast: `integer`

### LongText

Used for multi-line and long texts.  
Cast: `string`

### Datetime

Date timestamps.  
Cast: `datetime`

### Enum

Used for values taken from Enum classes. More about [Enum classes](https://artkonekt.github.io/enum/#/)  
Cast: `string`  
To use, provide enum class name as second argument:

```php
Enum::make('gender', Gender::class)
```

### Slug

Special attribute that can auto-generate URL-friendly slug from another attribute.  
Cast: `string`  
Props:

* `generateFrom(string $attribute)` - Attribute name to generate slug from. By default takes from `title`.

### Attachment

Used for store attachment files. Internally attribute uses [https://github.com/plank/laravel-mediable](https://github.com/plank/laravel-mediable) library to manage attachments.

When defining attribute, the name will be used as attachment tag for Mediable package. Besides, IDs of attached attached media files is stored in model property `$name . "_id"`.

```php
protected static function attributeDefinitions(): array
{
    return [
        Attachment::make('brochure'),
    ];
}

$listing = new Listing();
$listing->brochure_id = Request::file('brochure_id');
$listing->brochure_id = 'http://example.com/some-file-url';
$listing->save();

$listing = Listing::find(1);
dd($listing->brochure); // Attachment object
echo $listing->brochure_id; // Attachment ID
```

### Location

Stores latitude, longitude and address data.  
Internal data structure:

```text
Object {
    latlng: string
    address: string
    city: string
    zip: string
    region: string
    country: string
}
```

