# Entities

### Entity as extended Eloquent model

Entities are extended variant of Eloquent models with support of defining attributes \(properties\) for this model. Attributes give ability to describe each property in more convenient way. Attribute is responsible for such behaviors as:

* Data type & casting: String, Number, Date, Boolean, etc.
* Proper query building from Request parameters.
* Form input type & validation.
* Mass assignment & access.
* Extra options & features for your business logic.

To create an entity,  just extend your model from `\Stylemix\Base\Entity` and implement `attributeDefinitions` method:

```php
use Stylemix\Base\Attributes\Date;
use Stylemix\Base\Attributes\Id;
use Stylemix\Base\Attributes\Text;
use Stylemix\Base\Entity;

class Listing extends Entity
{

    public static function attributeDefinitions() : array
    {
        return [
            Id::make(),
            Text::make('title')
                ->fillable()
                ->required(),
            Number::make('level')
                ->integer()
                ->fillable(),
            Date::make('created_at'),
            Date::make('updated_at'),
            // ... other entity attributes
        ];
    }
}
```

Here is some usage case with Number attribute:

```php
$list1 = new Listing();
$list1->level = "31";

echo $list1->level; // 31 - integer due to cast
```

Build query by defined attributes:

```php
// Request:
// GET ?where[level][gt]=10

use Stylemix\Base\QueryBuilder;
use App\Listing;

// Builds query where 'level' greater than 10
$query = QueryBuilder::for(Listing::class)->build();

// Listing results
$results = $query->get();
```

{% page-ref page="../query-builder.md" %}

Build form fields for fillable attributes:

```php
use Stylemix\Base\Facades\EntityForm;
use App\Listing;

$fields = EntityForm::forAttributes(
    Listing::getAttributeDefinitions()->fillable()
);

// [{
//   "component": "text-field",
//   "attribute": "title",
//   "label": "Title",
//   "required": true,
// }, {
//   "component": "number-field",
//   "attribute": "level",
//   "label": "Level",
// }]
```

{% page-ref page="../entity-form.md" %}

